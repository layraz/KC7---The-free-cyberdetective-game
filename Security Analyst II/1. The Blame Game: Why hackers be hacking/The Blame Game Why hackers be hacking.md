# The Blame Game: Why hackers be hacking

Every cyberattack has a who and a why. Knowing the difference between a nation-state quietly living in your network, and a 22-year-old calling your helpdesk pretending to be IT, changes everything about how you respond, what you prioritize, and how scared you should be. Hint: you should be scared of both.

In this module you'll meet the four threat actor categories security professionals actually use, see why the lines between them blur in practice, and apply the framework to five real groups from 2024 and 2025.

# S1: Meet the Suspects

## Question 1

**Every cyberattack has a "who" and a "why."**

Knowing the who and the why changes everything about how you respond, what you prioritize, and how long you have before things get worse.

In this module you'll learn to tell four types of threat actors apart, understand why the lines between them blur, and practice applying the framework to real groups you'll actually encounter in the field.

By the end, you won't just know the categories. You'll know why they're hard to apply, and what to do with that uncertainty.

Type `let's hunt` to get started.

> `let's hunt`
> 

## Question 2

Before we talk about who's attacking, let's talk about what they want. That question, more than any technical indicator, tells you which type of attacker you're dealing with.

There are four main categories security professionals use to classify threat actors:

- State actors,
- cybercriminals,
- insider threats, and
- hacktivists

Each has a different primary motivation, and that motivation shapes everything from their target selection to how long they're willing to wait before striking.

Later in this module you'll see why these categories get complicated. But you need the foundation first.

**Which of the following is NOT a primary motivation category used to classify threat actors?**

- State actors,
- cybercriminals,
- script kiddie
- hacktivists

## Question 3

"Nation-state" actors are government-sponsored or government-directed hackers. They have resources most criminal groups can't match: dedicated infrastructure, full-time teams, access to zero-day vulnerabilities, and no fear of domestic prosecution. Their government protects them, funds them, and sometimes gives them their target list.

