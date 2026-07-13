# What Really Happens When You Make an HTTP Request?

### Understanding HTTP Requests

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/https.png)

- Let's say ReactJs frontend(run on local laptop)
  - local host --> 127.0.0.1(loop back IP) & port is 3000 (port par react js run ho raha hai)
  - Loop back IP -> laptop ke bahar iss IP ka koi significance nahi hota

- NodeJs backend(AWS EC2 pe deploy kiya hai)
  - Run on IP --> 10.0.0.1 & port 443 pe

- Let's assume host nodejs backend hosted with domain name -> `https://api.example.com`

`https` -> Protocol - Secured HTTP

Consider,
Source IP -> **127.0.0.1**
Destination IP -> **10.0.0.1**

Generally that _10.0.0.1_ in private IP range, assume its in public

As a Developer, Jab bhi aap code execute karte ho.

```js
const res = await fetch("https://api.example.com/profile");
```

#### Under the hood inside code

- DO DNS query becz --> domain name in feature ("https://api.example.com/profile")

- Local Laptop & AWS EC2 that are physically 2 different machines

- Inko connect karne ke liye over the internet travel karna padega

- Using the string(api.example.com) over the internet travel karna is not feasible.

- fetch() under the hood kya karega? -> **It will make DNS request**

### DNS and Name Resolution

DNS - **Domain Name System**

- fetch DNS query run karega _(api.example.com)_ leker & return the public ip 10.0.0.1 that's host name is holding

Assume,

```
Source IP: 127.0.0.1 (assume that is public id is lookback)
Source Port: 3000
Destination IP: 10.0.0.1
Destination PORT: 443
```

- ReactJS Frontend browser pe hi run hoga
- Jab bhi browser `https` dekhta hai 
- it will assume aap ka jo destination port hai voh 443 rahega

#### HTTP request

`Https request` is showing as below -

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/http-based-on-tcp.png)

- header --> Authorization: Bearer Token
- custom headers --> X-Forwared-for: abc

HTTP is based on --> **TCP**

- jab bhi http aap call karte ho under the hood --> vo **TCP protocol use karta hai**

TCP -> **Transmission Control Protocol**

> If you are curious, In a React appln, the browser (NOT React) is responsible for serializing and encoding the HTTP request into bytes.

`Https request` jo text hota hai unko Bytes main convert karte hai why?

- Over the n/w data jayega toh voh obviously voh `bits`(1's & 0's) main jayega.

- Kuch text encoders ka use karke bytes main convert kar deta hai

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/http-req-in-bytes.png)

- data are prepared to send Nodejs backend
  Send this data over the network --> that say to O.S.

- browser ka kaam yahi tak hai --> DNS queries chalaye, IP & public IP leke aaye, port leke aaye

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/os-tcp-ip-stack.png)

- O.S. level pe jo deal hota hai --> to O.S. TCP/IP stack

### Establishing TCP Connections

O.S. TCP/IP stack say,

- TCS is connection oriented protocol

- Connection oriented protocol :
  - Jo bhi source & destination hai uske bich main(HTTP request in bytes) ye data send kar ne se pehel we need to establish a TCP/IP connection.
  - Client is FE & server is BE
  - Client ko server par data bhej ne ke liye pipe ki jarurat hai jiske basis pe data bheje ga.

- vo pipe banane ke liye TCP does something called as -> `TCP 3 way handshake`

TCP 3 way handshake ==> Create TCP connection between in FE & BE jis connection ke through ye data pura flow hoga

- O.S. --> send the data over the network --> 1st create TCP connection(starting point)
- data divides into chunks agar _pdf file hai 1GB ka toh bohot bada data hai_

TCP say,

- sending this whole data in single time -> divide into small-small chunks
- chunking has algorithm depends on **MTU** & **MSS**
- MTU --> `Maximum Transmission Unit`
- MSS --> `Maximum Segment Size`

Assume 2 bytes chunking,

- Hexadecimal format break into chunks
- Chunking bhi O.S. ka TCP/IP stack karta hai

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/hexa-to-chunk.png)

- Actual data ko chunks main convert karne ke baad it will attach a `source port` & `destination port`
- TCP deals with **port number**

Port say,

- Physical machine main 1000's of process are running, un main se konse process ko muje data forward karna hai
- Source port say, segment kaha se aaya hai
- Destination port, EC2 instance(standalone machine) ke nodeJS backend ka port

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/tcp-segment.png)

