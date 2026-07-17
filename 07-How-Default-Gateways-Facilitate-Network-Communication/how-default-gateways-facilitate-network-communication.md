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