![Fancy Bear Crowdstrike image](https://www.crowdstrike.com/content/dam/crowdstrike/www/en-us/wp/2016/12/fancybear.png)

The classic example is China's APT41, which has conducted economic espionage against pharmaceutical, technology, and defense companies. Or Russia's APT28, known as Fancy Bear, which targeted the 2016 US presidential election. These groups operate with patience measured in months and objectives measured in geopolitical outcomes.

Nation-state actors are also sometimes referred to as Advanced Persistent Threats.

**What does the "P" in APT stand for?**

> `persistent`
> 

## Question 4

Some state actors deliberately disguise their operations to look like financially motivated crime.

They demand ransoms. They steal cryptocurrency. They mimic the tools and tactics of criminal groups so that investigators follow the wrong trail. If responders are chasing a ransomware gang, they're not building a case against a government.

The most documented example of this is North Korea. Facing international sanctions and desperately needing foreign currency, the North Korean government turned to cybercrime as economic policy. The group carrying out these operations is known as Lazarus Group.

![Nort Korea Hackers were responsible for Sony Hack](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/Sony-pictures-hacked-feat-img.webp)

*North Korea state actors were notoriously responsible for hacking Sony prior to the release of the movie "The interview" which would have been embarrassing for their leader*

Lazarus Group has stolen from banks, crypto exchanges, and gaming platforms. They've deployed ransomware. By the numbers, they look like a financially motivated criminal syndicate. But the money goes to the North Korean regime, and the operations are directed by the Reconnaissance General Bureau, a military intelligence unit.

**Lazarus Group is described as state-sponsored. Who sponsors them?**

> `North Korea`
> 

## Question 5

Now let's talk about cybercriminals, and clear up the biggest misconception about them: they are not lone hackers in basements.

Modern ransomware operations look more like logistics companies than underground gangs. There are developers, operators, recruiters, negotiators, and customer service representatives. There are affiliate programs with revenue splits. There are dark web job postings with competitive pay and performance reviews.

![Money moves](https://media.cnn.com/api/v1/images/stellar/prod/200703180629-02-nigerian-man-charged-money-laundering-conspiracy.jpg?q=x_2,y_0,h_899,w_1597,c_crop/h_653,w_1160/f_avif)

[He flaunted private jets and luxury cars on Instagram. Feds used his posts to link him to alleged cyber crimes](https://edition.cnn.com/2020/07/12/us/ray-hushpuppi-alleged-money-laundering-trnd)

The business model that made all of this possible is called Ransomware as a Service, or RaaS. It works exactly like it sounds. Ransomware developers build the malware, maintain the infrastructure, and lease it out to affiliates who do the actual intrusions. Affiliates keep 60 to 80 percent of whatever ransom they collect. The operator takes the rest and never has to touch a victim network.

This separated the people who build weapons from the people who use them, and dramatically lowered the barrier to entry for anyone who wants to run a ransomware campaign but doesn't know how to write malware.

**What does RaaS stand for?**

> `Ransomware as a Service`
> 

## Question 6

A modern ransomware attack does not start with encryption. It ends there.

By the time a victim's files are locked and a ransom note appears on screen, the attacker has typically been in the network for days or weeks. During that time they've mapped the environment, escalated their privileges, identified the most valuable data, and exfiltrated a copy of it. All of that happens before the encryption switch gets flipped.

![Leaksite](https://www.scworld.com/wp-content/uploads/2021/11/image2-1.jpg)

This setup enabled a tactic called double extortion. Pay to get your files back. And if you don't pay, the attacker publishes the stolen data publicly. Some groups have added a third lever: notifying your customers or regulators directly to create additional pressure.

There's also an entire specialty profession called Initial Access Brokers, or IABs. These are hackers who break into corporate networks and then sell that access to other criminals rather than using it themselves. The intrusion and the extortion are often carried out by completely different people.

**What is the tactic called where an attacker both encrypts your data AND threatens to publish it?**

> `double extortion`
> 

## Question 7

One of the most famous ransomware attacks in US history hit Colonial Pipeline in May 2021. A DarkSide affiliate gained access to Colonial's network, moved through it quietly, and deployed ransomware that forced the company to shut down the largest fuel pipeline in the United States.

The shutdown lasted five days. Fuel shortages spread across the southeastern US. People were filling gas cans at every station they could find. The federal government declared a state of emergency.

![Woman fill up gas after ransomware attack](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/woman_fills_up_gas_tank_during_ransomware.jpg)

[Photos show the impact at the pumps from the Colonial Pipeline hack](https://www.cnbc.com/2021/05/12/photos-show-the-impact-at-the-pumps-from-the-colonial-pipeline-hack.html)

DarkSide issued a public statement saying they were apolitical and only motivated by money, and that they would be more careful about affiliate target selection in the future. They shut down shortly after, likely under significant pressure.

**How many days was Colonial Pipeline shut down?**

> `5`
> 

## Question 8

Insider threats aren’t just another checkbox in a security policy. They’re the silent, patient killers of entire networks, the enemy already sitting behind your firewall with a badge and a smile. Anyone with legitimate access can become the breach: employees, ex‑staff whose accounts never got nuked, contractors, vendors — all of them already past the defenses you spent years building.

Some insiders go full rogue, stealing data for cash, sabotaging systems out of spite, or selling access to nation‑states and criminal crews. Others don’t mean harm but still unleash it. One sloppy click, one exposed credential, one misconfigured system, and suddenly you’re reading headlines about another “unforeseen” catastrophe.

This isn’t just theory; it’s the same playbook behind [engineers walking out of Google with AI models](https://thehackernews.com/2026/01/ex-google-engineer-convicted-for.html) and [the fake “dream job”](https://www.theblock.co/post/156038/how-a-fake-job-offer-took-down-the-worlds-most-popular-crypto-game) lure that helped North Korea crack Axie Infinity wide open.

With foreign intelligence sliding into LinkedIn DMs like it’s a recruitment marketplace — [MI5 literally had to launch a national campaign to warn people](https://www.youtube.com/watch?v=WwUxMZ-CGzI) — the line between outsider and insider has evaporated. The most dangerous threat in the building is the one you never see coming until the blast radius is already glowing.

![Think Before You Link - Glitch - YouTube](https://img.youtube.com/vi/WwUxMZ-CGzI/0.jpg)

Stay sharp. Stay suspicious. In this world, your access is a weapon, and someone out there is hunting for it.

- quiet and flashy
- negligent and malicious
- insider and outsider
- negligent and suspicious

## Question 9

Hacktivists hack for a cause. The cause varies. It might be political, ideological, environmental, or social, but there is always a message behind the action. Where a criminal wants money and a nation-state wants intelligence or leverage, a hacktivist wants attention.

The most recognizable hacktivist collective is **Anonymous**, a decentralized group with no leadership, no membership requirements, and no financial motive. Anyone can claim to be Anonymous. Operations are organized around shared targets and posted publicly. Anonymous has targeted governments, corporations, and organizations across the ideological spectrum, driven by a general commitment to free speech and opposition to censorship.

When Russia invaded Ukraine in 2022, Anonymous declared a cyber war on Russia and attacked Russian state media, government websites, and companies that continued operating in the country.

**Anonymous is described as decentralized. What does that mean in this context?**

# S2: Know Your Enemy

## Question 1

You just learned four attacker categories. Now here is the part the textbooks leave out: the categories blur.

Some of the most dangerous threat actors in the world don't fit cleanly into one bucket. They have multiple motivations, shift tactics depending on the target, or deliberately obscure what they're really after.

KillNet is a good example. They emerged in early 2022 at the start of Russia's invasion of Ukraine. They described themselves as patriotic volunteers defending Russian interests through DDoS attacks against NATO countries, their governments, hospitals, and airports. They denied any ties to the Kremlin.

Their operations aligned precisely with Russian strategic interests. They targeted exactly the countries and organizations supporting Ukraine. They maintained a military-like organizational structure with a hierarchy, subgroups, and a Telegram channel with tens of thousands of subscribers.

KillNet looks like hacktivists. But their behavior raises a question.

**Based on the description above, what is the most accurate label for KillNet?**

- State actors
- Ransomware
- Insider threats
- Hacktivists

## Question 2

On February 9, 2018, as the Winter Olympics opened in Pyeongchang, South Korea, attackers quietly tore through the Games' IT infrastructure. WiFi cut out, the official app stopped working, the press center lost access, and TV broadcasts went dark — all in the middle of the opening ceremony. Investigators would later call the malware behind it Olympic Destroyer.

When security researchers began pulling the code apart, they found what looked like fingerprints from Lazarus Group, the well-known North Korean state hacking collective, alongside snippets that resembled the work of a Chinese threat actor. Different firms reached different conclusions, and for a while, attribution was genuinely unclear.

It was eventually traced back to Sandworm, a unit of Russian military intelligence. The North Korean and Chinese code fragments hadn't appeared by accident — they were planted deliberately, designed to send investigators chasing the wrong countries. Russia's motive wasn't hard to understand: South Korea had just banned Russian athletes from competing over the state doping scandal, and the opening ceremony made for a very public target.

**What is the term for planting misleading evidence in malware to make it appear to come from a different attacker?**

> `false flag`
> 

![image.png](The%20Blame%20Game%20Why%20hackers%20be%20hacking/image.png)

## Question 3

False flags are not just a technical trick. They are a strategic choice.

When a nation-state plants evidence pointing to a criminal group, investigators spend their time hunting cybercriminals. When they point to a rival nation-state, they create diplomatic friction between two countries that had nothing to do with the attack. When they demand ransoms and steal crypto, they generate revenue and cover at the same time.

From the defender's perspective, the practical lesson is not that attribution is impossible. It is that attribution is hard, requires evidence from multiple independent sources, and should be held with appropriate confidence levels. A single technical indicator pointing at a group is not a confident attribution.

Your job in most security roles is not to name the attacker. It is to understand what they were after, stop the bleeding, and build defenses that hold regardless of who sent the attack.

**True or false: a defender's primary job during an active incident is to determine which nation-state is responsible.**

> `False`
> 

# S3: The Lineup

## Question 1

Now you're going to apply the framework to five real groups.

For each one you'll get a short profile and pick the best-fit category. Some are clean. Some aren't.

Starting with a clean one.

**Volt Typhoon** is a Chinese state-sponsored group first named and tracked by Microsoft. Since at least 2021 they have quietly compromised US critical infrastructure — water systems, energy grids, transportation networks, communications — and done almost nothing after getting in.

They use only built-in Windows tools so they leave minimal traces, a technique called "living off the land." CISA and the FBI confirmed in 2024 that Volt Typhoon maintained access inside some victim environments for five years before being detected. The joint US-UK-Australian intelligence advisory assessed with high confidence that they are pre-positioning for sabotage in the event of a military conflict with the United States, likely over Taiwan.

FBI Director Christopher Wray called Volt Typhoon "the defining threat of our generation" in 2024 congressional testimony.

**Which attacker category best describes Volt Typhoon?**

- State actors
- Ransomware
- Insider threats
- Hacktivists

## Question 2

**Salt Typhoon** is a Chinese state-sponsored group attributed by CISA to China's Ministry of State Security and named by Microsoft.

In 2023 and 2024, Salt Typhoon compromised at least nine US telecommunications providers, including AT&T, Verizon, and T-Mobile. They didn't go after customer credit cards or passwords. They went after the wiretapping infrastructure that telecoms use to comply with court-ordered law enforcement surveillance requests.

From there they accessed call metadata on over a million users, mostly in the Washington DC metro area, including staff from both 2024 presidential campaigns and the personal phones of Donald Trump and JD Vance. The US Senate Intelligence Committee chair called it "the worst telecom hack in our nation's history."

The US Treasury sanctioned a linked Chinese company in January 2025.

**Which attacker category best describes Salt Typhoon?**

- State actors
- Ransomware
- Insider threats
- Hacktivists

## Question 3

**Scattered Spider** (tracked by CrowdStrike; Microsoft calls them Octo Tempest) is a loosely organized group of English-speaking young people, mostly 19–25 years old, based in the US and UK. They don't write novel malware. They call helpdesks.

They impersonate employees. They SIM-swap phones to defeat MFA. They bomb users with authentication prompts until they give in. Once inside, they deploy ransomware from RaaS partners and demand payment.

In September 2023 they hit MGM Resorts by calling the IT helpdesk, impersonating an employee, and getting credentials reset. MGM lost over $100 million. Caesars Entertainment paid them a $15 million ransom the same month. In 2025 they were linked to an attack on Marks & Spencer that took down payment systems across 1,000+ UK stores for weeks. CrowdStrike and Microsoft were both called in to respond.

Five members were indicted by the DOJ in November 2024.

**Which attacker category best describes Scattered Spider?**

- State actors
- Ransomware
- Insider threats
- Hacktivists

## Question 4

This one is hard. Think carefully.

**FAMOUS CHOLLIMA** is a North Korean state-sponsored group tracked by CrowdStrike.

Their operation works like this: North Korean operatives use AI-generated or stolen identities, deepfake video technology, and forged documents to get hired as remote IT workers at Western technology companies. Once employed, they do just enough of the job to keep getting paid.

They also install remote access tools, exfiltrate sensitive data, and funnel their salaries back to Pyongyang to fund North Korea's weapons programs. CrowdStrike responded to 304 incidents in 2024 alone — nearly one per day — a 220% year-over-year increase. Microsoft tracks related activity under the name Jasper Sleet.

In 2024, a Nashville man was indicted for running a "laptop farm": he received laptops shipped by companies that had just hired what they thought were US-based developers, forwarded those laptops to North Korean operators, and handled the money transfers. The DOJ has indicted participants across multiple countries.

**Which attacker categories best describes FAMOUS CHOLLIMA?**

- State actors
- Ransomware
- Insider threats
- Hacktivists

## Question 5

Last one. This one is clean, but it illustrates something worth paying attention to.

**SiegedSec** is a collective that was active from 2022 to 2024 and documented in CrowdStrike's threat landscape reporting. They explicitly targeted US state government websites over domestic policy, specifically anti-transgender legislation.

They leaked government documents, defaced websites, and claimed attacks against government systems in multiple states. Their operations were accompanied by detailed public statements explaining their ideological motivation. They had no financial demands and no confirmed government sponsor. The collective announced it was disbanding in 2024.

Around the same time, **GhostSec** — originally formed to target ISIS infrastructure — pivoted to pro-Palestinian operations after October 2023 and claimed attacks against Israeli industrial control systems.

Both groups operated with clear ideological motivations, no financial motive, and no confirmed state backing.

**Which attacker category best describes SiegedSec?**

- State actors
- Ransomware
- Insider threats
- Hacktivists

## Question 6

You made it.

Here is what you know now that you didn't before. There are four threat actor categories: **nation-state actors**, **cybercriminals**, **insider threats**, and **hacktivists**. Each has a primary motivation that shapes their target selection, their patience, and their methods.

The categories blur in practice. Nation-states use criminal tools and demand ransoms. Hacktivists sometimes act as state proxies. Criminals get recruited by intelligence services. Insiders get manufactured wholesale by foreign intelligence agencies.

FAMOUS CHOLLIMA is still running. Scattered Spider members keep resurfacing. Volt Typhoon rebuilt its botnet within weeks of being disrupted.

Attribution is hard, sometimes deliberately so. Your job during an active incident is not to name the attacker. It is to stop the attack, understand what they were after, and build better defenses.

The groups you met in this module are not theoretical. Volt Typhoon, Salt Typhoon, Scattered Spider, FAMOUS CHOLLIMA, and SiegedSec are all active or recently documented groups with real victims and real reporting from Microsoft, CrowdStrike, and Mandiant. You will encounter their successors.

**Type `on it` to close out.**

> `on it`
>