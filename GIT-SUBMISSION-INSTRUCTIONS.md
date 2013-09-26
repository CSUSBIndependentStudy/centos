## Overview

These are generic instructions to be used by courses at CSUSB
in which students submit work via Git.

In order for your instructor to set up a private remote repository for you,
you need to send to your instructor the public key that your Git client
program uses when connecting to remote hosts.
After you send this, your instructor will create an empty
remote repository for you to push commits into.

Your instructor will also send you a username that you can use to
connect to this repository.
After you receive the username, you will be able
to clone the remote repository and use this to
track changes to the source code you develop and submit your work.

The command you will use to clone your repository will
look something like the following, with _username_
replaced by the username your instructor emails to you.

    git clone username@web2.ias.csusb.edu:201.git

Each time you complete an assignment,
you should commit your work into a local repository
and then push changes to the remote repository.

Also, you can push changes to your source code that is incomplete.
This lets your remote repository serve as a backup
of your work.  It also allows you to easily work from
multiple computers by synchronizing your local repositories with
your remote repository.

## Security

Email is not completely private.
There are a number of different actors who can read
emails, including employees of the companies that
provide email service to the sender and receiver,
workers who are part of government surveillance activities
and any criminals who may have compromized systems of one of the
email service providers.

With this in mind,
it's safer to email public keys than passwords.
If someone gets your password, they can
authenticate as you;
if they get your public key, they can not.

However, one danger with sending public keys by email
is that an imposter
can send the recipient their own public key.
To mitigate this threat,
we will accept only a single public key for each repository.
Consequently, if you want to access your repository from
multiple computers,
you need to copy your public-private key pair
to all these computers.
See the section _Multiple computers_ below
to see how to do this.

## Setup instructions

### Verify that Git is installed

Open a terminal window and enter the command _git_.
If your system reports that the command is not found,
then visit [the Git website](http://git-scm.com/) to download and install Git.

### Find or generate an SSH public-private key pair

Look in your home folder for a hidden folder named _.ssh._
Make sure that the tool you are using displays hidden folders.
Under OS X and Linux, the following command will show the contents
of your home directory including the hidden files and folders.

    ls -a ~

The ls command lists the contents of a folder (also called a directory).
The -a argument means list all the contents, including the hidden
files and folders.  The tilda character '~' refers to your home folder.

Under Windows, you can open the the Git Bash program
and run the commands given above for OS X and Limux.
alternatively, the following command will show the contents of your home folder.

    dir %HOME%

If you want to use the Windows file viewer (Windows Explorer),
you need to configure it to show hidden files and folders
if you want to look for the .ssh folder using this tool.

If you do not find a .ssh folder, then run the following command
to generate a public-private key pair, which will also create the
.ssh folder to contain them.
This comand will prompt you for a passphrase -- do NOT enter a passphrase,
just press enter when asked for one (which it does several times).
Also, accept the default file name and location.
Replace "Your name" with your name.

    ssh-keygen -t rsa -C "Your name"

After running this command, verify that the .ssh folder exists and
contains your private and public keys.
Your private key is stored in the following location.

    ~/.ssh/id_rsa

Your public key is stored in the following location.

    ~/.ssh/id_rsa.pub

You should keep you private key secret but you can distribute your
public key.  Other systems will use your public key to create a
cryptographic challenge that only the holder of the private key can solve.
This is called asymmetric key encryption and is the foundation for most
of the secure communication on the Internet.

If you do have a .ssh folder, but it does not contain file id_rsa.pub,
then you should run the ssh-keygen command given above to generate
a public-private key pair.

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

## Learn Git

In addition to setting up Git on your system,
you will also need to learn how to use it.
While you are waiting for me to create your repository,
you should complete the
[TryGit](http://try.github.io/levels/1/challenges/1) tutorial.
After this, familiarize yourself with
[the documentation for Git](http://git-scm.com/doc)
and [the Git book](http://git-scm.com/book).
I recommend that you read a few chapters of the
Git book and then carefully read 
[Chapter 9 on Git Internals](http://git-scm.com/book/en/Git-Internals).
The Git Internals chapter provides essential concepts
and details needed to understand Git.
