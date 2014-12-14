# omxremote

Web frontend and API for Raspberry Pi omxplayer media player

## Overview

[Omxplayer](http://elinux.org/Omxplayer) is made specifically for Raspberry Pi (RPi) and
is one of the most simpliest video players you can find. Although there are many others,
including [raspbmc](http://www.raspbmc.com/) that work as a full-blown media center,
omxplayer is a perfect fit for systems that do not have any GUI or peripherals and running stock 
[Rasbpian](http://www.raspbian.org/) operating system. However, omxplayer was designed to be
controlled via keyboard shortcuts and does not provide any way to control it remotely.
To control omxplayer programmatically one has to attach to process' STDIN and send commands as 
plaintext. 

Omxremote project is an attempt to build a web frontend and simple API to control 
video playback on a remote RPi device. Its built in Go, which has provides capability to 
cross-compile source code for ARM and as a result provides a single binary with 
no external dependencies to be downloaded and installed on your RPi.

[GUI Screenshot](screenshots/omxremote.png)

## Requirements

In order to use `omxremote` you must have `omxplayer` installed on your RPi. Most
recent distributions on Rasbpian should already come with `omxplayer` preinstalled.

In case if you dont have it installed, use the following commands:

```
sudo apt-get update
sudo apt-get install -y omxplayer
```

No special permissions are required in order to play videos with `omxplayer` and `omxremote`.

## Install

Use [Github Releases](https://github.com/sosedoff/omxremote/releases)

Or run the following snippet for quick install:

```
curl -s https://raw.githubusercontent.com/sosedoff/omxremote/master/install.sh | bash
```

## Compile

Compiling this project on RPi is a bit of a difficult task and does not necessarily makes
sense since Go provides ability to cross-compile source code for multiple platforms on
your local development environment. You can do that by following the steps:

```
make setup
make release
```

That will produce a binary that's ready to be transferred and executed on your RPi. 
In cases if you dont have Make available on your system, you can execute the following commands:

```
go get
GOOS=linux GOARCH=arm go build
```

## Usage

To start omxremote, run the following command:

```
omxremote /path/to/media
```

Server will start on port 8080 and listen on all network interfaces. You can
connect to it if you have any device (laptop, phone) on the same wifi network.
If you dont know the IP address of your RPi, run `ifconfig`.

### Running as daemon

You can run omxremote in a daemon mode with init.d. First, make sure omxremote
is installed in a right place:

```
which omxremote
# => /usr/bin/omxremote
```

Use the following [example](https://github.com/sosedoff/omxremote/blob/master/init.d/omxremote) to create a new init.d script:

```
sudo nano /etc/init.d/omxremote
sudo chmod +x /etc/init.d/omxremote
```

And then you can start the remote:

```
sudo /etc/init.d/omxremote start
```

### Troubleshooting

```
ERROR: COMXAudio::Decode timeout
```

If you see this error when playing video files, make sure to give more memory
to raspberry pi GPU. On B+ model the default is 16mb. Try setting it to 64/128mb.
To edit settings, run: `sudo raspi-config`.

## License

The MIT License (MIT)

Copyright (c) 2014 Dan Sosedoff, dan.sosedoff@gmail.com