> How TCP ensures ki jo bhi data jis order main bheja hai todke ussi order main destination ko mille?
> i.e. `Sequence number`

### Encapsulating Data into IP Packets

- TCP segment ko IP Packet ke undar encapsulate karnege
IP Packet?
- Jaha pe TCP src IP & Dest IP main encapsulate karta hau

Consider, Src IP(jaha pe react js running hai) & Dest IP(aws running ip),

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/srcip-destip.png)

> TCP segment encapsulate hota hai into IP packet Src IP & Dest IP 

#### Why do your need Src IP & Dest IP?

Src IP --> jaha se request bhej rahe hai i.e. ReactJs
Dest IP --> jis machine tak uss data ko send karna hai uska IP

Destination IP,
- Help to router to decide ki ye iss machine tak kaise pohachya ja sakta hai
- ReactJs jaise hi IP packet send karega - technically home router ko pata nahi AWS EC2 instance
- Dest IP ke basis pe voh ye decide karega ki next konse router pe bhejna du ki may be paata ho ki ye IP kaha hai
- hope pe decide hota hai *kya ye IP muje paata hai ki nahi?* by router.


Source IP,
- Ye data aaya kaha se? --> 127.0.0.1 is public IP yaha se aaye hai
- Jaise hi is request ka `(const res = await fetch("https://api.example.com/profile");)` mere backend ke pass response ready rahega 
- i.e. Profile DB se fetch kar liya, JSON bana liya, everything realted to profile
- Is request ka response return karna hai kaha pe? then check in request main Src IP aaya tha

### Role of MAC Addresses and ARP

Last step of encapsulation,
- IP packet ko Src & Dest Mack addresses ke sath encapsualate karte hai
MAC - `Media Access Control`

Dest IP say, 
- Over the internet travel & hope karte-karte, kaise hum EC2 instance tak pohach sakte hai, But once we reach to n/w of EC2 instance
- Jaise internet through travel hote jaunga, 1st we come to that routers, 1's reach till that router, reach that local n/w
- Router ko us EC2 instance pe data bhej ne ke liye MAC address is requirement
- MAC --> **Phyiscal address of device** 
- IP --> kind od **Virtual address of that device**

IP say,
- uus device ka internet pe kya location hai
- 1's we reach to local n/w of that device hamain MAC address ki jarurat hoti hai

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/mac.png)

Source MAC --> mere local laptop ke n/w interface card ka MAC address

N/W interface card --> `is physical device, ek antenna  hota jo data ko in the form of signal transmit or receive kar sakta hai`

Destination MAC --> technically AWS EC2 instance ke n/w interface card ka MAC address hona chaiye

- but *nodejs backend ka MAC muje pata nahi hai*, bez this is not in my lcoal network

Local Network kya hai?
- Machine, router connected, mobile connected, printer connected hai --> that devices are local area n/w ka part hai.
- Laptop is aware about the router, mobile & the printer
- All over world on internet ka direct access nahi hai.
- Laptop access through only router

Similarly, NodeJs AWS EC2 machine iska bhi koi n/w interface card hoga
- uska MAC address ReactJs ko bhi nahi paata hoga

Dest MAC nahi paata hota,
- I will add fallback MAC address --> which is default gateway (router MAC)

#### MAC address kese pata chalega?
- MAC address kese pata chalega becz, Router is different machine.

ARP -> `Address Resolution Protocol`
- kind of DNS, ye input leta hai as an IP

IP address as an input -> ARP -> MAC address of the device that is holding that IP

- Laptop jo device hai(Frontend running ReactJs) voh kind of request bhejega --> `ARP request` bhehjega to every device of n/w
- e.g. Laptop mobile ko, printer ko, router ko bhi app request bhejega 

- that say,
Who has IP 192.168.1.1 --> sabko request jayega but isko reply vahi karega jiska ye IP hai i.e Router

Router say,
MAC of device holding 192.168.1.1 is aa:bb:cc:dd::ee:ff

#### After frame is ready


![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/frame-is-ready.png)

frame is ready isko bits ke form main we will,
- Transmit this frame over the wire or air
- radio signals (WiFi)/Electric signals (Ethernet)/ Light signals (Fiber opt.)




