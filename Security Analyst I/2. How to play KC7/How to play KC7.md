# How to play KC7

# Section 1: What is KC7?

## Question 1

**Welcome to KC7!**

You're about to learn the most important skill for being a cybersecurity analyst, which is…well..er… analysis.

Like many worthwhile skills, analysis can be taught, but you can only get better at it by practicing. Hence this platform is a little different than most. It's all about practice.

![practice meme](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/blog_101_practicememe.jpeg)

KC7 games put you in the shoes of a security analyst and hand you a real problem to solve. Someone got hacked. Something went wrong. Your job is to figure out what happened.

You'll examine logs, trace attacker activity, and piece together the story. By the end, you'll understand the attack from start to finish. Then, you will do it all again until you get better at it. And if you follow our learning path, you WILL get better at it, so much so that others will be shocked.

## Question 2

**KC7 is not a game about KQL.**

Yes, you'll write queries. You'll learn the syntax. But the queries are just a tool, like a flashlight in a dark room. The real skill is knowing where to point it.

**KC7 is a game about security investigations**

In KC7, you'll learn to think like an analyst: following leads, connecting dots, recognizing patterns, and reconstructing events from fragments of evidence.

![kc7 slap meme](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/kql_kc7_meme.jpg)

This is harder than learning syntax. It's also far more valuable.

KQL syntax you can look up. Investigative intuition you cannot. The queries will come naturally with practice. The mindset is what you're really here to build.

## Question 3

**You are discovering a story, not being told one.**

Every KC7 game contains a complete attack hidden in the data. Someone did reconnaissance. Someone sent phishing emails. Someone clicked. Malware ran. Data was stolen. The whole thing happened, and the evidence is sitting in the logs.

The questions guide your attention, but you have to work to unearth what happened from the data.

The best moments in KC7 are when something clicks and you think: *oh! that's how that works!*

## Question 4

**The only way to get better at investigations is to practice.**

There is no shortcut. Although they can help, you cannot read a book or watch a video and suddenly become a skilled investigator. Investigative skill is built through repetition, through pattern recognition, through making mistakes and learning from them.

That's why we built KC7 as a series of modules. Each one is a new company, a new scenario, a new attacker, a new puzzle. Some are short. Some are long. Some are playful. Some are serious. Each one teaches you something different.

The more you play, the sharper you get.

If you finish a game and want more, there are more. If you struggle with one game and need something easier, there are easier ones. The whole point is to give you enough practice that the skills become second nature.

# Section 2: How to Play?

## Question 1

**The interface is straightforward.**

On the right side of your screen, you'll see a query pane. That's where you write KQL queries and run them against the data.

**You can write multiple KQL queries in the same query pane**, separated by blank lines. Run a query, check the results, then change it or try another one in the same pane. There’s no need to open multiple query panes or bounce back and forth, staying in one pane makes it much easier to compare results, tweak logic, and keep your investigation organized.

On the left, you'll see the questions. Each one guides you toward something specific in the data. Sometimes the question gives you a query to run. Sometimes it gives you a hint. Sometimes it just points you in a direction and lets you figure out the rest.

![Screenshot of KC7 interface with query pane and questions labeled](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/interface_kc7.png)

When you run a query, results appear below. You can scroll through them, click on individual cells to see full values (some of them get truncated), and copy data you need for later.

It takes about five minutes to feel comfortable. After that, you'll forget about the interface entirely and focus on the investigation.

## Question 2

**Don't be afraid to guess.**

