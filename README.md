Forked from:
https://github.com/jlund/docker-chrome-pulseaudio

Docker! Chrome! PulseAudio!
===========================

Run Google Chrome inside an isolated [Docker](http://www.docker.io) container on your Linux desktop! See its sights via X11 forwarding! Hear its sounds through the magic of PulseAudio and SSH tunnels!


Short instructions
============

1. Set the pulseserver in the launch script

1. Copy your SSH public key into place

        cp ~/.ssh/id_rsa.pub .

1. Build the container

        sudo docker build -t chrome .

1. Create an entry in your .ssh/config file for easy access. It should look like this:
        
        Host docker-chrome
          User      chrome
          Port      2222
          HostName  127.0.0.1
          RemoteForward 64713 localhost:4713
          ForwardX11 yes

1. Run the container and forward the appropriate ports

        sudo docker run -d -p 127.0.0.1:2222:22 chrome

1. Connect via SSH and launch Chrome using the provided PulseAudio wrapper script

        ssh docker-chrome chrome-pulseaudio-forward

1. Go watch Hulu, or whatever









Frequently Asked Questions
==========================
<unmodified>

Why would I want to do this?
----------------------------
Sometimes you absolutely need to look at a website that uses Flash even though Flash is basically the worst thing ever in every possible regard. This lets you run Flash on Linux in a compartmentalized fashion and reduces the risk that one of its never-ending security vulnerabilities will affect your precious files or other processes. [Docker](http://www.docker.io) and [LXC](http://linuxcontainers.org) will be on your side and they both love you very much.

How does it perform?
--------------------
Flawlessly. Playing HD video from [The Daily Show](http://www.thedailyshow.com) is no problem at all. Spotify's web interface works perfectly. Your favorite restaurant's Flash-only website will cower before you in fear and quickly reveal all of its secrets.

Why wouldn't I just install Google Chrome directly?
---------------------------------------------------
You certainly can, but it's an enormous package with an even bigger set of dependencies. It's also closed source. Oh, and it *bundles Flash* into its binary. Installing Chrome directly is like inviting a really cool guy over to your house when you know that he is definitely going to bring his friend with leprosy.

Why do you disable Chrome's sandbox using the `--no-sandbox` flag?
------------------------------------------------------------------
Chrome does a bunch of crazy stuff using SUID wrappers and several other techniques to try to keep Flash under control and to enhance its own internal security. Unfortunately, these techniques don't work inside a Docker container unless the container is run with the `-privileged` flag. So what's the problem with that? Well, here's what [Docker's documentation](http://docs.docker.io/en/latest/commandline/cli/#run) has to say about it: 

> The -privileged flag gives all capabilities to the container, and it also lifts all the limitations enforced by the device cgroup controller. In other words, the container can then do almost everything that the host can do. This flag exists to allow special use-cases, like running Docker within Docker.

It sounds like a decidedly awful idea to give Chrome and Flash the ability to do "almost everything that the host can do." And even though it makes my inner [Xzibit](http://knowyourmeme.com/memes/xzibit-yo-dawg) very sad, we are not running Docker inside of Docker. If you disagree with this choice, feel free to run the container with Docker's `-privileged` flag enabled and to strip the `--no-sandbox` flag from the launch wrapper in the Dockerfile. This will remove the "You are using an unsupported command-line flag..." warning that otherwise appears every time you start Chrome.


Author Information
==================

You can find me on [Twitter](https://twitter.com/joshualund) if you are so inclined. I also occasionally blog at [MissingM](http://missingm.co).
