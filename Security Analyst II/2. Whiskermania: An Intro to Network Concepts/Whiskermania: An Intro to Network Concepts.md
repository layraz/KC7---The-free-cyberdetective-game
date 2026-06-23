# Whiskermania: An Intro to Network Concepts

# About

You just got hired as the IT Administrator at a Cat Sanctuary, which would be great news if you had any idea what you were doing.

By following Jamie's trail and asking the right, you'll learn how networks actually work: IP addresses, VLANs, firewalls, ports, protocols, and DNS.

This is a fun and data-driven take on a classic networks class.

# Section 1: Welcome to the Chaos

## Question 1

Your badge says "IT Administrator." Your stomach says "I have made a terrible mistake."

You step through the front door of Whiskers & Wonders Cat Sanctuary and immediately have to sidestep a kitten. Then another. Someone rushes past with a carrier, a phone is ringing somewhere, and a cat is yowling from the back room.

Morgan Chen, the Operations Manager, spots you from across the lobby. They're holding a clipboard, a coffee cup, and somehow also a cat. "Oh thank god. You're the new IT person? Jamie's replacement?"

You nod. Confidently. You definitely know what you're doing.

Morgan doesn't look entirely convinced, but they're too busy to dig deeper. "Great. I'll get you oriented once I deal with this—" The cat in their arms makes a break for it. Morgan catches it. "Kitten season," they explain, as if that covers everything.

**Type `definitely ready` to continue.**

> `definitely ready`
> 

## Question 2

Morgan leads you through the shelter, dodging volunteers and explaining things at high speed. Intake area. Medical wing. Adoption center. A room full of kittens that makes you stop for a moment because, well, kittens.

"Whiskermania is in five days," Morgan says, and you can hear the stress in their voice. "Our biggest fundraiser of the year. Last year we raised forty thousand dollars in one day. Lots of donors, lots of web traffic, lots of things that need to not break."

They glance at you. "Jamie was supposed to handle the tech side. But then Jamie left, and…" They trail off.

You're starting to understand why they hired you so quickly.

> `no pressure`
> 

## Question 3

Morgan stops at a desk in the back office. It's still set up with someone else's stuff. A cat-shaped mug with cold coffee. Post-it notes stuck to the monitor. A notebook lying open.

"This was Jamie's desk," Morgan says quietly. "They left about two weeks ago. Kind of suddenly. Didn't really explain why." They look uncomfortable. "We haven't had time to clean it out."

A large orange cat is asleep on the server rack in the corner. "That's Biscuits," Morgan adds. "He comes with the office. Non-negotiable."

You sit down carefully, trying not to disturb anything. One post-it catches your eye: "Every device has an address. Learn the addresses, learn the network."

Seems like good advice. You're going to need it.

**Type `let's learn` to continue.**

> `let's learn`
> 

# Section 2: The Network Map

## Question 1

Let's start with the basics. You need to look like you know what you're doing, which means actually knowing what you're doing.

An IP address is like a mailing address for computers. Every device on a network needs one so other devices know where to send data. The format looks like this: `10.10.16.5`. Four numbers separated by dots. Each number ranges from 0 to 255.

You'll see IP addresses constantly in network logs. They're how you identify which device did what.

**What does IP stand for?**

> `Internet Protocol`
> 

## Question 2

IPv4 addresses have four numbers separated by dots. Each number is called an "octet" because it represents 8 bits of data (and 8 bits can represent values from 0 to 255).

`10.10.16.5` has four octets: 10, 10, 16, and 5.

**How many octets are in an IPv4 address?**

> `4`
> 

## Question 3

Morgan walks by your desk. "Getting oriented? The network here is pretty straightforward. We use private IP addresses internally."

"Private addresses," you repeat, nodding wisely. You have no idea what that means, but you're not about to admit that.

Here's what you need to know: certain IP ranges are reserved for internal networks and can't be reached directly from the internet. These are called private IP addresses:

- `10.0.0.0` - `10.255.255.255` (anything starting with 10)
- `172.16.0.0` - `172.31.255.255` (172.16 through 172.31)
- `192.168.0.0` - `192.168.255.255` (anything starting with 192.168)

Everything else is public, meaning it's routable on the internet.

**Is `10.10.16.5` a public or private IP address?**

> `private`
> 

## Question 4

`8.8.8.8` is Google's public DNS server.

It doesn't start with 10, it's not in the 172.16-31 range, and it's not 192.168. So it's not private.

