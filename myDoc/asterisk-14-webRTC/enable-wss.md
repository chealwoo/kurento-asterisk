

http.conf

[general]
enabled=yes
bindaddr=0.0.0.0
bindport=8088
tlsenable=yes
tlsbindaddr=0.0.0.0:8089
tlscertfile=/etc/asterisk/keys/foo.pem
tlsprivatekey=/etc/asterisk/keys/foo.pem
;tlsprivatekey=/etc/asterisk/keys/asterisk.key


rtp.conf

[general]
rtpstart=10000
rtpend=20000
icesupport=yes
stunaddr=stun.l.google.com:19302






enalbe tls

openssl req -new -x509 -days 365 -nodes -out /etc/asterisk/keys/foo.pem -keyout /etc/asterisk/keys/foo.pem


mkdir /etc/asterisk/keys
./ast_tls_cert -C pbx.mycompany.com -O "My Super Company" -d /etc/asterisk/keys
./ast_tls_cert -m client -c /etc/asterisk/keys/ca.crt -k /etc/asterisk/keys/ca.key -C phone1.mycompany.com -O "My Super Company" -d /etc/asterisk/keys -o malcolm



---

extensions.conf

[default]
[from-internal]
exten => 1000,1,Answer()
same => n,Playback(demo-congrats)
same => n,Hangup()




sip.conf

[general]
udpbindaddr=0.0.0.0:5060
realm=10.22.111.62  ;replace with your Asterisk server public IP address or host
transport=udp,ws,wss
tlsenable=yes
tlsbindaddr=0.0.0.0
tlscertfile=/etc/asterisk/keys/asterisk.pem
tlscafile=/etc/asterisk/keys/ca.crt
tlscipher=ALL
tlsclientmethod=tlsv1 ;none of the others seem to work with Blink as the client

[6001]
host=dynamic
secret=1234
context=from-internal
type=friend
encryption=yes
avpf=yes
force_avp=yes
icesupport=yes
directmedia=no
disallow=all
allow=ulaw
dtlsenable=yes
dtlsverify=fingerprint
dtlscertfile=/etc/asterisk/keys/asterisk.pem
dtlscafile=/etc/asterisk/keys/ca.crt
dtlssetup=actpass

[malcolm]
type=peer
secret=1234 ;note that this is NOT a secure password
host=dynamic
context=local
dtmfmode=rfc2833
disallow=all
allow=g722
transport=tls



--
pjsip.conf

[transport-ws]
type=transport
protocol=ws
bind=0.0.0.0

[transport-tls]
type=transport
protocol=tls
bind=0.0.0.0:5061
cert_file=/etc/asterisk/keys/asterisk.crt
priv_key_file=/etc/asterisk/keys/asterisk.key
method=tlsv1
 
[101]
type=aor
max_contacts=2
remove_existing=yes
 
[101]
type=auth
auth_type=userpass
password=101
username=101
 
[101]
type=endpoint
disallow=all
allow=ulaw
context=default
auth=101
aors=101
media_encryption=dtls
dtls_verify=fingerprint
dtls_cert_file=/etc/asterisk/keys/asterisk.pem
dtls_ca_file=/etc/asterisk/keys/ca.crt
dtls_setup=actpass
use_avpf=yes
ice_support=yes
media_use_received_transport=yes

[malcolm]
type=aor
max_contacts=1
remove_existing=yes
 
[malcolm]
type=auth
auth_type=userpass
username=malcolm
password=1234
 
[malcolm]
type=endpoint
aors=malcolm
auth=malcolm
context=local
disallow=all
allow=g722
dtmf_mode=rfc4733
media_encryption=sdes
