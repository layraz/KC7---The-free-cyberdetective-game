# Decoding 101: A beginner’s guide to decoding

# About

A threat actor is hiding in plain sight, and their commands look like complete nonsense. Garbled strings, machine code, and scrambled text are everywhere in your logs. The question isn't whether something malicious is happening, it's whether you can read it.

In **Decoding101**, you learn to translate the "gibberish" that attackers use to slip past defenses. From basic binary to layered multi-step encodings, you'll train your eye to recognize obfuscation patterns and your hands to peel them apart using **CyberChef** and **dcode.fr**. No coding required. Just curiosity, pattern recognition, and a growing tolerance for math.

By the end, you won't be scared of an encoded payload. You'll be the one doing the decoding.

## What You’ll Learn

1. **Recognize common encodings at a glance** including binary, decimal, hexadecimal, and Base64, and know which tool operation to reach for when you spot them.
2. **Use CyberChef confidently** to build, chain, and auto-detect decoding recipes, including using the Magic operation when the encoding isn't obvious.
3. **Decode basic transformations** such as reversed strings, ROT13 shifts, and XOR-encrypted data, including brute-forcing when a key is unknown.
4. **Unravel multi-layered encodings** where threat actors stack two or three transformations together to make detection harder.
5. **Analyze obfuscated scripts** and decode their payloads without running them, a critical skill for safe malware triage.
6. **Use dcode.fr as a fallback** when CyberChef's magic fails, and navigate cipher identifier results with appropriate skepticism.
7. **Tackle CTF-style cipher challenges** involving visual, audio, and symbol-based ciphers, the kind you'll encounter in competitions and occasionally in the wild.

# Section 1: Baby Steps

## Question 0

Welcome to Decoding101, the module in which we'll learn to turn what looks like gibberish into perfectly normal words 🤓

Why is this skill useful?

- Data can be formatted in a way that is easier to understand by machines, and way less by humans. Knowing how those formats look like will save you lots of headaches when trying to convert it back to something you can read.
- Threat actors will try to bypass a network's defenses by encoding their malicious commands, making them harder to detect. Being able to recognise those encodings will help you find them and translate them to understand what they're doing during your investigation.
- CTFs often have challenges in which you must decode/decrypt a hidden message. You might not always be able to recognise the encoding/cyber used by yourself, but knowing which tools can help in those tasks will save you from throwing your computer out the window (money saved!).

Before you start worrying, no, you do not need to have any coding skills. There are handy tools that will do all the "translation" work for you, and even the "recognition" work. The most popular is CyberChef, a free web app that you will learn to use here.

Let's dive in, shall we? 🤿

**Type `future secret master` to start your learning journey!**

> `future secret master`
> 

## Question 1

