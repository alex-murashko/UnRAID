# Overbyrns' Plugins for unRAID v5

## Details

This is my collection of plugins for UnRAID;


* **Air Video**: Stream videos in almost any format to your iPhone, iPad and iPod touch. You don't need to copy your videos to the device just to watch them.  If the videos in your collection are not in format supported by iPhone or iPad, Air Video will convert them on fly. You don't need to wait until the entire video is converted. You can start watching it almost immediately

* **Beets**: The purpose of beets is to get your music collection right once and for all. It catalogs your collection, automatically improving its metadata as it goes using the MusicBrainz database. (It also downloads cover art for albums it imports.) Then it provides a bouquet of tools for manipulating and accessing your music.

* **BTSync**: Synchronizes files using a peer-to-peer (P2P) protocol. This protocol is very effective for transferring large files across multiple devices, and is very similar to the powerful protocol used by applications like µTorrent and BitTorrent. The data is transferred in pieces from each of the syncing devices, and BitTorrent Sync chooses the optimal algorithm to make sure you have a maximum download and upload speed during the process.

* **DenyHosts**: Analyzes the sshd server log messages to determine what hosts are attempting to hack into your system. It also determines what user accounts are being targeted. It keeps track of the frequency of attempts from each host. Additionally, upon discovering a repeated attack host, the /etc/hosts.deny file is updated to prevent future break-in attempts from that host. 

* **Dropbox**: Free service that lets you bring all your photos, docs, and videos anywhere. This means that any file you save to your Dropbox will automatically save to all your computers, phones and the Dropbox website.  This plug-in makes it possible to use your UnRAID server as a Dropbox location.

* **NZBGet**: A cross-platform binary newsgrabber for nzb files, written in C++. It supports client/server mode, automatic par-check/-repair and a web-interface. NZBGet requires low system resources making it ideal for running alongside UnRAID.

* **OpenSSH**: Provides encrypted communication sessions over a computer network using the SSH protocol.  SSH is typically used to log into a remote machine and execute commands, but it also supports tunneling, forwarding TCP ports and it can transfer files using the associated SSH file transfer (SFTP) or secure copy (SCP) protocols.

* **SABnzbd**: UNSUPPORTED!  This is a fork of sabnzbd plugin as done by Influencer.  It is for personal use only and as such is not supported in any way.  If you choose to use it, please do NOT expect any support.  Posts on the UnRAID forum will be ignored.  In fact I'll riducule you for ignoring this very obvious message.


## Download Links
Use the following direct links to download each plugin:

