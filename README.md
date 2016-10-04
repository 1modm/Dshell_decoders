# Dshell
An extensible network forensic analysis framework.  Enables rapid development of plugins to support the dissection of network packet captures.

Key features:


* Robust stream reassembly
* IPv4 and IPv6 support
* Custom output handlers
* Chainable decoders

[Dshell](https://github.com/USArmyResearchLab/Dshell)


# Custom Dshell_decoders
SIP and RTP decoders for Dshell

```
The Session Initiation Protocol (SIP) decoder will extract the Call ID, User agent, Codec, Method, 
SIP call, Host, and Client MAC address from every SIP request or response packet found 
in the given pcap using by default the port 5060.  

General usage:

    decode -d sip <pcap> 
    or 
    decode -d sip --sip_port=5062 <pcap>

Examples:

    https://wiki.wireshark.org/SampleCaptures#SIP_and_RTP
    http://vignette3.wikia.nocookie.net/networker/images/f/fb/Sample_SIP_call_with_RTP_in_G711.pcap/revision/latest?cb=20140723121754

    decode -d sip metasploit-sip-invite-spoof.pcap
    decode -d sip Sample_SIP_call_with_RTP_in_G711.pcap

    Output:

    sip 2016-09-21 23:44:20         10.5.1.7:5060  --       10.1.30.60:5060  ** 
        --> SIP Request <--
        From: 10.5.1.7 (81:89:23:d6:c2:a1) to 10.1.30.60 (a1:03:fc:f2:01:bc) 
        Sequence and Method: 414 PUBLISH
        Via: SIP/2.0/UDP 10.5.1.7:5060;branch=z9hG1bK8e35adab-ba7e-e611-937f-68a3c4f0d5ce;rport
        SIP call: <sip:demo-alice@10.1.30.60> --> <sip:demo-alice@10.1.30.60> 
        With: Ekiga/4.0.1
        Call ID: ee8ace41-ab7e-e511-917f-64a3a4f0d5ce@ProBook
     **
    sip 2016-09-21 23:44:27         10.5.1.7:5060  --         10.5.1.8:5060  ** 
        --> SIP Response <--
        From: 10.5.1.7 (00:00:00:00:00:00) to 10.5.1.8 (a1:03:fc:f2:02:bc) 
        Sequence and Method: 1 INVITE
        Via: SIP/2.0/UDP 10.5.1.8:5060;branch=z2hG4bK25a8d5a4-8a13-1920-9d58-04002772a5e9;rport=5060;received=10.5.1.8
        SIP call: "M" <sip:M@10.5.1.8>;tag=0ba2d5c4-8a13-1910-9d55-08002772a6e9 --> "miguel" <sip:demo-alice@10.5.1.7>;tag=84548c9d-ba7e-e611-937f-68a3c4f0d5ce 
        With: Ekiga/4.0.1
        Call ID: 0ba2d7c4-8a13-1940-9d57-08002372a6e9@M-PC
        Allow: INVITE,ACK,OPTIONS,BYE,CANCEL,SUBSCRIBE,NOTIFY,REFER,MESSAGE,INFO,PING,PRACK
        Codec: PCMU
        Rate: 8000 Hz
     **
```


```
The real-time transport protocol (RTP) decoder will extract the Hosts, Payload Type, Synchronization source, 
Sequence Number, Padding, Marker and Client MAC address from every RTP packet found in the given pcap.

General usage:

    decode -d rtp <pcap> 

Examples:

    https://wiki.wireshark.org/SampleCaptures#SIP_and_RTP
    https://wiki.wireshark.org/SampleCaptures?action=AttachFile&do=get&target=rtp_example.raw.gz

    decode -d rtp rtp_example.pcap

    Output:
    
    rtp 2002-07-26 07:19:10        10.1.6.18:2006  --       10.1.3.143:5000  ** 
        From: 10.1.6.18 (00:08:21:91:64:60) to 10.1.3.143 (00:04:76:22:20:17) 
        Payload Type (7 bits): PCMA - Audio - 8000 Hz - 1 Channel
        Sequence Number (16 bits): 9825
        Timestamp (32 bits): 54240 
        Synchronization source (32 bits): 4090175489
        Arrival Time: 1027664350.17
        Contributing source (32 bits): 0, Padding (1 bit): 0, Extension (1 bit): 0, Marker (1 bit): 0
    **
    rtp 2002-07-26 07:19:10       10.1.3.143:5000  --        10.1.6.18:2006  ** 
        From: 10.1.3.143 (00:04:76:22:20:17) to 10.1.6.18 (00:d0:50:10:01:66) 
        Payload Type (7 bits): PCMA - Audio - 8000 Hz - 1 Channel
        Sequence Number (16 bits): 59364
        Timestamp (32 bits): 55680 
        Synchronization source (32 bits): 3739283087
        Arrival Time: 1027664350.2
        Contributing source (32 bits): 0, Padding (1 bit): 0, Extension (1 bit): 0, Marker (1 bit): 0
    **

```

