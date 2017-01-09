---
ID: 1651
post_title: >
  Deleting contents of a folder containing
  files with open file handles using
  PowerShell
author: Thomas Ardal
post_date: 2015-10-06 09:41:09
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/deleting-contents-of-a-folder-containing-files-with-open-file-handles-using-powershell/
published: true
---
Like every other project I've participated in, I recently experienced problems when trying to implement continuous delivery of Windows Services and websites. The problem arise when trying to update some files in a remote directory using PowerShell. The remoting part isn't an issue this time, because the customer I'm currently working for uses the awesome <a href="https://octopus.com/" target="_blank">Octopus Deploy</a>, which executes all of your PowerShell code through a remote agent called a tentacle. But problems start showing up when trying to delete the contents of the destination folder because of someone (the service, the website, somebody logged into the server and opened something in Notepad etc.) having file locks on some of the files in the folder I would like to delete. PowerShell errors like IOException, UnauthorizedException and similar keeps showing up all over the place.

I wanted to fix this once and for all. Unfortunatly the Remove-Item cmdlet in PowerShell doesn't offer the option of deleting files even though someone have a file lock. An approach would be to use a tool like <a href="https://technet.microsoft.com/en-us/sysinternals/bb896655.aspx" target="_blank">Handle</a> from Sysinternals, but I wanted something able to run without depending on anything else but PowerShell on the server. After combining my Googling and (limited) PowerShell skills, I came up with the following script:

<pre class="lang:powershell">function KillProcessesWithHandles
{
    param([string]$path)

    $allProcesses = Get-Process
    
    # Start by closing all notepad processes. Someone may have left a logor config file open
    $allProcesses | where {$_.Name -eq "notepad"} | Stop-Process -Force -ErrorAction SilentlyContinue
    
    # Then close all processes running inside the folder we are trying to delete
    $allProcesses | where {$_.Path -like ($path + "*")} | Stop-Process -Force -ErrorAction SilentlyContinue
    
    # Finally close all processes with modules loaded from folder we are trying to delete
    foreach($lockedFile in Get-ChildItem -Path $path -Include * -Recurse) {
        foreach ($process in $allProcesses) {
            $process.Modules | where {$_.FileName -eq $lockedFile} | Stop-Process -Force -ErrorAction SilentlyContinue
        }
    }
}</pre>

A quick walkthrough. The <code>KillProcessesWithHandles</code> function takes <code>path</code> as the input. This path is used as the base path for looking for files. Since open Notepad processes are a pattern I've experienced again and again, I start by stopping all notepad processes using a <code>where</code> filter on a list of all processes. The <code>where</code> filter is pretty sweat and easy to use when coming from a LINQ background. For each process, I call the <code>Stop-Process</code> cmdlet, which gracefully stops each instance or continue silently on errors.

Next issue would be running processes inside the folder we are trying to delete. To find these, I use the list of all processes again and search for instances with a path starting with the path of the folder we are trying to delete.

Finally I found a <a href="http://stackoverflow.com/questions/958123/powershell-script-to-check-application-thats-locking-a-file/13508676#13508676" target="_blank">script</a> on Google which I adjusted for my need. This script looks at modules loaded by all running processes. If any process loaded modules from inside the folder we are trying to delete, I close that process. This could be a Windows Service or an IIS website which have loaded an assembly or similar.

To end this post, I would love to hear from you if you've had similar problems and how you've decided to solve those.