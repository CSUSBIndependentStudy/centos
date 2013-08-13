# Setting User Storage Quotas

This page contains notes for configuring user disk quotas under centos.

When building the kernel, make sure that support for the quota utility is enabled. You can use make menuconfig to check and set this feature. On the default distribution of Redhat9 support for the quota utility is enabled.

Quotas are configured for partitions.
So, when creating the file system, allocate a partition for use with the quota utility.
For example, create a partition called _/home_ with file type _ext3_.

The following commands show how to place a quota on the user test1
with home directory _/home/test1_, which must reside in the _/home_ partition.

Install the quota utility and initialize for use.

    yum install quota

Edit _/etc/fstab_ so that _/home_ partition is marked for use for the quota utility.
Add the label _usrquota_ to the revelant line in this file as shown in the following example. 
Note 1: if you want to use group quotas, then add the label for this as well; 
see documentation for details). 
Note 2: filesystem type ext2 is OK.

~~~~
LABEL=/home    /home   ext3   defaults,usrquota   1  2 
Remount /home. (No need to reboot.)

mount -o remount /home
Create the file /home/aquota.user.

touch /home/aquota.user
chmod 600 /home/aquota.user
~~~~

Set the quota for user test1. 
The following command presents an editable view of the quota settings for test1.

    edquota -u test1

The file appears as follows.

~~~~
Disk quotas for user test1 (uid 501):
  Filesystem                   blocks       soft       hard     inodes     soft     hard
  /dev/hda3                    413664          0          0       1911        0        0
~~~~

The values under columns 3 and 4 represent the soft and hard block limits. 
The values under columns 6 and 7 represent the soft and hard block inode limits (number of files). 
Set these values according to your requirements, and save the file (using vi save command).

After setting quotas for all users, run quota check in order to update _/home/aquota.user_.

    quotacheck -vagum

If you see a warning message, it probably means that the quota system is on. 
In this case, turn it off before running quotacheck using the following command.

    quotaoff -a /home

After setting quotas for all users, turn quota checking on.

    quotaon -a -v

To examine the state of the quota system, you can generate a report as follows.

    repquota /home

The report appears as follows.

~~~~
*** Report for user quotas on device /dev/hda3
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --   70124       0       0              4     0     0
tongyu    --  145448       0       0          12960     0     0
turner    --  413664       0       0           1911     0     0
test1     --      48      50      52             10   148   150
~~~~

