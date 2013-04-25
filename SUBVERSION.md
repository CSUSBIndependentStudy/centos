## Overview

This page explains one way to set up a Subversion repository on a CentOS server. 
This page assumes that you have set up the server using the instructions in BASE.md.

## Install Required Packages

Run the following yum command to install various packages.

    yum install httpd mod_ssl mod_dav_svn

## Create Repository

Create the repository and make apache the owner so that it has sufficient write privileges.

    cd /home/turner
    mkdir repo
    svnadmin create repo
    chown -R apache:apache repo

## Configure Web Server

Make sure apache starts when system boots.

    chkconfig httpd on

All web transactions will be through port 443 over https. Run the following program to open the https port:

    system-config-securitylevel-tui

The following sub-sections explain how to set up access permissions to the repository in different ways. In all these cases, you need to have apache process the configuration directives when it initializes. One way to do this is to create file /etc/httpd/conf.d/repos.conf and place the relevant configuration into it.

## Read and Write to Valid Users

To enable svn access to user maintained repositories, add the following lines to repos.conf.

```
<Location /turner>
    DAV svn
    SVNPath /home/turner/repo
    AuthType Basic
    AuthName "Subversion repository"
    AuthUserFile /home/turner/svn-users
    Require valid-user
</Location>
```

Create svn-users with user turner with the following command.

    htpasswd -c /home/turner/svn-users turner

To add additional users, omit -c.

    htpasswd -c /home/turner/svn-users smith

## Read and Write to Group

Use the following directives to restrict access to the repository by group.

```
<location /turner>
      DAV svn
      SVNPath /home/turner/repo
      AuthType Basic
      AuthName "repository"
      AuthUserFile /home/turner/svn-users
      AuthGroupFile /home/turner/svn-groups
      Require group repo
</location>
```

Create file svn-groups with the following content, assuming that turner and smith are in the svn-users file.

    repo: turner smith

See the previous section to see how to create svn-users.

## Read to All, Write to Group

Use the following configuration to restrict write access to the repository by group, but allow read access to everyone.

```
 <location /turner>
    DAV svn
    SVNPath /home/turner/repo
    AuthType Basic
    AuthName "repository"
    AuthUserFile /home/turner/svn-users
    AuthGroupFile /home/turner/svn-groups
    <LimitExcept GET PROPFIND OPTIONS REPORT>
        Require group repo
    </LimitExcept>
 </location>
```

See previous sections to see how to create svn-users and svn-groups.

## Read to Read Group, Write to Write Group

Use the following configuration to provide read access to users in the group repo-read and read/write access to the group repo-write.

```
 <location /turner>
    DAV svn
    SVNPath /home/turner/repo
    AuthType Basic
    AuthName "repository"
    AuthUserFile /home/turner/svn-users
    AuthGroupFile /home/turner/svn-groups
    <Limit GET PROPFIND OPTIONS REPORT>
        Require group repo-read
    </Limit>
    Require group repo-write
 </location>
```

See previous sections to see how to create svn-users and svn-groups. In svn-groups, declare and use the two groups repo-read and repo-write.

## Start Apache

    service httpd start

## Automated Account Creation

I use the following 2 Python scripts to help set up Subversion accounts for use in my courses.

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

