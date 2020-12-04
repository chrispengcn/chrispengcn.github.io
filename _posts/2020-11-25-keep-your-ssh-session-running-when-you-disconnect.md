---
ID: 3807
post_title: >
  Keep Your SSH Session Running when You
  Disconnect
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://vpseo.com/2020/11/25/keep-your-ssh-session-running-when-you-disconnect/
published: true
post_date: 2020-11-25 19:42:27
---
Screen is like a window manager for your console. It will allow you to keep multiple terminal sessions running and easily switch between them. It also protects you from disconnection, because the screen session doesn’t end when you get disconnected.

You’ll need to make sure that screen is installed on the server you are connecting to. If that server is Ubuntu or Debian, just use this command:

sudo apt-get install screen

Now you can start a new screen session by just typing screen at the command line. You’ll be shown some information about screen. Hit enter, and you’ll be at a normal prompt.

To disconnect (but leave the session running)

Hit Ctrl + A and then Ctrl + D in immediate succession. You will see the message [detached]

To reconnect to an already running session

screen -r

To reconnect to an existing session, or create a new one if none exists

screen -D -r

To create a new window inside of a running screen session

Hit Ctrl + A and then C in immediate succession. You will see a new prompt.

To switch from one screen window to another

Hit Ctrl + A and then Ctrl + A in immediate succession.

To list open screen windows

Hit Ctrl + A and then W in immediate succession

There’s lots of other commands, but those are the ones I use the most.