First, a primer on [CyberChef](https://gchq.github.io/CyberChef/).

It's called " the Cyber Swiss Army Knife" because it offers a wide range of capabilities. It might seem a bit overwhelming at first, and some options you might never use yourself.

Here is a look at the interface:

![screenshot of the CyberChef interface with numbers from 1 to 5 for each section, explained below](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/D101S1Q1.png)

1. On the left-hand side, you have the menu. They're all categories of "operations" to perform. If you click on any of them, it will expand with all the options in that category. Note that the category *Favourites* contains the default most used operations, and you can edit it as you see fit.
2. The search bar, just above the menu, to find the operation you want more quickly. If you know what it is called.
3. In the middle is the "recipe" panel. A recipe is one or more operations you want to perform on the data. You can see at the bottom that performing the operation is to "bake." A chef indeed 🧑‍🍳
4. The top-right panel is where you will write/paste the data you want transformed. The "ingredients", if you will.
5. The bottom-right panel will show the result of the recipe once it's done baking. The cake I guess 🧁

Each operation has an explanation attached to it and most of them also contain a link to the related wikipedia article. Try it yourself: click on the *Favourites* menu, then hover over the operations.

**Which symbol is used as an example in the "Url Decode" operation?**

> `=`
> 

```sql
Converts URI/URL percent-encoded characters back to their raw values.

e.g. %3d becomes =
```

## Question 2

Nothing better than a real example to understand how a tool works. So let's explore the first data format of this module:

🤖 the binary format 🤖

Binary is the most basic computer language, made up of 1 and 0 only. It is very easy to recognise.

Every character and number and symbol you can see on your keyboard (or not) has a binary representation. You can easily find conversion tables online.

But let's get back to CyberChef.

In the search field, type "binary" to get all the relevant results.

You would use `to binary` to convert a sentence into binary. Try it. You can either double-click on the operation or drag it into the recipe field. Then in the input field, type your name. CyberChef is by default set to "autobake" and the result will appear in the bottom panel without you having to click on `bake`.

![screenshot of the to binary recipe with kellamity as example](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/D101S1Q2.png)

You can see by the length of the result that each letter of your name is equal to a set of eight 0 and 1. That is called a byte.

Click on the bin icon in both the recipe and input panels to clear everything. Now get

```
from binary
```

into the recipe panel. Copy and paste this message into the input panel:

`01100010 01100101 01100101 01110000 00100000 01100010 01101111 01101111 01110000 00100000 01110100 01101000 01101001 01110011 00100000 01101001 01110011 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00101110`

**What does this binary string say?**

> `beep boop this is binary.`
> 

## Question 3

The decimal data format is a shorter representation of the binary format. The number is a result of the addition of the 1s in the binary. It is between 1 and 3 digits long.

Let's do a very short intro to binary math with 4-bits examples. Binary is read from right to left, where the rightmost position = 1, and every position after is doubled.

```sql
0  0  0  0  // binary
8  4  2  1  // position
```

To figure out the number it represents, you need to add the "positions" together.

```sql
// This example = 1
0  0  0  1 // only the first position is toggled "on"
8  4  2  1 // therefore only 1 counts

// This example = 5
0  1  0  1 // the first and third positions are toggled "on"
8  4  2  1 // 1 + 4 = 5

// This example = 15
1  1  1  1 // all positions are toggled "on"
8  4  2  1 // 1 + 2 + 4 + 8 = 15

```

Remember that the binary format uses a full byte, or 8 bits, which is double the length of our examples here. But it works the same, and the 1, 5 and 15 we got here could be decimal data. If you want to have fun, you could try doing the math with the first letter of your name, since you checked out its binary format in the previous question, and then verify you got it right by doing `To Decimal` in CyberChef on that letter.

Otherwise, use `From Decimal` and check out the following message:

`73 32 104 97 116 101 32 109 97 116 104 115 32 232 45 233`

**What does the decimal encoded string says?**

> `I hate maths è-é`
> 

## Question 4

Hexadecimal is another shorter way of representing binary data. And it is easily recognisable because it is made up of the numbers 0-9 and letters A-F.

If you've ever dabbled in html/css or digital art, you know colours have an hexadecimal code. For example, the orange used by KC7 has the code FF5700.

For a more relevant cybersecurity example, if you were to look at network traffic using Wireshark, you would see the data represented in hexadecimal (the bottom panel in the picture below).

![picture of Wireshark main window as seen in their official documentation](https://www.wireshark.org/docs/wsug_html_chunked/images/ws-main.png)

Let's link hexadecimal back to binary and decimal. A byte (8 bits) of binary data is represented by only two characters in hex, each one the equivalent of a set of 4 bits. Each half works like decimal would, as in it adds the positions of the 1s together. But because it has to be only one character, hex cannot go beyond 9 (while decimal could), and 10 to 15 are represented by A to F.

One more math example to make it clearer, but this time with 8 bits:

```sql
1  0  1  0  0  0  1  1 // binary
128  64  32  16  8  4  2  1 // decimal: 1 + 2 + 32 + 128 = 163
8  4  2  1  8  4  2  1 // hexa: 8 + 2 = A and 2 + 1 = 3 => A3
```

In the search field of CyberChef, type "hex" to get the hexadecimal operations. You can try out `To Hex` with your name to see what it would look like in hexadecimal.

Otherwise, put `From Hex` in the recipe panel, and this string in the input panel:

`4f 68 20 6e 6f 21 20 59 6f 75 27 76 65 20 62 65 65 6e 20 68 65 78 65 64 21`

**What is the decoded message?**

> `Oh no! You've been hexed!`
> 

## Question 5

Have you noticed the small wand next to the word "Output" in CyberChef?

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image.png)

It means it has detected the encoding, and you don't even need to come up with a recipe yourself!

You can hover over the wand to see what CyberChef thinks the recipe is and its result. Or you can click on it and the recipe panel will fill with the detected recipe and the decoded message will appear in the output panel.

Try it with the following secret:

`00110101 00110000 00100000 00110111 00110101 00100000 00110111 00110010 00100000 00110110 00110101 00100000 00110010 00110000 00100000 00110110 01100100 00100000 00110110 00110001 00100000 00110110 00110111 00100000 00110110 00111001 00100000 00110110 00110011 00100000 00110010 00110001`

**What recipe was used?** (give the answer in the same order that CyberChef did, without comma in between)

> `from binary from hex`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%201.png)