There are no penalties for wrong answers in KC7. You will not lose points for guessing wrong. You will not be judged. You will not break anything. Your favorite football team will not start to suck (unless they already do, then we can't help you). The game just tells you whether your answer was right or wrong, and you move on.

Stuck between two possible answers? Try one. If it's wrong, try the other. Not sure if you have the timestamp in the right format? Submit it and see what happens. Think you might have the right IP address but you're not certain? Just guess.

Real investigations involve uncertainty. You form hypotheses and test them. You think something might be true and you check. KC7 works the same way. Guessing is not cheating. It's how investigations actually work.

## Question 3

**Take notes. Take notes. Take notes.**

You are working an investigation, not answering questions in a personality test. The questions are connected. They build on each other. Something you discover in question 5 might become critical in question 45.

We will refer back to earlier findings (because we are evil). An IP address you found in Section 1 will reappear in Section 3. A timestamp from question 12 will matter in question 38. A username, a domain, a file hash. These things come back.

![take notes](https://i.pinimg.com/originals/3b/06/79/3b0679d6f27b7bef7335b45b817336a7.gif)

If you didn't write them down, you'll find yourself scrolling through old questions, re-running queries, trying to remember what you found twenty minutes ago. This is frustrating. It wastes time. It breaks your flow.

Keep a document open. When you find something important, write it down: IPs, domains, timestamps, hostnames, filenames, email addresses, usernames. Write down how you found it, and why it was important. Copy and paste data over from the ADX pane. You will thank us later.

## Question 4

**Pivot, pivot, pivot.**

This is the core mechanic of KC7, and of real-world investigations. One piece of information leads to another. You pull a thread and see where it goes.

![pivot](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/pivot.jpeg)

Here's what it looks like in practice:

- You find a suspicious email. The email contains a link.
- The link has a domain. You look up the domain in DNS records.
- The domain resolves to an IP. You search for that IP in network logs.
- The network logs show someone visited it. You identify who.
- That person's machine has new files. You find malware.

Each answer unlocks the next question. You're not answering questons in isolation. Each pivot gets you closer to understanding what happened.

## Question 5

**Read the questions carefully. Like.. ACTUALLY READ**

This might sound obvious, but it matters more than you think.

The questions are not just asking for data. They're guiding your attention. Often, the way a question is phrased contains a hint about where to look or what to notice.

We write the questions carefully. They're not tricks, but they are precise. The exact wording usually matters.

When you're stuck, re-read the question. Sometimes the answer to 'where do I look?' is hiding in plain sight.

## Question 6

**Time matters. Timestamps, that is.**

Pay attention to timestamps. The order of events matters.

When did the phishing email arrive? When did someone click the link? How long after the click did the malware appear? When did the attacker first connect to the machine? What did they do in the minutes and hours that followed?

When you find a timestamp, write it down. When you're confused about what happened, sort by time. The sequence almost always clarifies things.

# Section 3: Getting Unstuck

## Question 1

**When you get stuck: Hints**

Every question has a hint available. Look for the small circle with a question mark icon, usually to the right of the question.

![hint](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/hint.png)

Hints are quick nudges. They don't give away the answer, but they point you in the right direction. A hint might tell you which table to look in, or remind you of something you found earlier, or suggest a different approach.

Use hints freely. They're not cheating. They're part of the game. We put them there because we want you to succeed.

If you've been stuck on a question for more than a few minutes, check the hint.

## Question 2

**When you get stuck: Analyst Notes**

Some questions include Analyst Notes, which are longer explanations that go deeper than hints.

Where hints give you a quick nudge, analyst notes explain the thinking behind the question. Why are we asking this? What concept are we trying to teach? What would a real analyst be thinking about at this point in an investigation?

Analyst notes try to help you understand without simply handing you the answer. They're like having a mentor look over your shoulder and say 'here's how I'd think about this.'

Not every question has analyst notes. But when they're available, they're worth reading. Especially if the hint didn't get you unstuck.

## Question 3

**When you get stuck: The Community**

Sometimes hints and analyst notes aren't enough. Maybe you're misunderstanding something fundamental. Maybe there's a concept you haven't encountered before. Maybe you just need a human to talk to.

That's what the Discord is for: **kc7cyber.com/community**

We have an active community of players and volunteers who are happy to help. But you need to help us help you. When you ask a question, make it easy for someone to assist.

1. **Go to the right channel.** Each game has its own channel. Don't post Valdorian Times questions in the general chat.
2. **State your question clearly.** 'Section 2, Question 7' is helpful. 'I'm stuck' is not.
3. **Give context.** What is the question asking? We don't have time to load up the game and find it ourselves.
4. **Share what you tried.** What queries did you run? What results did you get? What do you think the answer might be?
5. **Ask something specific.** 'What am I missing?' is better than 'help.'

And please don't post spoilers. Don't give away answers. The person after you deserves the same chance to discover things for themselves.

# Section 4: The Investigative Mindset

## Question 1

**Our logs are fake but the learning is real**

The data in KC7 games uses schemas we designed ourselves. The tables, the column names, the way information is organized. We made it up.

This means the logs aren't identical to any real-world product. They don't look exactly like Splunk, or Microsoft Sentinel, or CrowdStrike, or any other tool you might encounter professionally.

But the patterns are the same.

Every security tool organizes this information slightly differently, but the underlying concepts don't change. If you can investigate in KC7, you can learn any real-world tool. The mental model transfers.

## Question 2

**You will run into dead ends. Learn to recognize them**

Not every lead goes somewhere. This is true in KC7, and it's true in real life.

Sometimes you investigate a machine and find nothing interesting. Sometimes a suspicious-looking domain turns out to be completely benign. Sometimes you spend twenty minutes chasing something that doesn't matter.

This is not failure. This is investigation.

Eliminating possibilities is progress. Knowing that the attacker didn't do something is valuable information. Confirming that a machine wasn't compromised lets you focus elsewhere.

When you hit a wall, step back. What else do you know? What haven't you checked yet? Is there another angle?

The answer is somewhere in the data. If one path is blocked, find another.

## Question 3

**What you learn here transfers.**

KC7 teaches two kinds of things.

The first kind is implicit skill. This is the investigative intuition you build through practice. How to follow a lead. When to pivot. What questions to ask next. How to recognize patterns. This isn't something we teach directly. It's something you develop by doing investigations over and over until the thinking becomes natural.

The second kind is explicit knowledge. These are specific concepts, tools, and terminology that come up during investigations. What phishing looks like. How malware establishes persistence. What attackers do after they get access. What terms like 'lateral movement' and 'exfiltration' and 'C2' actually mean in practice.

Both kinds of learning transfer to the real world. The implicit skills make you a better investigator regardless of what tools you use. The explicit knowledge gives you vocabulary and concepts that the security industry expects you to know.

# Section 5: Join Us

## Question 1

**KC7 is made by volunteers. <3**

There's no company behind KC7. No investors. Just a group of people who care about cybersecurity education and think these skills should be accessible to everyone.

We build these games in our spare time. We maintain the platform. We answer questions on Discord. We do it because we've seen how hard it is to break into this field, and we want to make it easier.

If KC7 has helped you learn something, or if you've had fun, or if you've gotten a job or passed an interview or just felt a little more confident, that's why we do this.

## Question 2

**Help us help others.**

If KC7 has been valuable to you, here's how you can give back.

**Spread the word.** Tell your friends, your classmates, your colleagues. Share it on social media. Mention it when people ask how to get into security. The more people who know about KC7, the more people we can help.

**Leave a review.** Wherever you found us, [reviews help other people discover KC7.](https://kc7cyber.com/testimonials)

**Follow us on Instagram!** [Instagram](https://www.instagram.com/kc7cyber/)

**Join the community.** [Come hang out on Discord](http://kc7cyber.com/community). When you've gotten good at this, stick around and help the people who are just starting out. Answer their questions. Nudge them in the right direction.

## Question 3

**You're ready.**

You know what KC7 is: a game about security investigations, not syntax memorization.

You know how to play: guess freely, take notes, pivot from clue to clue, follow the timeline, read the questions carefully.

You know what to do when you're stuck: check hints, read analyst notes, ask the community for help.

You know what you're building: investigative intuition that transfers to the real world, plus concrete security knowledge you'll use throughout your career.

[Now go pick a module and start investigating.](https://kc7cyber.com/vault)

**Enter `let's hunt` to finish**

> `let's hunt`
>