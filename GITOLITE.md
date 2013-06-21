## Overview

__UNDER SERIOUS CONSTRUCTION__

This page explains one way to set up a Git repository on a CentOS server. 
This page assumes that you have set up the server using the instructions in BASE.md.
[Adam Weschler](https://github.com/aweschler) helped me develop these instructions.

I developed these instructions to help me create a large number of private 
remote git repositories to support courses that I teach.
In these courses, students submit work for grading by pushing into a private remote
repository that I create for them.

The approach presented here relies on gitolite, which is a free open source
system that provides a mechanism to control user access to remote git repositories
through public/private keys.
Under gitolite, the files that configure access permissions are stored in a remote repository
named _gitolite-admin_ on the server.  Changes to the gitolite configuration is accomplished
by pushing revisions to these files. 

## Notes

I will look at [gitolite](http://git-scm.com/book/en/Git-on-the-Server-Gitolite) next.

[Here](http://stackoverflow.com/questions/2083807/using-git-shell-and-restricting-developers-to-commit-to-their-own-projects) is a simple approach to controlling user access.  I may try this.

[Here](http://planzero.org/blog/2012/10/24/hosting_an_admin-friendly_git_server_with_git-shell) is a
good article about a simple setup where all users have the same access privileges.

## Instructions 

The following commands are to be executed on the server.

Create a user named _git_, but don't set a password for the account.
To do this, run the following as root.

    useradd git

Run the following as root to install required packages.

    yum install git-core python-setuptools

Assume the identity of git.

    sudo su - git

Do the following as a non-root user.

    cd
    git clone https://github.com/tv42/gitosis.git
    cd gitosis
    sudo python setup.py install

Initialize the admin repository (where user accounts are managed).

As the git user, do the following.

    ssh-keygen -t rsa
    gitosis-init < /home/git/.ssh/id_rsa.pub

Add the following to the end of _/etc/ssh/sshd_config_.

    Match User git
    PasswordAuthentication no


## Create Repository

???

## Configure ???

Make sure ??? starts when system boots.

    chkconfig ??? on

All transactions will be through port ???. Run the following program to open this port:

    system-config-securitylevel-tui

## Other Instructions ...

???

## Automated Account Creation

I use the following 2 Python scripts to help set up Subversion accounts for use in my courses.
I would like to change these to setup Git accounts.

### create_accounts.sh

```
#!/usr/bin/env python

# This script is used to configure subversion repos
# from a file of student account names and passwords.

import subprocess

# This script reads a file of comma-delimited usernames
# and passwords, where each username/password pair is
# in a line terminated by new line character.
# The file should include a empty line at the end of
# the file.

infile_name = "accounts.csv"

# The script creates repos in the following dir.
svn_path_dir_name = "/home/turner/201/repos/"

# The script appends to the auth user file and the auth group file.
auth_user_file_name = "/home/turner/201/users"
auth_group_file_name = "/home/turner/201/groups"
teachers = "turner gwillis"

# The script appends to the apache configuration file.
conf_file_name = "/home/turner/201/201.conf"
location_prefix = "/201/"

# Open all files.

infile = open(infile_name, 'r')
conf_file = open(conf_file_name, 'a')
auth_group_file = open(auth_group_file_name, 'a')

# Read an initial line.

line = infile.readline().strip(' \n')

# Process each line.

while len(line) > 0 :

    # Extract name and password.
    lst = line.split(',')
    username = lst[0].strip(' ')
    password = lst[1].strip(' ')
    username = username.strip('"')
    password = password.strip('"')

    # Create repo.
    svn_path_name = svn_path_dir_name + username
    subprocess.call("mkdir " + svn_path_name, shell=True)
    subprocess.call("svnadmin create " + svn_path_name, shell=True)
    subprocess.call("chown -R apache:apache " + svn_path_name, shell=True)

    # Add password to user file.
    cmd = "htpasswd -b " + auth_user_file_name + " " + username + " " + password
    subprocess.call(cmd, shell=True)

    # Add to group file.
    auth_group_file.write(username + ": " + username + " " + teachers + " \n")

    # Append to configuration file.
    conf_file.write("<location " + location_prefix + username + ">\n")
    conf_file.write("    DAV svn\n");
    conf_file.write("    SVNPath " + svn_path_name + "\n");
    conf_file.write("    AuthType Basic\n");
    conf_file.write("    AuthName \"repository\"\n");
    conf_file.write("    AuthUserFile " + auth_user_file_name + "\n");
    conf_file.write("    AuthGroupFile " + auth_group_file_name + "\n");
    conf_file.write("    Require group " + username + "\n");
    conf_file.write("</location>\n");
    conf_file.write("\n");

    # Get next line.
    line = infile.readline().strip(' \n')

infile.close()
conf_file.close()
auth_group_file.close()
```

### email_passwords.sh

```
#!/usr/bin/env python

# This script is used to email student usernames and
# passwords for access to their subversion repos.
#
# This script reads a file of comma-delimited usernames
# passwords and email addresses.
# Each line is terminated by new line character.
# The file should include a empty line at the end of
# the file. (Maybe blank line is not required.)

import subprocess
import smtplib

# Set the following variables accordingly.
SERVER = "mail.csusb.edu"
FROM = "dturner@csusb.edu"
SUBJECT = "Your CSE 201 Repository Account"
INFILE = "accounts.csv"

infile = open(INFILE, 'r')

# Read an initial line.

line = infile.readline().strip(' \n')

# Process each line.

# Process each line.

while len(line) > 0 :

    # Extract username, password and email.
    lst = line.split(',')
    username = lst[0].strip(' "')
    password = lst[1].strip(' "')
    recipient = lst[2].strip(' "')

    # Create email.

    body = "Username: " + username + "\nPassword: " + password
    message = ("From: %s\r\nTo: %s\r\nSubject: %s\r\n\r\n%s"
              %(FROM, recipient, SUBJECT, body))

    server = smtplib.SMTP(SERVER)
    server.sendmail(FROM, [recipient], message)
    server.quit()

    # Get next line.
    line = infile.readline().strip(' \n')

infile.close()
```
