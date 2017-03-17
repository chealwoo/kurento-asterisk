[![logo](https://webrtc.ventures/wp-content/uploads/2017/01/webrtc-logo.png)](https://webrtc.ventures)

# Integration of Kurento with Asterisk

##### To install this sample follow these steps:
1) npm install  
2) npm start  
3) open this url in a WebRTC compatible browser: https://localhost:8443/

###### To learn more about this work visit our post blog: [Kurento and Asterisk: a powerful couple](https://webrtc.ventures/2017/02/kurento-asterisk-powerful-couple/)
original source code is at (https://github.com/Sfinx/k2s)



### clone project
git clone https://github.com/chealwoo/kurento-asterisk.git


### background 
ubuntu 16

Requires g++ to run npm install 
sudo apt-get install g++



*** Ubuntu Tip
install nodejs
https://tecadmin.net/install-latest-nodejs-npm-on-ubuntu/
$ sudo apt-get install python-software-properties
$ curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
$ sudo apt-get install nodejs










-- System Installation 

Following document of
https://webrtc.ventures/2017/02/kurento-asterisk-powerful-couple/


Install Setup Asterisk with WebRTC

1. down load Asterisk
$ wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-14-current.tar.gz

2. install dependency
*** this command has error remove get at the end. (uuid-dev not uuid-devget)
$ sudo apt-get install build-essential wget libssl-dev libncurses5-dev libnewt-dev libxml2-dev linux-headers-$(uname -r) libsqlite3-dev libjansson-dev uuid-dev

* I had to install uuid-dev separately > Run two separate command
$ sudo apt-get install build-essential wget libssl-dev libncurses5-dev libnewt-dev libxml2-dev linux-headers-$(uname -r) libsqlite3-dev libjansson-dev
$ sudo apt-get install uuid-dev

2.1 installing pjproject
A comment int the following document says Asterisk needs pjproject to support ICE, without it an error prevents sipml5 receives media from Asterisk.
https://wiki.asterisk.org/wiki/display/AST/Interactive+Connectivity+Establishment+(ICE)+in+Asterisk
Follow the instructions to install pjproject at
https://wiki.asterisk.org/wiki/display/AST/Building+and+Installing+pjproject

pjsip home page http://www.pjsip.org/

error --> solution to me is installing pjproject.
Called with a SDP without ice-ufrag and ice-pwd (http://forums.asterisk.org/viewtopic.php?f=1&t=90178) 
[WEBRTC]Asterisk12:Called with SDP without ice-ufrag ice-pwd (http://forums.asterisk.org/viewtopic.php?f=1&t=91686)
Called with a SDP without ice-ufrag and ice-pwd (https://community.asterisk.org/t/called-with-a-sdp-without-ice-ufrag-and-ice-pwd/46021)
SetRemoteDescription failed: Called with an SDP without ice-



3. unzip Asterisk source 
$ tar zxfv asterisk-14-current.tar.gz

4. To build Asterisk, follow the page
*WebRTC tutorial using SIPML5* (https://wiki.asterisk.org/wiki/display/AST/WebRTC+tutorial+using+SIPML5)
---------------------------------------------------------------------------------------
sudo apt-get install build-essential libncurses5-dev libxml2-dev libsqlite3-dev libssl-dev libsrtp0-dev uuid-dev

cd asterisk/contrib/scripts
sudo ./install_prereq install
sudo ./install_prereq install-unpackaged
cd asterisk
./configure && make menuselect
make 
sudo make install 
sudo make samples
---------------------------------------------------------------------------------------


4.1 Enable tls, https, wss
SIPML5 connection to Asterisk 13 over wss (http://serverfault.com/questions/748428/sipml5-connection-to-asterisk-13-over-wss)

4.2 free stun servers
stun.l.google.com:19302
stun1.l.google.com:19302
stun2.l.google.com:19302



5. Asterisk commands

sip set debug on
rtp set debug on
http show status


6
Chrome
Press CTRL+SHIFT+J  -- show log




Links

Troubleshooting WebRTC Issues http://forums.asterisk.org/viewtopic.php?p=199275





Need in future
Secure Calling Tutorial (https://wiki.asterisk.org/wiki/display/AST/Secure+Calling+Tutorial)


I have not tried this
Webrtc calling using sipml5 and Asterisk (http://techvoiper.blogspot.com/2016/01/webrtc-calling-using-sipml5-and-asterisk.html)
(http://sipjs.com/guides/server-configuration/asterisk/)
