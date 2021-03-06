This document details the steps to set up DmD.

Prerequisites:
==============
A server running RHEL or CentOS.

Install Gnome Desktop
---------------------
sudo yum groupinstall “Desktop” “Desktop Platform”

Install VNC Server
sudo yum install tigervnc-server

sudo yum install vnc 
sudo chkconfig vncserver on
vim /etc/sysconfig/vncservers
add:
VNCSERVERS=“1:username 2:anotherusername”
VNCSERVERARGS[1]=“-geometry 1024x600 -nolisten tcp -localhost”

useful:
http://www.golinuxhub.com/2013/02/running-multiple-vnc-server-sessions.html

Allow vncserver through SELinux
vim /etc/sysconfig/iptables
like this:

*filter
:INPUT ACCEPT [1:52]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [2:360]

-A INPUT -m state --state NEW -m tcp -p tcp --dport 5900 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 5901 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 5902 -j ACCEPT 

COMMIT

sudo service iptables restart

*must run vncpasswd on all accounts that are defined in /etc/sysconf/vncservers before you can start vncserver..

sudo service vncserver start

Enable Gnome User Session
in each user profile vim .vnc/xstartup
modify the end of the file to look like this:
#twm &
exec gnome-session &


Open SSH Tunnel to server
ssh -L 7070:127.0.0.1:5901 ehmpd1.vaftl.us

Get rid of update manager asking for root password
gnome-session-properties 
uncheck PackageKit Update Applet

Enable EPEL Repository (for LLVM)
wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -ivh epel-release-6-8.noarch.rpm 

Install NodeJS NPM
sudo yum install npm nodejs
npm install -g underscore

For Scrubber
sudo yum install python-devel libX11-devel 

Java 1.6 JDK
http://www.if-not-true-then-false.com/2010/install-sun-oracle-java-jdk-jre-6-on-fedora-centos-red-hat-rhel/

## java ##
alternatives --install /usr/bin/java java /usr/java/jdk1.6.0_45/jre/bin/java 20000
## javaws ##
alternatives --install /usr/bin/javaws javaws /usr/java/jdk1.6.0_45/jre/bin/javaws 20000
 
## Java Browser (Mozilla) Plugin 32-bit ##
alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so libjavaplugin.so /usr/java/jdk1.6.0_45/jre/lib/i386/libnpjp2.so 20000
 
## Java Browser (Mozilla) Plugin 64-bit ##
alternatives --install /usr/lib64/mozilla/plugins/libjavaplugin.so libjavaplugin.so.x86_64 /usr/java/jdk1.6.0_45/jre/lib/amd64/libnpjp2.so 20000
 
## Install javac only if you installed JDK (Java Development Kit) package ##
alternatives --install /usr/bin/javac javac /usr/java/jdk1.6.0_45/bin/javac 20000
alternatives --install /usr/bin/jar jar /usr/java/jdk1.6.0_45/bin/jar 20000

---
alternatives --set java /usr/java/jdk1.6.0_45/jre/bin/java
alternatives --set javaws /usr/java/jdk1.6.0_45/jre/bin/javaws
alternatives --set libjavaplugin.so /usr/java/jdk1.6.0_45/jre/lib/i386/libnpjp2.so
alternatives --set libjavaplugin.so.x86_64 /usr/java/jdk1.6.0_45/jre/lib/amd64/libnpjp2.so
alternatives --set javac /usr/java/jdk1.6.0_45/bin/javac
alternatives --set jar /usr/java/jdk1.6.0_45/bin/jar

Apache & mod_perl
# this installs all apache and mod_perl dependencies
yum install mod_perl -y

Install MarkLogic NoSQL XML Database
------------------------------------
Goto http://marklogic.com
Create a developer account
Download MarkLogic 7 as an RPM file for RHEL 6 / CentOS
sudo su
yum install compat-glibc
yum install glibc.i686
rpm -i MarkLogic…


