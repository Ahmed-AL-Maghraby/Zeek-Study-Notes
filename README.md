<h1 align="center">Zeek Study Notes<p>
  
<p align="center">
<img src="https://netquestcorp.com/wp-content/uploads/2022/02/Zeek-Featured.png" height=400 >
</p>
  
# Introduction To Zeek
Bro’s creators describe it as an open-source traffic analyzer. It is typically used to inspect all traffic on a link (in depth) for signs of malicious or suspicious activity.
That being said Bro can also be used for network troubleshooting and various measurements within a network.
By deploying Bro, blue teams can immediately access a variety of log files that contain all kinds of network activity, at a high level. More specifically, those logs contain not only detailed records of every connection on the wire but also application-layer transcripts (for example - DNS requests and the respective replies, whole HTTP sessions, etc.). Bro does a lot more than just keeping track of the aforementioned.
It is also shipped with a whole range of analysis and detection capabilities/functions. 

## Basic Commands

+ Print Version - start - stop
  ```CSS
  zeek -v
  zeekctl status
  zeekctl start 
  zeekctl stop
  ```
### Analysis Pcap Files
```css
 zeek -C -r test.pcap
```
+ -C :  Ignoring checksum errors
+ -r :  Reading option
+ -s : Use signature file. 

### Cut column from result
```css
 cat dhcp.log | zeek-cut host_name
```



### Linux Commands

- cat
- head
- tail
- cut -f 1
- cut -c 1
- grep
- grep -v 'test'
- sort -n
- sort -r
- rev		: reverse string 
- uniq
- uniq -c 
- wc -l
- nl
- sed -n '11p' 		: print lin 11
- sed -n '10,15p' 	: print lin between 11-15
- awk 'NR < 11 {print $0}' : print lines below 11

- cut -d '.' -f 1-2	: 	Split the string on every dot and print keep the first two fields.




# Zeek Signatures

-> logical paths

- signature id :  Unique signature name.
- conditions :
  + Header: Filtering the packet headers
	+ Content: Filtering the packet payload for specific value/pattern.
- Action : 
	+ Default action: Create the "signatures.log" 
	+ Additional action: Trigger a Zeek script.



## Condition Field

-> Header
	+ src-ip: Source IP.
	+ dst-ip: Destination IP.
	+ src-port: Source port.
	+ dst-port: Destination port.
	+ ip-proto: Target protocol. Supported protocols; TCP, UDP, ICMP, ICMP6, IP, IP6

-> Content
	+ payload: Packet payload.
	+ http-request: Decoded HTTP requests.
	+ http-request-header: Client-side HTTP headers.
	+ http-request-body: Client-side HTTP request bodys.
	+ http-reply-header: Server-side HTTP headers.
	+ http-reply-body: Server-side HTTP request bodys.
	+ ftp: Command line input of FTP sessions.

-> Context
	+ same-ip: Filtering the source and destination addresses for duplication.

-> Action
	+ event: Signature match message.

>**NOTE**	
> Filters accept string, numeric and regex values.





## Example | Cleartext Submission of Password

```zeek
signature http-password {
     ip-proto == tcp
     dst_port == 80
     payload /.*password.*/
     event "Cleartext Password Found!"
}
```
\# signature: Signature name <br />
\# ip-proto: Filtering TCP connection <br />
\# dst-port: Filtering destination port 80 <br />
\# payload: Filtering the "password" phrase <br />
\# event: Signature match message <br />



## Example | FTP Brute-force

```zeek
signature ftp-admin {
     ip-proto == tcp
     ftp /.*USER.*dmin.*/
     event "FTP Admin Login Attempt!"
}
```

or
```zeek
signature ftp-brute {
     ip-proto == tcp
     payload /.*530.*Login.*incorrect.*/
     event "FTP Brute-force Attempt"
}
```



# Zeek Scripts

- /opt/zeek/share/zeek/base
- /opt/zeek/share/zeek/site
- /opt/zeek/share/zeek/policy
- /opt/zeek/share/zeek/site/local.zeek


=> Extract DHCP hostnames.
```zeek
event dhcp_message (c: connection, is_orig: bool, msg: DHCP::Msg, options: DHCP::Options)
{
print options$host_name;
}
```

