**Is `8.8.8.8` public or private?**

> `public`
> 

## Question 5

`192.168.1.1` is a common home router address.

**Public or private?**

> `private`
> 

## Question 6

`172.20.50.1`

The private range for 172 is 172.16 through 172.31. This is 172.20, which falls in that range.

**Public or private?**

> `private`
> 

## Question 7

`52.214.98.1` is an AWS cloud server.

52 isn't in any private range.

**Public or private?**

> `public`
> 

## Question 8

You find another of Jamie's post-it notes stuck to the side of the monitor:

"`185.174.137.42` - WHO IS THIS? Check again."

The handwriting gets more urgent toward the end. Jamie circled the IP twice.

**Is `185.174.137.42` public or private?**

> `public`
> 

## Question 9

You pocket Jamie's note. That IP might be important later.

Morgan pulls up a network diagram on the whiteboard and grabs a marker. "Let me show you how we've got things set up. We split our network into zones, different zones for different purposes. It's called network segmentation."

> `5`
> 

```sql
DeviceInfo
| summarize device_count=count() by vlan
| order by device_count desc
```

```sql
workstations	36
servers	      9
dmz	          6
restricted	  1
perimeter	    1
```

## Question 10

Morgan sketches the network layout:

```sql
                              ┌──────────────┐
                              │   INTERNET   │
                              └──────┬───────┘
                                     │
                              ┌──────▼───────┐
                              │   FIREWALL   │
                              └──────┬───────┘
                                     │
                    ┌────────────────┼────────────────┐
                    │                │                │
             ┌──────▼───────┐ ┌──────▼───────┐ ┌──────▼───────┐
             │     DMZ      │ │   SERVERS    │ │  RESTRICTED  │
             │  10.10.0.x   │ │  10.10.1.x   │ │  10.10.2.x   │
             │              │ │              │ │              │
             │ Web, Proxy,  │ │ Mail, DNS,   │ │   Domain     │
             │ Donations    │ │ Databases    │ │  Controller  │
             └──────────────┘ └──────────────┘ └──────────────┘
                                     │
                              ┌──────▼───────┐
                              │ WORKSTATIONS │
                              │  10.10.16.x  │
                              │              │
                              │ 35 employees │
                              └──────────────┘

```

"Each zone has its own IP range," Morgan explains. "If I see `10.10.16.x`, I know it's a workstation. `10.10.0.x` means DMZ. The IP tells you where the device lives."

```sql
DeviceInfo
| summarize device_count=count() by vlan
| order by device_count desc
```

**Which VLAN has the most devices?**

> `workstations`
> 

## Question 11

The DMZ, or demilitarized zone, faces the internet. It contains systems that the outside world needs to reach: web servers, the donation portal, VPN gateway.

"This is where Whiskermania traffic will hit," Morgan says. "Lots of donors visiting the site, making donations. It all comes through here."

```sql
DeviceInfo
| where vlan == "dmz"
| project name, ip_address, host_type
```

**What is the IP address of the DonationPortal?**

> `10.10.0.3`
> 

## Question 12

Morgan's expression gets serious. "The DonationPortal is critical. We're a nonprofit. Every donation matters, and Whiskermania is when most of them come in."

They pause. "Jamie seemed really stressed about the donation system before they left. Kept asking questions about traffic to it. I thought they were just being thorough."

```sql
DeviceInfo
| where vlan == "servers"
| project name, ip_address
```

**What is the IP address of MailServer01?**

> `10.10.1.2`
> 

## Question 13

Morgan lowers their voice. "And the restricted zone is where we keep the domain controller. It handles all authentication for the network. If someone compromises that, game over.”

```sql
DeviceInfo
| where vlan == "restricted"
| project name, ip_address, host_type
```

**What device is in the restricted zone?**

> `DomainController`
> 

## Question 14

You're starting to see the pattern. Each VLAN has its own IP range:

- DMZ: `10.10.0.x`
- Servers: `10.10.1.x`
- Restricted: `10.10.2.x`
- Workstations: `10.10.16.x`

This means you can look at any IP and immediately know what part of the network it's in.

```sql
DeviceInfo
| where ip_address startswith "10.10.1"
| project name, ip_address, vlan
| take 5
// 10.10.1.
```

**What VLAN do IPs starting with `10.10.1` belong to?**

> `servers`
> 

## Question 15

"So if I see an IP starting with `10.10.0`," you say, "I know it's in the DMZ."

