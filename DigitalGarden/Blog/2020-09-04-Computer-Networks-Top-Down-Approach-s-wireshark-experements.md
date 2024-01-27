---
layout: post
title: "Wireshark experements of Computer Networks: Top-Down Approach"
date: 2021-09-04 21:35:04
tags: [Computer Network]
authors: [xigou]
---

# intruduction

> "Tell me and I forget. Show me and I remember. Involve me and I understand."  
> *Chinese proverb*

I read the *Computer Networks: Top-Down Approach* about 3 years ago preparing for my CN-GRE, after read this book, I learned a lot about computer network, the reading helped me to fill in the blanks on the test paper, but I don't have the confidence to tell others I know the principles behind computer network, expecially after years of forgetting process.

So I decide to do a wireshark homework in this weekend.


<!-- more -->
<!--truncate-->

# Experiments
## Basic skill: capturing packets using Wireshark
*Wiresharks* is a powerful tools to capture, mastering it needs much time, but I think knowing three steps below is enough to start these labs:
1. select an interface, and click the "shark fin" icon to start capturing packets;
2. do something can trigger the protocal you want to observe;
3. click stop icon to stop capturing and start research.
   
Need to know more, just go to reference[<sup>1</sup>](#refer-anchor-1).
## HTTP
### The Basic HTTP GET/response interaction
What I did:
Open Microsoft Edge browser and open this url:  [http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html](http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html).


Captured packets:

| "No."    | "Time"       | "Source"         | "Destination"    | "Protocol" | "Length" | "Info"                                                    |
|----------|--------------|------------------|------------------|------------|----------|-----------------------------------------------------------|
| "370872" | "899.553481" | "192.168.1.3"    | "128.119.245.12" | "HTTP"     | "581"    | "GET /wireshark-labs/HTTP-wireshark-file1.html HTTP/1.1 " |
| "372140" | "899.824395" | "128.119.245.12" | "192.168.1.3"    | "HTTP"     | "540"    | "HTTP/1.1 200 OK  (text/html)"                            |

Detail of packets:
Request:
```
Frame 370872: 581 bytes on wire (4648 bits), 581 bytes captured (4648 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 4, Src: 192.168.1.3, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 17536, Dst Port: 80, Seq: 1, Ack: 1, Len: 527
Hypertext Transfer Protocol
    GET /wireshark-labs/HTTP-wireshark-file1.html HTTP/1.1\r\n
    Host: gaia.cs.umass.edu\r\n
    Connection: keep-alive\r\n
    Cache-Control: max-age=0\r\n
    Upgrade-Insecure-Requests: 1\r\n
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36 Edg/93.0.961.38\r\n
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\n
    Accept-Encoding: gzip, deflate\r\n
    Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7\r\n
    \r\n
    [Full request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html]
    [HTTP request 1/1]
    [Response in frame: 372140]

```
Response:

```
Frame 372140: 540 bytes on wire (4320 bits), 540 bytes captured (4320 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Fiberhom_35:92:b1 (74:5d:68:35:92:b1), Dst: Micro-St_06:62:ec (00:d8:61:06:62:ec)
Internet Protocol Version 4, Src: 128.119.245.12, Dst: 192.168.1.3
Transmission Control Protocol, Src Port: 80, Dst Port: 17536, Seq: 1, Ack: 528, Len: 486
Hypertext Transfer Protocol
    HTTP/1.1 200 OK\r\n
    Date: Sun, 05 Sep 2021 09:03:10 GMT\r\n
    Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.22 mod_perl/2.0.11 Perl/v5.16.3\r\n
    Last-Modified: Sun, 05 Sep 2021 05:59:01 GMT\r\n
    ETag: "80-5cb393aaf07a9"\r\n
    Accept-Ranges: bytes\r\n
    Content-Length: 128\r\n
    Keep-Alive: timeout=5, max=100\r\n
    Connection: Keep-Alive\r\n
    Content-Type: text/html; charset=UTF-8\r\n
    \r\n
    [HTTP response 1/1]
    [Time since request: 0.270914000 seconds]
    [Request in frame: 370872]
    [Request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html]
    File Data: 128 bytes
Line-based text data: text/html (4 lines)
    <html>\n
    Congratulations.  You've downloaded the file \n
    http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html!\n
    </html>\n

```

Q&As:
1.	Is your browser running HTTP version 1.0, 1.1, or 2?  What version of HTTP is the server running?
From the header of request packet, I can see the HTTP version of my browser and server are both HTTP/1.1.

2.	What languages (if any) does your browser indicate that it can accept to the server?
It is in the "Accept-Language" of the request packet.

3.	What is the IP address of your computer?  What is the IP address of the gaia.cs.umass.edu server?
The Internet Protocal layer indicates my IP(Src) is 192.168.1.3, IP of gaia.cs.umass.edu server(Dst) is 128.119.245.12.

4.	What is the status code returned from the server to your browser?
200 OK.

5.	When was the HTML file that you are retrieving last modified at the server?
Last-Modified: Sun, 05 Sep 2021 05:59:01 GMT\r\n

6.	How many bytes of content are being returned to your browser?
540 bytes.

7.	By inspecting the raw data in the packet content window, do you see any headers within the data that are not displayed in the packet-listing window?  If so, name one.
Content-Length.


### The HTTP CONDITIONAL GET/response interaction
Operations:
- Enter the following URL into your browser
[http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html](http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html)
Your browser should display a very simple five-line HTML file. 
- Quickly enter the same URL into your browser again (or simply select the refresh button on your browser)

Captured packets:

| "No." | "Time"     | "Source"         | "Destination"    | "Protocol" | "Length" | "Info"                                                    |
|-------|------------|------------------|------------------|------------|----------|-----------------------------------------------------------|
| "142" | "6.368786" | "192.168.1.3"    | "128.119.245.12" | "HTTP"     | "555"    | "GET /wireshark-labs/HTTP-wireshark-file2.html HTTP/1.1 " |
| "183" | "6.633550" | "128.119.245.12" | "192.168.1.3"    | "HTTP"     | "784"    | "HTTP/1.1 200 OK  (text/html)"                            |
| "237" | "8.389410" | "192.168.1.3"    | "128.119.245.12" | "HTTP"     | "667"    | "GET /wireshark-labs/HTTP-wireshark-file2.html HTTP/1.1 " |
| "268" | "8.655003" | "128.119.245.12" | "192.168.1.3"    | "HTTP"     | "293"    | "HTTP/1.1 304 Not Modified "                              |

Detail of packets:

```
Frame 142: 555 bytes on wire (4440 bits), 555 bytes captured (4440 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 4, Src: 192.168.1.3, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 1053, Dst Port: 80, Seq: 1, Ack: 1, Len: 501
Hypertext Transfer Protocol
    GET /wireshark-labs/HTTP-wireshark-file2.html HTTP/1.1\r\n
    Host: gaia.cs.umass.edu\r\n
    Connection: keep-alive\r\n
    Upgrade-Insecure-Requests: 1\r\n
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36 Edg/93.0.961.38\r\n
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\n
    Accept-Encoding: gzip, deflate\r\n
    Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7\r\n
    \r\n
    [Full request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html]
    [HTTP request 1/2]
    [Response in frame: 183]
    [Next request in frame: 237]
```

```
Frame 183: 784 bytes on wire (6272 bits), 784 bytes captured (6272 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Fiberhom_35:92:b1 (74:5d:68:35:92:b1), Dst: Micro-St_06:62:ec (00:d8:61:06:62:ec)
Internet Protocol Version 4, Src: 128.119.245.12, Dst: 192.168.1.3
Transmission Control Protocol, Src Port: 80, Dst Port: 1053, Seq: 1, Ack: 502, Len: 730
Hypertext Transfer Protocol
    HTTP/1.1 200 OK\r\n
    Date: Sun, 05 Sep 2021 12:49:23 GMT\r\n
    Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.22 mod_perl/2.0.11 Perl/v5.16.3\r\n
    Last-Modified: Sun, 05 Sep 2021 05:59:01 GMT\r\n
    ETag: "173-5cb393aaefbf1"\r\n
    Accept-Ranges: bytes\r\n
    Content-Length: 371\r\n
    Keep-Alive: timeout=5, max=100\r\n
    Connection: Keep-Alive\r\n
    Content-Type: text/html; charset=UTF-8\r\n
    \r\n
    [HTTP response 1/2]
    [Time since request: 0.264764000 seconds]
    [Request in frame: 142]
    [Next request in frame: 237]
    [Next response in frame: 268]
    [Request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html]
    File Data: 371 bytes
Line-based text data: text/html (10 lines)
    \n
    <html>\n
    \n
    Congratulations again!  Now you've downloaded the file lab2-2.html. <br>\n
    This file's last modification date will not change.  <p>\n
    Thus  if you download this multiple times on your browser, a complete copy <br>\n
    will only be sent once by the server due to the inclusion of the IN-MODIFIED-SINCE<br>\n
    field in your browser's HTTP GET request to the server.\n
    \n
    </html>\n


```

```
Frame 237: 667 bytes on wire (5336 bits), 667 bytes captured (5336 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 4, Src: 192.168.1.3, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 1053, Dst Port: 80, Seq: 502, Ack: 731, Len: 613
Hypertext Transfer Protocol
    GET /wireshark-labs/HTTP-wireshark-file2.html HTTP/1.1\r\n
    Host: gaia.cs.umass.edu\r\n
    Connection: keep-alive\r\n
    Cache-Control: max-age=0\r\n
    Upgrade-Insecure-Requests: 1\r\n
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36 Edg/93.0.961.38\r\n
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\n
    Accept-Encoding: gzip, deflate\r\n
    Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7\r\n
    If-None-Match: "173-5cb393aaefbf1"\r\n
    If-Modified-Since: Sun, 05 Sep 2021 05:59:01 GMT\r\n
    \r\n
    [Full request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html]
    [HTTP request 2/2]
    [Prev request in frame: 142]
    [Response in frame: 268]

```

```
Frame 268: 293 bytes on wire (2344 bits), 293 bytes captured (2344 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Fiberhom_35:92:b1 (74:5d:68:35:92:b1), Dst: Micro-St_06:62:ec (00:d8:61:06:62:ec)
Internet Protocol Version 4, Src: 128.119.245.12, Dst: 192.168.1.3
Transmission Control Protocol, Src Port: 80, Dst Port: 1053, Seq: 731, Ack: 1115, Len: 239
Hypertext Transfer Protocol
    HTTP/1.1 304 Not Modified\r\n
    Date: Sun, 05 Sep 2021 12:49:25 GMT\r\n
    Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.22 mod_perl/2.0.11 Perl/v5.16.3\r\n
    Connection: Keep-Alive\r\n
    Keep-Alive: timeout=5, max=99\r\n
    ETag: "173-5cb393aaefbf1"\r\n
    \r\n
    [HTTP response 2/2]
    [Time since request: 0.265593000 seconds]
    [Prev request in frame: 142]
    [Prev response in frame: 183]
    [Request in frame: 237]
    [Request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html]
```

Q&As:
8.	Inspect the contents of the first HTTP GET request from your browser to the server.  Do you see an “IF-MODIFIED-SINCE” line in the HTTP GET?
No.

9.	Inspect the contents of the server response. Did the server explicitly return the contents of the file?   How can you tell?
Yes, in the File Data field of the response packet.

10.	Now inspect the contents of the second HTTP GET request from your browser to the server.  Do you see an “IF-MODIFIED-SINCE:” line in the HTTP GET ? If so, what information follows the “IF-MODIFIED-SINCE:” header?
Yes,  "If-Modified-Since: Sun, 05 Sep 2021 05:59:01 GMT\r\n".

11.	What is the HTTP status code and phrase returned from the server in response to this second HTTP GET?  Did the server explicitly return the contents of the file?   Explain.
Status code is 304(Not Modified), server didn't return the contents of the file explicitly,  because the browser keep a copy of this file last modified at  "Sun, 05 Sep 2021 05:59:01 GMT", and put this timestamp in “IF-MODIFIED-SINCE:” header field to inform the server no need to return the file didn't modified after the timestamp.

### Retrieving Long Documents

Operations:
- Enter the following URL into your browser
[http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file3.html](http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file3.html)
Your browser should display the rather lengthy US Bill of Rights.

Packets Captured:

| "No." | "Time"     | "Source"         | "Destination"    | "Protocol" | "Length" | "Info"                                                    |
|-------|------------|------------------|------------------|------------|----------|-----------------------------------------------------------|
| "146" | "4.150099" | "192.168.1.3"    | "128.119.245.12" | "HTTP"     | "555"    | "GET /wireshark-labs/HTTP-wireshark-file3.html HTTP/1.1 " |
| "176" | "4.411527" | "128.119.245.12" | "192.168.1.3"    | "HTTP"     | "679"    | "HTTP/1.1 200 OK  (text/html)"                            |



Packets Details:

```
Frame 146: 555 bytes on wire (4440 bits), 555 bytes captured (4440 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 4, Src: 192.168.1.3, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 1028, Dst Port: 80, Seq: 1, Ack: 1, Len: 501
Hypertext Transfer Protocol
    GET /wireshark-labs/HTTP-wireshark-file3.html HTTP/1.1\r\n
        [Expert Info (Chat/Sequence): GET /wireshark-labs/HTTP-wireshark-file3.html HTTP/1.1\r\n]
        Request Method: GET
        Request URI: /wireshark-labs/HTTP-wireshark-file3.html
        Request Version: HTTP/1.1
    Host: gaia.cs.umass.edu\r\n
    Connection: keep-alive\r\n
    Upgrade-Insecure-Requests: 1\r\n
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36 Edg/93.0.961.38\r\n
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\n
    Accept-Encoding: gzip, deflate\r\n
    Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7\r\n
    \r\n
    [Full request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file3.html]
    [HTTP request 1/1]
    [Response in frame: 176]

```

```
Frame 176: 679 bytes on wire (5432 bits), 679 bytes captured (5432 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Fiberhom_35:92:b1 (74:5d:68:35:92:b1), Dst: Micro-St_06:62:ec (00:d8:61:06:62:ec)
    Destination: Micro-St_06:62:ec (00:d8:61:06:62:ec)
    Source: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
    Type: IPv4 (0x0800)
Internet Protocol Version 4, Src: 128.119.245.12, Dst: 192.168.1.3
Transmission Control Protocol, Src Port: 80, Dst Port: 1028, Seq: 4237, Ack: 502, Len: 625
[4 Reassembled TCP Segments (4861 bytes): #172(1412), #173(1412), #175(1412), #176(625)]
    [Frame: 172, payload: 0-1411 (1412 bytes)]
    [Frame: 173, payload: 1412-2823 (1412 bytes)]
    [Frame: 175, payload: 2824-4235 (1412 bytes)]
    [Frame: 176, payload: 4236-4860 (625 bytes)]
    [Segment count: 4]
    [Reassembled TCP length: 4861]
    [Reassembled TCP Data: 485454502f312e3120323030204f4b0d0a446174653a2053756e2c203035205365702032…]
Hypertext Transfer Protocol
    HTTP/1.1 200 OK\r\n
        [Expert Info (Chat/Sequence): HTTP/1.1 200 OK\r\n]
        Response Version: HTTP/1.1
        Status Code: 200
        [Status Code Description: OK]
        Response Phrase: OK
    Date: Sun, 05 Sep 2021 13:12:00 GMT\r\n
    Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.22 mod_perl/2.0.11 Perl/v5.16.3\r\n
    Last-Modified: Sun, 05 Sep 2021 05:59:01 GMT\r\n
    ETag: "1194-5cb393aaeb1b8"\r\n
    Accept-Ranges: bytes\r\n
    Content-Length: 4500\r\n
    Keep-Alive: timeout=5, max=100\r\n
    Connection: Keep-Alive\r\n
    Content-Type: text/html; charset=UTF-8\r\n
    \r\n
    [HTTP response 1/1]
    [Time since request: 0.261428000 seconds]
    [Request in frame: 146]
    [Request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file3.html]
    File Data: 4500 bytes
Line-based text data: text/html (98 lines)

```



Q&As:

12.	How many HTTP GET request messages did your browser send?  Which packet number in the trace contains the GET message for the Bill or Rights?
Only one, packet 146.

13.	Which packet number in the trace contains the status code and phrase associated with the response to the HTTP GET request?
Packet 176.

14.	What is the status code and phrase in the response?
200(OK).

15.	How many data-containing TCP segments were needed to carry the single HTTP response and the text of the Bill of Rights?
4.

###  HTML Documents with Embedded Objects

Operations:
- Enter the following URL into your browser
[http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file4.html](http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file4.html)
Your browser should display a short HTML file with two images. These two images are referenced in the base HTML file.  That is, the images themselves are not contained in the HTML; instead the URLs for the images are contained in the downloaded HTML file. As discussed in the textbook, your browser will have to retrieve these logos from the indicated web sites.   Our publisher’s logo is retrieved from the gaia.cs.umass.edu web site.   The image of our 8th edition cover (one of our favorite covers) is stored at a server in France. 

Packets Captured:

| "No." | "Time"     | "Source"         | "Destination"    | "Protocol" | "Length" | "Info"                                                    |
|-------|------------|------------------|------------------|------------|----------|-----------------------------------------------------------|
| "28"  | "2.033457" | "192.168.1.3"    | "128.119.245.12" | "HTTP"     | "581"    | "GET /wireshark-labs/HTTP-wireshark-file4.html HTTP/1.1 " |
| "41"  | "2.304704" | "128.119.245.12" | "192.168.1.3"    | "HTTP"     | "1355"   | "HTTP/1.1 200 OK  (text/html)"                            |
| "42"  | "2.338977" | "192.168.1.3"    | "128.119.245.12" | "HTTP"     | "501"    | "GET /pearson.png HTTP/1.1 "                              |
| "47"  | "2.530706" | "192.168.1.3"    | "178.79.137.164" | "HTTP"     | "468"    | "GET /8E_cover_small.jpg HTTP/1.1 "                       |
| "55"  | "2.609925" | "128.119.245.12" | "192.168.1.3"    | "HTTP"     | "841"    | "HTTP/1.1 200 OK  (PNG)"                                  |
| "58"  | "2.695694" | "178.79.137.164" | "192.168.1.3"    | "HTTP"     | "225"    | "HTTP/1.1 301 Moved Permanently "                         |


Packets Details:
...



Q&As:
16.	How many HTTP GET request messages did your browser send?  To which Internet addresses were these GET requests sent?
Totally 3 GET requests, 2 to 128.119.245.12 to get html file and pearson.png, 1 to 178.79.137.164 for the 8E_cover_small.jpg.

17.	Can you tell whether your browser downloaded the two images serially, or whether they were downloaded from the two web sites in parallel?  Explain.
In parallel.


```mermaid
sequenceDiagram
    participant Browser
    participant Server
    Browser ->> Server: GET /pearson.png time:2.338977
    Browser -->> Server: GET /8E_cover_small.jpg time:2.530706
    Server ->> Browser: pearson.png time:2.609925
    Server -->>Browser: 8E_cover_small.jpg time:2.695694
```

### HTTP Authentication

Operations:
- Enter the following URL into your browser
http://gaia.cs.umass.edu/wireshark-labs/protected_pages/HTTP-wireshark-file5.html
Type the requested user name and password into the pop up box.  The username is “wireshark-students” (without the quotes), and the password is “network” (again, without the quotes).

Packets Captured:

| No. | Time      | Source         | Destination    | Protocol | Length | Info                                                                   |
|-----|-----------|----------------|----------------|----------|--------|------------------------------------------------------------------------|
| 125 | 5.558621  | 192.168.1.3    | 128.119.245.12 | HTTP     | 597    | GET /wireshark-labs/protected_pages/HTTP-wireshark-file5.html HTTP/1.1 |
| 140 | 5.823233  | 128.119.245.12 | 192.168.1.3    | HTTP     | 771    | HTTP/1.1 401 Unauthorized  (text/html)                                 |
| 726 | 26.553076 | 192.168.1.3    | 128.119.245.12 | HTTP     | 656    | GET /wireshark-labs/protected_pages/HTTP-wireshark-file5.html HTTP/1.1 |
| 737 | 26.823450 | 128.119.245.12 | 192.168.1.3    | HTTP     | 544    | HTTP/1.1 200 OK  (text/html)                                           |

Packets Details:

```
Frame 125: 597 bytes on wire (4776 bits), 597 bytes captured (4776 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 4, Src: 192.168.1.3, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 28007, Dst Port: 80, Seq: 1, Ack: 1, Len: 543
Hypertext Transfer Protocol
    GET /wireshark-labs/protected_pages/HTTP-wireshark-file5.html HTTP/1.1\r\n
        [Expert Info (Chat/Sequence): GET /wireshark-labs/protected_pages/HTTP-wireshark-file5.html HTTP/1.1\r\n]
        Request Method: GET
        Request URI: /wireshark-labs/protected_pages/HTTP-wireshark-file5.html
        Request Version: HTTP/1.1
    Host: gaia.cs.umass.edu\r\n
    Connection: keep-alive\r\n
    Cache-Control: max-age=0\r\n
    Upgrade-Insecure-Requests: 1\r\n
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36 Edg/93.0.961.38\r\n
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\n
    Accept-Encoding: gzip, deflate\r\n
    Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7\r\n
    \r\n
    [Full request URI: http://gaia.cs.umass.edu/wireshark-labs/protected_pages/HTTP-wireshark-file5.html]
    [HTTP request 1/1]
    [Response in frame: 140]

```


```
Frame 726: 656 bytes on wire (5248 bits), 656 bytes captured (5248 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 4, Src: 192.168.1.3, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 1028, Dst Port: 80, Seq: 1, Ack: 1, Len: 602
Hypertext Transfer Protocol
    GET /wireshark-labs/protected_pages/HTTP-wireshark-file5.html HTTP/1.1\r\n
        [Expert Info (Chat/Sequence): GET /wireshark-labs/protected_pages/HTTP-wireshark-file5.html HTTP/1.1\r\n]
        Request Method: GET
        Request URI: /wireshark-labs/protected_pages/HTTP-wireshark-file5.html
        Request Version: HTTP/1.1
    Host: gaia.cs.umass.edu\r\n
    Connection: keep-alive\r\n
    Cache-Control: max-age=0\r\n
    Authorization: Basic d2lyZXNoYXJrLXN0dWRlbnRzOm5ldHdvcms=\r\n
        Credentials: wireshark-students:network
    Upgrade-Insecure-Requests: 1\r\n
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36 Edg/93.0.961.38\r\n
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\n
    Accept-Encoding: gzip, deflate\r\n
    Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7\r\n
    \r\n
    [Full request URI: http://gaia.cs.umass.edu/wireshark-labs/protected_pages/HTTP-wireshark-file5.html]
    [HTTP request 1/1]
    [Response in frame: 737]

```

Q&As:

18.	What is the server’s response (status code and phrase) in response to the initial HTTP GET message from your browser?

401 Unauthorized.

19.	When your browser’s sends the HTTP GET message for the second time, what new field is included in the HTTP GET message? 
Authorization: Basic d2lyZXNoYXJrLXN0dWRlbnRzOm5ldHdvcms=\r\n

## DNS

### Trace DNS from web browser

Operations:
- Clear DNS cache by 'ipconfig' command
- Open the following URL into your browser
[http://gaia.cs.umass.edu/kurose_ross/index.php](http://gaia.cs.umass.edu/kurose_ross/index.php)

Captured packets:

| No. | Time     | Source                    | Destination               | Protocol | Length | Info                                                                |
|-----|----------|---------------------------|---------------------------|----------|--------|---------------------------------------------------------------------|
| 20  | 4.898665 | fe80::604d:b1a2:55ef:5877 | fe80::1                   | DNS      | 97     | Standard query 0xcba9 A gaia.cs.umass.edu                           |
| 29  | 4.908337 | fe80::1                   | fe80::604d:b1a2:55ef:5877 | DNS      | 113    | Standard query response 0xcba9 A gaia.cs.umass.edu A 128.119.245.12 |

Detail of packets:

```
Frame 20: 97 bytes on wire (776 bits), 97 bytes captured (776 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 6, Src: fe80::604d:b1a2:55ef:5877, Dst: fe80::1
User Datagram Protocol, Src Port: 61277, Dst Port: 53
Domain Name System (query)
    Transaction ID: 0xcba9
    Flags: 0x0100 Standard query
    Questions: 1
    Answer RRs: 0
    Authority RRs: 0
    Additional RRs: 0
    Queries
        gaia.cs.umass.edu: type A, class IN
            Name: gaia.cs.umass.edu
            [Name Length: 17]
            [Label Count: 4]
            Type: A (Host Address) (1)
            Class: IN (0x0001)
    [Response In: 29]

```

```
Frame 29: 113 bytes on wire (904 bits), 113 bytes captured (904 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Fiberhom_35:92:b1 (74:5d:68:35:92:b1), Dst: Micro-St_06:62:ec (00:d8:61:06:62:ec)
Internet Protocol Version 6, Src: fe80::1, Dst: fe80::604d:b1a2:55ef:5877
User Datagram Protocol, Src Port: 53, Dst Port: 61277
Domain Name System (response)
    Transaction ID: 0xcba9
    Flags: 0x8180 Standard query response, No error
    Questions: 1
    Answer RRs: 1
    Authority RRs: 0
    Additional RRs: 0
    Queries
        gaia.cs.umass.edu: type A, class IN
            Name: gaia.cs.umass.edu
            [Name Length: 17]
            [Label Count: 4]
            Type: A (Host Address) (1)
            Class: IN (0x0001)
    Answers
        gaia.cs.umass.edu: type A, class IN, addr 128.119.245.12
    [Request In: 20]
    [Time: 0.009672000 seconds]


```


Q&As:

1. Locate the first DNS query message resolving the name gaia.cs.umass.edu. What is the packet number  in the trace for the DNS query message?  Is this query message sent over UDP or TCP?

20, UDP.

2.	Now locate the corresponding DNS response to the initial DNS query. What is the packet number in the trace for the DNS response message?  Is this response message received via UDP or TCP?   

29, UDP.

3.	What is the destination port for the DNS query message? What is the source port of the DNS response message?

53 .

4.	To what IP address is the DNS query message sent? 

fe80::1.

5.	Examine the DNS query message. How many “questions” does this DNS message contain? How many “answers” answers does it contain?

Questions: 1
Answer RRs: 0

6.	Examine the DNS response message to the initial query message. How many “questions” does this DNS message contain? How many “answers” answers does it contain?

Questions: 1
Answer RRs: 1


### Trace DNS from nslookup

Operations:
- Clear DNS cache by 'ipconfig' command
- Do an nslookup on www.cs.umass.edu

Captured packets:

| No. | Time      | Source                    | Destination               | Protocol | Length | Info                                                               |
|-----|-----------|---------------------------|---------------------------|----------|--------|--------------------------------------------------------------------|
| 56  | 10.981691 | fe80::604d:b1a2:55ef:5877 | fe80::1                   | DNS      | 96     | Standard query 0x7a54 A www.cs.umass.edu                           |
| 57  | 10.981916 | fe80::604d:b1a2:55ef:5877 | fe80::1                   | DNS      | 96     | Standard query 0xd102 AAAA www.cs.umass.edu                        |
| 58  | 11.232144 | fe80::1                   | fe80::604d:b1a2:55ef:5877 | DNS      | 96     | Standard query response 0xd102 AAAA www.cs.umass.edu               |
| 59  | 11.255062 | fe80::1                   | fe80::604d:b1a2:55ef:5877 | DNS      | 112    | Standard query response 0x7a54 A www.cs.umass.edu A 128.119.240.84 |

Detail of packets:

```
Frame 56: 96 bytes on wire (768 bits), 96 bytes captured (768 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 6, Src: fe80::604d:b1a2:55ef:5877, Dst: fe80::1
User Datagram Protocol, Src Port: 51185, Dst Port: 53
Domain Name System (query)
    Transaction ID: 0x7a54
    Flags: 0x0100 Standard query
    Questions: 1
    Answer RRs: 0
    Authority RRs: 0
    Additional RRs: 0
    Queries
        www.cs.umass.edu: type A, class IN
            Name: www.cs.umass.edu
            [Name Length: 16]
            [Label Count: 4]
            Type: A (Host Address) (1)
            Class: IN (0x0001)
    [Response In: 59]

```

```
Frame 59: 112 bytes on wire (896 bits), 112 bytes captured (896 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Fiberhom_35:92:b1 (74:5d:68:35:92:b1), Dst: Micro-St_06:62:ec (00:d8:61:06:62:ec)
Internet Protocol Version 6, Src: fe80::1, Dst: fe80::604d:b1a2:55ef:5877
User Datagram Protocol, Src Port: 53, Dst Port: 51185
Domain Name System (response)
    Transaction ID: 0x7a54
    Flags: 0x8180 Standard query response, No error
    Questions: 1
    Answer RRs: 1
    Authority RRs: 0
    Additional RRs: 0
    Queries
        www.cs.umass.edu: type A, class IN
            Name: www.cs.umass.edu
            [Name Length: 16]
            [Label Count: 4]
            Type: A (Host Address) (1)
            Class: IN (0x0001)
    Answers
        www.cs.umass.edu: type A, class IN, addr 128.119.240.84
    [Request In: 56]
    [Time: 0.273371000 seconds]

```

Q&As:

7.	What is the destination port for the DNS query message? What is the source port of the DNS response message?

53

8.	To what IP address is the DNS query message sent? Is this the IP address of your default local DNS server?

fe80::1, yes.

9.	Examine the DNS query message. What “Type” of DNS query is it? Does the query message contain any “answers”?

No.

10.	Examine the DNS response message to the query message. How many “questions” does this DNS response message contain? How many “answers”?

Questions: 1
Answer RRs: 1

### Trace DNS from nslookup with NS type record


Operations:
- Clear DNS cache by 'ipconfig' command
- Do an nslookup -type=NS umass.edu

Captured packets:

| No. | Time     | Source                    | Destination               | Protocol | Length | Info                                                                                           |
|-----|----------|---------------------------|---------------------------|----------|--------|------------------------------------------------------------------------------------------------|
| 37  | 1.731935 | fe80::604d:b1a2:55ef:5877 | fe80::1                   | DNS      | 89     | Standard query 0x0002 NS umass.edu                                                             |
| 38  | 1.738860 | fe80::1                   | fe80::604d:b1a2:55ef:5877 | DNS      | 143    | Standard query response 0x0002 NS umass.edu NS ns2.umass.edu NS ns1.umass.edu NS ns3.umass.edu |

Detail of packets:

```
Frame 37: 89 bytes on wire (712 bits), 89 bytes captured (712 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Micro-St_06:62:ec (00:d8:61:06:62:ec), Dst: Fiberhom_35:92:b1 (74:5d:68:35:92:b1)
Internet Protocol Version 6, Src: fe80::604d:b1a2:55ef:5877, Dst: fe80::1
User Datagram Protocol, Src Port: 56756, Dst Port: 53
Domain Name System (query)
    Transaction ID: 0x0002
    Flags: 0x0100 Standard query
    Questions: 1
    Answer RRs: 0
    Authority RRs: 0
    Additional RRs: 0
    Queries
        umass.edu: type NS, class IN
            Name: umass.edu
            [Name Length: 9]
            [Label Count: 2]
            Type: NS (authoritative Name Server) (2)
            Class: IN (0x0001)
    [Response In: 38]

```

```
Frame 38: 143 bytes on wire (1144 bits), 143 bytes captured (1144 bits) on interface \Device\NPF_{0A0B317B-E1D8-4D27-9D8A-9E3EC63057D4}, id 0
Ethernet II, Src: Fiberhom_35:92:b1 (74:5d:68:35:92:b1), Dst: Micro-St_06:62:ec (00:d8:61:06:62:ec)
Internet Protocol Version 6, Src: fe80::1, Dst: fe80::604d:b1a2:55ef:5877
User Datagram Protocol, Src Port: 53, Dst Port: 56756
Domain Name System (response)
    Transaction ID: 0x0002
    Flags: 0x8180 Standard query response, No error
    Questions: 1
    Answer RRs: 3
    Authority RRs: 0
    Additional RRs: 0
    Queries
        umass.edu: type NS, class IN
            Name: umass.edu
            [Name Length: 9]
            [Label Count: 2]
            Type: NS (authoritative Name Server) (2)
            Class: IN (0x0001)
    Answers
        umass.edu: type NS, class IN, ns ns2.umass.edu
        umass.edu: type NS, class IN, ns ns1.umass.edu
        umass.edu: type NS, class IN, ns ns3.umass.edu
    [Request In: 37]
    [Time: 0.006925000 seconds]

```

Q&As:

11.	To what IP address is the DNS query message sent? Is this the IP address of your default local DNS server?

fe80::1, yes.

12.	Examine the DNS query message. How many questions does the query have? Does the query message contain any “answers”?

No.

13.	Examine the DNS response message.  How many answers does the response have?  What information is contained in the answers? 

3 answers, name servers of the name in query.

## TCP

### Upload a 150+ KB file

Captured data:

Answer the following questions , either from your own live trace, or by opening the Wireshark captured packet file tcp-wireshark-trace1-1 in  http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces-8.1.zip  

| No. | Time     | Source         | Destination    | Protocol | Length | Info                                                                                                                       |
|-----|----------|----------------|----------------|----------|--------|----------------------------------------------------------------------------------------------------------------------------|
| 1   | 0.000000 | 192.168.86.68  | 128.119.245.12 | TCP      | 78     | 55639  >  80 [SYN] Seq=0 Win=65535 Len=0 MSS=1460 WS=64 TSval=725607509 TSecr=0 SACK_PERM=1                                |
| 2   | 0.022414 | 128.119.245.12 | 192.168.86.68  | TCP      | 74     | 80  >  55639 [SYN, ACK] Seq=0 Ack=1 Win=28960 Len=0 MSS=1460 SACK_PERM=1 TSval=3913851370 TSecr=725607509 WS=128           |
| 3   | 0.022505 | 192.168.86.68  | 128.119.245.12 | TCP      | 66     | 55639  >  80 [ACK] Seq=1 Ack=1 Win=131712 Len=0 TSval=725607531 TSecr=3913851370                                           |
| 4   | 0.024047 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=1 Ack=1 Win=131712 Len=1448 TSval=725607532 TSecr=3913851370 [TCP segment of a reassembled PDU]     |
| 5   | 0.024048 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=1449 Ack=1 Win=131712 Len=1448 TSval=725607532 TSecr=3913851370 [TCP segment of a reassembled PDU]  |
| 6   | 0.024049 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=2897 Ack=1 Win=131712 Len=1448 TSval=725607532 TSecr=3913851370 [TCP segment of a reassembled PDU]  |
| 7   | 0.052671 | 128.119.245.12 | 192.168.86.68  | TCP      | 66     | 80  >  55639 [ACK] Seq=1 Ack=1449 Win=31872 Len=0 TSval=3913851399 TSecr=725607532                                         |
| 8   | 0.052676 | 128.119.245.12 | 192.168.86.68  | TCP      | 66     | 80  >  55639 [ACK] Seq=1 Ack=2897 Win=34816 Len=0 TSval=3913851400 TSecr=725607532                                         |
| 9   | 0.052774 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=4345 Ack=1 Win=131712 Len=1448 TSval=725607560 TSecr=3913851399 [TCP segment of a reassembled PDU]  |
| 10  | 0.052775 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=5793 Ack=1 Win=131712 Len=1448 TSval=725607560 TSecr=3913851399 [TCP segment of a reassembled PDU]  |
| 11  | 0.052854 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=7241 Ack=1 Win=131712 Len=1448 TSval=725607560 TSecr=3913851400 [TCP segment of a reassembled PDU]  |
| 12  | 0.052855 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=8689 Ack=1 Win=131712 Len=1448 TSval=725607560 TSecr=3913851400 [TCP segment of a reassembled PDU]  |
| 13  | 0.053626 | 128.119.245.12 | 192.168.86.68  | TCP      | 66     | 80  >  55639 [ACK] Seq=1 Ack=4345 Win=37760 Len=0 TSval=3913851400 TSecr=725607532                                         |
| 14  | 0.053710 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=10137 Ack=1 Win=131712 Len=1448 TSval=725607560 TSecr=3913851400 [TCP segment of a reassembled PDU] |
| 15  | 0.053711 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=11585 Ack=1 Win=131712 Len=1448 TSval=725607560 TSecr=3913851400 [TCP segment of a reassembled PDU] |
| 16  | 0.080768 | 128.119.245.12 | 192.168.86.68  | TCP      | 66     | 80  >  55639 [ACK] Seq=1 Ack=5793 Win=40576 Len=0 TSval=3913851421 TSecr=725607560                                         |
| 17  | 0.080771 | 128.119.245.12 | 192.168.86.68  | TCP      | 66     | 80  >  55639 [ACK] Seq=1 Ack=7241 Win=43520 Len=0 TSval=3913851422 TSecr=725607560                                         |
| 18  | 0.080772 | 128.119.245.12 | 192.168.86.68  | TCP      | 66     | 80  >  55639 [ACK] Seq=1 Ack=8689 Win=46336 Len=0 TSval=3913851422 TSecr=725607560                                         |
| 19  | 0.080772 | 128.119.245.12 | 192.168.86.68  | TCP      | 66     | 80  >  55639 [ACK] Seq=1 Ack=10137 Win=49280 Len=0 TSval=3913851422 TSecr=725607560                                        |
| 20  | 0.080845 | 192.168.86.68  | 128.119.245.12 | TCP      | 1514   | 55639  >  80 [ACK] Seq=13033 Ack=1 Win=131712 Len=1448 TSval=725607588 TSecr=3913851421 [TCP segment of a reassembled PDU] |


```
Frame 1: 78 bytes on wire (624 bits), 78 bytes captured (624 bits) on interface en0, id 0
Ethernet II, Src: Apple_98:d9:27 (78:4f:43:98:d9:27), Dst: Google_89:0e:c8 (3c:28:6d:89:0e:c8)
Internet Protocol Version 4, Src: 192.168.86.68, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 55639, Dst Port: 80, Seq: 0, Len: 0
    Source Port: 55639
    Destination Port: 80
    [Stream index: 0]
    [TCP Segment Len: 0]
    Sequence Number: 0    (relative sequence number)
    Sequence Number (raw): 4236649187
    [Next Sequence Number: 1    (relative sequence number)]
    Acknowledgment Number: 0
    Acknowledgment number (raw): 0
    1011 .... = Header Length: 44 bytes (11)
    Flags: 0x002 (SYN)
    Window: 65535
    [Calculated window size: 65535]
    Checksum: 0xa1e4 [unverified]
    [Checksum Status: Unverified]
    Urgent Pointer: 0
    Options: (24 bytes), Maximum segment size, No-Operation (NOP), Window scale, No-Operation (NOP), No-Operation (NOP), Timestamps, SACK permitted, End of Option List (EOL)
        TCP Option - Maximum segment size: 1460 bytes
        TCP Option - No-Operation (NOP)
        TCP Option - Window scale: 6 (multiply by 64)
        TCP Option - No-Operation (NOP)
        TCP Option - No-Operation (NOP)
        TCP Option - Timestamps: TSval 725607509, TSecr 0
        TCP Option - SACK permitted
        TCP Option - End of Option List (EOL)
    [Timestamps]

```

```
Frame 2: 74 bytes on wire (592 bits), 74 bytes captured (592 bits) on interface en0, id 0
Ethernet II, Src: Google_89:0e:c8 (3c:28:6d:89:0e:c8), Dst: Apple_98:d9:27 (78:4f:43:98:d9:27)
Internet Protocol Version 4, Src: 128.119.245.12, Dst: 192.168.86.68
Transmission Control Protocol, Src Port: 80, Dst Port: 55639, Seq: 0, Ack: 1, Len: 0
    Source Port: 80
    Destination Port: 55639
    [Stream index: 0]
    [TCP Segment Len: 0]
    Sequence Number: 0    (relative sequence number)
    Sequence Number (raw): 1068969752
    [Next Sequence Number: 1    (relative sequence number)]
    Acknowledgment Number: 1    (relative ack number)
    Acknowledgment number (raw): 4236649188
    1010 .... = Header Length: 40 bytes (10)
    Flags: 0x012 (SYN, ACK)
    Window: 28960
    [Calculated window size: 28960]
    Checksum: 0x47b4 [unverified]
    [Checksum Status: Unverified]
    Urgent Pointer: 0
    Options: (20 bytes), Maximum segment size, SACK permitted, Timestamps, No-Operation (NOP), Window scale
        TCP Option - Maximum segment size: 1460 bytes
        TCP Option - SACK permitted
        TCP Option - Timestamps: TSval 3913851370, TSecr 725607509
        TCP Option - No-Operation (NOP)
        TCP Option - Window scale: 7 (multiply by 128)
    [SEQ/ACK analysis]
    [Timestamps]

```

```
Frame 4: 1514 bytes on wire (12112 bits), 1514 bytes captured (12112 bits) on interface en0, id 0
Ethernet II, Src: Apple_98:d9:27 (78:4f:43:98:d9:27), Dst: Google_89:0e:c8 (3c:28:6d:89:0e:c8)
Internet Protocol Version 4, Src: 192.168.86.68, Dst: 128.119.245.12
Transmission Control Protocol, Src Port: 55639, Dst Port: 80, Seq: 1, Ack: 1, Len: 1448
    Source Port: 55639
    Destination Port: 80
    [Stream index: 0]
    [TCP Segment Len: 1448]
    Sequence Number: 1    (relative sequence number)
    Sequence Number (raw): 4236649188
    [Next Sequence Number: 1449    (relative sequence number)]
    Acknowledgment Number: 1    (relative ack number)
    Acknowledgment number (raw): 1068969753
    1000 .... = Header Length: 32 bytes (8)
    Flags: 0x010 (ACK)
    Window: 2058
    [Calculated window size: 131712]
    [Window size scaling factor: 64]
    Checksum: 0xbd21 [unverified]
    [Checksum Status: Unverified]
    Urgent Pointer: 0
    Options: (12 bytes), No-Operation (NOP), No-Operation (NOP), Timestamps
        TCP Option - No-Operation (NOP)
        TCP Option - No-Operation (NOP)
        TCP Option - Timestamps: TSval 725607532, TSecr 3913851370
    [SEQ/ACK analysis]
    [Timestamps]
    TCP payload (1448 bytes)
    [Reassembled PDU in frame: 153]
    TCP segment data (1448 bytes)

```


Q&As:


1.	What is the sequence number of the TCP SYN segment that is used to initiate the TCP connection between the client computer and gaia.cs.umass.edu? (Note: this is the “raw” sequence number carried in the TCP segment itself; it is NOT the packet # in the “No.” column in the Wireshark window.  Remember there is no such thing as a “packet number” in TCP or UDP; as you know, there are sequence numbers in TCP and that’s what we’re after here.  Also note that this is not the relative sequence number with respect to the starting sequence number of this TCP session.). What is it in this TCP segment that identifies the segment as a SYN segment? Will the TCP receiver in this session be able to use Selective Acknowledgments (allowing TCP to function a bit more like a “selective repeat” receiver, see section 3.4.5 in the text)?

Sequence Number (raw): 4236649187. 

The 'Flags: 0x002 (SYN)' identifies the segment as a SYN segment.

Yes, there is an option 'SACK permitted'.

2.	What is the sequence number of the SYNACK segment sent by gaia.cs.umass.edu to the client computer in reply to the SYN? What is it in the segment that identifies the segment as a SYNACK segment? What is the value of the Acknowledgement field in the SYNACK segment? How did gaia.cs.umass.edu determine that value? 


Sequence Number (raw): 1068969752

Flags: 0x012 (SYN, ACK) identifies the segment as a SYNACK segment.

Acknowledgment number (raw): 4236649188

Ack = Seq<sub>(peer)</sub> + 1.


3.	What is the sequence number of the TCP segment containing the header of the HTTP POST command?  Note that in order to find the POST message header, you’ll need to dig into the packet content field at the bottom of the Wireshark window, looking for a segment with the ASCII text “POST” within its DATA field , .  How many bytes of data are contained in the payload (data) field of this TCP segment? Did all of the data in the transferred file alice.txt fit into this single segment?

Sequence Number (raw): 4236649188. 

TCP payload (1448 bytes).

No.

4.	Consider the TCP segment containing the HTTP “POST” as the first segment in the data transfer part of the TCP connection.  
•	At what time was the first segment (the one containing the HTTP POST) in the 
data-transfer part of the TCP connection sent?  

0.024047s.

•	At what time was the ACK for this first data-containing segment received?  

0.052671s.

•	What is the RTT for this first data-containing segment? 

[The RTT to ACK the segment was: 0.028624000 seconds]

•	What is the RTT value the second data-carrying TCP segment and its ACK? 

[The RTT to ACK the segment was: 0.028628000 seconds]

•	What is the EstimatedRTT value (see Section 3.5.3, in the text) after the ACK for the second data-carrying segment is received? Assume that in making this calculation after the received of the ACK for the second segment, that the initial value of EstimatedRTT is equal to the measured RTT for the first segment, and then is computed using the EstimatedRTT equation on page 242, and a value of $\alpha$ = 0.125.
Note: Wireshark has a nice feature that allows you to plot the RTT for each of the TCP segments sent.  Select a TCP segment in the “listing of captured packets” window that is being sent from the client to the gaia.cs.umass.edu server.  Then select: Statistics->TCP Stream Graph->Round Trip Time Graph.

```
# mesured RTT for these two packets.
[The RTT to ACK the segment was: 0.028624000 seconds]
[The RTT to ACK the segment was: 0.028628000 seconds]```
```
EstimatedRTT = (1 – α) • EstimatedRTT + α • SampleRTT

So EstimatedRTT is $0.875*0.028624000 + 0.125*0.028628000 = 0.02862449s$.


5.	What is the length (header plus payload) of each of the first four data-carrying TCP segments? 

```
Transmission Control Protocol, Src Port: 55639, Dst Port: 80, Seq: 1, Ack: 1, Len: 1448
    TCP segment data (1448 bytes)
```

1448 Bytes.

6.	What is the minimum amount of available buffer space advertised to the client by gaia.cs.umass.edu among these first four data-carrying TCP segments ?  Does the lack of receiver buffer space ever throttle the sender for these first four data-carrying segments?
```
[Calculated window size: 31872]
[Calculated window size: 34816]
[Calculated window size: 37760]
[Calculated window size: 40576]
```
The lack of receiver buffer space **never** throttle the sender for these first four data-carrying segments

7.	Are there any retransmitted segments in the trace file? What did you check for (in the trace) in order to answer this question?

 In the TimeSequence-Graph (Stevens) of this trace (if it's a straight line, click the reset direction button), all sequence numbers are incresing monotonically.


8.	How much data does the receiver typically acknowledge in an ACK among the first ten data-carrying segments sent from the client to gaia.cs.umass.edu?  Can you identify cases where the receiver is ACKing every other received segment (see Table 3.2 in the text) among these first ten data-carrying segments?

1448 bytes, No.8 packet Ack=2897, and No.7 Ack=1449, it is Acking 1448 bytes.


9.	What is the throughput (bytes transferred per unit time) for the TCP connection?  Explain how you calculated this value.

The last Ack is 153426, time is 0.191496s, and the first data carrying packet's time is 0.024047, so $bandwidth = 153426 byte / (0.191496s -  0.024047s) \approx 916255 byte/s$


10.	Use the Time-Sequence-Graph(Stevens) plotting tool to view the sequence number versus time plot of segments being sent from the client to the gaia.cs.umass.edu server.  Consider the “fleets” of packets sent around t = 0.025, t = 0.053, t = 0.082 and t = 0.1. Comment on whether this looks as if TCP is in its slow start phase, congestion avoidance phase or some other phase. Figure 6 shows a slightly different view of this data.

Yes, in $t=0.025$ about 4 packets received, $t=0.053$ about 6 packets received, $t=0.082$ about 12 packets received, $t=0.1$ about 24 packets received, It is 'slow start' mechanism of TCP, the window size grows up exponentially.

11.	These “fleets” of segments appear to have some periodicity. What can you say about the period?

The period is close to the RTT, because the TCP need a RTT time at averange to calculate the new Esitimated RTT and decides how many packets will be sent.

## UDP

### Trigger UDP packets through nslookup



Captured data:

Answer the following questions , either from your own live trace, or by opening the Wireshark captured packet trace file dns-wireshark-trace1-1 in  http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces-8.1.zip  


1.	Select the first UDP segment in your trace. What is the packet number  of this segment in the trace file?  What type of application-layer payload or protocol message is being carried in this UDP segment?  Look at the details of this packet in Wireshark.  How many fields there are in the UDP header? (You shouldn’t look in the textbook! Answer these questions directly from what you observe in the packet trace.) What are the names of these fields? 

```
User Datagram Protocol, Src Port: 47931, Dst Port: 1900
    Source Port: 47931
    Destination Port: 1900
    Length: 283
    Checksum: 0xf7a8 [unverified]
    [Checksum Status: Unverified]
    [Stream index: 0]
    [Timestamps]
        [Time since first frame: 0.000000000 seconds]
        [Time since previous frame: 0.000000000 seconds]
    UDP payload (275 bytes)

```
5th, SSDP(Simple Service Discovery Protocol), There are 4 fields in UDP Header: Source Port, Destination Port, Length, Checksum.


2.	By consulting the displayed information in Wireshark’s packet content field for this packet (or by consulting the textbook), what is the length (in bytes) of each of the UDP header fields?

Through observation, length of each field is 2 bytes.

3.	The value in the Length field is the length of what? (You can consult the text for this answer). Verify your claim with your captured UDP packet. 

The value of the length field is 283, which is  (UDP payload (275 bytes) + Header length(8 bytes) ).

4.	What is the maximum number of bytes that can be included in a UDP payload?  (Hint: the answer to this question can be determined by your answer to 2. above)

The Maximum value of length field is 0xFFFF(65535 in decimal), so maximum bytes of payload is $65535 - 8 = 65527bytes$.

5.	What is the largest possible source port number? (Hint: see the hint in 4.)

It is 65535.

6.	What is the protocol number for UDP? Give your answer in decimal notation. To answer this question, you’ll need to look into the Protocol field of the IP datagram containing this UDP segment (see Figure 4.13 in the text, and the discussion of IP header fields). 

```
Internet Protocol Version 4, Src: 10.0.0.254, Dst: 239.255.255.250
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 303
    Identification: 0xa1c3 (41411)
    Flags: 0x40, Don't fragment
    Fragment Offset: 0
    Time to Live: 4
    Protocol: UDP (17)
    Header Checksum: 0xd902 [validation disabled]
    [Header checksum status: Unverified]
    Source Address: 10.0.0.254
    Destination Address: 239.255.255.250

```

It is 17.

7.	Examine the pair of UDP packets in which your host sends the first UDP packet and the second UDP packet is a reply to this first UDP packet. (Hint: for a second packet to be sent in response to a first packet, the sender of the first packet should be the destination of the second packet).  What is the packet number  of the first of these two UDP segments in the trace file?  What is the packet number  of the second of these two UDP segments in the trace file? Describe the relationship between the port numbers in the two packets. 

```
request:
User Datagram Protocol, Src Port: 58350, Dst Port: 53
    Source Port: 58350
    Destination Port: 53
    Length: 43
    Checksum: 0xc31d [unverified]
    [Checksum Status: Unverified]
    [Stream index: 1]
    [Timestamps]
        [Time since first frame: 0.000000000 seconds]
        [Time since previous frame: 0.000000000 seconds]
    UDP payload (35 bytes)

response:
User Datagram Protocol, Src Port: 53, Dst Port: 58350
    Source Port: 53
    Destination Port: 58350
    Length: 59
    Checksum: 0x4af2 [unverified]
    [Checksum Status: Unverified]
    [Stream index: 1]
    [Timestamps]
        [Time since first frame: 0.023908000 seconds]
        [Time since previous frame: 0.023908000 seconds]
    UDP payload (51 bytes)


```

The destination port of the response packet is same as the request packet's source port, reverse is also true.


## IP

### Capturing packets from an execution of traceroute

Captured data:

Answer the following questions , either from your own live trace, or by opening the Wireshark captured packet trace file ip-wireshark-trace1-1.pcapng in  http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces-8.1.zip  

#### Part 1: Basic IPv4

1.	Select the first UDP segment sent by your computer via the traceroute command to gaia.cs.umass.edu. (Hint: this is 44th packet in the trace file in the ip-wireshark-trace1-1.pcapng file in footnote 2). Expand the Internet Protocol part of the packet in the packet details window.  What is the IP address of your computer?

```
Internet Protocol Version 4, Src: 192.168.86.61, Dst: 128.119.245.12
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 56
    Identification: 0xfda1 (64929)
    Flags: 0x00
    Fragment Offset: 0
    Time to Live: 1
    Protocol: UDP (17)
    Header Checksum: 0x2faa [validation disabled]
    [Header checksum status: Unverified]
    Source Address: 192.168.86.61
    Destination Address: 128.119.245.12

```

IP of local computer is 192.168.86.61 .

2.	What is the value in the time-to-live (TTL) field in this IPv4 datagram’s header?

1 .

3.	What is the value in the upper layer protocol field in this IPv4 datagram’s header? [Note: the answers for Linux/MacOS differ from Windows here].

Protocol: UDP (17).

4.	How many bytes are in the IP header? 

.... 0101 = Header Length: 20 bytes (5).

5.	How many bytes are in the payload of the IP datagram?  Explain how you determined the number of payload bytes.

$Payload Length = Total Length - Header Length = 56 - 20 = 36 Bytes$

6.	Has this IP datagram been fragmented?  Explain how you determined whether or not the datagram has been fragmented.

No, if it is fragmented, the payload length of IP layer should be less than the Length of upper layer.




7.	Which fields in the IP datagram always change from one datagram to the next within this series of UDP segments sent by your computer destined to 128.119.245.12, via traceroute?  Why?

TTL and destination port, because traceroute set the TTL to n to trigger a ICMP response packet (ttl exceeded) on the $n_{th}$ hot of the route of the packet. destination port is used to track the coresponding response of each packet. 

8.	Which fields in this sequence of IP datagrams (containing UDP segments) stay constant? Why?

The destination IP and the source port/IP, because the function of traceroute is to trace the route between the source IP and a specific destination IP.

9.	Describe the pattern you see in the values in the Identification field of the IP datagrams being sent by your computer.

Plus 1 after every packet sent.

10.	What is the upper layer protocol specified in the IP datagrams returned from the routers? [Note: the answers for Linux/MacOS differ from Windows here].

```
Internet Protocol Version 4, Src: 192.168.86.1, Dst: 192.168.86.61
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0xc0 (DSCP: CS6, ECN: Not-ECT)
    Total Length: 84
    Identification: 0x6889 (26761)
    Flags: 0x00
    Fragment Offset: 0
    Time to Live: 64
    Protocol: ICMP (1)
    Header Checksum: 0xe3d0 [validation disabled]
    [Header checksum status: Unverified]
    Source Address: 192.168.86.1
    Destination Address: 192.168.86.61

```
Protocol: ICMP (1).

11.	Are the values in the Identification fields (across the sequence of all of ICMP packets from all of the routers) similar in behavior to your answer to question 9 above?

Yes.

12.	 Are the values of the TTL fields similar, across all of ICMP packets from all of the routers?

All equals to 1.

#### Part 2: Fragmentation

14.	What information in the IP header indicates that this datagram been fragmented?  
```
Internet Protocol Version 4, Src: 192.168.86.61, Dst: 128.119.245.12
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 1500
    Identification: 0xfda2 (64930)
    Flags: 0x20, More fragments
        0... .... = Reserved bit: Not set
        .0.. .... = Don't fragment: Not set
        ..1. .... = More fragments: Set
    Fragment Offset: 0
    Time to Live: 1
    Protocol: UDP (17)
    Header Checksum: 0x0a05 [validation disabled]
    [Header checksum status: Unverified]
    Source Address: 192.168.86.61
    Destination Address: 128.119.245.12
    [Reassembled IPv4 in frame: 181]
```
Flags: 0x20, More fragments.


15.	What information in the IP header for this packet indicates whether this is the first fragment versus a latter fragment?  

Fragment Offset: 0 indicates it is the first fragment, offset is non-zero value indicates it is a latter fragment.

16.	How many bytes are there in is this IP datagram (header plus payload)?

Total Length: 1500.

17.	Now inspect the datagram containing the second fragment of the fragmented UDP segment. What information in the IP header indicates that this is not the first datagram fragment?  

Fragment Offset: 1480.

18.	What fields change in the IP header between the first and second fragment?

Fragment Offset and Header checksum.

19.	Now find the IP datagram containing the third fragment of the original UDP segment. What information in the IP header indicates that this is the last fragment of that segment?  

```
Internet Protocol Version 4, Src: 192.168.86.61, Dst: 128.119.245.12
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
    Total Length: 40
    Identification: 0xfda2 (64930)
    Flags: 0x01
        0... .... = Reserved bit: Not set
        .0.. .... = Don't fragment: Not set
        ..0. .... = More fragments: Not set
    Fragment Offset: 2960
```
Flags: 0x01, ..0. .... = More fragments: Not set.

### Capturing IPV6 packets

Captured data:

Answer the following questions , either from your own live trace, or by opening the Wireshark captured packet trace file  ip-wireshark-trace2-1.pcapng in  http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces-8.1.zip  

#### Part 3: IPv6

Let’s start by taking a closer look at the 20th packet in this trace, sent at t=3.814489.  This is a DNS request (contained in an IPv6 datagram) to an IPv6 DNS server for the IPv6 address of youtube.com.

20.	What is the IPv6 address of the computer making the DNS AAAA request?  This is the source address of the 20th packet in the trace. Give the IPv6 source address for this datagram in the exact same form as displayed in the Wireshark window .

```
Internet Protocol Version 6, Src: 2601:193:8302:4620:215c:f5ae:8b40:a27a, Dst: 2001:558:feed::1
    0110 .... = Version: 6
    .... 0000 0000 .... .... .... .... .... = Traffic Class: 0x00 (DSCP: CS0, ECN: Not-ECT)
    .... .... .... 0110 0011 1110 1101 0000 = Flow Label: 0x63ed0
    Payload Length: 37
    Next Header: UDP (17)
    Hop Limit: 255
    Source Address: 2601:193:8302:4620:215c:f5ae:8b40:a27a
    Destination Address: 2001:558:feed::1

```
2601:193:8302:4620:215c:f5ae:8b40:a27a

21.	What is the IPv6 destination address for this datagram? Give this IPv6 address in the exact same form as displayed in the Wireshark window.  

2001:558:feed::1

22.	What is the value of the flow label for this datagram?

.... .... .... 0110 0011 1110 1101 0000 = Flow Label: 0x63ed0

23.	How much payload data is carried in this datagram?

Payload Length: 37

24.	What is the upper layer protocol to which this datagram’s payload will be delivered at the destination?

Next Header: UDP (17)

Lastly,  find the IPv6 DNS response to the IPv6 DNS AAAA request made in the 20th packet in this trace.  This DNS response contains IPv6 addresses for youtube.com.

25.	How many IPv6 addresses are returned in the response to this AAAA request?

```
Domain Name System (response)
    Transaction ID: 0x920d
    Flags: 0x8180 Standard query response, No error
    Questions: 1
    Answer RRs: 1
    Authority RRs: 0
    Additional RRs: 0
    Queries
    Answers
        youtube.com: type AAAA, class IN, addr 2607:f8b0:4006:815::200e
    [Request In: 20]
    [Time: 0.140916000 seconds]

```
Only one.

26.	What is the first of the IPv6 addresses returned by the DNS for youtube.com (in the ip-wireshark-trace2-1.pcapng trace file, this is also the address that is numerically the smallest)? Give this IPv6 address in the exact same shorthand form as displayed in the Wireshark window.

2607:f8b0:4006:815::200e.

## NAT

### Capturing packets from an execution of traceroute

Captured data:

Answer the following questions, either from your own live trace, or by opening the Wireshark captured packet trace file nat-inside-wireshark-trace1-1.pcapng and nat-outside-wireshark-trace1-1.pcapng in  http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces-8.1.zip  

Let’s first take a look at what’s happening on the LAN side of the NAT router. Open the nat-inside-wireshark-trace1-1.pcapng trace file. In this file, you should see an HTTP GET request addressed to the external web server at IP address 138.76.29.8, as well as the subsequent HTTP response message (“200 OK”). Both of these messages in the trace file were captured on the LAN side of the router. 

1.	What is the IP address of the client that sends the HTTP GET request in the nat-inside-wireshark-trace1-1.pcapng trace? What is the source port number of the TCP segment in this datagram containing the HTTP GET request? What is the destination IP address of this HTTP GET request? What is the destination port number of the TCP segment in this datagram containing the HTTP GET request?

Internet Protocol Version 4, Src: 192.168.10.11, Dst: 138.76.29.8

Transmission Control Protocol, Src Port: 53924, Dst Port: 80, Seq: 1, Ack: 1, Len: 330

2.	At what time  is the corresponding HTTP 200 OK message from the webserver forwarded by the NAT router to the client on the router’s LAN side? 

[Time since first frame in this TCP stream: 0.030672101 seconds]
.

3.	What are the source and destination IP addresses and TCP source and destination ports on the IP datagram carrying this HTTP 200 OK message?

Internet Protocol Version 4, Src: 138.76.29.8, Dst: 192.168.10.11

Transmission Control Protocol, Src Port: 80, Dst Port: 53924, Seq: 1, Ack: 331, Len: 547



In the following we’ll focus on these two HTTP messages (GET and 200 OK). Our goal below will be to locate these two HTTP messages in the trace file nat-outside-wireshark-trace1-1.pcapng, captured on the Internet-side link between the router and the ISP. Because the captured packets heading towards the server will have already been forwarded through the NAT router, some of the IP address and port numbers will have been changed as a result of NAT translation.

Open the trace file nat-outside-wireshark-trace1-1.pcapng. Note that the time stamps in this file and the nat-inside-wireshark-trace1-1.pcapng file are not necessarily synchronized. 

In the nat-outside-wireshark-trace1-1.pcapng trace file, find the HTTP GET message that corresponds to the HTTP GET message that was sent from the client to the 138.76.29.8 server at time t=0.27362245, where t=0.27362245 is the time at which this message was sent, as recorded in the nat-inside-wireshark-trace1-1.pcapng trace file.  
4.	At what time does this HTTP GET message appear in the nat-outside-wireshark-trace1-1.pcapng trace file? 

[Time since first frame in this TCP stream: 0.027356291 seconds]
.

5.	What are the source and destination IP addresses and TCP source and destination port numbers on the IP datagram carrying this HTTP GET (as recorded in the nat-outside-wireshark-trace1-1.pcapng trace file)? 

Internet Protocol Version 4, Src: 10.0.1.254, Dst: 138.76.29.8
Transmission Control Protocol, Src Port: 53924, Dst Port: 80, Seq: 1, Ack: 1, Len: 330


6.	Which of these four fields are different than in your answer to question 1 above?

The source IP changed.

7.	Are any fields in the HTTP GET message changed? 

None.


8.	Which of the following fields in the IP datagram carrying the HTTP GET are changed from the datagram received on the local area network (inside) to the corresponding datagram forwarded on the Internet side (outside) of the NAT router: Version, Header Length, Flags, Checksum?

Checksum changed.

Let’s continue to look at the nat-outside-wireshark-trace1-1.pcapng trace file. Find the HTTP reply containing the “200 OK” message that was received in response to the HTTP GET request you just examined in questions 4-8 above.

9.	At what time does this message appear in the nat-outside-wireshark-trace1-1.pcapng trace file? 

[Time since first frame in this TCP stream: 0.030625966 seconds]
.

10.	What are the source and destination IP addresses and TCP source and destination port numbers on the IP datagram carrying this HTTP reply (“200 OK”) message (as recorded in the nat-outside-wireshark-trace1-1.pcapng trace file)? 


Internet Protocol Version 4, Src: 138.76.29.8, Dst: 10.0.1.254

Transmission Control Protocol, Src Port: 80, Dst Port: 53924, Seq: 1, Ack: 331, Len: 547


Lastly, let’s consider what happens when the NAT router receives this datagram that you examined in questions 9 and 10, performs NAT translation, and finally forwards that datagram to the destination host on the LAN side. Based on your answers to questions 1 through 10 above and your knowledge of how NAT works, you should be able to answer the following question without actually looking at the nat-inside-wireshark-trace1-1.pcapng trace file:

11.	What are the source and destination IP addresses and TCP source and destination port numbers on the IP datagram carrying the HTTP reply (“200 OK”) that is forwarded from the router to the destination host in the right of Figure 1?

Internet Protocol Version 4, Src: 138.76.29.8, Dst: 192.168.10.11

Transmission Control Protocol, Src Port: 80, Dst Port: 53924


## DHCP
## ICMP
## Ethernet and ARP	
## 802.11 WiFi	
## SSL (currently being updated to TLS)	
## IP


# Refs
<div id="refer-anchor-1"></div>
- [1] [Umass Wireshark labs](https://gaia.cs.umass.edu/kurose_ross/wireshark.php)

