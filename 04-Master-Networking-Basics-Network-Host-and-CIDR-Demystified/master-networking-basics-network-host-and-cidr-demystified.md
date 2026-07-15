# 04 Master Networking Basics: Network, Host, and CIDR Demystified

#### What we can learn?

- What a N/W?
- What is a Host?
- What is a CIDR Notation?
- From IP, which part can reserved for n/w & which part for host?

## Understanding the Network and Host Basics

E.g.
Let mere pass ek router hai & uske sath laptop connect hai & mobile, tab & other devices connected ho sakte hai

- Voh jo connection hai between in then --> `Network` form karta hai

If I'm in single n/w,

- Mobile & laptop are connected with router, that means in single n/w
- Laptop technically mobile ke sath **directly connect kar sakta hai even without internet.**

- Uss n/w main ek connection hona chaiye jis main mere IP assign hoga & mobile pe ek IP assign hoga.
- Using of that IP, I can communicate between them if they are in same n/w.

### What is the Host?

- Jis bhi machine i.e. laptop or mobile that can hold IP --> `Host`

> `N/W` - Something formed the between the devices
> `Host` - Some one who hold individual unique IP in that n/w

## Dynamic Host Configuration Protocol (DHCP)

![alt text](/04-Master-Networking-Basics-Network-Host-and-CIDR-Demystified/assets/dhcp.png)

- We can goes, wifi settings sync with laptop you will information about your IP in the wifi settings,
- Jo apke router ne apke mobile ko assign kiya hai using --> `DHCP`

DHCP - `Dynamic Host Configuration Protocol`

- DHCP sirf ek protocol hai, jisaka use karke router new device jo connect hau hai usko ek IP dynamically assign kar deta hai.
  e.g.
- Jaise hi mobile connect kiya to router ne usko ek kind of memory main tha IP assign kar diya
- That is unique IP of mobile.

Ye jo connect form hau hai called as --> `Network` --> `Local Area Network - LAN`

![alt text](/04-Master-Networking-Basics-Network-Host-and-CIDR-Demystified/assets/lan.png)

- That all device technically communicate with each other using IP.
- Without internet laptops se tab main request kar sakte ho & mobile & other using that particular IP becz that are in same n/w

- Individual IP hold kiya hai --> `Host` (e.g. mobile)

### CIDR

192.168.1.0/x --> _generally its 1st IP of that entire n/w_

- reserve IP that can hold n/w (n/w ko bhi ek IP ki tarha treat kiya jaye)

> That n/w actually use hota hai for decide any device I want to reach is inside a n/w or outside a n/w

![alt text](/04-Master-Networking-Basics-Network-Host-and-CIDR-Demystified/assets/cidr.png)

- Since the n/w itself is not a host or physical device, calling 192.168.1.0 the "networks IP" wasn't technically accurate.
- The correct term is network address (or n/w identifier), as it identifies the subnet 192.168.1.0/24 and is not assigned to any host.

## IP Address and Binary Representation

CIDR say,

- mere n/w ka (192.168.1.0/x) jo n/w ip hai uske /x hai
- `x` --> integer between 0-32 (bits)
- Integer is representing kitne bits mere n/w ko reserved hai
  e.g.
- binary representation of 192.168.1.0 -> 11000000.10101000.00000001.00000000

  92.168.1.0/24 --> 24 is no. of bits reserved for n/w part (**x is 24**)

means,

1st 24 bits or 1st 3 octets i.e. 11000000.10101000.00000001 --> reserved for n/w

- Iss n/w main jitne bhi devices & host honge unke IP ka 1st 3 octets same honge

e.g. printer 1st 3 octets -> are always constant becz, CIDR range saying,

- iss n/w ke 1st 3 octets always n/w ko reserved honge

## CIDR Notation and Host Calculation

![alt text](/04-Master-Networking-Basics-Network-Host-and-CIDR-Demystified/assets/cidr-range.png)

That also called as `CIDR Notation` (This only represent no of bits reserved for n/w)

If you have,

- 1 bit or 1 octet reserved for -> `Host` only use this part to configure IP for a new host.

- The above network can only host 2^8 machines - becz, I have onl 8 bits jisko assign kart sakta hau multiple devices.
- 2^8 = 256 hosts per network

  192.168.1.0/24 contains 256 total IP addreses (192.168.1.0 to 192.168.1.255) However, 2 addresses are reserved

- 192.168.1.0 -> **Network address**
- 192.168.1.255 -> **Broadcast address**

This leaves 254 usable addresses that ca be assign to devices.

## AWS and CIDR Notation

- We can work AWS we might dealing with CIDR notation (e.g. 192.168.1.0/24)

`0.0.0.0/0` --> famous CIDR notation --> entire internet pe jitne bhi IPs hai voh sab IP iss 0.0.0.0/0 CIDR notation main aate hai

- All octet of 0.0.0.0/0 can be dynamic

e.g.

192.168.1.0/24 --> 24 say -> 24 bits reserved for n/w, 8 bits for host.

- 8 bits for host > 2^ 8 host per network -> 256 hosts

  192.168.1.0/16 --> 16 say -> 1st 16 bits reserved for n/w, other 2 octets (remaining 16 bits) are host in that n/w.

- 16 bits for host > 2^ 16 host per network -> 65,000 hosts.