"Exactly." Morgan looks pleased that you're catching on. "And `10.10.16` means workstations. `10.10.2` means restricted. Once you know the ranges, you can read the network like a map."

**What IP range are workstations in? (format: 10.10.X)**

> `10.10.16`
> 

## Question 16

```sql
DeviceInfo
| where name == "DNS-Server"
| project name, ip_address, vlan
```

DNS is how computers translate domain names like `google.com` into IP addresses. When you type a website name, your computer asks the DNS server "what's the IP for this?" and gets an answer. Every lookup on the network starts here.

**What is the IP address of the DNS-Server?**

> `10.10.1.7`
> 

## Question 17

You dig through Jamie's desk drawer and find more notes. IP addresses everywhere, some circled, some crossed out.

One note says: "Internal DNS: `10.10.1.7` - all queries go here first"

Another note, crumpled like Jamie threw it away and then fished it back out: "Why is `10.10.16.x` talking to `185.174.137.42`? That's not normal."

Jamie was tracking traffic from workstations to an external IP. They thought something was wrong.

**What table would show you DNS lookups?**

> `DnsEvents`
> 

<aside>
💡

Type DNS in console and autofill will show the table name.

</aside>

## Question 18

The firewall sits between network zones and controls what traffic can pass between them. Think of it as a security checkpoint that inspects every connection and decides whether to allow or block it.

"Traffic between zones has to pass through the firewall," Morgan explains. "That's how we control what goes where. Workstations can reach the internet, but the internet can't reach workstations directly."

You find another Jamie note: "Check firewall logs. Something is getting through that shouldn't."

**What zone contains the Domain Controller?**

> `Restricted`
> 

## Question 19

You lean back in Jamie's chair. Biscuits has migrated from the server rack to the desk and is now sitting on a pile of papers, watching you with mild interest.

Private IPs for internal use. Public IPs for the internet. VLANs to segment the network. IP ranges to identify zones. You're starting to see how the pieces fit together.

Morgan gets called away to deal with a kitten emergency. You're alone with Jamie's notes and a lot of questions.

**Type `time to dig deeper` to continue.**

> `time to dig deeper`
> 

# Section 3: Following the Trail

## Question 2

Certain ports are standardized. You'll see these constantly:

- **22** - SSH (secure shell, remote command line access)
- **25** - SMTP (sending email)
- **53** - DNS (domain name lookups)

## Question 1

With Morgan gone, you have time to dig into Jamie's notebook properly.

There's a page titled "Network Flow Analysis" with a note at the top: "Every connection has two endpoints: source IP + port → destination IP + port"

You know about IP addresses now. But what's a port?

Think of it this way: the IP address gets you to the right building, but the port gets you to the specific apartment. A server might run multiple services, and each service listens on a different port.

```html
          SERVER: 142.250.80.46
     ┌─────────────────────────────┐
     │  ┌─────┐ ┌─────┐ ┌─────┐    │
     │  │ :22 │ │ :80 │ │:443 │    │
     │  │ SSH │ │HTTP │ │HTTPS│    │
     │  └─────┘ └─────┘ └─────┘    │
     └─────────────────────────────┘
```

Port numbers range from 0 to 65535.

**What is the maximum port number?**

> `65535`
> 
- **80** - HTTP (unencrypted web traffic)
- **443** - HTTPS (encrypted web traffic)
- **3389** - RDP (Windows remote desktop)

Memorize these. When you see a connection on port 443, you know it's HTTPS. When you see port 22, someone is using SSH.

**What port number is used for HTTPS?**

> `443`
> 

## Question 3

**What port number is used for DNS?**

> `53`
> 

## Question 4

**What port number is used for SSH?**

> `22`
> 

## Question 5

**What port number is used for RDP?**

> `3389`
> 

## Question 6

Let's see what ports are most common on this network.

```sql
NetworkFlow
| summarize traffic=count() by dest_port
| order by traffic desc
| take 5
```

**What destination port has the most traffic?**

> `25`
> 

## Question 7

Port 25 is SMTP, which handles email transmission. High email traffic is expected for an organization that communicates regularly with donors, adopters, and volunteers.

**What does SMTP stand for?**

> `Simple Mail Transfer Protocol`
> 

## Question 8

Jamie's notebook has a section on protocols. There are two main ways data moves on a network:

**TCP** (Transmission Control Protocol) is reliable and ordered. It establishes a connection first (a "handshake"), confirms delivery, and resends anything that gets lost. Most traffic uses TCP: web browsing, email, file transfers.

