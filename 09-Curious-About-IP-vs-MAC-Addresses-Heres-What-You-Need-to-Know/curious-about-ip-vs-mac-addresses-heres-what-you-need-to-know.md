# 09 Curious About IP vs MAC Addresses? Here's What You Need to Know!

#### What we can learn?
- What is difference between IP address and MAC address?
- Why we need to address scheme?

## Difference Between IP and MAC Address
**IP** 
- **192.168.1.10** -> `32 bits integer separated by . (dot)`
- IP this can change (Laptop again connected to router not compulsory IP same as before it laptop connect)
- IP can be duplicated.
	- I can have another n/w, with same IP but both are on different n/w.
	- Home 1 router & another home router 2, but both are in different n/w.
	- n/w 1 & n/w 2 but **IP are same**.
- In IP (subnet -> **192.168.1.0/24**) - 1st 3 octets (*192.168.1*) are reserved for n/w & 1 last octet (*0/24*) is reserved for host.

![alt text](/09-Curious-About-IP-vs-MAC-Addresses-Heres-What-You-Need-to-Know/assets/ip.mac-address.png)

**MAC**
- **00:04:1a:af:33:2a** -> `48 bits integer separated by : (colon) or - (hyphen)`
- MAC dos't change (always constant)
- MAC universally unique.
- MAC can't be duplicated. 
	- On same laptop, two n/w interface cards, both MAC address are always unique.
- In MAC (**00:04:1a:af:33:2a**) 1st 3 bytes (*00:04:1a:*) are reserved for manufacturer (e.g. identify intel as manufacturer) & last 3 bytes (*af:33:2a*) are represent for host that assigned that MAC.

> NOTE : MAC addresses are designed to be globally unique, though there are some exceptions. (e.g. spoofing, Manufacturing mistakes or locally administered MACs) exist.


## Role of IP in Internet Communication
Why we need 1 address over the other?

- Kisi device ka public IP pata hai that means - know the exact location of the device over the internet.
- Internet main travel karne ke liye I need an IP 
- E.g. EC2 machine public IP -> 10.0.0.1
	- Ye detect kaise hua ki, using subnet mask (10.0.0.1) - I send my data to default gateway (router), router kuch calculation karega & it will detect ki kya ye device or IP (10.0.0.1) mere kisi aur n/w main hai.

- EC2 instance router ko bhi nahi pata ki voh kis n/w main hai? becz, EC2 AWS ke data center main hai & mere router ko bhi pata nahi hai ki EC2 ke konse n/w main hai.
- Then, Mera router kisi aur router ko voh forward kar dega (my router & other router kind of default gateway)

- Uss router ko bhi nahi pata ki IP kaha pe hai, usne kisi aur router ko bheja... ayese hope kar-kar ke some how mere EC2 tak pohach jata hu.
- All this hope I need an IP? becz, muje kuch calculation karne hai, jo ye IP main reach kar raha hu kya vo mere n/w main hai to do this aur nahi hai toh to do this.

- `IP address has logical significance.` - Which I can used to calculate something on the basis of which I can take a decision.

- `MAC address ke liye asa kuch nahi hota.` - MAC address is completely random. You can't have logical significance of MAC address.

## MAC Address in Local Networks
- But, once you reach the local n/w, to send that data to exact machine- exact n/w interface, you need a MAC address that the requirement of the n/w.
- IP is virtual this can't change but, if I know the MAC address I can actually send that data physically to exact device jis ka MAC address match ho raha hai there is something called as `ARP` 

ARP - `Address Resolution Protocol`
- Ye IP leta hai & it will return the respective MAC address of that IP.
- Har ek device ke pass ek ARP table hota hai & usme mapping hoti hai IP to MAC.
- IP is globally present but MAC only work inside a local n/w.
- You have to reach to the n/w 1st before using the MAC.
- Jab bhi main uss destination ke n/w tak pohchunga uss EC2 ke uska bhi toh koi ek n/w hoga.

- Once I reach to the router of the EC2, ti send that packet to EC2 I need a MAC address.
- MAC address,
	- **ka need tabhi hota hai jab n/w tak pohach ta hu - local n/w tak pohach ta hu jis me mera voh device present hai**.

## Using IP for Network Routing Decisions
- But, IP address,
	- **ka need tabhi kaam aata hai, jab mere source ya destination tak - jo bhi n/w hai udhar tak pohach ne ki liye jo internet ke through travel karta hu, har ek step pe I need a IP address to make some decsion.**

### Summary
- `MAC` --> One work only in the local n/w.
- `IP` --> Once help to you reach that local n/w.
- You can't avoid any of this if you want to communicate internet over the n/w.