# Section 2: Adult Steps

## Question 1

Now that you've seen the basic machine data formats, let's dive into more complex formats and other data transformations. Those are often used by threat actors to obfuscate their real intentions. Sometimes even in combination!

While playing KC7, you'll stumble upon such instances, so it's a good thing to be able to recognise them.

We will start with an easy transformation: reversing a string.

It's like if you were reading the sentence in a mirror.

In CyberChef, look for the `Reverse` operation.

**What does `rorrim weivraer` spell out?**

> `rearview mirror`
> 

## Question 2

A common, but slightly harder transformation is ROT13.

Imagine all the characters were on a wheel. Then rotate that wheel 13 times, and you'll get the new character.

For example, KC7cyber would become XP0plore.

Try it yourself. Search ROT13 on CyberChef and add it to the recipe. Don't get the same result? Notice how there are checkboxes in the recipe? Sometimes, you have to take those specificities into account. Here our string, "KC7cyber", has both capital letters and numbers in it. We also want to rotate them, so you should check those boxes.

![checkboxes](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/D101S2Q2.png)

Check the result again. Better, right?

13 is the default. Which means, you can turn it more or less. When you're not sure how many times the wheel was turned, you can use `ROT13 Brute Force`. CyberChef will then compute a small number of attempts.

Try it yourself with this string: `Gwc axqv um zqopb zwcvl jijg.`

**CyberChef prints out the amount of rotations it did by default. How many times did it do it here?**

<aside>
💡

Note that CyberChef rotates in the same direction in ROT13 and ROT13 Brute Force. Which means the number of times it rotated to land on the cleartext string is *not* the same as the number of times it was rotated to obfuscate it.

</aside>