**UDP** (User Datagram Protocol) is fast but unreliable. It just sends packets without confirming they arrived. This is fine for things like DNS lookups or video streaming where speed matters more than perfection.

**What does TCP stand for?**

> `Transmission Control Protocol`
> 

## Question 9

**What does UDP stand for?**

> `User Datagram Protocol`
> 

## Question 10

```sql
NetworkFlow
| where dest_port == 53
| summarize count() by protocol
```

**What protocol does DNS primarily use?**

> `UDP`
> 

## Question 11

```sql
NetworkFlow
| summarize count() by protocol
| order by count_ desc
```

**What protocol carries the majority of network traffic on this network?**

> `TCP`
> 

## Question 12

Morgan pokes their head back in. "Still alive in here?"

"Just reviewing the network architecture," you say, which is technically true.

"Uh huh." Morgan doesn't look entirely convinced, but they're too busy to press. "Holler if you need anything. I'll be dealing with the new kitten intake."

Biscuits, still on the desk, yawns.

**Type `totally normal` to continue.**

> `totally normal`
> 

## Question 13

Now for the key insight that ties everything together: DNS.

When you type a website name into your browser, your computer doesn't actually know where that website lives. It has to ask. It sends a query to the DNS server saying "what's the IP address for `google.com`?" and the DNS server responds with the answer.

```sql
┌─────────────┐                    ┌────────────┐
│ WORKSTATION │   1. Query         │ DNS SERVER │
│ User types: │───────────────────►│            │
│ google.com  │                    │ "What IP   │
│             │   2. Response      │  is this?" │
│             │◄───────────────────│            │
│             │   142.250.80.46    │            │
└─────────────┘                    └────────────┘
```

This happens before any actual connection. DNS is the first step.

**What does DNS stand for?**

> `Domain Name System`
> 

## Question 14

```sql
DnsEvents
| take 5
| project timestamp, client_ip, query_name, resolved_ips, dns_server
```

The DnsEvents table logs every DNS lookup on the network. You can see which device asked, what domain they asked about, and what IP address was returned.

**What column contains the domain name that was looked up?**

> `query_name`
> 

## Question 15

DNS records come in different types:

- **A record** - Maps a domain to an IPv4 address (`google.com` → `142.250.80.46`)
- **AAAA record** - Maps a domain to an IPv6 address
- **MX record** - Identifies the mail server for a domain
- **CNAME record** - Creates an alias pointing to another domain

**What type of DNS record maps a domain to an IPv4 address?**

> `A`
> 

## Question 16

```sql
DnsEvents
| take 5
| project query_name, query_type, ttl
```

**TTL** (Time To Live) tells the client how long to cache the result before asking again. A TTL of 300 means "remember this answer for 300 seconds."

**What does TTL stand for?**

> `Time To Live`
> 

## Question 17

You're starting to understand why Jamie's notes kept mentioning DNS. If malware needs to contact an attacker's server, it has to look up the address first. That lookup gets logged. DNS is a record of intent: you can see what domains a device tried to reach, even if the connection itself was encrypted.

Jamie wrote: "Check DNS again. The answer is in the lookups."

**Type `following the breadcrumbs` to continue.**

> `following the breadcrumbs`
> 

## Question 18

Let's trace how a single web request moves through the network. When someone browses to a website, multiple things happen in sequence, and each step creates a log entry.

```sql
┌─────────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   WORKSTATION   │      │     DNS     │      │    PROXY    │      │  INTERNET   │
│   10.10.16.14   │      │  10.10.1.7  │      │  10.10.0.1  │      │             │
└────────┬────────┘      └──────┬──────┘      └──────┬──────┘      └──────┬──────┘
         │                      │                    │                    │
         │  1. DNS Query (:53)  │                    │                    │
         │─────────────────────►│                    │                    │
         │◄─────────────────────│                    │                    │
         │  2. Response: IP     │                    │                    │
         │                      │                    │                    │
         │  3. HTTPS Request (:443)                  │                    │
         │──────────────────────────────────────────►│                    │
         │                                           │  4. Forward        │
         │                                           │─────────────────────►
         │                                           │◄────────────────────
         │◄──────────────────────────────────────────│  5. Response       │

```

First the DNS lookup (logged in DnsEvents), then the network connection (NetworkFlow), then the proxy processes it (ProxyEvents). One user action, multiple log entries.

Let's trace a real request. Someone browsed to `docs.google.com`.

