A Complete Beginners Guide to Installing a Bitcoin Full Node on Linux (2021 Edition)
Compile Bitcoin on Kubuntu 20.4 (v0.21.1)
StopAndDecrypt 

Published on 06/18/2021 
Editor- satoshi nakamoto 


Primary:
Running Bitcoin & Lightning Nodes Over The Tor Network (2021 Edition)
And Connecting Your Phone to Use Lightning On-The-Go
stopanddecrypt.medium.com

Standalone:
A Complete Beginners Guide to Installing a Bitcoin Full Node on Linux

A Complete Beginners Guide to Installing a Lightning Node on Linux

Table Of Contents

Introduction
Part 0 — Just The Commands (For Quick Reference)
Part 1 — Installing Linux & Setting Up
Part 2 — Prerequisites & Dependencies
Part 3 — Compiling Bitcoin Core 0.21.1
Part 4 — Node Configuring & Familiarization
Extra Guidance
How To Create A Transaction Index
How To Recompile/Update Bitcoin Core


Introduction
Never used Linux? Don’t know what a “pruned node” means? Perfect. This one’s for you. I want you to learn Linux, and I want Bitcoin to motivate you to switch. This will be as much a “Linux for Dummies” guide as it is a guide to setting up a Bitcoin node.
If you already know a thing or two and want to skip all the useless words:
Just copy and paste the commands from this section.

Many tutorials just give you the steps, and while some are actually pretty good at elaborating a bit this one is literally going to spoon feed you all the questions you might have, down to what each command and flag does. The only precursor knowledge I’ll assume you have is the ability to figure out how to download & mount the ISO image I link below, boot it, and follow the default install instructions. If you don’t, you can follow Ubuntu’s official tutorial for Windows or MacOS. I’ll be making no major changes to the default install configuration to avoid complications, which is why I won’t be installing anything else first besides the screen capturing software, so any dependency issues along the way we should both have.

If you run into any other issues, get confused somewhere, or think I should include something, just comment below or reach out to me on Twitter and I’ll try to assist. I still get people reaching out to me 3 years since publishing that original guide, which has partially motivated me put out this updated version.

Part 0 — For Those Who Just Want The Commands
You’ll notice that this section is very short, but the tutorial is pretty long. I’m putting this in the beginning to demonstrate that this is all we are really doing. This tutorial is designed for beginners to Linux, so all facets of the following steps will be explained in detail, and then some.

Update The OS:
sudo apt-get update

Install Git:
sudo apt-get install git

Make And Move To The Install Directory:
mkdir -p ~/code && cd ~/code

Clone The Bitcoin repository:
cd ~/code
git clone https://github.com/bitcoin/bitcoin.git

Install libraries:
sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3 libevent-dev

sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-test-dev libboost-thread-dev

sudo apt-get install libsqlite3-dev
sudo apt-get install libminiupnpc-dev
sudo apt-get install libzmq3-dev
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools
sudo apt-get install libqrencode-dev

Install the Berkeley DB:
cd ~/code/bitcoin
./contrib/install_db4.sh `pwd`

Prepare for installing Bitcoin:
cd ~/code/bitcoin
git checkout tags/v0.21.1
./autogen.sh

Replace /satoshi/ with your username:
export BDB_PREFIX='/home/satoshi/code/bitcoin/db4'


More preparation:
./configure BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"

Compile & Install Bitcoin:
make
sudo make install

Start Bitcoin:
bitcoin-qt &


Part 1— Setting Up
(Skip this section entirely if you’re already on Linux.)

Download Kubuntu ISO image
Kubuntu is Ubuntu, but shiny. The most recent LTS release is 20.04.1.
(You don’t need to use Kubuntu. Ubuntu will work with this tutorial, or any other Debian based operating system, although you may have to install other packages.)

Install ISO image to USB or CD
If you’ve never used Linux, it’s probably safe to assume you’re using either Windows or MacOS. Follow Ubuntu’s official tutorial for Windows or MacOS.

Install the Operating System
You’ll set up a computer name, user, and password. My username for this tutorial will be satoshi, and the computer name will be nakamoto.

