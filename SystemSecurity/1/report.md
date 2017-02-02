[back to Manifest](/a2/README.md)

__Vulnerability__: linux single (GRUB):

   __Status__:
        This vulnerability has been removed from the Ubuntu804 server, as outlined in the tutorial, 
    
   __Resolution__: 
        No action taken at this time

   __Defense__:
        No Defense is needed

__Vulnerability__: set-uid/set-gid vulnerabilities

   __Procedure__: 
   I typed in:
   find / -perm -04000 -uid 0 -print > Set-UID-root-programs
   find / -perm -02000 -gid 0 -print > Set-GID-root-programs
   
   __Status__: 
   The output of the program was:
   ```shell
   >/lib/dhcp3-client/call-dhclient-script
   
   >/usr/lib/openssh/ssh-keysign
   
   >/usr/lib/eject/dmcrypt-get-device
   
   >/usr/lib/pt_chown
   
   >/usr/lib/apache2/suexec
   
   >/usr/sbin/sensible-mda
   
   >/usr/sbin/pppd
   
   >/usr/bin/sudoedit
   
   >/usr/bin/traceroute6.iputils
   
   >/usr/bin/mtr
   
   >/usr/bin/gpasswd
   
   >/usr/bin/procmail
   
   >/usr/bin/chfn
   
   >/usr/bin/chsh
   
   >/usr/bin/newgrp
   
   >/usr/bin/arping
   
   >/usr/bin/passwd
   
   >/usr/bin/sudo
   
   >/bin/fusermount
   
   >/bin/umount
   
   >/bin/ping6
   
   >/bin/ping
   
   >/bin/mount
   
   >/bin/su
   ```

   
   The permissions of the files that look like they are supposed to be private:   
    ```shell
   -rwsr-xr-x 1 root root 29104 2008-12-08 04:14 /usr/bin/passwd
   -rwsr-xr-x 2 root root 107936 2009-02-16 22:17 /usr/bin/sudo
   -rwsr-xr-x 2 root root 107936 2009-02-16 22:17 /usr/bin/sudoedit
   -rwsr-xr-x 1 root root 81368 2008-09-26 08:43 /bin/mount
   -rwsr-xr-x 1 root root 168340 2008-05-14 10:35 /usr/lib/openssh/ssh-keysign
    ```
    
   __Defense__:
   Locate all programs that are settuid root, determine who should be allowed to run these, whether they need root privilege.     For example, you probably do not want the web server to be able to run many of the set-uid programs. The programs that we do    not want the web server to have access to includes "passwd", "sudo", "sudoedit", "mount", and "ssh-keysign". We need to     
   ensure the web server is not able to run these programs or have access to them. 

__Vulnerability__: Hacker can DOS the system (use up resources):

   __Defense__:
   To prevent the Hacker from being able to use up all the resources in the system, we go into the "limits.conf" file and limit    the number of processes a user can make. This way, we prevent the system from being DOSed.
   For example, we have
   

    |...| ...Domain     |      Type     |       Item    |    Value |
    |---| ------------- |:-------------:| -------------:|---------:|
    |   |      #*         |      soft         |    core           | 0 |
    |   |       #*        |      hard         |    rss           | 10000 |
    |   |      #@student         |     hard          |   rss            | 20 |
    |   |      #@faculty         |     soft          |      nproc         | 20 |
    |   |   #@faculty            |     hard          |      nproc         | 50 |
    |   |   #ftp            |     hard           |       nproc        | 0
    |   |      .         |               |               | |
    |   |       .        |               |               | |
    |   |       .        |               |               |
  
   
   We can change the "value" in each of the respective rows to a lower number to make sure the system processes don't get abused.
   
__Vulnerability__: hacker can cause a core dump and then gdb it to find out how processes work. Find hidden data in memory e    etc. This is similar to SQL injecting a database to get more information from the schema.

   __Defense__:
   We can prevent these core dumps by going to the "limits.conf" file in /etc/security/ and then limit the number of processes    per user to 150 and also the size of any one file to 100mb

```shell
#limit user processes per user to 150
* soft nproc 100
* hard nproc 150
```
```shell
#limit size of any one file to 100MB
* hard fsize 100000
```
5. __Vulnerability__:  umask is too permissive by default. When umask is too permissive, files that are newly created in the system have too much control over the system. Restricting the settings in umask will ensure this won't happen. Here is more info on it: (https://en.wikipedia.org/wiki/Umask)

