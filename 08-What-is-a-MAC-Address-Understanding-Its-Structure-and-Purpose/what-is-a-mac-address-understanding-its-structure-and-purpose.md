# 08 What is a MAC Address? Understanding Its Structure and Purpose

#### What we can learn?
- What is structure of a MAC Address?
- What is MAC address?


## Understanding MAC Address Basics
- Aapka jo router hai ya phir apka jo mobile hai uskme kuch **network interfaces** hote hai

**Network Interfaces** --> is kind of connecting piece, jis se apka device over the *n/w wireless or with wire* communicate kar sakta hai with other devices.

Different kinds of n/w interfaces,
NIC - `Network Interface Card`
	- Visuals like integrate PCB's card
	- Antenna help receive or emit signal from that
  - Ethernet cable which also deal with electric signals
	- Bluetooth have n/w interface (kind of short range) 
- All n/w interfaces are physical things -> antenna, ethernet, bluetooth etc.
- To Address that physical things we need some kind of address
- IP we can't use(becz, *IP can change - it virtual thing*), but n/w interface card not change every time.

- We need some addressing which is permanent, uniques to that interface, this n/w card or ethernet.
- Device is same but, IP is changed.
- IP is not a reliable kind of addressing mechanism to identify device.

## Physical Devices and MAC Addresses

- On devices, n/w card, laptop, ethernet (actual physical device) - We use something called as `MAC address`.

**MAC address** --> which is unique to device (vo kabhi bhi change nahi hota)

- On some NICs have the MAC address printed on them.

MAC Address ==> `Media Access Control Address`

- Ye ek address hai jo bata ta hai ki actual physically voh device kaha pe hai.


# Structure of a MAC Address

MAC Address --> `: or - separated set of hexadecimal numbers`
`:` -> mostly in Mac book or Linux main hota hai
`-` -> on window

Example,
fa:16:3e:87:65:43
fa-16-3e-87-65-43

### What every byte represent?
fa ==> is 1 byte & whole thing is 6 bytes

MAC addresses are of `6 bytes` or 48 bits (1 bytes = 8 bits, 6*8 = 48 bits)

Every where (constant) weight of MAC address --> `6 bytes`

## Hexadecimal Numbers in MAC Addresses
- Each byte (fa) consist of `2 hexadecimal number`

```
0-9 decimals
0-1 are binary
0-16 are hexadecimals number
hexadecimals number --> 0 1 2 3 4 5 6 7 8 9 A B C D E F
```

## MAC Address Parts: OUI and Device Identifier

MAC address have divide into 2 parts,
1st part -> `OUI` -->  `fa:16:3e:`
2nd part ->  --> `87:65:43`

OUI --> Organizationally Unique Identifier
- Becz, MAC address present on physical device, that physical device koi toh manufacture karega.
- Ye jo manufacturing organization hote hai unko ek MAC address dena compulsory hota hai.

Example,
Intel manufacture that device 1st 3 bytes or 1st 24 bits are **reserved for organizations**.
- Intel all over world 1st half of MAC address (3 bytes) any device are common always.

> Large manufacturers like Intel often own multiple OUIs. just an FYI

last 3 bytes are actual jo physical piece hota hai i.e. n/w interface card uniquely identify hota hai.

- Other half of MAC address are reserved for host.

> As developer we can't directly deal with MAC address manually.


