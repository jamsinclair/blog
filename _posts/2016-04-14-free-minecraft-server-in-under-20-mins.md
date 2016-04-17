---
layout:  post
title:   Free Minecraft Server in Under 20 mins
date:    2016-04-17 16:43
summary: Time is wasting, let's get that free server setup on an Amazon EC2 Instance!
categories:
---

You've got a small group of friends and want to run a small casual server (2-5 people)? Perfect, in 15 mins we can get
you up and running with a free 24 hr minecraft server. Let's not waste any more time!

## Prerequisites
- Sign up for a [free AWS account](http://aws.amazon.com/free), we will use this service to run a virtual server.
You'll need a credit card to sign up, but what's on offer with the _'Free Tier'_ is amazing and you won't be charged
unless you step over the free tier limits.

- If you're on a windows machine, bad luck, you're going to need to download [PuTTY](http://www.putty.org/). We will
need this in order to log into our server instance. Linux and Mac users you're fine, the standard terminal will be
perfect.

## Spawning an instance
Sign into the [AWS Console](https://console.aws.amazon.com), it is a powerful but scary beast. Navigating the menus and setup is much like playing a game of _'Where's Waldo?'_

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

    Rather than use an username/password setup to log in to the server, AWS by default requires you to setup and
    download a secure key pair. If you're interested, rather than badly explain it here, jump on over to Reddit's [Explain SSH keys and validation like I'm five?](https://www.reddit.com/r/learnprogramming/comments/1enupy/explain_ssh_keys_and_validation_like_im_five/) thread.

    __tldr;__ It's more secure to use these complex keys to access the server.

    Select __'Create a new key pair'__, give it the name like _'aws-key'_ and download the key pair. Do not lose this!
    You will be unable to download this key again and is required to access your server.

9. Now finally click __'Launch Instance'__ and pat yourself on the back. You've managed to understand my tangled
instructions and now have your own little virtual server running up there in the clouds.

## Connecting to your virtual server

### On Windows?

Your setup to connect to the server is a little bit tougher, [see Amazon's instructions](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html).

[Skip to Setting Up Minecraft Server](#finally-lets-get-that-minecraft-server-up).

### On Linux/Mac?
We're gonna need to put that key pair we just downloaded to use. You can store this anywhere, but I prefer to keep it
in my home directory in a directory called `.ssh`. The key pair file should be called `aws-key.pem` or whatever you chose to name it.

Let's go find the public dns of your server instance. It's essentially a website address that points to our server's IP.
Go to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home), click on __'Running Instances'__, we should have
one instance show up as running. Click on it and look for the text __'Public DNS'__, it should contain an address like
`ec2-198-51-100-1.compute-1.amazonaws.com`, copy this down and keep it at the ready.

Right, fire up your terminal.

<div class="terminal-wrap">
    <div class="title-bar"><span class="title">terminal — bash — 80x<span class="terminal-height">20</span></span></div>
    <div class="text-body">
    	<span class="text-comment">
            # We should update the permissions to your key file <br>
            # so it's not publicly viewable
        </span>
        <span class="text-command"><span class="text-prefix">$ </span>chmod 400 ~/.ssh/aws-key.pem</span>
        <br>
        <span class="text-comment">
            # Now we can use the ssh command, your key file and<br>
            # the server's public DNS to connect to it.<br>
            # Replace and copy your Public DNS into the following command
        </span>
        <span class="text-command">
            <span class="text-prefix">$</span>
            ssh -i ~/.ssh/aws-key.pem ec2user@copy-public-dns-here
        </span>
        <br>
        <span class="text-comment">
            # You'll next see a response like:
        </span>
        <span class="text-command">
            The authenticity of host 'ec2-198-51-100-1.compute-1.amazonaws.com (10.254.142.33)'
            can't be established.
            RSA key fingerprint is 1f:51:ae:28:bf:89:e9:d8:1f:25:5d:37:2d:7d:b8:ca:9f:f5:f1:6f.
            Are you sure you want to continue connecting (yes/no)?
        </span>
        <span class="text-comment">
            # Type yes and we should be then connected to your server!
        </span>
        <span class="text-command"><span class="text-prefix">$</span> yes<span class="typed-cursor">|</span></span>
    </div>
</div>

After the first time you'll be able to connect with just the one command like:

`ssh -i ~/.ssh/aws-key.pem ec2user@ec2-198-51-100-1.compute-1.amazonaws.com`

Encountering problems or want more detailed instructions? [See Amazon's overview](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html).

## Finally let's get that minecraft server up!

Great if you've followed the Windows or Linux/Mac instructions we should be now connected remotely to our virtual server
and can type commands. Progress.

<div class="terminal-wrap">
    <div class="title-bar"><span class="title">terminal — bash — 80x<span class="terminal-height">20</span></span></div>
    <div class="text-body">
    	<span class="text-comment">
            # First let's update your server's software. Type:
        </span>
        <span class="text-command">
            <span class="text-prefix">[ec2-user ~] $</span>
            sudo yum update -y
        </span>
        <span class="text-comment">
            # This will update various software packages on your server.
        </span>
        <br>
        <span class="text-comment">
            # Next up, we will run a script I've written <br>
            # to install and setup minecraft for us! <br>
            # Copy and paste the following command:
        </span>
        <span class="text-command">
            <span class="text-prefix">[ec2-user ~] $</span>
            wget "https://gist.github.com/jamsinclair/3bf32c1dcf48187a0dfa4d07abab9f81/raw" - O minecraft-setup.sh &&
            bash minecraft-setup.sh
        </span>
        <br>
        <span class="text-command">
          Great let's get minecraft setup!<br>
          Which version of minecraft server do you want to install? e.g. 1.9.2
        </span>
        <span class="text-comment">
            # This will now prompt for which server version you'd like <br>
            # to install, enter the version you'd like then press enter:
        </span>
        <span class="text-command">
            <span class="text-prefix">[ec2-user ~] $</span>
            1.9.2
        </span>
        <br>
        <span class="text-comment">
            # If all goes well you should see the following text:
        </span>
        <span class="text-command">
          Downloading minecraft server version 1.9.2...<br>
          Minecraft server downloaded successfully!<br>
          Looks like minecraft is setup correctly and running!<br>
          Use the Public DNS of your instance for the "Server Address" in your Minecraft Client to play!<br>
          <br>
          Public DNS: ec2-198-51-100-1.compute-1.amazonaws.com
        </span>
    </div>
</div>

Great you can now exit out of the terminal. Relax, log into minecraft and take some time punching wood... or sheep.

__Note 1:__ Running an external script on your server is not advised, unless you trust the author and have read through
the script. Malicious commands could be run very easily. For those concerned feel free to check [my bash setup script](https://gist.github.com/jamsinclair/3bf32c1dcf48187a0dfa4d07abab9f81).

__Note 2:__ The Public DNS for your server may change from time to time. If you can't connect, try going to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home) and find the Public DNS again for your server instance.

## Final notes

I will note the free virtual server we've setup is not that powerful and will support 2-5 players at most. I currently
have one running with 3 players with only occasional lag and performance problems. If you need something bigger you're
better off going with a dedicated minecraft server service.

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