```sql
DnsEvents
| where query_name == "docs.google.com"
| where query_type == "A"
| project timestamp, client_ip, query_name, resolved_ips
| take 1
```

**What IP address did `docs.google.com` resolve to?**

> `152.52.146.99`
> 

## Question 19

Step 1 complete: DNS resolved `docs.google.com` to `152.52.146.99`.

Now let's see the network flow for that DNS query. DNS uses port 53. When the our user `10.10.16.14` conducts a dns query, and we can observe some corresponding traffic to the DNS server over port 53 around the same time.

```sql
NetworkFlow
| where src_ip == "10.10.16.14"
| where timestamp between (
    datetime(6/3/2025, 8:39:19 AM) .. datetime(6/3/2025, 8:59:19 AM)
)
| where dest_port == "53"
| project timestamp, src_ip, dest_ip, dest_port, protocol
| limit 10
```

**What protocol was this DNS query using?**

> `UDP`
> 

## Question 20

After DNS resolved the domain, the computer connected to the destination IP on port 443 for HTTPS.

```sql
NetworkFlow
| where src_ip == "10.10.16.14"
| where dest_ip == "152.52.146.99"
| project timestamp, src_ip, dest_ip, dest_port, protocol
| take 1
```

**What destination port was used for the HTTPS connection?**

> `443`
> 

## Question 21

The proxy sits between internal users and the internet. It logs the full URL of every web request, even for HTTPS traffic (the shelter does SSL inspection).

```sql
ProxyEvents
| where src_ip == "10.10.16.14"
| where domain == "docs.google.com"
| where timestamp between (datetime(2025-06-03 08:50:00) .. datetime(2025-06-03 09:00:00))
| project timestamp, src_ip, url, status_code
| take 1
```

**What HTTP status code was returned?**

> `200`
> 

```sql
"timestamp": 2025-06-03T08:54:56.1240500Z,
"src_ip": 10.10.16.14,
"url": https://docs.google.com/document/d/fN5AwbZMfNu1uatdHZM7LCN77,
"status_code": 200
```

## Question 22

You just traced a single web request across three different log sources:

| **Step** | **Table** | **What it shows** |
| --- | --- | --- |
| 1 | DnsEvents | Domain → IP resolution |
| 2 | NetworkFlow | UDP to DNS server (port 53) |
| 3 | NetworkFlow | TCP to website (port 443) |
| 4 | ProxyEvents | Full URL and response |

One user action creates multiple log entries across multiple tables. When you're investigating, you need to correlate across all of them to see the full picture.

**What happens first when browsing to a website: the DNS lookup or the TCP connection?**

> `DNS Lookup`
> 

## Question 23

The proxy sees everything that goes through it, including the full URL, not just the domain.

```sql
ProxyEvents
| take 5
| project src_ip, url, method, status_code
```

**What column contains the full URL?**

> `url`
> 

## Question 24

HTTP status codes tell you what happened with a request:

- **200** - OK, success
- **301/302** - Redirect
- **403** - Forbidden
- **404** - Not Found
- **500** - Server Error

**What status code indicates a successful request?**

> `200`
> 

## Question 25

HTTP methods describe what the client is trying to do:

- **GET** - Retrieve data (most common for browsing)
- **POST** - Submit data (forms, uploads)
- **PUT** - Update data
- **DELETE** - Remove data

**What HTTP method is used to submit form data?**

> `POST`
> 

## Question 26

You've learned how to trace traffic across DNS, NetworkFlow, and ProxyEvents. You understand ports, protocols, and how the network is segmented into zones. You can look at an IP address and know what part of the network it belongs to.

This is exactly what Jamie was doing. Following traffic patterns. Looking for anomalies. Building a picture of what was happening on the network.

That IP from Jamie's note, `185.174.137.42`, the one circled twice with "WHO IS THIS?" scrawled next to it. Where does it show up in the logs?

**Type `time to investigate` to continue.**

> `time to investigate`
> 

# Section 4: The Picture Emerges

## Question 1

Morgan gets pulled away again. Something about a server issue and a volunteer who accidentally unplugged the wrong thing.

"Can you hold down the fort? I'll be back in an hour."

You're alone with the data, Jamie's notes, and Biscuits, who has curled up on the warm part of the desk near the monitor. Time to investigate properly.

Let's start where Jamie left off: that suspicious external IP. If anyone connected to `185.174.137.42`, they had to look up the domain first. DNS will tell us what domain points to that IP.

