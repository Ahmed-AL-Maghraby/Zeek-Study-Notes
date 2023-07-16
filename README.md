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

