Log In And Get Acquainted
After installation your desktop should look like the screenshot below:

Feel free to poke around and get familiar, but at some point navigate to the Application Launcher (“Start Menu”) and run Terminal (Konsole). We’re going to be working within this single window for the majority of this tutorial until we get to the end, but we’re also going to open up the File Manager, called Dolphin. Navigating this is very similar to the file explorers on Windows & MacOS.

Before we enter anything into the terminal let’s take a look at what we already see. At the top of the terminal window it says “Konsole”. That’s just the name of the software specific to this desktop environment. Sometimes you’ll see it referred to as the terminal, command line, shell, or whatever it may be named in another Linux operating system.



satoshi is the username. nakamoto is the computer’s hostname and will show up on whatever network you may be connected to. Yours will be whatever you selected during the installation.
In between the : and $ you’ll see a ~ .

This is an abbreviation for your /home/<username> directory.
/home/<username> is like “My Documents” in Windows.

These two mean the same thing, but you’ll always just see ~ when in there:
satoshi@nakamoto:~$
satoshi@nakamoto:/home/satoshi$

Whatever directory the terminal says you are in is equivalent to being there in the graphic based file manager.

Part 2— Prerequisites & Dependencies
(Skim this section for the commands if you’re already on Linux.)

Updating Linux
The first thing we’re going to do in the terminal is check for updates. Via the command line in the terminal we just opened, go ahead and type the following command to begin updating the operating system. Along the way you’ll be prompted to type “y” for yes, and your password:
satoshi@nakamoto:~$ sudo apt-get update

sudo is sometimes called “superuser do”. It’s kind of like using “Run as administrator” in Windows, but better. It’s necessary throughout this tutorial because the commands that follow it will try to do things that require superuser privileges.

apt-get lets you interface with available software libraries so you can download software straight from the terminal.

update is one of a few commands that must follow the use of apt-get, and it will check for updates to any packages you have installed and install them.

Installing Git
Next we’re going to install Git. It’s widely used open-source software designed for handling other open-source (and closed) projects. We’ll be using Git to access the Bitcoin repository and download its code.

satoshi@nakamoto:~$ sudo apt-get install git
install should be self explanatory, it’s like update, but for when you’re installing a specific package for the first time. Using it requires a package name.

git is the name of the Git package, and is recognized as such by the list of sources the apt-get command refers to. It will also function as a command after it is installed.

Cloning The Bitcoin Core Repository
Now we’re going to make a folder called “code” within our home directory, and then change to that directory. This will set us up for both this tutorial, and later for the Lightning tutorial. We could clone Bitcoin into any folder we want, this is just the path I chose to create within the home folder.
First, enter the following line:
satoshi@nakamoto:~$ mkdir -p ~/code && cd ~/code

Now it should look like:
satoshi@nakamoto:/home/satoshi/code$

mkdir makes a directory. This is like right clicking on the desktop or in a window and selecting “New > Folder”.

-p is a flag. Flags are command-line options and will start with a — . Each command (like mkdir) has their own set of options, so -p may do something else for another command. In this case -p overrides some errors you might get when trying to create a directory, and actually does what you’d probably want mkdir to do in the first place. If you wanted to create the directory /test1/abc123/haha, without -p, it thinks you just want /haha and you would get an error saying /test1and /abc123don’t exist. With -p those errors are ignored and both parent directories that “don’t exist” are created as well.

code is just the directory/folder name we’re going to create.

&& allows you to execute another command on the same line, but will only execute the second command if the first command doesn’t fail with an error.

cd will change the current directory to the one you specify. In this case it will change to the ~/code directory we just created.

Then enter:
$ git clone https://github.com/bitcoin/bitcoin.git
git clone will copy the Bitcoin repository from Github.com into the directory you’re in when you enter the command. Since were in ~/code, this will create the directory ~/code/bitcoin and place all the necessary files in there.

(When we install Lightning, it will go into the ~/code/lnd directory.)