```sql
DnsEvents
| where resolved_ips has "185.174.137.42"
| distinct query_name
```

**What domain resolves to `185.174.137.42`?**

> `update-cdn-service.xyz`
> 

## Question 2

The domain `update-cdn-service.xyz` is not a legitimate CDN. The `.xyz` top-level domain is cheap and frequently used by attackers because it draws little attention. The name itself is designed to look like boring infrastructure, the kind of thing that wouldn't raise alarms in a casual log review.

This pattern is common for C2 (Command and Control) domains. Make it look technical and forgettable.

```sql
DnsEvents
| where query_name contains "update-cdn-service.xyz"
| distinct client_ip
```

**How many distinct internal IPs queried this domain?**

> `3`
> 

```sql
10.10.16.14
10.10.16.7
10.10.16.24
```

## Question 3

Multiple machines are querying this suspicious domain.

```sql
DnsEvents
| where query_name contains "update-cdn-service.xyz"
| summarize count() by client_ip
| order by count_ desc
```

Each of these IPs belongs to a potentially compromised host. Their DNS queries prove they've been reaching out to the attacker's infrastructure.

**Is `update-cdn-service.xyz` an internal domain or an external domain?**

> `external`
> 

## Question 4

Now let's look at the actual network connections. If they resolved the domain, they probably connected to it.

```sql
NetworkFlow
| where dest_ip == "185.174.137.42"
| project timestamp, src_ip, dest_ip, dest_port, protocol, bytes
| take 10
```

**What destination port is this traffic using?**

> `443`
> 

## Question 5

Port 443 is HTTPS. The attackers are using encrypted web traffic to communicate with compromised machines. From a firewall's perspective, this looks exactly like normal web browsing: outbound HTTPS to an external server. Nothing unusual about that, which is exactly the point.

Attackers choose port 443 because it's almost always allowed through firewalls.

**What protocol normally runs on port 443?**

> `HTTPS`
> 

## Question 6

Let's check the proxy logs for more detail.

```sql
ProxyEvents
| where domain == "update-cdn-service.xyz"
| project timestamp, src_ip, url, method, status_code
| order by timestamp asc
```

Look at the URLs being accessed. There's a pattern here.

**What URL path appears repeatedly in these requests?**

> `/api/v1`
> 

## Question 7

The paths `/api/v1/status` and `/api/v1/config` look like legitimate API endpoints, which is intentional. But they're not real API calls. This is C2 beaconing: the malware checks in periodically to let the attacker know it's still running and to receive any commands.

- `/status` means "I'm still here, any instructions?"
- `/config` means "Send me my configuration"

**What does C2 stand for?**

> `Command and Control`
> 

## Question 8

When malware contacts its C2 server at regular, predictable intervals, it's called beaconing. The infected machine "checks in" periodically to report status and receive instructions.

Common beacon intervals are 5 minutes, 15 minutes, 30 minutes, or 1 hour. The regularity is what makes it detectable: legitimate user browsing is random, but beacons are predictable.

**What is the term for malware regularly checking in with its C2 server?**

> `beaconing`
> 

## Question 9

Morgan texts you: "Server issue more complicated than expected. Might be another hour. You okay?"

You look at the screen. Three compromised machines. C2 traffic. Active beaconing.

You are not okay, but that's not what you text back.

**Type `all good here` to continue.**

> `all good here`
> 

## Question 10

Let's trace the attack chain. Where did this start? Most compromises begin with phishing.

```sql
Email
| where sender !endswith "@whiskersandwonders.org"
| where sender contains "whiskersandwonders"
| project timestamp, sender, recipient, subject
```

**What domain in the sender address is impersonating the sanctuary?**

> `whiskersandwonders-hr.com`
> 

```sql
"timestamp": 2025-06-02T16:00:00.000Z,
"sender": noreply@whiskersandwonders-hr.com,
"recipient": alex_rivera@whiskersandwonders.org,
"subject": [EXTERNAL] Important: Employee Benefits Update Required
```

## Question 11

The domain `whiskersandwonders-hr.com` is not `whiskersandwonders.org`. Someone registered a lookalike domain and sent emails pretending to be HR. Classic credential phishing.

```sql
Email
| where sender contains "whiskersandwonders-hr.com"
| project timestamp, sender, recipient, subject
```

**What was the subject line of the phishing email?**

> `[EXTERNAL] Important: Employee Benefits Update Required`
> 

## Question 12

