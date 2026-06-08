# How the Internet Works

## What

When you type a website address and hit enter, a series of events happens in under a second. Here is what actually occurs, without jargon.

## The Restaurant Analogy

Imagine you're at a restaurant:

1. You look at the menu and decide what you want (you type a URL)
2. You tell the waiter your order (your browser sends a request)
3. The waiter takes the order to the kitchen (the request travels across the internet)
4. The kitchen prepares your food (the server processes the request)
5. The waiter brings the food back to you (the server sends a response)
6. You get your meal (your browser displays the webpage)

That's it. Request goes out, response comes back.

## What Happens in More Detail

### Step 1: You Type an Address

You type `www.example.com` into your browser. Your computer doesn't know where `www.example.com` is. It needs to look it up.

### Step 2: DNS Lookup

DNS is the phone book of the internet. It translates human-readable names (`www.example.com`) into computer-readable addresses (`93.184.216.34`).

Your computer asks a DNS server: "Where is www.example.com?"
The DNS server answers: "It's at 93.184.216.34"

This happens every time you visit a new website. Your computer caches (remembers) the answer for a while so it doesn't have to ask again.

### Step 3: The Request Travels

Your computer sends a request to the address it got from DNS. The request travels through your internet connection, across various networks and cables (including undersea cables between continents), until it reaches the server.

### Step 4: The Server Responds

The server receives your request, figures out what you're asking for, prepares the response (the webpage, the data, the image), and sends it back.

### Step 5: Your Browser Displays It

Your browser receives the response and renders it as the webpage you see.

## Key Concepts in Plain Language

- **Browser** — The app you use to visit websites (Chrome, Safari, Firefox)
- **Server** — A computer somewhere else that stores websites and responds to requests
- **DNS** — The system that translates names to addresses
- **Request** — What your computer sends to the server ("I want this page")
- **Response** — What the server sends back ("Here is the page")
- **HTTP/HTTPS** — The language computers use to talk to each other. The "S" means secure (encrypted)

## Why This Matters

When a website is slow, the problem could be at any step:
- DNS lookup taking too long
- The request traveling slowly (network issue)
- The server taking too long to respond (server issue)
- The response being too large (bandwidth issue)

Understanding the chain helps you understand where problems come from and what "the internet is down" actually means (it's usually one link in the chain, not the entire internet).
