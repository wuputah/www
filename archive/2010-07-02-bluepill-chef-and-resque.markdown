---
title: Bluepill, chef, and resque
date: 2010-07-02
layout: single
---

[Bluepill](http://github.com/arya/bluepill) is an alternative for process monitoring, competing in the same space as heavyweight [monit](http://mmonit.com/monit/) and hubristic [god](http://god.rubyforge.org/). Like `god`, bluepill itself and its recipes are written in pure Ruby. [Bluepill's goal](http://asemanfar.com/Why-We-Wrote-Bluepill) is to be very lightweight, and while it weighs in at greater than 1400 LOC, it's much smaller than both `monit` and `god`, and easily wins over `god` on memory usage:

<p class="img"><img src="http://asemanfar.com/system/bluepill_memory_usage.png" alt="Bluepill vs God memory usage" /></p>

At WegoWise, we began to investigate using bluepill on recommendation from EngineYard as bluepill would be an independent system apart from EY's own use of monit. We were previously using monit to monitor and control our [resque](http://github.com/defunkt/resque) processes; bluepill would replace this role.

### The Good

There are some very nice things about bluepill. Bluepill removes the need to create `init.d`-style scripts to control your processes. Bluepill will handle creating PID files, daemonizing, and sending a standard `TERM` signal to end the process (as well as number of other handy things). For our resque processes, the only truly necessary configuration is a start command.

If that's the case, then why is our resulting chef recipe and bluepill template ([which we've released to the public](http://github.com/wegowise/chef-recipes/tree/master/resque)) so complicated? Well, most of the complication is queue logic, but there's some other tricks in there that are worth explaining.

### The Bad

Overall, our experience with bluepill has been very positive, but it is not without a few drawbacks. Bluepill doesn't do graceful shutdowns, nor will it ever issue a `KILL` signal if the process becomes unresponsive to `TERM`. To solve this, we included a [<code>QUIT</code>&rarr;<code>TERM</code>&rarr;<code>KILL</code> signal chain in our bluepill configuration](http://github.com/wegowise/chef-recipes/blob/b00c047f2597316eb3a4420bf09ae5ad0df2e4c9/resque/templates/default/resque.pill.erb#L32-45).

Another problem is there's no way to set up alerts should something go terribly wrong (we've yet to solve this problem). For instance, bluepill allows you to monitor flapping (rapid restarting), but bluepill will simply unmonitor the process when this occurs, and you will be none the wiser. This also occurs is if the process doesn't start, stop, or restart within the allotted grace time. Therefore, a shutdown script like the one we used is a real necessity.

For now, we've solved our most critical issue directly in our pill (each bluepill configuration file is called a _pill_), but we might look into adding signal chains and alerts to bluepill itself. Fortunately, resque is quite stable and so alerts--even using a `TERM` signal--has yet to be necessary, and bluepill has been rock solid as well.
