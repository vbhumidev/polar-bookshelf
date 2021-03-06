
# How do I import my existing data?

If you have a large number of PDF files that you would like to import you can 
select ```File | Import``` and select multiple files and they will all be 
imported into your repository.  

This process is fairly quick taking about 1 minute per 100-200MB.   

# Where is my data kept?

Your data is kept in a ```.polar``` directory in your home dir.  

This changes between platforms depending if you're on Windows, MacOS or Linux.

You can find out the exact directory by going to ```Help | About```.

# Why can't I open files directly?  

Polar has somewhat strict data requirements for the files it's managing.  The 
goal of Polar is to keep your data for as long as it's valuable to you so we 
need to make sure it's always consistent.

By using a dedicated data directory we can avoid accidental deletions of files, 
mutations of files, etc.

Additionally, since all your data is now in one place, we can support features
like cloud sync, filesystem consistency checks, sharing, etc. 

Some of these features aren't implemented yet but are on the roadmap.

# Can I export data from Polar?

Yes.  The PDF and PHZ (web capture) files are in your ```stash``` directory in 
your local datastore.  You can just copy them out if you wish to export.

The annotations are another issue.  We don't currently have any exporters defined
to export the annotations but the file is in JSON format which means anyone could
easily write an exporter.   

# How do I build and run from source?

Polar is VERY easy to compile from source.  Polar is based on Typescript, Electron, 
and other important dependencies so these must be fetched first.  

## Install NodeJS + npm

First, install the lasted version of NodeJS and npm for your platform.  At the
time of this writing we're using the 10.x series to build Polar.

## Build from Source

First, fetch the latest version of Polar from git then run:

```bash
npm install
```

Make sure to run this periodically when pulling a fresh version from git as 
dependencies may have changed.

Then run:

```bash
npm compile && npm start
```

At this point you should have a version of Polar running on your machine.

```npm install``` only needs to be run occassionally.  Usually when you pull 
from git and the ```package.json``` file changes with new dependencies.  

# How do I enable advanced logging?

There are two ways to enable advanced logging:

## Update environment (temporary)

Set the ```POLAR_LOG_LEVEL``` environment variable.

Linux/Mac run ```export POLAR_LOG_LEVEL=DEBUG```

NOTE: Make sure it's exported. If you just set it child processes can't see the value.

Windows run ```set POLAR_LOG_LEVEL=DEBUG``` 

then run Polar either via ```npm start``` for source builds or run the binary
directly.

## Update your config (permanent)

NOTE: This is no longer the recommended way to change your log level. We 
recommend setting POLAR_LOG_LEVEL.  When permanently setting the log level to
DEBUG there can be sever performance degredations - especially when moving 
pagemarks which can lock up Polar and make it feel that the app has crashed when
in reality it's just being amazingly slow logging thousands of messages.

Create a file in your ```.polar/config``` directory named ```logging.json``` 
with the following content:

```json
{
  "level": "DEBUG",
  "target": "CONSOLE"
} 
```

By default we use logging level WARN to improve performance and also so it does
not log pointless messages to the console which would just be confusing to an
end user.

The available log levels are:

```text
DEBUG
VERBOSE
INFO
WARN
ERROR
```

We currently only support a log target of ```CONSOLE``` due to performance reasons.

There IS an on-disk version enabled but it usually ends up locking up Electron
essentially defeating the point.  

We plan on implementing a logger implemented on Websockets in the future.

# Commons Errors

## Quiting.  App is single instance.

This happens because another version of Polar is running in the background.

Either quit this version or run:

```bash
killall electron
killall polar-bookshelf
``` 

... on MacOS and Windows you probably want to kill either the ```Electron``` or 
```Polar Bookshelf``` processes if they're running in the background.

# Aren't Electron Apps Bloated?

## RAM

A default install of Polar uses about 350MB of RAM after a fresh start.

As of 2018 this is about $5 worth of RAM.

Electron and web apps provide for an amazingly powerful development platform.

Without PDF.js, React, Node, and other frameworks, it would be prohibitively 
expensive to re-implement Polar (and not very fun either).

Now add the cost of porting to Windows, Linux, MacOS, Android and iOS.  

You're asking to spend hundreds of thousands of dollars hiring a developer to 
save $5 on RAM.

It's just not a very practical solution.

I'd like to get memory consumption down. It's possible that there are some 
features we can remove but right now it's not a priority.

Just spend the extra $5...
 
## Disk 

The binary download is only 100MB. Fairly reasonable for modern apps.
