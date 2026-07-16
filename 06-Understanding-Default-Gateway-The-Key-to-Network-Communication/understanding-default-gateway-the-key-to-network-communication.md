# 06 Understanding Default Gateway: The Key to Network Communication

#### What we can learn?
- What is a Default Gateway?

## Direct Data Transfer in Same Network
- How subnet help us to determine, jo bhi IP hum reach karna cahate hai voh kya hamare n/w ka part hai ki nahi?

Agar voh n/w ka part hai toh my laptop can send data directly to mobile.

But agar mobile usse n/w ka part nahi hai, then ye data mobile ko kaise send hoga?
- Laptop knowns the IP of mobile, but doesn't know that IP is present in n/w or not? --> using subnet mask
- Laptop doesn't know ki kaise mobile directly data send karna hai is using something called as --> `default gateway`.
- Default gateway is --> `router` 
- My Router --> is noting but my default gateway.

## Role of Router in Data Transmission

- Laptop kya karega usk ne default gateway ko voh data bhej dega & say,

*Mere ko pata nahi ye mobile kaha pe hai internet pe?*

- But, I'm assuming ki tuje (router), ye pata hoga ess liye ye data tuje bhej raha hu --> isko mobile tak bhej de --> This `default gateway`.

*Laptop ko kaise patha ki deafult gateway kya hai?*
- Laptop main ye pre configure hota hai, ki jab bhi laptop apka router ke sath connect hota hai, toh usko pata hoga ki laptop ka default gateway kya hai.


