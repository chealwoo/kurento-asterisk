[![logo](https://webrtc.ventures/wp-content/uploads/2017/01/webrtc-logo.png)](https://webrtc.ventures)

# Integration of Kurento with Asterisk

##### To install this sample follow these steps:
1) npm install  
2) npm start  
3) open this url in a WebRTC compatible browser: https://localhost:8443/

###### To learn more about this work visit our post blog: [Kurento and Asterisk: a powerful couple](https://webrtc.ventures/2017/02/kurento-asterisk-powerful-couple/)
original source code is at (https://github.com/Sfinx/k2s)



#### clone this nodejs project
```
git clone https://github.com/chealwoo/kurento-asterisk.git
```

### My test Environment
OS: Ubuntu 16

Requires g++ to run npm install 
```
sudo apt-get install g++
```

*** Ubuntu Tip
### install nodejs
https://tecadmin.net/install-latest-nodejs-npm-on-ubuntu/
```
$ sudo apt-get install python-software-properties
$ curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
$ sudo apt-get install nodejs
```

### upgrade nodejs (http://askubuntu.com/questions/426750/how-can-i-update-my-nodejs-to-the-latest-version)
```
sudo npm cache clean -f
sudo npm install -g n
sudo n stable

sudo ln -sf /usr/local/n/versions/node/<VERSION>/bin/node /usr/bin/node 
```

To upgrade to latest version (and not current stable) version, you can use
```
sudo n latest
```
To undo:
```
sudo apt-get install --reinstall nodejs-legacy     # fix /usr/bin/node
sudo n rm 6.0.0     # replace number with version of Node that was installed
sudo npm uninstall -g n
```


# Install Setup Asterisk with WebRTC
Based on document https://webrtc.ventures/2017/02/kurento-asterisk-powerful-couple/


## 1. down load Asterisk  (my version is 14.3.0)
```
$ wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-14-current.tar.gz
$ tar zxfv asterisk-14-current.tar.gz
```

## 2. WebRTC dependency
```
$ sudo apt-get install build-essential wget libssl-dev libncurses5-dev libnewt-dev libxml2-dev linux-headers-$(uname -r) libsqlite3-dev libjansson-dev uuid-dev
```
Note the document has error; remove get at the end. (uuid-dev not uuid-devget)


#### 2.1 installing pjproject
Note
* After 13 version is necessary to install pjproject to add ICE support to Asterisk. You could add a note about this.
A comment int the following document says Asterisk needs pjproject to support ICE, without it an error prevents sipml5 receives media from Asterisk.
https://wiki.asterisk.org/wiki/display/AST/Interactive+Connectivity+Establishment+(ICE)+in+Asterisk

##### 2.1.1 without install pjproject separately.
```
$ ./configure --with-pjproject-bundled
```

#### 2.1.2 install pjproject
Follow the instructions to install pjproject at
https://wiki.asterisk.org/wiki/display/AST/Building+and+Installing+pjproject

pjsip home page http://www.pjsip.org/

Without pjproject, Asterisk has the following errors
Called with a SDP without ice-ufrag and ice-pwd (http://forums.asterisk.org/viewtopic.php?f=1&t=90178) 
[WEBRTC]Asterisk12:Called with SDP without ice-ufrag ice-pwd (http://forums.asterisk.org/viewtopic.php?f=1&t=91686)
Called with a SDP without ice-ufrag and ice-pwd (https://community.asterisk.org/t/called-with-a-sdp-without-ice-ufrag-and-ice-pwd/46021)
SetRemoteDescription failed: Called with an SDP without ice-

### 2.2 installing opus codec
requires 
```
sudo apt-get install xmlstarlet
```

Try: https://pbxinaflash.com/community/threads/opus-codec-installation.20121/
* Result: once this is done, run ./configure and make menuselect > opus option is enabled now.
Found the codec is below including previous step.
```
wget http://downloads.digium.com/pub/telephony/codec_opus/asterisk-14.0/x86-64/codec_opus-14.0_current-x86_64.tar.gz
```
README (http://downloads.digium.com/pub/telephony/codec_opus/asterisk-14.0/x86-64/README)

Other links
opus codec (https://community.asterisk.org/t/asterisk14-opus-transcoding/68601/5)
https://wiki.asterisk.org/wiki/display/AST/Codec+Opus


## 3. install Asterisk with WebRTC

### 3.1 all commands used
```
$ sudo apt-get install build-essential libncurses5-dev libxml2-dev libsqlite3-dev libssl-dev libsrtp0-dev uuid-dev
$ sudo apt-get install xmlstarlet
$ wget http://downloads.xiph.org/releases/opus/opus-1.1.3.tar.gz
$ tar xvzf opus-1.1.3.tar.gz
$ cd opus-1.1.3/
$ ./configure
$ make
$ sudo make install
$ ldconfig
$ /sbin/ldconfig -v | grep opus
```
* You should get something like
libopus.so.0 -> libopus.so.0.5.3

```
$ cd ~/asteriak-14.3.0
$ cd asterisk/contrib/scripts
$ sudo ./install_prereq install
$ sudo ./install_prereq install-unpackaged
$ cd ~/asteriak-14.3.0
$ ./configure --with-pjproject-bundled
$ make menuselect
$ make 
$ sudo make install 
$ sudo make samples
```

Reference Pages
* WebRTC tutorial using SIPML5 (https://wiki.asterisk.org/wiki/display/AST/WebRTC+tutorial+using+SIPML5)


## 4. Configuration.

### 4.1 Enable tls, https, wss
SIPML5 connection to Asterisk 13 over wss (http://serverfault.com/questions/748428/sipml5-connection-to-asterisk-13-over-wss)

### 4.2 free stun servers
stun.l.google.com:19302
stun1.l.google.com:19302
stun2.l.google.com:19302



## 5. Asterisk commands

sip set debug on
rtp set debug on
http show status

http://www.asteriskguru.com/tutorials/cli_cmd_14.html


## 6 Others
Chrome
Press CTRL+SHIFT+J  -- show log




Links

Troubleshooting WebRTC Issues http://forums.asterisk.org/viewtopic.php?p=199275





Need in future
Secure Calling Tutorial (https://wiki.asterisk.org/wiki/display/AST/Secure+Calling+Tutorial)


I have not tried this
Webrtc calling using sipml5 and Asterisk (http://techvoiper.blogspot.com/2016/01/webrtc-calling-using-sipml5-and-asterisk.html)
(http://sipjs.com/guides/server-configuration/asterisk/)
