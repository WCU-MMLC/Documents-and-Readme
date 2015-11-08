# Linux 101

*This document assumes you're using the BASH shell. An interesting way to see your shell is: grep myUserName /etc/passwd*

## Basic Navigation

**cd** (Change Directory)

    # Move into a directory using absolute path
    cd /etc/ssh
    # Move up a directory
    cd ..
    # Go to scripts directory in your home
    cd ~/scripts/

The *cd* command is how you traverse directories in Linux. The root directory is represented as a */* (slash), and the home directory as a *~* (tilda). You can provide absolute or relative paths. A single *.* (period) represents the current directory, and a double *..* (periods) are one directory up.

**pwd** (Print Working Directory)

    # Move to your home directory
    cd ~
    # Output current directory, e.g. /home/username
    pwd

**pushd** (Push Directory)

    # Push the current directory onto the stack
    pushd .
    # Now if you change directories
    cd /some/impossible/to/remember/path
    # You can easily return to the old one
    popd

**ls** (List Directory Conents)

    # List directory contents
    ls
    # List contents, and hidden (dot) files
    ls -a
    # Same as above, but show permissions
    ls -al
    # Same as above, but sort by time
    ls -alt
    # Same as above, but show newest files last
    ls -altr
    # List files recursively, newest first
    ls -tR
    # Find all mp3 files, recursively
    ls -R *.mp3

**cat** (Concatenate)

    # Show the contents of .bashrc
    cat ~/.bashrc

    # Show the contents of all rc files
    cat ~/.*rc

Though not related directly to directory traversal, the *cat* command is a way to quickly view the contents of a file, or combine multiple text files.

## Setting Permissions

Linux permissions are broken down into 3 sets: User, Group, and Other. Within each set, there are permissions for **r**ead, **w**rite, and e**x**ecute.
You can see the permissions of a file by issuing the *ls -l* command:

    -rw------- 1 ribsdad ribsdad     44 Aug 29 12:11 .bash_history
    -rw-r--r-- 1 ribsdad ribsdad    220 Nov 12  2014 .bash_logout
    -rw-r--r-- 1 ribsdad ribsdad   3515 Nov 12  2014 .bashrc
    -rw------- 1 ribsdad ribsdad 108326 Aug 31 05:56 mbox
    drwxr-xr-x 2 ribsdad ribsdad   4096 Aug 29 12:13 .ssh

In the example above, the *mbox* (Mailbox) file is only readable and writeable by user (owner). The *.ssh* directory is readable, writeable, and executable by the user; readable and executable by the group; and readable and executable by others.

Just to the right of the permissions, the user and group are displayed.

**chmod** (Change File Mode)

    # Make a file executable to user
    chmod u+x startup.py

    # Remove read permissions for others
    chmod o-r diary.txt

**chown** (Change Ownership)

    # Change the ownership to wife
    chown wife diary.txt

    # Change ownership and group
    chown wife:family diary.txt

    # Recursively change ownership
    chown -R husband:family .

## Starting/Stopping Services

**systemctl** (Systemd Control)

    # Start postfix in Debian 7
    service postfix start

    # Start postfix in Debian 8
    systemctl start postfix
    # Get the status of postfix
    systemctl status postfix
    # See all Services
    systemctl --type=service
    # Enable postfix on boot
    systemctl enable postfix

Starting with Debian 8 (available with *apt-get dist-upgrade* on the Pi), the default init system is *systemd*. This is the same init system that RedHat, Arch Linux, and now Ubuntu use. Prior to systemd, the *service* command was used.

## Using a Firewall

**ufw** (Uncomplicated Firewall)

    # Install UFW
    apt-get update && apt-get install ufw -y
    # Enable UFW
    ufw enable
    # See existing rules
    ufw status
    # Allow outside SSH connections
    ufw allow 22/tcp
    # Allow SSH from 152.30.X.X subnet
    ufw allow proto tcp from 152.30.0.0 to any port 22

Default firewalls vary from distro to distro, but the *ufw* firewall is popular in Ubuntu and Debian.  By default, in Debian, no rules are defined -- so make sure to add a rule for SSH if connecting remotely!

## Securing SSH

**ssh-keygen** (Generate SSH Key)

    # Generate an SSH key
    ssh-keygen -t rsa -b 4096
    # Copy public key to another server
    ssh-copy-id ribsdad@ribsdad.com

An SSH key has two parts, a public key and a private key. The public key can be shared with other servers to identify the user. The private key must be kept secret, and is used to sign-in to servers (verified with public key.)

The *PuTTYGen* program for windows can also be used to generate public/private key pairs.
