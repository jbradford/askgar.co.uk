---
layout: post
title:  "Rearchitecting using the Event Store: Getting Started"
date:   2013-05-02 12:00:00
categories: event store eventstore joliver ravendb
---
Being part of the [Event Store](http://www.geteventstore.com/) team, and having written a number of projects using the [JOliver Event Store](https://github.com/joliver/EventStore), taking the opportunity to convert an existing project to the Event Store while working on a major new version was an easy decision to make.  At the same time we will be phasing out [RavenDB](http://ravendb.net/) and switching to in memory view models, which can be rebuilt quickly, and have much lower overheads than RavenDB.

I plan on documenting the process of getting the Event Store up and running, and how we go about converting our current methods to utilize the features of the Event Store.

In this first introduction post I’m going to focus on getting, and setting up the Event Store, and how to use the test client to ensure the Event Store node is functioning correctly.

## Getting the Event Store

Head on over to the [downloads page](http://download.geteventstore.com/) and get the latest server build for your platform.  Extract the file to the location you wish to run the Event Store from, a separate data directory can be specified using command line parameters when you run the server.

Note: Event Store is built for 64-bit CPUs so it is essential to be running the 64-bit version of your chosen OS.  If you plan on using Linux then you will need to run a patched version of Mono, details on how to do this are available from the [GitHub page](https://github.com/EventStore/EventStore#prerequisites-1).

## Running the Event Store

The [Event Store docs page](http://docs.geteventstore.com) contains detailed documentation on [running the Event Store](http://docs.geteventstore.com/introduction/), I will repeat the important commands here.

On Windows and .NET:

{% highlight powershell %}
EventStore.SingleNode.exe --db .\ESData --run-projections
{% endhighlight %}
On Linux and Mono:

{% highlight bash %}
mono EventStore.SingleNode.exe --db ./ESData --run-projections
{% endhighlight %}
By default the Event Store server will create its database in a temporary location, and each run will result in a new database being created, using the –db command line parameter will allow for the database directory to be set wherever you wish.  By default projections are disabled as they are currently considered an experimental feature, using the –run-projections parameter will enable them.

Note: On Windows the user that runs the Event Store server must have permission to listen to incoming HTTP requests, details on how to do this are available on [MSDN](http://msdn.microsoft.com/en-us/library/ms733768.aspx).  For testing purposes running the Event Store server as administrator will accomplish this.

Using the Test Client

In the Event Store folder there is also a test client, this can be used for reading from and writing to an Event Store server.  The command to run this is:

{% highlight powershell %}
EventStore.TestClient.exe -i 127.0.0.1 -t 1113 -h 2113
{% endhighlight %}
These three parameters set the IP address, TCP and HTTP ports on the Event Store server to connect to, the defaults are those shown in the above command, using the server as detailed above means these do not need to be specified.

When the TestClient is running a prompt will accept commands.  To test the server with a write flood we will use:

{% highlight bash %}
>>> wrfl 10 1000000
{% endhighlight %}
This will send 1000000 test events to the server using 10 threads and will report how many request per second were processed by the Event Store every 100000 writes.

Presuming it has been successful, you should expect to see an output similar to this

{% highlight bash %}
[21724,13,12:25:58.607]
##teamcity[buildStatisticValue key='WRFL-10-1000000-reqPerSec' value='16005']
[21724,13,12:25:58.607]
##teamcity[buildStatisticValue key='WRFL-10-1000000-failureSuccessRate' value='0']
[21724,13,12:25:58.607]
##teamcity[buildStatisticValue key='WRFL-c10-r1000000-st1000-s256-reqPerSec' value='16005']
[21724,13,12:25:58.607]
##teamcity[buildStatisticValue key='WRFL-c10-r1000000-st1000-s256-failureSuccessRate' value='0']
{% endhighlight %}
This will tell you the average number of requests per second to the Event Store (in this case 16005) and the failure rate (which in this case is 0).