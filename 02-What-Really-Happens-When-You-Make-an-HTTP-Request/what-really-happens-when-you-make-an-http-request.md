# What Really Happens When You Make an HTTP Request?

#### Do HTTP request

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

Generally that *10.0.0.1* in private IP range, assume its in public

As a Developer,
Jab bhi aap code execute karte ho -
```js
const res = await fetch("https://api.example.com/profile")
```

### Under the hood inside code
- DO DNS query becz --> domain name in feature ("https://api.example.com/profile")

- Local Laptop & AWS EC2 that are physically 2 different machines

- Inko connect karne ke liye over the internet travel karna padega

- Using the string(api.example.com) over the internet travel karna is not feasible.

- fetch() under the hood kya karega? -> **It will make DNS request**

DNS - **Domain Name System**

- fetch DNS query run karega *(api.example.com)* leker & return the public ip 10.0.0.1 that's host name is holding

Assume,
```
Source IP: 127.0.0.1 (assume that is public id is lookback)
Source Port: 3000
Destination IP: 10.0.0.1
Destination PORT: 443
```
- ReactJS Frontend browser pe hi run hoga
- Jab bhi browser `https` dekhta hai - it will assume aap ka jo destination port hai voh 443 rahega

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

- Kuch text encders ka use karke bytes main convert kar deta hai

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/http-req-in-bytes.png)


- data are prepared to send Nodejs backend
Send this data over the network --> that say to O.S.

- browser ka kaam yahi tak hai --> DNS queries chalaye, IP & public IP leke aaye, port leke aaye

![alt text](/02-What-Really-Happens-When-You-Make-an-HTTP-Request/assets/os-tcp-ip-stack.png)

- O.S. level pe jo deal hota hai --> to O.S. TCP/IP stack

O.S. TCP/IP stack say,
- TCS is connection oriented protocol

Connection oriented protocol - `jo bhi source & destination hai uske bich main **HTTP request in bytes** ka data send kar ne se pehel to establish TCP/IP connection koi bhi data bhej ne ke liye pipee ki zarrorat hai jiske bais pe data bheje ga`

- vo pipe banane ke liye TCP does somthing called as -> `TCP 3 way handshake`