You can check to see if the files installed by using the ls command, or you can browse to that directory in the File Manager.
$ ls
$ ls bitcoin

ls will output all the non-hidden folders in the directory you’re currently in.

ls bitcoin will look for a /bitcoin folder within the directory you’re in, and then output all the non-hidden folders in that directory.

ls -a will output all folders, including hidden folders if any exist. Hidden folders begin with a . and will look like this: /home/satoshi/.abc123

The output from the terminal matches the files shown in the file manager. One is text, the other is graphical.


Installing Libraries
Now we need to install some libraries. When installing libraries you can sometimes list many in a single command and separate them with a single space. In this tutorial I’ve split them into groups similar to the build documentation on Github for Ubuntu. Combining certain ones together can produce errors.

Libraries:
$ sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3 libevent-dev

$ sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-test-dev libboost-thread-dev

Some more libraries:
$ sudo apt-get install libsqlite3-dev
$ sudo apt-get install libminiupnpc-dev
$ sudo apt-get install libzmq3-dev
$ sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools
$ sudo apt-get install libqrencode-dev

Installing The Berkeley Database
Next we’ll install the Berkeley Database. Eventually the Berkeley Database will no longer be necessary, but it’s still required for now. There is a script included in the Bitcoin repository we cloned earlier to install it, located in /bitcoin/contrib. To run it, navigate into the /bitcoin directory, then run the script:

$ cd ~/code/bitcoin
$ ./contrib/install_db4.sh `pwd`

(You could also just type cd bitcoin, provided you haven’t changed directories since the previous step. Remember, I’m assuming you’re brand new to Linux.)

pwd is a command that simply outputs what directory you are currently in. When the script above is ran it will install the Berkeley DB into the directory specified. In the case above, we have no directory specified, but the command will replace pwdwith the directory we are currently in (home/satoshi/code/bitcoin).

This will install the Berkeley DB into /home/satoshi/code/bitcoin/db4 .

Video cuts off after 60 seconds.

When that completes the output should end with the following instructions, which we will use in the next section:

That concludes the prerequisites, now onto actually installing Bitcoin.


Part 3— Compiling Bitcoin Core 0.21.1
(The version installed in this section may change with new releases.)
After completing the Berkeley DB step, we should be in the /bitcoin directory, but just in case, let’s make sure we are there and then begin compiling & installing Bitcoin.

$ cd ~/code/bitcoin
$ git checkout tags/v0.21.1
$ ./autogen.sh

git checkout tags/v0.21.1 will reference a specific specific tag that was “commited” from the git history. “Branches” can change as updates occur so referencing a branch may make the command not work in the future.

./autgen.sh will, simply put, prepare the build files for install. This is another script included in the Bitcoin repository we downloaded.

This video was recorded before Bitcoin Core v0.21.1 was released.
Pay attention to this step.

export is going to define the variable BDB_PREFIX as the full Berkeley DB install directory specified: /home/satoshi/code/bitcoin/db4
You are not satoshi!!! Insert your username...

If your user is John, change the next line to: /home/john/code/bitcoin/db4
$ export BDB_PREFIX='/home/satoshi/code/bitcoin/db4'

That command is the only command in this tutorial where the username (and the correct one, yours) needs to be specifically provided. As mentioned in the beginning, ~ is equivalent to /home/<your-username> , so all of the other commands we enter will just use ~ instead. That specific export command above requires the full path to be typed out, which is why we aren’t using ~/code/bitcoin/db4 instead.

$ ./configure BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"

./configure and the additional text on that line (notice the BDB_PREFIX) will amend the configure.ac file in the Bitcoin repository with the Berkeley DB directory above. This allows the install process to know where we installed the Berkeley DB.

Now for actually compiling and installing. This will take the longest time out of them all. Just let it run and complete:
$ make
$ sudo make install
The make command will compile, and be the longer of the two. When that is complete, don’t forget to run sudo make install afterword, so the software is actually installed. (I’ve received messages where this was the case.)


Once that completes you have Bitcoin Core installed!

