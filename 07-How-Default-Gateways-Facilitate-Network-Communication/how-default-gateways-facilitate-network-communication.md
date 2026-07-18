# 07 How Default Gateways Facilitate Network Communication

#### What we can learn?
- How router helps you to communicate between 2 networks?
- Calling a router --> calling a default gateway
- Default gateway kaise do different n/w ke bich main communicate karne main help karta hai?
- Its help you to communicate in between the host in same network.
Example in,
- Where we want to be communicate in same n/w?
- Where we want to be communicate in different n/w?

## Router as a Default Gateway

![alt text](/07-How-Default-Gateways-Facilitate-Network-Communication/assets/router-default-gatgway.png)

- We have n/w A (blue area), we have n/w B (red area), we have router
N/w A -> have a`subnet` --> `192.168.1.0/24`
- `/24` means IP 1st 3 octet (1 octet is is **8 bit** = 3*8=24) --> 24 bit of an IP address are reserved for the whole n/w.
- Host A1,A2, A3 -> have almost same IP with 3 octet --> begin `192.168.1` 
- & host part is -> 2,3,4

## Router Network Interfaces Setup

- Router also having with IP in this n/w --> `192.168.1.50`
- Router can multiple n/w interfaces
- Router can have 2 ethernet connections 
	- Router 1 side end --> see can you 
	- Router 2 side end --> see your service provider

N/w B -> have a`subnet` --> `192.168.2.0/24`
- That is different n/w than A
- Iss side bhi router ko ek IP assign hai --> `192.168.2.50` becz, of router ke 2 n/w interface ho sakte hai

## First Scenario: Intra-Network Communication
1st: Host A1 wants to send data to host A3
What happen,
- It will take its own IP -> `192.168.1.2` & it take subnet mask -> `255.255.2555.0`

- Host A1 applying subnet mask on its own ip
```
	192.168.1.2
& 255.255.255.0 
---------------
	192.168.1.0
```

Do **& operation** with 1 with anything ==> result keep as same
- 1st 3 octets are same, but last 1 octet is zero

- Host A1 applying subnet mask on A3 ip
```
	192.168.1.4
& 255.255.255.0
---------------
	192.168.1.0
```

> 192.168.1.0 == 192.168.1.0
> Host A1 is in the same network as host A3

- A1 do not route packet through default gateway (some time do through router)

## Second Scenario: Inter-Network Communication

- Host A1 wants to send data to host B3
- We are a 2 different n/w


- Host A1 applying subnet mask on its own ip
```
	192.168.1.2
& 255.255.255.0 
---------------
	192.168.1.0
```

Do **& operation** with 1 with anything ==> result keep as same
- 1st 3 octets are same, but last 1 octet is zero

- Host A1 applying subnet mask on B3 ip
```
	192.168.2.4
& 255.255.255.0 
---------------
	192.168.2.0
```

> 192.168.1.0 != 192.168.2.0
> Host A1 & B3 are not in the same network

Then, 
- Host A1 will send this packet to its default gateway then,
- Default gateway it look at the ip (jaha pe ye data jana hai) & it will check kya ye mere kisi aur n/w main present hai.
then router see, 
- I have n/w B on different n/w interface (ek ethernet port pe blue area n/w control kar raha hai & red area n/w par another control kar raha hai)

- Jab bhi my subnet mask ka result doesn't match tab host hai default gateway pe datpack bheje ga & tell to default gateway muje pata nahi ke ye ip kaha pe hai?
- So directly can't send that data.
- Router takes that data & see jo ye IP (`192.168.2.4`) host A voh mere dusre n/w ka part hai & it will directly forward to host B3.

## Future Topics: MAC and ARP
- What is MAC address?
- How address resolution protocol works under the hood?
- What is switch?
- Using switch actual kya-kya work hota hai data host A to host B tak jane se pehele?

