---
layout:  post
title:   Setup a Free AWS Minecraft Server
date:    2016-08-27 16:50
summary: A simple guide to setup an AWS instance and start running your very own Minecraft server from it.
meta_robots: noindex, nofollow
categories:
---

Want to run a small casual minecraft server for your friends? Are you a bit of a cheapskate like me?

Perfect. With a little work we can quickly make this a reality!

## Prerequisites
- Sign up for a [free AWS account](https://aws.amazon.com/free), we will use this service to run a virtual server.
You'll need a credit card to sign up, but what's on offer with the _'Free Tier'_ is amazing and you won't be charged
unless you step over the free tier limits.

- On a Windows machine? Coolness, you'll need to download [PuTTY](http://www.putty.org/). We will
need this in order to log into our server instance.

- Use OSX or a Linux Distro? Have your favourite terminal program at the ready.

## Spawning an instance
Sign into the [AWS Console](https://console.aws.amazon.com), it is a powerful but scary beast. Navigating the menus and setup is much like playing a game of _'Where's Waldo?'_

Hopefully the below video helps speed you through the process:

<iframe width="560" height="315" src="https://www.youtube.com/embed/knfAzeU8DOY" frameborder="0" allowfullscreen></iframe>

<p></p>
<p id="spawn-instance-instructions">
    <a href="#spawn-instance-instructions" style="cursor: pointer">Show text instructions?</a>
</p>
1. Find and click on the __'EC2'__ item on the dashboard.

2. Now in the top right of your screen in the navigation click on the region name. This will display a dropdown with a
list of regions. This is very important, we want to choose a region closest to the majority of your friends for better
latency and ping.

3. Click __'Launch Instance'__.

4. We are now presented with options for the software configuration for our virtual server. Select the default
__'Amazon Linux AMI'__ (Currently, Amazon Linux AMI 2016.03.0 (HVM)) option.

5. Next we can select the instance type. We want to select the __'t2.micro'__ instance. On the free tier you can use one instance like this 24/7 for 12 months free of charge. Perfect for our small minecraft server.

6. Now we can just skip ahead to step 6, __'Configure Security Group'__. By default your server is only accessible via
port 22, which is used for the protocol SSH _(Which we will be using to log into our server and type commands)_.
In order to allow yourself and other friends to connect we're going to need to open a port for Minecraft.
  - Select the option __'Create a new security group'__.
  - For the __'Security group name'__ enter _'Minecraft'_, and for the description _'Allows connections to minecraft
  server'_.
  - Add a new rule, select __'Custom TCP Rule'__ in the dropdown.
  - For the __'Port Range'__ field, enter the default minecraft port `25565`.
  - Select from the source dropdown __'Anywhere'__.

7. Time to review and launch and we're good to go. Have a quick scan through the list. Ensure the instance type is __'t2.micro'__ and nothing else looks too out of place. Click __'Launch'__!

8. Hold on, we're now presented with a prompt asking if we'd like to __'Select an existing or create a new key
pair?'__, what does that mean you ask?

    Rather than use a username/password setup to log in to the server, AWS by default requires you to setup and
    download a secure key pair. If you're interested, rather than explain it here, jump on over to Reddit's [Explain SSH keys and validation like I'm five?](https://www.reddit.com/r/learnprogramming/comments/1enupy/explain_ssh_keys_and_validation_like_im_five/) thread.

    __tldr;__ It's more secure to use these complex keys to access the server.

    Select __'Create a new key pair'__, give it a name like _'aws-key'_ and download the key pair. Do not lose this!
    You will be unable to download this key again and is required to access your server.

9. Now finally click __'Launch Instance'__ and pat yourself on the back. You've managed to understand my tangled
instructions and now have your own little virtual server running up there in the clouds.

## Connecting to your virtual server

### On Windows?

Your setup to connect to the server is a little bit tougher, [see Amazon's instructions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html).

[Skip to Setting Up Minecraft Server](#finally-lets-get-that-minecraft-server-up).

### On Linux/Mac?
We're gonna need to put that key pair we just downloaded to use. You can store this anywhere, but I prefer to keep it
in my home directory in a directory called `.ssh`. The key pair file should be called `aws-key.pem` or whatever you chose to name it.

Let's go find the public dns of your server instance. It's essentially a website address that points to our server's IP.
Go to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home), click on __'Running Instances'__, we should have
one instance show up as running. Click on it and look for the text __'Public DNS'__, it should contain an address like
`ec2-198-51-100-1.compute-1.amazonaws.com`, copy this down and keep it at the ready.

*The below GIF should show you how to find it in the current interface for AWS:*

![Locate the Public DNS of your EC2 Server instance to connect to it](https://i.imgur.com/PDvtB75.gif?1)

Now that we have your ssh key pair and the public dns, fire up your terminal!

The following video will walk through how to prepare and connect to your AWS instance from your terminal.
<script type="text/javascript" data-speed="2" src="https://asciinema.org/a/1rzhpnmxbtj1ezp3p9schgecs.js" id="asciicast-1rzhpnmxbtj1ezp3p9schgecs" async></script>

After the first time you'll be able to connect with just the one command like:

`ssh -i ~/.ssh/aws-key.pem ec2user@ec2-198-51-100-1.compute-1.amazonaws.com`

Encountering problems or want more detailed instructions? [See Amazon's guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html).

## Finally let's get that minecraft server up!

Great if you've followed the Windows or Linux/Mac instructions we should be now connected remotely to our virtual server
and can type commands. Progress.

Follow along with the video below to get the Minecraft server setup on your amazon instance.
<script type="text/javascript" data-speed="1.3" src="https://asciinema.org/a/152oj5te8qxnzsghxkxv7gylr.js" id="asciicast-b6b9x7jjirp6pb6b5iyza0se7" async></script>

To run my install script, I use the following command in my video above:

```
wget "https://gist.github.com/jamsinclair/3bf32c1
dcf48187a0dfa4d07abab9f81/raw" -O minecraft-setup.sh && bash minecraft-setup.sh
```

Follow the prompts to install the version of minecraft you want.

Great you can now exit out of the terminal. Relax, log into minecraft and take some time punching wood... or sheep.

![Connect and play around on your now setup Minecraft server](https://i.imgur.com/YdHH3E2.gif?1)

__Note 1:__ Running an external script on your server is not advised, unless you trust the author and have read through
the script. Malicious commands could be run very easily. For those concerned feel free to check [my bash setup script](https://gist.github.com/jamsinclair/3bf32c1dcf48187a0dfa4d07abab9f81).

__Note 2:__ The Public DNS for your server may change from time to time. If you can't connect, try going to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home) and find the Public DNS again for your server instance.

## Final notes

I will note the free virtual server we've setup is not that powerful and will support 2-5 players at most. I currently
have one running with 3 players with only occasional lag and performance problems. If you need something bigger you're
better off going with a dedicated Minecraft server service.

__Need to stop or start your server?__ There's two ways we can go about this.

1. Reboot the EC2 Instance of your Virtual Server from the [EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home)   
  - Click on __'Running Instances'__, and right click on the instance in the table. Hover over __'Instance State'__ and
  click reboot, and confirm your action. Your minecraft server is configured to be started on start of the EC2 Instance.

2. Connect to your Amazon EC2 server again ([see the previous section](#connecting-to-your-virtual-server))
  - Once connected you can type the commands `minecraft-start` or `minecraft-stop` to control the minecraft server state.
  - If you get an error `command not found`, type `source ~/.bash_aliases` and try again

__Want to update server settings or get the minecraft world files?__

All the files are located in the directory `~/minecraft` on your amazon server instance. You can connect to your
instance and change text files from within the terminal.

To update the minecraft server config, you could use the command `nano ~/minecraft/server.properties` this will open up
a special text editor within the terminal ([Tips how to use the editor](http://www.howtogeek.com/howto/42980/the-beginners-guide-to-nano-the-linux-command-line-text-editor/)).

Once you've made your desired changes you'll need to restart the minecraft server.

Thanks for following along and I hope this helps someone out there! :)