Part 4— Configuring And Familiarizing Yourself With Your New Node
(Much of this section isn’t required to just run the software, but we still need to create the bitcoin.conf file so do not skip this. In the future you’ll likely just run it in the background and let it be. This is all to help you get a feel for what’s going on behind the scenes, not just trusting a background process to work.)

Before we launch Bitcoin and let it sync, we need to create & configure the bitcoin.conf file. But before we configure Bitcoin, let’s start & stop the software a couple of different ways to get a feel for what we are doing.

Launching The Bitcoin Core GUI And Viewing Logs
The first thing I want you to do is set up a few windows before we run Bitcoin for the first time. We’re going to run the GUI version first, called bitcoin-qt, then we’ll exit it and run the non-GUI version called bitcoind, and then back to bitcoin-qt, with some steps and configuration in between so you can understand what’s really happening and feel comfortable starting and stopping the software when you need to.

Close all open windows, and open two brand new terminal windows and the file manager. In the file manager navigate to /home/satoshi.

Pressing Home on the sidebar will take you to /home/satoshi.
The “Home” button will take you to your current user’s home.
If you log out and log back in as Jessica, it will be /home/jessica.

Then from the menu bar at the top check the box for Show Hidden Files:

You’ll know it’s been selected because more files will appear, and you’ll notice they all have a “ . ” in the begging of their names.We’re going to create another hidden file (the Bitcoin data directory).

In one of the terminal windows we opened, enter the following:
$ mkdir ~/.bitcoin
$ cd ~/.bitcoin

You should now see a folder named .bitcoin appear in the file manager as well. Navigate into that folder, and we’re now redundantly in this directory both in the terminal and file manager, but for a reason.

Now we’re going to create a file called debug.log. When you first launch Bitcoin, both this hidden folder, and the debug.log file are automatically created, but you’ll see why I want to do this ahead of time in a moment:
$ touch ~/.bitcoin/debug.log

touch will create the file we specify (debug.log) into the directory we specify.

Now we’re going to tail the debug.log file. Log files get updated continuously with new lines of information as the program takes a log of its actions. The tail command shows you the most recent entries into that file, but only once. Using the -f flag will give you a continuously running stream of those updates.

We can’t tail a file that doesn’t exist, which is why we created debug.log ahead of launching Bitcoin Core for the first time.

When you enter the following command you’ll see nothing because Bitcoin isn’t running yet, but we’ll leave it like this for now:
$ tail -f ~/.bitcoin/debug.log

In the other terminal window we opened, run Bitcoin by entering the following:
$ bitcoin-qt

You’ll see the loading image, and then a setup window prompting you to select where you want to store the blockchain and the node data. We’re going to use the default directory.
Go ahead and click OK, and you’ll start to see activity in the terminal where you tailed the debug.log file.

All of the above should look like this on your screen:

Normally when launching Bitcoin Core for the first time, it will ask you where you want to install the data directory (/.bitcoin). Since we already created it ahead of time to tail the debug.log file, the application detected that and skipped asking us.

You can watch this for however long you want because it will take a long time to sync, but we still need to change some configurations and restart.

At some point, in the same terminal where you entered bitcoin-qt, now press CONTROL+C. You’ll see the GUI close down, and the log file will stop. You can read the exit messages in the log, and you can scroll up and read all the different events that occurred. Now that it’s stopped, close all windows.



Launching Bitcoin In The Terminal
The purpose of this short section is to demonstrate how to we’ll run Bitcoin when we move on to the Lightning tutorial. Bitcoin can be launched via the GUI using bitcoin-qt, or via the terminal using bitcoind.

Open a new terminal and type:
$ bitcoind

Bitcoin is now running via the terminal, and outputting the logs on its own.
No tail is required.

Press CONTROL+C to stop it again. Bitcoin will shut down and you can see the log output as it shuts down:

Creating The bitcoin.conf File
We need to create a configuration file now, so in the file explorer create a file called bitcoin.conf . Open it, paste the following, and save the file:

# Needed for full validation
assumevalid=0
# Improves LND performance
# Needs to be set now if you're going to install Lightning later
txindex=1
# Not needed, but will show us useful info later in the tutorial
debug=net
Alternatively, this next config is exactly the same but without the comments:
assumevalid=0
txindex=1
debug=net

debug=net will just show us some extra network activity in the log output. This can be left out if you really don’t care, or removed after you are done with the tutorial.

By default, not all of the debug information is included in the log file. Setting it to 1 will include all of it, but there’s way too many lines and all the info flies by too fast when tailing the log. It’s useful for diagnostic purposes, but we’re not going to use it.

All the debug config options you can set are: net, tor, mempool, http, bench, zmq, db, rpc, estimatefee, addrman, selectcoins, reindex, cmpctblock, rand, prune, proxy, mempoolrej, libevent, coindb, qt, leveldb.



Disclaimer: Do not leave debug set to 1 indefinitely or your log file will grow larger than the entire blockchain and fill up your hard drive. It’s happened to me.

txindex=1 will create a transaction index as the blockchain syncs, which will ready the node for my Lightning tutorial that follows this.

assumevalid=0 will force your node to validate every transaction & block from the Genesis block (the very first block mined).

This is also where you can optionally set your node to prune the blockchain as it goes along. 

Right now the entire blockchain is about 160 GB in size. If you don’t have enough storage space, you could prune the data down to under 5 GB at the moment. I don’t recommend doing that unless you need to, but this is what you would enter on its own line to bring it down to 10 GB:

#(Example config)
debug=net
prune=10000

If you’re going to follow the Lightning guide, do not prune your node.

Your config file can have many options set, and it doesn’t matter what order they’re in, so it could also look like this if you hypothetically want to restrict your node’s mempool to 100 MB worth of transactions:

#(Example config)
debug=net
prune=10000
maxmempool=100

Or like this if you want to (and should) force your node to check the validity of every signature. The definition of a full node is contested, but my stance is a full node is one that has fully validated the entire blockchain, so this “assume valid” setting in the config file will be necessary. Setting it to 0 tells your node to not assume anything. Without this set your node will skip over validating older blockchain data:

#(Example config)
debug=net
assumevalid=0

For now just save the bitcoin.conf file with debug set to net, txindex set to 1, add a prune value if you need it, along with assumevalid set to 0 if you want your node to fully validate the blockchain (recommended, and it shouldn’t add too much time to the syncing process unless you’re on very low end hardware).


Again, the order they are listed in the config file doesn’t matter.
Alternatively you can use something like Jameson Lopp’s config generator if you’re interested in setting additional config options.

We’re going to relaunch the Bitcoin GUI, but this time the command will be slightly different. Before we do that, let’s make sure we still have two terminal windows open (video further below).

In the first terminal, enter the following:
$ tail -f ~/.bitcoin/debug.log | grep "UpdateTip:"

Again because it’s a tail, you’ll see nothing until you re-run bitcoin-qt or bitcoind.
grep is a command that has a few functions, but in this context, it will take the output from the first command and filter for it so it only shows lines that include the text within the quotes. They way we’re using it here will take the output of tail -f (all of the logs) and only show lines that include the text “UpdateTip:”.

UpdateTip: is specific verbiage that only appears when a new block is added.
The | is commonly called a “pipe”, and all it really means is “take the output of the first command and send it through the second command”. You’ll hear people say terms like “pipe it to grep” or “pipe it to more”, and this is what they mean.

In somewhat simpler terms: tail -f will output the log as it updates, pipe will send that output to grep, and grep will filter out all the lines that don’t include “UpdateTip:”, and then show you the remaining lines (that do include UpdateTip:) in the terminal.

In the 2nd terminal we opened, launch Bitcoin by typing the following:
$ bitcoin-qt &

The & symbol will allow it to run in the background, so we can continue to make use of that terminal.

After launching Bitcoin, in that same terminal enter the following:
$ tail -f ~/.bitcoin/debug.log | grep -v "UpdateTip:\|Requesting block\|sending getdata\|received block\|received: block"

This is the same grep command but with the -v flag, and will do the opposite of the previous command and filter out any line with the text we specify.