Try using [this page](https://www.dcode.fr/rot-cipher) (more on dcode.fr later): put the string in the decoder panel and click on `Decrypt`. On the left-hand side, you get all the results. The top one is the right one, see what it says? Only +8, which is the number of rotations used.

> `18`
> 

<aside>
💡

Encoding Shift (8) + Decoding Shift (18) = Total letters (26) 

• **dcode.fr** tells you the original encryption key used to obfuscate the text (**+8**).
• **CyberChef's Brute Force** tells you how many times *it* had to rotate forward to crack it and land on the cleartext (**18**).

</aside>

![Cyberchef output](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%202.png)

Cyberchef output

![[dcode.fr](http://dcode.fr) output](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%203.png)

[dcode.fr](http://dcode.fr) output

## Question 3

Base64 is just one of the encoding in the Base family, but it is the most widely used.

It uses 64 characters: A-Z, a-z, 0-9, and + / =.

You will in fact often recognize it thanks to the = at the end of the encoded strings, though it is only used for padding. Put simply, padding is used when a byte (8 bits) contains empty space, which needs to be filled for the encoding to work.

Here is what such a string would look like: `SSdtIGFsbCBhYm91dCB0aGF0IGJhc2UsICdib3V0IHRoYXQgYmFzZQ==`

Load the `From Base64` recipe into CyberChef.

**What is the decoded version of that string?**

> `I'm all about that base, 'bout that base`
> 

## Question 4

XOR is evil. XOR is math. Like, real math. If you look at the Wikipedia entry you will cry.

XOR is short for "exclusive or". Basically, it only returns true when both sides are different.

It'll be easier to understand with a small table, and because it's computer stuff, back to 1s and 0s.

0 xor 0 = False

0 xor 1 = True

1 xor 0 = True

1 xor 1 = False

Yeah. I know.

To encrypt something with XOR, you need to give it a key. Bring up the `XOR` operation in CyberChef. You see there are a few options. You can decide the formatting of the key and the Scheme. For the following example, the key is in HEX: BEEF, and the scheme is standard. Complete the [following recipe](https://gchq.github.io/CyberChef/#recipe=XOR(%7B'option':'Hex','string':''%7D,'Standard',false)&input=6ofXnJ6Gzc/moOyM253Hz8bBxg&oeol=NEL).

**What does it say?**

> `This is XORcery x.x`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%204.png)

## Question 5

The problem with a XOR encoded string is that you do not necessarily have the key. Sometimes you can find it hardcoded in a script, but sometimes you don't have any other choice than to brute force.

Thankfully, CyberChef also has a `XOR Brute Force` operation. For the sake of this exercise, you don't have to change anything in the default recipe.

Here is the string to brute force: `W>}s{>wp>rwu{>>il{}uwpy>|rr0`.

A lot of results appear, it's all the attempts made by CyberChef, with the key tested at the beginning of the line. You'll have to find the result that is a human readable sentence. I made sure you didn't have to scroll too far down, don't worry.

**What is the key used here?**

> `1e`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%205.png)

# Section 3: Running

## Question 1

Let's put into practice all you've learned so far by solving some challenges!

`36 38 20 31 31 31 20 31 31 37 20 39 38 20 31 30 38 20 31 30 31 20 33 32 20 31 30 31 20 31 31 30 20 39 39 20 31 31 31 20 31 30 30 20 31 30 35 20 31 31 30 20 31 30 33 20 33 32 20 31 30 30 20 31 31 31 20 31 31 30 20 33 39 20 31 31 36 20 33 32 20 31 31 35 20 39 39 20 39 37 20 31 31 34 20 31 30 31 20 33 32 20 31 30 39 20 31 30 31 20 34 36`

**First, can you recognise this encoding?**

> `hex`
> 

## Question 2

**What did the previous string decode to?**

> `Double encoding don't scare me.`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%206.png)

## Question 3

`MDEwMDExMTAgMDExMDExMTEgMDExMTAxMDAgMDAxMDAwMDAgMDExMDAxMDEgMDExMTAxMTAgMDExMDAxMDEgMDExMDExMTAgMDAxMDAwMDAgMDExMTAxMTEgMDExMDEwMDAgMDExMDAxMDEgMDExMDExMTAgMDAxMDAwMDAgMDExMTEwMDEgMDExMDExMTEgMDExMTAxMDEgMDAxMDAwMDAgMDExMTAxMDAgMDExMDEwMDAgMDExMTAwMTAgMDExMDExMTEgMDExMTAxMTEgMDAxMDAwMDAgMDEwMDAwMTAgMDAxMTAxMTAgMDAxMTAxMDAgMDAxMDAwMDAgMDExMDEwMDEgMDExMDExMTAgMDAxMDAwMDAgMDExMTAxMDAgMDExMDEwMDAgMDExMDAxMDEgMDAxMDAwMDAgMDExMDExMDEgMDExMDEwMDEgMDExMTEwMDAgMDAxMDAwMDE=`

**What about this one?**

> `Not even when you throw B64 in the mix!`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%207.png)

## Question 4

The previous string looked very repetitive before decoding, didn't it?

Noticing this kind of pattern and remembering them will help you figure out decoding faster.

**Which repeating segment was equal to `01` ?**

> `MDE`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%208.png)

## Question 5

You're good! Let's ramp it up 😈 The magic wand won't save you now!

`3d 3d 77 50 6c 31 47 49 79 39 6d 5a 67 49 58 5a 6b 4a 58 59 6f 42 79 5a 75 6c 47 61 30 56 57 62 76 4e 48 49 6c 5a 58 59 6f 42 53 64 76 6c 48 49 30 64 69 62 76 52 45 49 2f 55 47 62 77 6c 6d 63 30 42 53 51`

**Can you manage to decode that one?**

> `A triple? Don't you have something harder for me?`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%209.png)

## Question 6

Do you feel challenged yet? 😅