The subject line "Employee Benefits Update Required" with an urgency marker is textbook social engineering. Create pressure, impersonate authority, get people to click before they think.

```sql
Email
| where sender contains "whiskersandwonders-hr.com"
| distinct recipient
```

**How many employees received the phishing email?**

> `3`
> 

```sql
alex_rivera@whiskersandwonders.org
jessica_huang@whiskersandwonders.org
david_okonkwo@whiskersandwonders.org
```

## Question 13

Three phishing targets. Multiple machines querying the C2 domain. That's not a coincidence.

Let's verify: the employees who received the phishing emails should be the same ones whose machines are now beaconing.

```sql
// Get phishing recipients
Email
| where sender contains "whiskersandwonders-hr.com"
| distinct recipient

// Compare to C2 DNS queries
DnsEvents
| where query_name contains "update-cdn-service.xyz"
| distinct client_ip
```

**Did all three phishing recipients become compromised? (yes/no)**

> `yes`
> 

```sql
//Output 1
//alex_rivera@whiskersandwonders.org
//jessica_huang@whiskersandwonders.org
//david_okonkwo@whiskersandwonders.org

//Output 2
// 10.10.16.14
// 10.10.16.7
// 10.10.16.24

// To correlate them we can use:
let _ip=DnsEvents
| where query_name contains "update-cdn"
| distinct client_ip;
Employees
| where ip_addr in (_ip)
| project name, ip_addr
```

```sql
Alex Rivera	  10.10.16.7
David Okonkwo	10.10.16.14
Jessica Huang	10.10.16.24
```

## Question 14

The attack chain is clear now: the attacker registered a fake HR domain, sent phishing emails to targeted employees, and they clicked. Malware installed on their machines and began beaconing to `update-cdn-service.xyz` over port 443, blending in with normal HTTPS traffic.

Jamie found this. That's why the notes were so urgent. That's why they left.

**Type `jamie was right` to continue.**

> `jamie was right`
> 

## Question 15

Let's build a timeline. When did the phishing emails arrive?

```sql
Email
| where sender contains "whiskersandwonders-hr.com"
| project timestamp, recipient
| order by timestamp asc
```

**What date was the first phishing email sent? (copy and paste)**

> `6/2/2025, 4:00:00 PM`
> 

## Question 16

Now when did the C2 traffic start?

```sql
DnsEvents
| where query_name contains "update-cdn-service.xyz"
| summarize first_query=min(timestamp) by client_ip
| order by first_query asc
```

**Approximately how many hours between the phishing email and the first C2 beacon?**

> `less than 8`
> 

```sql
10.10.16.14	6/2/2025, 11:52:31 PM
10.10.16.7	6/5/2025, 6:12:41 PM
10.10.16.24	6/7/2025, 7:11:52 PM
```

## Question 17

First saw the beacon activity in the DNS events, as the compromised computer tried to resolve the malicious domain. Now we see the compromise computer having web requests to the malicious domain.

```sql
ProxyEvents
| where domain == "update-cdn-service.xyz"
| project timestamp, src_ip
| order by timestamp asc
```

**How many requests did the victim computer make to the malicious domain?**

> `6`
> 

## Question 18

Now let's build the full IOC (Indicator of Compromise) list. These are the specific artifacts that identify this attack.

```sql
// Malicious domain
DnsEvents 
| where query_name contains "update-cdn-service.xyz" 
| distinct query_name

// Malicious IP
DnsEvents 
| where query_name == "update-cdn-service.xyz" 
| distinct tostring(resolved_ips)

// Phishing domain
Email 
| where sender contains "whiskersandwonders-hr" 
| distinct sender
```

**What is the C2 domain?**

> `update-cdn-service.xyz`
> 

## Question 19

**What is the C2 IP address?**

> `185.174.137.42`
> 

## Question 20

**What is the phishing sender domain?**

> `whiskersandwonders-hr.com`
> 

## Question 21

**What destination port is the C2 traffic using?**

> `443`
> 

```sql
NetworkFlow
| where dest_ip == "185.174.137.42"
| distinct dest_port
```

## Question 22

Let's find out who specifically was compromised.

```sql
DnsEvents
| where query_name contains "update-cdn-service.xyz"
| distinct client_ip
| join kind=inner (Employees | project ip_addr, name, username, role) on $left.client_ip == $right.ip_addr
| project name, username, role
```

**What is the name of the compromised Development Officer?**

> `Alex Rivera`
> 

