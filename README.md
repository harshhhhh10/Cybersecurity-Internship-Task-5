Task 5 – Capture and Analyze Network Traffic Using Wireshark (Kali Linux)

 Overview

For this task, I used Kali Linux (running on VirtualBox) to capture and analyze live network traffic using Wireshark. The objective was simple on paper: capture packets, identify protocols, and understand what’s actually happening behind everyday browsing and ping commands.

But once I started looking at the packets closely, it became clear that even something as small as opening Google triggers a surprising amount of network activity.

Environment Used

Kali Linux (Virtual Machine – VirtualBox)

Wireshark (v4.6.2 shown in capture)

Firefox Browser

Terminal (for ping command)

 Steps I Followed

Opened Wireshark in Kali Linux.

Selected the active interface (eth0).

Started packet capture.

Generated traffic by:

Running ping google.com

Opening Google in Firefox

Let the capture run for about a minute.

Applied filters like:

dns

tls

tcp.flags.syn == 1

Inspected packet details (handshakes, DNS queries, etc.).

Saved the capture file as .pcapng.

Protocols Identified

While analyzing the capture, I observed multiple protocols interacting together. At first, it looked overwhelming — but breaking it down helped.

1️⃣ DNS (Domain Name System)

When I pinged or opened Google, the system first sent a DNS query to resolve google.com into an IP address.

I noticed:

Standard query A (IPv4)

Standard query AAAA (IPv6)

Corresponding DNS responses

It’s interesting how the browser makes multiple DNS requests, not just for the main site but also for related services (like Mozilla CDN entries).

2️⃣ ICMP (Ping Traffic)

Using:

ping google.com


I observed:

ICMP Echo Request

ICMP Echo Reply

This clearly showed round-trip communication between my Kali machine and Google’s server. It’s probably the simplest way to see packet exchange in real time.

3️⃣ TCP (Transmission Control Protocol)

Filtering with:

tcp.flags.syn == 1


helped me observe the start of TCP handshakes.

I could see:

SYN

SYN-ACK

ACK

This three-step process confirms reliable connection establishment before data transfer begins. Watching it happen live made the concept much clearer than just reading about it.

4️⃣ TLS (Encrypted HTTPS Traffic)

After DNS resolution and TCP handshake, encrypted traffic begins.

Using:

tls


I observed:

Client Hello

Server Hello

Certificate exchange

Change Cipher Spec

Application Data



 Packet Details Observed

From the packet details pane, I analyzed:

Source and Destination IP addresses

Source and Destination ports

TCP sequence and acknowledgment numbers

TLS handshake stages

DNS query names

ICMP types and codes

The packet byte view helped visualize raw hexadecimal data, which initially looked confusing but gradually started making sense when mapped to protocol layers.

 Interview Question Answers

1. What is Wireshark used for?
Wireshark is a network protocol analyzer used to capture and inspect live network traffic for analysis and troubleshooting.

2. What is a packet?
A packet is a small unit of data transmitted across a network containing headers and payload.

3. How to filter packets in Wireshark?
By using display filters such as:

dns

tcp

tls

tcp.flags.syn == 1

4. Difference between TCP and UDP?
TCP is reliable and connection-oriented.
UDP is faster but connectionless and does not guarantee delivery.

5. What is a DNS query packet?
A DNS query packet is sent by a client to request the IP address of a domain name.

6. How can packet capture help in troubleshooting?
It helps identify connection failures, packet loss, incorrect DNS responses, handshake failures, and other network issues.

7. What is a protocol?
A protocol is a defined set of rules that govern communication between devices on a network.

8. Can Wireshark decrypt encrypted traffic?
Only if encryption keys are available. By default, HTTPS traffic remains encrypted.

 What I Learned

At first, network traffic just looked like random noise. But once I started applying filters and focusing on one protocol at a time, patterns began to emerge.

What stood out most is how many background connections happen when you simply open a website. It’s not just one connection — it’s dozens.

This task helped me understand:

TCP/IP layering in practice

How DNS resolution actually works

TLS handshake sequence

How filtering simplifies analysis

Overall, it made theoretical networking concepts feel much more concrete.