Air Video - (https://github.com/overbyrn/UnRAID/raw/master/airvideo_overbyrn.plg)<br>
Beets - (https://github.com/overbyrn/UnRAID/raw/master/beets_overbyrn.plg)<br>
BTSync - (https://github.com/overbyrn/UnRAID/raw/master/btsync_overbyrn.plg)<br>
DenyHosts - (https://github.com/overbyrn/UnRAID/raw/master/denyhosts_overbyrn.plg)<br>
Dropbox - (https://github.com/overbyrn/UnRAID/raw/master/dropbox_overbyrn.plg)<br>
NZBGet - (https://github.com/overbyrn/UnRAID/raw/master/nzbget_overbyrn.plg)<br>
OpenSSH - (https://github.com/overbyrn/UnRAID/raw/master/openssh_overbyrn.plg)<br>

## Installation
<p>
1. Download the plugin<br>
2. Copy the plug-in to /boot/config/plugins on your flash drive<br>
3. Reboot your UnRAID server or alternatively if you are currently using your server eg. it is working on something else, parity check, preclearing etc, you can install it from command line:

<pre><code>eg. installplg /boot/config/plugins/&lt;plugin_name&gt;_overbyrn.plg</pre></code>
    
4. Go to UnRAID WebGui -> Settings -> &lt;Plugin Name&gt; and configure your initial settings
</p>

## Plug-in Updates
From time to time, changes may be required to the plug-in.  eg. bug fixes, enhancements.  With the exception of Beets, the plug-ins are designed to check for updates to themselves upon entering the plug-in settings page. If a later version is found, you will be presented with an option to update. The update process will stop the application (if running), perform the update and restart the application.  A backup copy of the plug-in is saved to /boot/config/plugins/&lt;plugin_name&gt; as &lt;plugin_name&gt;_overbyrn.plg.sav.

Two plugins: NZBGet & Dropbox also allow for updates to the application.  As with the plugin update, the plugin will check for an update and display a message if one is available.

***
## Plugin Specific Information - SSH

It is important to understand the way this plug-in decides what users are allowed SSH access and there are a number of settings which work together to provide a flexible and yet secure set of login options.  Please spend a few minutes reading the details below as it will help you configure your system correctly.

### Creating & Selecting Users
The plug-in allows multiple users to have SSH access.  Use the standard **Users** page in the UnRAID WebGUI to create and maintain your users.  Alternatively, you may create users from the command line - see **Advanced User Creation** section below.

When you enter the SSH plug-in, all users are listed and you are able to choose between none, one or many. If no users are selected, user *root* will be assumed - however read below to understand how to configure for *root* access:

### Configuration Options
* `Permit Root Login`
   
  This option determines if user *root* is enabled for SSH.  NB. Review the other options below as they affect whether root (or other users) can login, depending on the configuration of your users.  eg. password assigned/empty etc. 
   
* `Password Authentication`

  If this option is set to "no", then it is not possible to login using SSH unless the user has been configured with a public / private key pair. (See ***Creating Key-Pairs*** section)
  
* `Permit Empty Passwords`

  If this option is set to "no" but the user you wish to login with has a blank password, then SSH login will not be possible.  The exception to this is rule is when a user has been configured with a public / private key pair.

>**Scenario 1:** you require SSH access for *root*.  *root* already has a password...
>- Enable option `PermitRootUser`
>- Enable option `Password Authentication`

>**Scenario 2:** you require SSH access for *root*.  *root* does **not** have a password...
>- Enable option `PermitRootUser`
>- Enable option `Password Authentication`
>- Enable option `Permit Empty Passwords`
>- or
>- Assign a private / public key-pair (See ***Creating Key-Pairs*** section)

>**Scenario 3:** you require SSH access for standard user.  User already has a password...
>- Select the user from the selection list
>- Enable option `Password Authentication`

### Advanced User Creation
When a user is created from within the UnRAID WebGUI, it is assigned a user id (UID) of 1000 or greater.  When you create a user from outside of UnRAID at command line level, you must ensure your user is created with a suitable UID.  The plug-in assumes if a user has a UID >= 1000, the user is *not* a system user as is therefore presented as an option in the plug-in.  You can check the UID settings on your UnRAID system by viewing /etc/login.defs:
>\#<br>
>\# Min/max values for automatic uid selection in useradd<br>
>\#<br>
>UID_MIN                  1000<br>
>UID_MAX                 60000<br>
>\# System accounts<br>
>SYS_UID_MIN               101<br>
>SYS_UID_MAX               999<br>

Create users from command line using the following;
<pre><code>useradd -m -g users -s /bin/bash someuser</pre></code>
The above command creates user "someuser", including home dir and valid shell.

### Creating Key-Pairs
You may have your users authenticate with a public / private key-pair.  This allows you to remove password authentication altogether if so desired.  Key-pairs provide an additional level of security, especially when assigned a passphrase.

The plug-in has been designed to check for the existance of a public key for each user set-up for SSH (inc. root).  Perform the following steps to configure a user for a key-pair.  We will use the example user `fred`:

<p>
1. In UnRAID **Users** page; create a new user (or do it from command line)<br>
2. Go to OpenSSH settings page and select that user for SSH.  Hit Apply and/or Start SSH<br>
3. Browse to the plug-in directory on your flash drive : /boot/config/plugins/ssh
- There should now be a sub-directory called `fred`
- Inside this directory is another called `.ssh`
- Open the **read_me.txt** file which you will find in this directory. This file contains full instructions on how to create a key-pair and configure for the plug-in.  It also contains steps on how to convert the OpenSSH private key into a Putty compatible format.  (all utilities needed are supplied as part of the plug-in installation). A master copy of *read_me.txt* is also stored in the root plugin directory (/boot/config/plugins/ssh).

***
## Support
Please provide feedback to any problems encountered or to request enhancements or missing features that you would like to see added.  

Support requests should be made in the appropriate UnRAID forum.  Threads are at the following locations:

Air Video - (http://lime-technology.com/forum/index.php?topic=20049.0)<br>
Beets - (http://lime-technology.com/forum/index.php?topic=27230.0)<br>
BTSync - (http://lime-technology.com/forum/index.php?topic=27153.msg241607#msg241607)<br>
DenyHosts - (http://lime-technology.com/forum/index.php?topic=20848.msg184998#msg184998)<br>
Dropbox - (http://lime-technology.com/forum/index.php?topic=23287.0)<br>
NZBGet - (http://lime-technology.com/forum/index.php?topic=23423.0)<br>
OpenSSH - (http://lime-technology.com/forum/index.php?topic=20848.0)<br>


Kind Regards,<br>
overbyrn
