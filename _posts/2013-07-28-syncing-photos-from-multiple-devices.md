---
ID: 1003
post_title: Syncing photos from multiple devices
author: Thomas Ardal
post_date: 2013-07-28 06:31:59
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/syncing-photos-from-multiple-devices/
published: true
---
This is an off topic post, but since I'm on vacation, I thought it would be alright to write a little about something other than code. Vacation means taking time for family, friends and hobbies. One of the things that have bugged me for a long time is the mess of photos we have, which were taken with multiple different devices during the last 20 years.

<a href="http://thomasardal.com/wp-content/uploads/2013/07/folders.png"><img class="alignright size-full wp-image-1023" style="margin-left: 10px;" alt="folders" src="http://thomasardal.com/wp-content/uploads/2013/07/folders.png" width="98" height="112" /></a>

You probably know it all too well: a Photos folder on some shared dir, containing both ordered and unordered files from the last decades. In fact, I was already in good shape before structuring this further, cause I already had a photos folder on my <a href="http://www.dpbolvw.net/click-7209205-10897007" target="_blank">Drobo</a> containing all of the photos from our digital camera, sorted by the folder pattern “[year]_[month]_[day]”.

The underscore is caused by legacy; my first digital camera uploaded the files this way. I choose to stick with this pattern a long time ago, even though it doesn’t follow the Danish date pattern, but sorting the folders by date actually works this way.

So what’s the problem here? Our digital camera already uploads the photos into the correct folders, but we take more pictures with our mobile phones these days, so I wanted a solution for those devices, which plugged into the existing setup. Trying to explain this without any illustration would be hard, so here’s my poor attempt to do anything graphical:

<a href="http://thomasardal.com/wp-content/uploads/2013/07/sync.png"><img class="aligncenter size-full wp-image-1004" alt="sync" src="http://thomasardal.com/wp-content/uploads/2013/07/sync.png" width="827" height="983" /></a>

To start from the top, both mobile phones and digital cameras now sync photos directly with my server. I have a <a href="http://www.nokia.com/us-en/phones/phone/lumia820/" target="_blank">Lumia 820</a>, which syncs to a folder on my server through <a href="https://skydrive.live.com/" target="_blank">SkyDrive</a>. SkyDrive is built into Windows Phone and even though other solutions are supported on Windows Phone, I did not find anything that worked like I wanted. My wife’s <a href="http://www.apple.com/iphone/" target="_blank">iPhone 5</a> syncs with <a href="http://db.tt/1Unk7HTq" target="_blank">Dropbox</a>, which like SkyDrive, syncs to a folder on the server. I bought the brilliant <a href="http://www.eye.fi/" target="_blank">Eye-Fi SD card</a> for my <a href="http://www.canon.com/camera-museum/camera/dcc/data/2011-/2011_ixy_410f.html" target="_blank">Canon Ixus</a> digital camera a couple of years ago. The SD card automatically syncs new photos with my server when it is in range of my hotspot.

In the flow, all of the photos are now stored on my server. I own a Drobo box with 4 discs, giving me more than 5 TB available space (pretty nice, right?). All of our photos, videos, music and movies are stored on the Drobo, which is why I wanted new photos from the three devices there as well. Remember the file pattern described earlier? The new photos should of course be sorted beneath new folders as well.

I found a nice little application called <a href="http://www.robobasket.com/" target="_blank">RoboBasket</a>. RobotBasket places itself in the tray and constantly looks for new photo files in the SkyDrive, Dropbox and Eye-Fi folders. When new files arrive, the files are automatically copied to the Drobo in a sub-folder named by the EXIF data in each file. Even though the Drobo supports redundancy, I still need remote backup to be able to sleep at night. I bought Mozy years back, but switched to <a href="http://www.backblaze.com/partner/af3612" target="_blank">Backblaze</a> when Mozy prices sky-rocketed.

The solution may look complex and maybe it is. But best of all: it works exactly like I want it to :) Hope this blog post has inspired you to handle your photos!