`48 49 49 49 48 49 48 48 32 49 48 48 48 49 49 49 48 32 48 49 48 48 49 49 49 48 32 49 48 49 48 48 49 49 48 32 48 49 49 49 48 49 49 48 32 48 48 48 48 49 49 49 48 32 48 49 49 48 48 49 49 48 32 48 48 48 48 48 49 48 48 32 49 49 49 48 48 49 49 48 32 49 49 49 48 48 49 48 48 32 49 48 48 48 48 49 49 48 32 48 49 49 48 49 49 49 48 32 48 49 49 49 48 49 49 48 32 48 48 48 48 48 49 48 48 32 48 49 49 48 49 48 49 48 32 48 48 48 48 48 49 48 48 32 48 48 49 49 48 49 48 48 32 49 49 49 48 48 49 49 48 32 49 48 48 48 48 49 49 48 32 48 49 49 49 48 49 49 48 32 48 49 48 49 48 49 49 48 32 48 48 48 48 48 49 48 48 32 48 48 48 49 48 49 49 48 32 48 49 48 48 48 49 49 48 32 48 48 49 49 48 49 49 48 32 48 48 48 48 48 49 48 48 32 48 49 49 48 48 49 49 48 32 48 49 49 49 48 49 49 48 32 48 48 48 48 48 49 48 48 32 48 49 49 48 48 49 49 48 32 49 49 48 48 48 49 49 48 32 48 49 48 48 49 49 49 48 32 49 49 49 48 48 49 49 48 32 48 49 49 48 48 49 49 48 32 48 48 48 48 48 49 48 48 32 48 48 49 49 48 49 49 48 32 49 48 48 48 48 49 49 48 32 48 49 49 49 48 49 49 48 32 48 49 48 49 49 49 49 48 32 48 48 48 48 48 49 48 48 32 48 49 49 48 48 49 49 48 32 48 49 49 49 48 49 49 48 32 48 48 48 48 48 49 48 48 32 49 48 48 48 49 49 49 48 32 49 48 48 48 49 49 49 48 32 48 49 49 49 48 49 49 48 32 48 48 48 48 48 49 48 48 32 49 48 48 48 48 49 49 48 32 48 49 49 49 48 49 49 48 32 48 48 48 48 49 49 49 48 32 48 48 48 48 48 49 48 48 32 48 48 48 49 48 49 49 48 32 48 49 48 48 48 49 49 48 32 48 48 49 49 48 48 49 48`

**That one is a bit trickier, no?**

> `You can add as many steps as you want, I ain't scared.`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2010.png)

## Question 7

We found this script on a compromised machine. Can you make sense of it?

```sql
#Obfuscated payload
$key = 0x8F
$payload = "jbe+q/6+m3+ru/K/uj/rqHO4vyv5nv/r8zv66j+rG/65uru9"

# Decode
$s1 = -join ($payload.ToCharArray()[-1..-($payload.Length)])
$s2 = [System.Text.Encoding]::UTF8.GetString([Convert]::FromBase64String($s1))
$msg = -join ($s2.ToCharArray() | ForEach-Object { [char]([byte][char]$_ -bxor $key) })

# Execute
Write-Host $msg

# Cleanup traces
Remove-Item $MyInvocation.MyCommand.Path -Force -ErrorAction SilentlyContinue
```

This might look daunting, but a script is just a recipe when you really think about it. It also has ingredients ($variables for example), steps to follow, and a final result once its done cooking/running.

⚠️ DO NOT RUN THIS SCRIPT ⚠️

Just decode its payload with CyberChef.

**What is the payload message?**

> `yeah I guess this one was a bit evil`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2011.png)

# Section 4: Cartwheels

## Question 1

This section is a bonus, to have a little bit of fun. We're going to look at CTF style cipher challenges and how to tackle them.

Here's the first challenge:

```sql
EEEEEEEEEeeEeEeEEEEEEEEEEeeEeEEeEEEEEEEEEeeEEEeEEEEEEEEEEeeEEEeEEEEEEEEEEeeEEEEe EEEEEEEEEeeEeeEEEEEEEEEEEeeEeeeeEEEEEEEEEeeeEeeEEEEEEEEEEeeEEeEeEEEEEEEEEeeeEEee EEEEEEEEEeeeEeEEEEEEEEEEEeeEEEEeEEEEEEEEEeeEeeEEEEEEEEEEEeeEeEeeEEEEEEEEEeeEeEEeEEEEEEEEEeeEeeeEEEEEEEEEEeeEEeee EEEEEEEEEeeeEeEEEEEEEEEEEeeEeeee EEEEEEEEEeeEEeEEEEEEEEEEEeeEeeeeEEEEEEEEEeeEeeEEEEEEEEEEEeeeEEEEEEEEEEEEEeeEeEEEEEEEEEEEEeeEeEEeEEEEEEEEEeeEeeeEEEEEEEEEEeeeEEee
```

You can feed it into CyberChef, and as you can see, the wand appears, meaning it's detected it. But refrain from clicking on it for a second, as there is another thing you should know about CyberChef: it has a [Magic operation](https://gchq.github.io/CyberChef/#recipe=Magic(3,false,false,'')). Sometimes, even when the wand doesn't work, you can try using `Magic` to try and find what the possibilities are. Be aware it does not always work.

Try it here.

**What is the name of the recipe recommended by the `Magic` operation, as seen in its table of results?**

> `Cetacean_Cipher_Decode()`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2012.png)

## Question 2

