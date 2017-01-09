---
ID: 1387
post_title: Another take on an agile process
author: Thomas Ardal
post_date: 2014-08-04 07:00:55
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/another-take-on-an-agile-process/
published: true
---
Looking through this year’s program for <a href="http://gotocon.com/aarhus-2014/" target="_blank">GOTO Aarhus</a>, I quickly spotted a trend. Even though the Scrum training is there, the usual talks titled something like, “This is how you do Scrum” and “An introduction to Kanban” are missing from this year’s program. Don’t get me wrong; I see this as a good thing. Having worked at multiple companies which were all trying to run perfect Scrum or perfect Kanban taught me, that running “perfect” <insert random process> simply doesn’t make sense. Agile is all about inspect and adapt. I can’t count the number of times I’ve argued with team lead types of persons, who were trying to run Scrum by the book, rather than doing what’s right for the project. I even wrote a <a href="http://thomasardal.com/scrum-kanban-agile-who-the-f-cares/">blog post</a> about that same subject last year.

This post documents how we’ve evolved the Scrum/Kanban processes at the team I’m working on at <a href="http://www.ebaycareers.com/home.aspx" target="_blank">eBay</a>. It may be tl;dr for some, but I think it’s important to describe each development stage in detail. Unlike the official process books, I’m not saying that this would work for everyone. Furthermore, our process probably won’t look the same a year from now. Again, inspect and adapt folks.

Our process is focused around a pull oriented process, visualized on two whiteboards as shown on the following diagram:

<a href="http://thomasardal.com/wp-content/uploads/2014/07/process.png"><img src="http://thomasardal.com/wp-content/uploads/2014/07/process-580x138.png" alt="process" width="580" height="138" class="aligncenter size-medium wp-image-1388" /></a>

The physical boards are split between the QA Spec column and the Ready column, but that’s because of the dimensions of the board and nothing else.

Every task we need to do is created in Jira. I’m not in love with Jira or anything, but that’s what eBay bought for us. Luckily, we don’t use any of Jira’s failed attempts to implement Scrum/Kanban boards in software. Furthermore, Jira is pretty good for documenting discussions.

Let’s get back to the diagram. The first column, which is named Backlog, contains all of the tasks that we think we should do within the next 3 months or so. Tasks are pretty much just a headline at this point. The developers work with the product owners to prioritize which tasks should be solved next. The POs knows what the users want/need and the developers know which technical tasks we should solve next.

The next three columns are used to prepare each task for development. The reason for having three columns may look a bit waterfall, but they are there to make sure that product owner, developers and QA looks at each task. In this phase, the product owners write a detailed description of the task. A developer (typically the tech lead), as well as the QA responsible, reads all of the tasks and sends questions back to the POs. When everybody agrees what to implement, the task is given an estimate in story point and moved to the ready column. The reason for splitting this stage into three columns does not necessarily mean that we cannot work together on getting a task ready. People involved in this phase are typically busy and we have a hard time finding time to sit down all at the same time to discuss a task.

The ready column acts as your To-do list in Kanban. All of the tasks are prioritized and developers always take from the top. Priorities can be changed from minute to minute, but only before a developer moves a task into Development. Notice the simple queue system implemented using simply a whiteboard marker. After having watched Silicon Valley, I’ve tried to convince the team to call the Ready column “ICEBOX”, but we’ve always called it “The Snake”, and this name will probable stick :).

When a developer is ready for work, they take the first task from the top and put it onto the Development column. We have a rule that you may only start a new task, if no-one else needs help closing the task they are working on. We try to pair program on anything complex, without being too extreme about it. Also there are a limited number of swim lanes available. This way we limit work in progress as specified in the Kanban process.

Each task needs different actions in order to be marked as done. At first we had an In Progress column, but somehow we sometimes forgot to do a code review and other essential actions to be able to release it. I personally hate checklists, but the team convinced me that a simple checklist would be a good idea to remember to do what we agreed to on every task. In the end, the five column checklist turned out to work pretty well: Kick Off, Development, Review, QA and Demo. Some of the actions are dependent on the action to the left being done, but Review, QA and Demo are not always executed in that order.

The Kick Off action is always the first thing to do when starting a new task. We don’t do sprint planning, and that’s why the developers need an introduction to each task, before starting to work on them. Participants in a Kick Off are always a developer and a product owner, but it sometimes makes sense to include QA and/or a Tech Lead.

Development pretty much speaks for itself. As mentioned, we pair program on a lot of tasks and love writing unit tests.

When development is finished, the developer(s) working on the task are responsible for finding another member of the team to help to do a code review and test. We use pull requests on GitHub for code review and created a 100 % automatic deployment of all feature branches using TeamCity.

The demo phase includes the developer showing it to the product owner and other roles interested in the feature that has been made. A demo is nothing fancy like the sprint review in Scrum and can be on localhost as well. Because we do continuous delivery of every commit to a new environment, the product owners typically play around with the features themselves. We get a lot of valuable feedback this way that I haven’t experienced in the past when doing Sprint Review.

When everything is done, the feature is merged into our develop branch. We use git-flow to handle the branching flow, but plan to do something simpler in the future. At the moment, we bundle features and release them together, but we are working against releasing every feature into production.