UpdateTip: is included first, because we’re already pulling that specific information into a different window. What you’ll see next are the two symbols \|and what these do is tell the grep command “filter out x and y and z and …”

So now we’ve effectively split a single log file up into two outputs so we can more accurately watch what’s going on, and filter out some other info I’ve chosen so you have a slower scrolling output. This way you can keep direct track of the blocks coming in with the first tail command without it forcing the other information out of the way. Feel free to play around with what you want to include and exclude until you’re comfortable using the command.

Now you should have Bitcoin syncing, and two terminals with tail commands running. One showing you the new blocks coming in, and the other showing the rest of the log output.
That’s it! Just let it sync. Depending on your hardware & bandwidth it could take anywhere from a handful of hours to multiple weeks (unlikely).

When it’s finally synced, blocks will start coming in once every ~10 minutes on average and the the debug.log file will start showing a lot more activity, including transaction relaying information which doesn’t begin until after the Initial Block Download (IBD) period.

One final thing that is specific to laptops:

Debian based operating systems running on laptops (like Ubuntu & Kubuntu) may enter sleep mode if the laptop is left unattended too long. This will cause the sync to stop, delaying the overall time you’ll need to wait before it can finish. The Power Management settings are just a layer on top of the operating system. Those settings only let you customize additional power management functionality, they don’t turn it off at the system level. To prevent this, you’ll want to enter this command:

$ sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
$ systemctl restart systemd-logind.service

If you’re going to be running your node nonstop you’ll want to keep this set, but to reverse these settings at any time, just enter:

$ sudo systemctl unmask sleep.target suspend.target hibernate.target hybrid-sleep.target

When you’re ready you can proceed with installing Lightning:

A Complete Beginners Guide to Installing a Lightning Node on Linux (2021 Edition)

Running LND With a Bitcoin Core Full Node
stopanddecrypt.medium.com






Extra Guidance

In the future you may want to upgrade your Bitcoin node, or you may need to resync Bitcoin Core for this tutorial because you didn’t have the transaction index (txindex=1) that LND uses for performance, or you may need to recompile Bitcoin Core because you didn’t have thelibzmq3-dev dependency required for LND. The steps below will help you for these scenarios.
How To Create A Transaction Index

If you don’t have a transaction index, you’ll need your node to create one before running LND. If you didn’t have txindex=1 in your bitcoin.conf file then you don’t have a transaction index. 

There’s two scenarios for creating one:
Your node is pruned, with no txindex.
Your node is not pruned, with no txindex.

The first thing you want to do for both scenarios is follow the bitcoin.conf steps in this tutorial and make sure you have txindex=1 set in the config file. When that is complete, follow the proper steps below:

If your node is pruned: Start bitcoind with a reindex and let it resync the whole blockchain:
$ bitcoind -reindex

If your node is not pruned: Just start bitcoind normally and let it create the txindex, now that the bitcoin.conf file has the instructions for your node:
$ bitcoind

How To Recompile/Update Bitcoin Core
Recompiling Bitcoin Core and updating Bitcore Core are essentially the same procedure. We’re going to delete or rename the Bitcoin install directory, and then just reinstall Bitcoin all over again with the newer version.

We do not need to:

re-sync or re-validate the blockchain
create the bitcoin.conf file again

re-install any packages or dependencies (unless you’re specifically recompiling because you did not have libzmq3-dev installed, then you would want to install that package first, then recompile Bitcoin Core)

If you followed this tutorial, the install directory is ~/code/bitcoin The non-hidden folder.
If you did not follow this tutorial, the install directory may be ~/bitcoin.

In contrast, the data directory is the hidden folder with the period before the word bitcoin: ~/.bitcoin . Do not delete this folder. Leaving this alone will prevent you from having to re-sync or re-validate the blockchain. Your bitcoin.conf file is there, along with your list of peers, among other things.

If you’re running and old Bitcoin client and want to follow this tutorial, I recommend renaming the install directory to something like “bitcoin-old”, and then just follow the steps from the beginning.


You can double check your client version running this command:
$ bitcoind --version