__Defense__: modify umask in /etc/bashrc and /etc/csh.login

6.__Vulnerability__: All users can run 'cron' jobs on the system. Cron jobs allow hackers to install jobs that run intermittantly. Some of these jobs are normal and turned on by a system administrator or the system itself, while other ones that have more suspicious characteristics can be placed by a hacker to malciously attack your computer. Here is how to check for cron jobs:

```shell
cd /etc/cron.hourly
cd /etc/cron.daily
cd /etc/cron.weekly
```
Here is what I see in each of them
```shell
Cron.hourly
"backup"
```
```shell
Cron.daily
"apache2"
"aptitude"
"logrotate"
"mlocate"
"standard"
"apt"
"bsdmainutils"
"man-db"
"sendmail"
"sysklogd"
```
```shell
cron.weekly
"man-db"
"popularity-contest"
"sysklogd"
```
```shell
cron.monthly
"standard"
```
```shell
cron.d
"php5"
"postgresql-common"
"sendmail"
```
Here, we can see the scripts and processes running during the respective intervals, as well as scripts not running yet, but scheduled to do so at a certain point. We can further look into the scripts scheduled to run by opening them and observing the code to see if there is anything malicious. In terms of the executable processes, we look out for names that should not be there. In our example, processes such as "popularity-contest" is not a default system file.

__Defense__:
In order for us to prevent these jobs being scheduled, we need to ensure proper permissions are set in the system. For example, we need to control the access to cron to specific users to ensure it is not used by the wrong hands. The current VM does not have "cron.allow" present in /etc/cron.allow. When this is present, we only let the listed accounts in the file have access to cron. If this file does not exist, anyone can have access to cron. However, if another file, "cron.deny" is present in "/etc/cron.deny", then only accounts that are not listed in that file are allowed to have access to cron. However, we recall that it is always better to whitelist than blacklist. 


__Vulnerability__: Users can run 'at' jobs on the system. These at jobs, are similar to timers. Users can set at jobs to run within a certain time for the present time. These are different then cron jobs as "at" jobs only occur once rather than on a schedule. The same fixes and flaws occur with "at" jobs as "cron" jobs. To see what is scheduled, we go to /var/spool/at and see if there are any scheduled jobs. We can look for "at" files in the system, the current one that exists in the VM is "a.deny". The file has:
```shell
alias
backup
bin
daemon
ftp
games
gnats
guest
irc
lp
mail
man
nobody
operator
proxy
qmaild
qmaill
qmailp
qmailq
gmailr
gmails
sync
sys
www-data
```
Here, we can observe the processes that are not allowed to create "at" requests, which is blacklisting. Since no "at.allow" file is present, anybody who is not blacklisted is allowed to create "at" requests.

__Defense__: To defend against this, it is beneficial for the system to create an "at.allow" file and whitelist the users that are allowed to create these requests.

__Vulnerability__: World writeable files and directories. These writeable files are a bad thing in general, especially for system files because anyone is allowed to modify these. Therefore, these files can be modified to have a malicious purpose.

__Defense__: To defend against these, we run commands that let us view the files that are world writeable, and audit them to see if any of them are suspicious. The only FHS-mandated directories that are commonly world-writable are /tmp and /var/tmp. In both cases, that's because they are intended for storing temporary files that may be made by anyone.

Also common is /dev/shm, as a tmpfs (filesystem backed by RAM), for fast access to mid-sized data shared between processes, or just creating files that are guaranteed to be destroyed on reboot.There may also be a /var/mail or /var/spool/mail, and sometimes other spooler directories. Those are used to hold mail temporarily before it's processed. They aren't always world-writable, depending on the tools in use. When they are, it's because files can be created there by user tools for processing by daemons. 

Otherwise, if any of the system files are made world-writable, then we can run into problems in terms of system security.

__Vulnerability__: User can boot off another device (they pressed F2 during boot), then they are allowed software that they would generally not be able to. From here, they can access the harddrive on the computer to attack the files stored in the local harddrive. One method to do this, is:
1) Boot from CD/USB/Network
2) mount /dev/hda1 /mnt
3) echo "r00t::0:0:r00t:/:/bin/bash" >> /etc/passwd (etc.)

This way, they can obtain all the files stored in the psswrd folder.

__Defense__: To avoid this, we can go into the BIOS and change the boot options to make sure it boots from the harddrive first. Afterwards, we can add a password into the BIOS to make sure the hacker cannot access it to change the boot options back. 
