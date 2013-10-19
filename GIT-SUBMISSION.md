## Overview

These are generic instructions to be used by courses at CSUSB
in which students submit work via Git.

In order for your instructor to set up a private remote repository for you,
you need to send to your instructor the public key that your Git client
program uses when connecting to remote hosts.
After you send this, your instructor will create an empty
remote repository for you to clone.

Your instructor will also send you a username to
connect to this repository.
After you receive the username, you will be able
to clone the remote repository and use this to submit your work.

The command you will use to clone your repository will
look something like the following, with _username_
replaced by the username your instructor emails to you.

    git clone username@web2.ias.csusb.edu:201.git

Each time you complete an assignment,
you should commit your work into a local repository
and then push changes to the remote repository.

You can also push changes to your source code at any point
prior to completion of your work.
This lets your remote repository serve as a backup
of your work.  It also allows you to easily work from
multiple computers by synchronizing your local repositories with
your remote repository.

## Security

Email is not completely private.
There are a number of different actors who can read
emails, including employees of the companies that
provide email service to the sender and receiver.

With this in mind,
it's safer to email public keys than passwords.
If someone gets your password, they can authenticate as you;
if they get your public key, they can not.

However, one danger with sending public keys by email
is that an imposter
can send the recipient their own public key.
To mitigate this threat,
your instructor may only accept a single public key for each repository.
Consequently, you will need to securely deliver to the instructor
any keys for additional computers you work from.
An alternative is to copy your public-private key pair to all additional computers,
but this approach makes it easier for an attacker to compromise all your systems
if they were to compromise one.
The section _Multiple computers_ below explains how to copy your ssh keys
from one computer to another.

## Setup instructions

### Verify that Git is installed

Open a terminal window and enter the following command.

    git

If your system reports that the command is not found,
then visit [the Git website](http://git-scm.com/) to download and install Git.

### Find or generate an SSH public-private key pair

Look in your home folder for a hidden folder named _.ssh._
Make sure that the tool you are using displays hidden folders.
Under OS X and Linux, the following command will show the contents
of your home directory including the hidden files and folders.

    ls -a ~

The _ls_ command lists the contents of a folder (also called a directory).
The _-a_ argument means list all the contents, including the hidden
files and folders.  The tilda character '~' refers to your home folder.

Under Windows, you can start the Git Bash program
and run commands similar to OS X and Limux.
Alternatively, the following command will show the contents of your home folder.

    dir %HOMEPATH%

If you want to use the Windows file viewer (Windows Explorer),
you need to configure it to show hidden files and folders
if you want to look for the .ssh folder using this tool.
Use the tool's menu to do this.

If you do not find a .ssh folder, then run the following command
to generate a public-private key pair, which will also create the
.ssh folder to contain them.
__Do NOT enter a passphrase_ for this command;
just press enter when asked for one (which it does several times).
Also, _accept the default file name and location_, and
replace "Your name" with your name.

    ssh-keygen -t rsa -C "Your name"

After running this command, verify that the .ssh folder was created and
contains your private and public keys.
Your public key is stored in the following location.

    ~/.ssh/id_rsa.pub

Your private key is stored in the following location.

    ~/.ssh/id_rsa

You can distribute your public key, but keep your private key secret.
Other systems will use your public key to create a
cryptographic challenge that only the holder of the private key can solve.
This is called asymmetric key encryption and is the foundation for most
of the secure communication on the Internet.

If you do have a .ssh folder, but it does not contain file id_rsa.pub,
then you should run the ssh-keygen command given above to generate
a public-private key pair.

### If you are working under Linux ...

If you are working under Linux, then run
_ssh-add_ to load your keys into the SSH agent.

    ssh-add

### Email your instructor the pubic key

Copy the contents of id_rsa.pub into the clipboard and paste it
into an email to your instructor.

Under OS X and Linux, you can see the contents of the public key file
by using the cat command as follows.

    cat ~/.ssh/id_rsa.pub

Under Windows, you can copy the contents of the public key file
with the following command.

    clip < id_rsa.pub

The file will look something like the following.

````
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDymdGr+QUAhvClI/7c8+ulzSxEH783b9tatMlB4ou53YgOTYrsJEN2rLilpgPeM6pxHt3EtD5aVO8boklZmzpwy/eDHSq8Dxzdhv+lxzv8KmRm8wX7vkBgezrQHoBcjWDyiztH/2MoE5uL42yT3goGPBXsbx/rq0QrwUxnzqNMjJ0R2HsWqF5VV/t0G0mJfgZVuCVBokSMmmuKof1KtUk+R0zTlxCMUhc7EMWf39gVXc6+JWJJqthV71VY8mX4y0CSsNa0/ILMIlyUV7kd4OLPi7qwjAlA292tsh+n3McaQAwWIuKJmO6gIq5rAvDsiIXbKQGaoVd4Sb6ABUuMgVo9 turner@turner-retina.home
````

Copy this text and email it to your instructor with your name.

### PROBLEM: Agent admitted failure to sign using the key

If you are working under Linux and receive the message
_Agent admitted failure to sign using the key_, then try running
ssh-add to load your keys into the SSH agent.

    ssh-add

### Multiple computers

If you want to interact with git from more than one machine,
you need to make sure that the private and public key files 
are the same across all the computers.
The private and public key files are contained in the following
files, respectively.
Both of these files contain character data,
so you can easily access and manipulate their contents.

- ~/.ssh/id_rsa
- ~/.ssh/id_rsa.pub

If you use multiple computers to complete work in this course,
remember to start a work session by pulling changes from the
remote repository, and to end a work session
by pushing changes to the remote repository.
To avoid synchronization problems, adhere to the following 2 rules.

- When you start a work sesion, pull from your remote repository.
- When you end a work session, push to your remote repository.

## Configure Git with your name and email

Before committing your work to your remote repository,
run the following commands to set your name and email.
Use your real name so your instructor can recognize you more easily.

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

Run these commands immediately;
you do not need to wait for your remote repository to be created.

You only need to do the above once (for each computer you work on).

## Learn Git

In addition to setting up Git on your system,
you will also need to learn how to use it.
While you are waiting for your instructor to create your repository,
you should complete the
[TryGit](http://try.github.io/levels/1/challenges/1) tutorial.
After this, familiarize yourself with
[the documentation for Git](http://git-scm.com/doc)
and [the Git book](http://git-scm.com/book).
After reading a few chapters of the Git book, carefully read 
[Chapter 9 on Git Internals](http://git-scm.com/book/en/Git-Internals).
The Git Internals chapter provides essential concepts
and details needed to fully understand Git.