Sometimes, no magic in the world will help CyberChef find how the string has been encrypted.

That's where [dcode.fr](https://www.dcode.fr/cipher-identifier) comes in handy, as they offer a cipher identifier. Note that it's not always accurate either, and the results are to be taken with a grain of salt. Sometimes it's bang on, other times absolutely not, or it can help you point you in the right direction. Note that dcode.fr also offers explanations about all the encodings/ciphers they handle.

Let's try the following message:

`55 33 555 555 2 6 444 8 999 0 9 777 666 8 33 0 7777 6 7777 0 8 44 444 7777 0 9 2 999 0 666 66 222 33 0 88 7 666 66 0 2 0 8 444 6 33 0 3 666 66 8 0 6 33 66 8 444 666 66 0 444 8 0 8 666 0 8 44 33 6`

If you try some CyberChef magic, you'll come back empty handed. Try feeding it into dcode.fr's cipher identifier. You should get 18 potential ciphers. The green bar is supposed to represent the likelihood of the encoding/cipher being the right one. In this case, it is pretty accurate.

Remember, this is the type of challenge you might find in a CTF, and research is often needed. If a top result doesn't produce a readable outcome, don't hesitate to explore the other proposed solutions, see what they are, what they look like.

**What's the message?**

> `KELLAMITY WROTE SMS THIS WAY ONCE UPON A TIME DONT MENTION IT TO THEM`
> 

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2013.png)

Click on the result and decrypt multi-tap as:

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2014.png)

## Question 3

Sometimes challenges will be symbols or pictures.

Like this one:

![image of the challenge](https://kc7photos-cfhbcabpaxb2f8fn.z03.azurefd.net/photos/D101S4Q3.png)

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2015.png)

What can you do in this case? A websearch will often do the trick. Reverse image search in particular can be helpful.

Once you've identified a potential cipher, use dcode.fr to decode it, they have a page on it.

**What's the question you decoded?**

> `IS THIS REALLY HISTORY OR IS IT ALIENS`
> 

From Google Image search, it is a Pigpen cipher, a geometric substitution cipher.

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2016.png)

## Question 4

Here's another challenge that is more visual, but that technically could also be done with sound:

`- --- .-. ... . / .. ... / .- .-.. ... --- / - .... . / ..-. .-. . -. -.-. .... / .-- --- .-. -.. / ..-. --- .-. / .-- .- .-.. .-. ..- ...`

Note that the / are word separators, they might throw off some identifiers, like CyberChef's.

**What piece of useless knowledge is hidden there?**

> `MORSE IS ALSO THE FRENCH WORD FOR WALRUS`
> 

After identifying the cipher using `dcode.fr`, it is supposed to be Morse Code.

![image.png](Decoding%20101%20A%20beginner%E2%80%99s%20guide%20to%20decoding/image%2017.png)

## Question 5

One last cipher for the road. Try to figure it out on your own using the tools you acquainted yourself with in the previous questions.

Just a note: sometimes ciphers don't carry the formatting (spacing, punctuations…) of the original message. It's up to you to recreate them.

```sql
AAAAA BAAAB AABBB AAAAA ABABB AABAA BABBA ABBAB BAABB AAABA AAAAA ABBAA ABBAA ABBAB BAABA AABAA AAAAA BAABA BAABA AABBB ABAAA BAAAB ABBAB ABBAA AABAA AAAAA ABABA BAAAB ABBAB ABABA ABBAB ABBAB ABAAB BAAAB AAAAB ABAAA ABBAA AAAAA BAAAA BABBA
```

> `A SHAME YOU CANNOT EAT THIS ONE ALSO LOOKS BINARY`
> 

<aside>
💡

(Note: In Bacon's cipher, the letter **X** is often used as a placeholder for a comma or a pause, similar to how "STOP" was used in old telegrams!)

</aside>

## Question 6

🎉Congratulations🎉

You've made it to the end of this module.

You've learned to recognise common encoding:

- binary
- decimal
- hexadecimal
- base64And also basic transformations:
- reverse
- ROT13

You've also seen the evil XOR.

You know they can be mixed together to make it more difficult for you to figure out.

You've learned how to use CyberChef, and you've practiced a bit on dcode.fr.

And finally, you've handled CTF-like challenges, not something useful on the day-to-day of a cybersecurity analyst, but nice to have when you want to compete for fun on your downtime ;)

**Type `I feel stronger now` to finish the module.**

> `I feel stronger now`
>