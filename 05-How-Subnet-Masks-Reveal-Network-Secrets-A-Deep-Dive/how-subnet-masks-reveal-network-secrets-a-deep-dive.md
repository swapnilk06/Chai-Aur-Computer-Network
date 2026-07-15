# 05 How Subnet Masks Reveal Network Secrets: A Deep Dive

#### What we can learn?
- About subnet mask

## Understanding Subnet Mask Basic

CIDR notation - `192.168,1.0/24`
- Its kind of representation of IP ke kitne bits are reserved for my n/w. (*/* ke baad number i.e. 24)

192.168.1.4 -> 11000000.10101000.00000001.00000100

- Usse n/w main jitne bihi devices honge unke IP (IP represent binary no) ka unke 24 bits hoge (3 octets) that are constant & same.

CIDR notation also called as --> `Subnet`
`192.168,1.4` that is --> `Subnet` of that IP --> `192.168,1.0/24`

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/subnet.png)

Subnet - is kind of representation ki mera `n/w kya hai`

E.g. 
If my mobile is part of same n/w or not?
There are 2 decision are made -
- If I want to send Data from laptop to mobile -> I don't need internet connection or I don't need router.
- Directly send data to mobile form laptop if mobile is part of same n/w.

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/mobile-outside-nw.png)

- But mobile is not part of same n/w (mobile is outside of n/w)
- Then send that data to the mobile we need to go through the router(*alag hi n/w hoga mobile jo router ke sath connect nahi hoga*)
- I need internet or router king of route that packet to that mobile
- Jo bhi IP main reach karna chahata hu, kya voh mere n/w ka part hai?

> In future we can learn data kase out of exactly laptop se mobile tak kaise jata hai - even is mobile is outside n/w?

## Role of Subnet Mask in Networking
- Laptop by default doesn't know about mobile in same n/w.
- So to know that laptop use called as --> `Subnet Mask`

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/subnet-mask.png)

Subnet Mask say,
- is also kind of IP jo usable nahi hota hai but kind of thing we can use to determine if any IP we want to reach is part of our own n/w.
- Subnet mask ka use yahi hai kisi bhi machine ko jo usse n/w ka part hai usko kisi bhi IP pe agar request ya data bhejna hai, kya voh IP iss n/w ka part hai ki nahi voh usse subnet mask dhund kar deta hai.


## Binary Representation of IP Addresses

How uses subnet mask to determine if an IP is part of our own n/w?

255.255.255.0 --> Subnet mask in binary format --> `11111111.11111111.11111111.00000000`

- Laptop kay karenga - before sending data bhejna hai kya vo mere n/w ka part hai?
- 1stly vo khud ka IP lega.
192.168.1.4 --> binary form --> `11000000.10101000.00000001.00000100`

> Machines only knowns 1's & 0's

Laptop khud ka IP lega & it apply on to subnet ka mask own IP.

apply -> logical bit wise `&` operator use karke --> make another 1 IP.
- any thing & with 1 will give 1 i.e. 1 & 1 = 1, 0 & 1 = 0, 1 & 0 = 0, 0 & 0 = 0


Laptop IP -> 192.168.1.4
```
	`11000000.10101000.00000001.00000100` 
& `11111111.11111111.11111111.00000000`
= `11000000.10101000.00000001.00000000`
```

11000000.10101000.00000001.00000000 --> What is the IP --> `192.168.1.0`

Mobile ke IP ko binary main convert karega -> 192.168.1.2 --> `11000000.10101000.00000001.00000010`

255.255.255.0 --> Subnet mask in binary format --> `11111111.11111111.11111111.00000000`


Mobile IP -> 192.168.1.2
```
	`11000000.10101000.00000001.00000010` 
& `11111111.11111111.11111111.00000000`
= `11000000.10101000.00000001.00000000`
```

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/same-nw-ip.png)

Gives same result for mobile & laptop IP same then -> `Mobile & laptop are in same network`

> Once a device detects that the destination is in the same subnet/network, ARP (Address Resolution Protocol) kicks in to find the destination's MAC address.


## Detecting Network Membership with Subnet

Laptop kya karega,
 khud ka IP lega & it apply subnet mask

Mobile IP -> 10.0.0.1 --> Binary form --> `00001010.00000000.00000000.00000001`
& 
255.255.255.0 --> Subnet mask in binary format --> `11111111.11111111.11111111.00000000`

That operation by laptop becz, laptop can send to mobile 

```
	`00001010.00000000.00000000.00000001` 
& `11111111.11111111.11111111.00000000`
= `00001010.00000000.00000000.00000000`
```
10.0.0.0

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/not-same-nw-ip.png)

Result are not same that is different

i.e. Mobile and laptop are not in same network.


> If they are in the same network I don't need a router
> but laptop & mobile are not same network, I have send this data through the router & need internet connection send data to mobile.


## Practical Application of Subnet Mask
- Wifi settings main subnet mask present
- kabhi kabhi 255.255.0.0 ye bhi subnet mask hota hai & that subnet becomes,
Subnet --> 192.168.0.0/16

- 2 octets (`192.168`) --> i.e. 16 bits of IP that reserved for IP

- last 2 octets (`0.0`) are reserved for host.

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/practical-subnet-mask.png)

> As a developer, subnet ke sathi interact nahi hota.

> But, your machines actually use subnet to determines & take decision whether data you went send go through the router or we can send directly to device.

> It actually improve your latency.

- Jo data hai if its in the same network then you have --> `Less latency`. becz, apko routers ke hops through nahi jana hota hai.
- You can directly send data.

E.g. Postgres DB in same N/W

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/postgres-same-nw.png)

- If your Postgres DB is in the same network, is in the same subnet.
- Apka Nodejs server jab bhi DB query karega you know need to hops between the network.
- Usko query bhej sakte ho & directly apko result de dega.


E.g. Postgres DB outside N/W

![alt text](/05-How-Subnet-Masks-Reveal-Network-Secrets-A-Deep-Dive/assets/postgres-outside-nw.png)

- Apka postgres outside n/w hai, you have to send that query through the router.
- Again postgres return karega response vo bho use path se hops le leke dega.




