---
layout: "intro"
page_title: "Develop"
sidebar_current: "gettingstarted-dev"
description: |-
  Create a development environment with Otto in the Otto getting started guide.
---

# Develop

Let's start a development environment for our example application.
Run the following commands in your terminal:

```
$ git clone https://github.com/hashicorp/otto-getting-started.git
$ cd otto-getting-started
$ otto compile
$ otto dev
```

Now imagine every project being this easy to get started with.

If you inspect the repository you cloned, you'll notice that there
isn't any configuration for Otto. For simple cases, Otto doesn't need
any configuration. Otto detected the application is a Ruby application
and automatically built us an environment tailored to Ruby development.

Before we explain in more detail what happened, let's start the application.

```
$ otto dev ssh
> bundle && rackup
...
```

Then, in another terminal, run `otto dev address` to get the address
of your development environment. Visit port 9292 of that address in your
browser. At the time of writing this, mine is "172.16.1.215:9292".
You should see the application running!

TODO SCREENSHOT

Let's go over what just happened, step-by-step.

## Compilation

The first thing we ran was `otto compile`. This is the key magical step
of Otto. Otto inspects your environment and your configuration (if you
have one), and compiles this data into dozens of output files that
are used to develop and deploy your application.

For our example, `otto compile` detected our application is Ruby and used
opinionated defaults for the rest. In a future step, we'll write an
[Appfile](/docs/appfile/index.html) to more precisely configure Otto.

You can see the result of the compilation in the ".otto" directory, which
probably looks something like this:

```
$ tree .otto
.otto
├── appfile
│   ├── Appfile.compiled
│   └── version
├── compiled
│   ├── app
│   │   ├── build
│   │   │   ├── build-ruby.sh
│   │   │   └── template.json
│   │   ├── deploy
│   │   │   └── main.tf
│   │   ├── dev
│   │   │   └── Vagrantfile
...

24 directories, 41 files
```

With zero configuration, Otto generated dozens of configurations for
other battle-hardened software that will build development environments,
start servers, deploy the application, and more. Otto automatically manages
and orchestrates all of this software, so you don't have to.

This is the beauty of Otto: with a simple input and workflow, Otto manages
tried and true software under the covers to develop and deploy your
application using industry best practices. You only need to learn how to
use Otto, then Otto does the rest.

-> **NOTE:** You never need to use the ".otto" directory manually. It is an
internal directory that Otto uses. However, you can always inspect the
compilation output to see how Otto will develop and deploy your application.

## Building the Development Environment

Once compiled, we used `otto dev` to start a development environment.

This built a local virtualized development environment, shared our application
into that environment, and configured it with an address we can use to
reach it from our machine.

Because Otto detected our application is Ruby, Otto also automatically
installed Ruby and Bundler, since those are usually required for every
Ruby development environment. In a future step we'll see how to configure
things such as the version of Ruby installed.

At the end of the `otto dev` command, Otto outputs instructions for
working with the development environment in green. These are also tailored
specifically to your application type. These instructions change for PHP,
Node, etc.

As a reminder, Otto has done all of this _without any configuration_
and in only a few commands. Otto just works.

## SSH and Shared Files

While the development environment is assigned a unique address that we
can use to reach it from your own machine, Otto has a shortcut for quickly
opening an SSH terminal: `otto dev ssh`.

This command automatically drops you into an SSH terminal within the
local development environment, and also puts you in the working directory
with your files. If you run `ls`, you'll see the repository contents:

```
$ ls
Gemfile		Gemfile.lock	README.md	app.rb		config.ru	views
```

Otto also automatically sets up synced folders between the development
environment and your machine. This allows you to edit files locally on
your own machine with your own editor, and the changes quickly transfer
into the development environment.

Otto places you into this synced directory by default when you
`otto dev ssh`.

## Application-Specific Software

As mentioned earlier, Otto automatically installed Ruby and Bundler
since it detected that this is a Ruby application. As next steps, we
downloaded the dependencies for our application (using `bundle`)
and started the application (with `rackup`):

```
$ bundle && rackup
Using rack 1.6.4
Using rack-protection 1.5.3
Using tilt 2.0.1
Using sinatra 1.4.6
Using bundler 1.7.6
Your bundle is complete!
Use `bundle show [gemname]` to see where a bundled gem is installed.
[2042-01-01 12:55:28] INFO  WEBrick 1.3.1
[2042-01-01 12:55:28] INFO  WEBrick::HTTPServer#start: pid=12965 port=9292
```

If this environment were a PHP environment, we'd have PHP installed,
if it were Node, we'd have Node and NPM installed, and so on. Otto
automatically builds a development environment for your application.

## Development Environment Address

Every development environment created by Otto is assigned a unique
address so that you can access it from your own machine. You can retrieve
this address easily with `otto dev address`:

```
$ otto dev address
172.16.1.215
```

Your address output will likely be different.

With the web server running from the previous step, you can use this
address and port 9292 (the default listening port for this application)
and view the running application.

TODO SCREENSHOT

## Other Commands

`otto dev` accepts a variety of subcommands. You can view them all by
running `otto dev help`.

Note that these commands can also change per application type. There aren't
any right now for Ruby, but in the future you can imagine special commands
that would open a Ruby console for us, automatically start our application,
etc. Otto will add these over time.

## Next

In this step, you learned how easy it is to use Otto. You experienced
Otto compilation for the first time and also saw how Otto works with
_zero configuration_. You hopefully are beginning to sense the power of
Otto, even if we've only covered development so far.

Next, we'll start the process of deploying this application by
first [building an infrastructure](/intro/getting-started/infra.html)