```sql
Alex Rivera	Development Officer	alrivera
David Okonkwo	Adoption Coordinator	daokonkwo
Jessica Huang	Adoption Coordinator	jehuang
```

## Question 23

**What role do two of the compromised users share?**

> `Adoption Coordinator`
> 

## Question 24

Look at who got targeted: a Development Officer who handles donor relationships, and Adoption Coordinators who have access to donor and adopter data. This wasn't random. The attacker went after people with access to the donation system.

And Whiskermania, the big fundraiser, is in five days.

**Type `they want the money` to continue.**

> `they want the money`
> 

## Question 25

Now let's think about remediation from a network perspective. To stop this attack, you need to block the C2 communication. There are multiple points where you can do this:

1. **DNS level** - Block the domain lookup so the malware can't resolve the C2 address
2. **Firewall level** - Block the IP address directly
3. **Proxy level** - Block the URL pattern

**What domain should be blocked at DNS?**

> `update-cdn-service.xyz`
> 

## Question 26

**What IP address should be blocked at the firewall?**

> `185.174.137.42`
> 

## Question 27

**What URL path pattern should be blocked at the proxy?**

> `/api/v1`
> 

## Question 28

Let's write detection rules based on what we learned.

```sql
// Detection 1: DNS query to known C2
DnsEvents
| where query_name in ("update-cdn-service.xyz")

// Detection 2: Traffic to C2 IP
NetworkFlow
| where dest_ip == "185.174.137.42"

// Detection 3: Proxy pattern matching
ProxyEvents
| where url matches regex @"/api/v\d+/(status|config)"
| where domain endswith ".xyz"
```

**What table should you query to detect C2 domain lookups?**

> `DnsEvents`
> 

## Question 29

**What table should you query to detect network connections to the C2 IP?**

> `NetworkFlow`
> 

## Question 30

**What table should you query to detect the `/api/v1/status` beaconing pattern?**

> `ProxyEvents`
> 

## Question 31

You've mapped the entire attack chain:

```sql
ATTACK CHAIN (Network View):

Phishing Email ────► Click ────► Malware Install ────► C2 Beacon
     │                              │                      │
     ▼                              ▼                      ▼
   Email               (FileCreation)              DnsEvents
   Table               (ProcessEvents)             NetworkFlow
                                                   ProxyEvents
```

Every step left traces in the network logs. That's how you found it. That's how Jamie found it.

**Type `almost there` to continue.**

> `almost there`
> 

## Question 32

Let's build a final summary of the timeline.

```sql
let c2_domain = "update-cdn-service.xyz";
let compromised_ips = DnsEvents
| where query_name contains c2_domain
| distinct client_ip;
DnsEvents
| where client_ip in (compromised_ips)
| where query_name contains c2_domain
| summarize
    first_beacon = min(timestamp),
    last_beacon = max(timestamp),
    total_queries = count()
  by client_ip
```

This shows when each compromised host started beaconing and how many times they've checked in.

**Approximately how many days has the C2 been active?**

> `5`
> 

![image.png](Whiskermania%20An%20Intro%20to%20Network%20Concepts/image.png)

## Question 33

**CASE SUMMARY: Payment Shadow Campaign**

You've uncovered what Jamie couldn't finish investigating:

**Attack Summary:**

- **Initial Access**: Phishing via `whiskersandwonders-hr.com`
- **Targets**: 3 employees with donation system access
- **C2 Domain**: `update-cdn-service.xyz`
- **C2 IP**: `185.174.137.42`
- **Port**: 443 (HTTPS, blending with normal traffic)
- **Duration**: ~4 days active

**Indicators of Compromise:**

- DNS: `update-cdn-service.xyz`
- IP: `185.174.137.42`
- Proxy: `/api/v1/status`, `/api/v1/config` patterns

**Recommended Actions:**

1. Block C2 domain at DNS
2. Block C2 IP at firewall
3. Block URL patterns at proxy
4. Isolate compromised hosts
5. Reset credentials for affected users

Morgan stares at the screen. "We need to tell someone. The board. Maybe law enforcement."

"Definitely," you say.

"And Jamie…" Morgan pauses. "Should we tell Jamie what you found?"

You look at the post-it notes. The cold coffee. Biscuits, still asleep on the desk, completely unbothered by the crisis you just uncovered.

"I think Jamie already knows. That's why they left us the breadcrumbs."

**Type `thank you jamie` to complete the investigation.**

> `thank you jamie`
>