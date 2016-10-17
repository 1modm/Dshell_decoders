# Dshell
An extensible network forensic analysis framework.  Enables rapid development of plugins to support the dissection of network packet captures.


[Dshell Repo](https://github.com/USArmyResearchLab/Dshell)


# Custom Dshell decoders
SIP and RTP decoders for Dshell

```
The Session Initiation Protocol (SIP) decoder will extract the Call ID, User agent, Codec, Method, 
SIP call, Host, and Client MAC address from every SIP request or response packet found in the given pcap.  

General usage:
    decode -d sip <pcap> 

Detailed usage:
    decode -d sip --sip_showpkt <pcap> 

Layer2 sll usage:
    decode -d sip --no-vlan --layer2=sll.SLL <pcap> 

SIP over TCP:
    decode -d sip --bpf 'tcp' <pcap> 

SIP is a text-based protocol with syntax similar to that of HTTP, so you can use followstream decoder:
    decode -d followstream --ebpf 'port 5060' --bpf 'udp' <pcap>

Examples:

    https://wiki.wireshark.org/SampleCaptures#SIP_and_RTP
    http://vignette3.wikia.nocookie.net/networker/images/f/fb/Sample_SIP_call_with_RTP_in_G711.pcap/revision/latest?cb=20140723121754

    decode -d sip metasploit-sip-invite-spoof.pcap
    decode -d sip Sample_SIP_call_with_RTP_in_G711.pcap

Output:

    <-- SIP Request --> 
    Timestamp: 2016-09-21 22:44:28.220185 UTC - Protocol: UDP - Size: 435 bytes
    Sequence and Method: 1 ACK
    From: 10.5.1.8:5060 (00:20:80:a1:13:db) to 10.5.1.7:5060 (15:2a:01:b4:0f:47)
    Via: SIP/2.0/UDP 10.5.1.8:5060;branch=z9hG4bK940bdac4-8a13-1410-9e58-08002772a6e9;rport
    SIP call: "M" <sip:M@10.5.1.8>;tag=0ba2d5c4-8a13-1910-9d56-08002772a6e9  -->  "miguel" <sip:demo-alice@10.5.1.7>;tag=84538c9d-ba7e-e611-937f-68a3c4f0d6ce
    Call ID: 0ba2d5c4-8a13-1910-9d57-08002772a6e9@M-PC

    --> SIP Response <-- 
    Timestamp: 2016-09-21 22:44:27.849761 UTC - Protocol: UDP - Size: 919 bytes
    Sequence and Method: 1 INVITE
    From: 10.5.1.7:5060 (02:0a:40:12:30:23) to 10.5.1.8:5060 (d5:02:03:94:31:1b)
    Via: SIP/2.0/UDP 10.5.1.8:5060;branch=z9hG4bK26a8d5c4-8a13-1910-9d58-08002772a6e9;rport=5060;received=10.5.1.8
    SIP call: "M" <sip:M@10.5.1.8>;tag=0ba2d5c4-8a13-1910-9d56-08002772a6e9  -->  "miguel" <sip:demo-alice@10.5.1.7>;tag=84538c9d-ba7e-e611-937f-68a3c4f0d6ce
    Call ID: 0ba2d5c4-8a13-1910-9d57-08002772a6e9@M-PC
    Codec selected: PCMU 
    Rate selected: 8000 

Detailed Output:

    --> SIP Response <-- 
    Timestamp: 2016-09-21 22:44:25.360974 UTC - Protocol: UDP - Size: 349 bytes
    From: 10.5.1.7:5060 (15:2a:01:b4:0f:47) to 10.5.1.8:5060 (00:20:80:a1:13:db) 
    SIP/2.0 100 Trying
    content-length: 0
    via: SIP/2.0/UDP 10.5.1.8:5060;branch=z9hG4bK26a8d5c4-8a13-1910-9d58-08002772a6e9;rport=5060;received=10.5.1.8
    from: "M" <sip:M@10.5.1.8>;tag=0ba2d5c4-8a13-1910-9d56-08002772a6e9
    to: <sip:demo-alice@10.5.1.7>
    cseq: 1 INVITE
    call-id: 0ba2d5c4-8a13-1910-9d57-08002772a6e9@M-PC

    --> SIP Response <-- 
    Timestamp: 2016-09-21 22:44:25.387780 UTC - Protocol: UDP - Size: 585 bytes
    From: 10.5.1.7:5060 (15:2a:01:b4:0f:47) to 10.5.1.8:5060 (00:20:80:a1:13:db)
    SIP/2.0 180 Ringing
    content-length: 0
    via: SIP/2.0/UDP 10.5.1.8:5060;branch=z9hG4bK26a8d5c4-8a13-1910-9d58-08002772a6e9;rport=5060;received=10.5.1.8
    from: "M" <sip:M@10.5.1.8>;tag=0ba2d5c4-8a13-1910-9d56-08002772a6e9
    require: 100rel
    rseq: 694867676
    user-agent: Ekiga/4.0.1
    to: "miguel" <sip:demo-alice@10.5.1.7>;tag=84538c9d-ba7e-e611-937f-68a3c4f0d6ce
    contact: "miguel" <sip:miguel@10.5.1.7>
    cseq: 1 INVITE
    allow: INVITE,ACK,OPTIONS,BYE,CANCEL,SUBSCRIBE,NOTIFY,REFER,MESSAGE,INFO,PING,PRACK
    call-id: 0ba2d5c4-8a13-1910-9d57-08002772a6e9@M-PC
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

