# Memory Forensics & Disk Forensics Basics: My Learning Notes

Today I want to start with memory forensics: what it actually is and why it's useful for us.

Quick honesty first: I'm a beginner at this, probably more of a beginner than most of you reading this. I'm not in the SIEM / defense side at all. But for hacking and pentesting, I think you need to know how this stuff is built and how it records things. And maybe one day you'll be the one who gets hacked.

---

## Basic concepts

### RAM

RAM is the temporary memory of the computer. I think we all know what it is and what it does. Everything that is running right now is inside RAM.

None of this information is persistent: when you shut the device down, the RAM is wiped. The disk (SSD and so on) is the other side, where data is stored for as long as we want.

This is a really important point (I only just learned it myself): we learn memory forensics mostly to detect malware. Some malware (often called fileless malware) lives only in RAM and leaves no trace on the disk.

### Process

Everything that gets created has a creator. The computer world is the same: a process is started by another process, so it has a parent.

Every process has two numbers:

- **PID**: the process ID of the process itself (the child)
- **PPID**: the process ID of its parent

Example:

```
explorer.exe (PID 1000)
   └── cmd.exe (PID 2000, PPID 1000)
```

`cmd.exe` has PID 2000, and its PPID is 1000, which is `explorer.exe`.

So when you see a process in Volatility's output, the PPID tells you who created it, and if you find that PID in the list you get the parent's name.

### Memory dump

In short: a photo of the RAM, so you can analyze it later. Usual extensions: `.mem` `.raw` `.dmp` `.vmem`

### What is Memory Forensics?

It's analyzing that dump file to find out what was happening on the system at that moment.

It matters because, like I said, a lot of malware doesn't leave anything on the disk, or it hides itself. But to run, it has to be in RAM, so it gets seen there. You take a photo of the RAM and analyze it.

### What is Volatility?

An open-source tool. You give it the dump file (the photo) and it pulls information out of it using plugins. Each plugin does one specific job, for example listing processes or listing network connections.

**What is a plugin?**

The main problem: a dump file is just a huge file of bytes. Say 8 GB of numbers with no labels, and nowhere does it say "process list is here" or "network connections are here". Each plugin knows where and how to find one specific piece of information.

I imagine it like this:

- The dump file is a city. Everything is in there, but there are no street signs.
- Volatility is an investigation agency that knows how to enter the city and move around in it.
- A plugin is a specialist workshop. Each one knows what to look for and where to find it, how to find every citizen.

The first step is installing Volatility. A simple search will tell you how to install it.

---

## My plan

I believe theory alone is useless. You have to see it yourself, try it, and even break things.

So the plan is:

1. Watch a couple of YouTube videos to understand what's what and how memory forensics works
2. Do the TryHackMe room **Volatility Essentials**
3. Most likely, do some CyberDefenders labs after that so it really clicks for me

One correction to something I might have blurred above: memory forensics and disk forensics are different things. Here I'm talking about memory forensics (RAM). Disk forensics has its own tools.

---

## First hands-on: MemLabs, Lab 0

I found a GitHub repo called **MemLabs** that has memory forensics practice files. I downloaded Lab 0. It's also nice that the author wrote the write-ups for them, so if you get stuck you can go and see what's what.

I downloaded `chall.raw.xz`, which had to be decompressed. This command decompresses it:

```bash
tar -xf chall.raw.xz
```

The output is this file:

```
chall.raw
```

That gave me a memory dump, so now it's time to use what I've learned:

```bash
vol -f chall.raw windows.info
```

Output:

```
Volatility 3 Framework 2.28.2
Progress:  100.00		PDB scanning finished

Variable	Value
Kernel Base	0x82604000
DTB	0x185000
Symbols	file:///home/karma/.local/lib/python3.12/site-packages/volatility3/symbols/windows/ntkrpamp.pdb/EDD3760CEE2B45D2A63BF8C26EE11FAF-2.json.xz
Is64Bit	False
IsPAE	True
layer_name	0 WindowsIntelPAE
memory_layer	1 FileLayer
KdDebuggerDataBlock	0x8273cb78
NTBuildLab	7601.24260.x86fre.win7sp1_ldr.18
CSDVersion	1
KdVersionBlock	0x8273cb50
Major/Minor	15.7601
MachineType	332
KeNumberProcessors	1
SystemTime	2018-10-23 08:30:51+00:00
NtSystemRoot	C:\Windows
NtProductType	NtProductWinNt
NtMajorVersion	6
NtMinorVersion	1
PE MajorOperatingSystemVersion	6
PE MinorOperatingSystemVersion	1
PE Machine	332
PE TimeDateStamp	Sun Sep  9 00:14:23 2018
```

### What is windows.info?

It's the first plugin you run on any Windows dump. It's like a spec sheet of the system, and it gives us the information above.

An important thing: you need to know the OS family beforehand, because plugin names start with it:

```
windows.   linux.   mac.
```

In most CTF challenges they tell you the OS, but I want to try it the real-world way. You can just try all of them, it's not that hard, and if it's the wrong one you can tell that it isn't the OS you entered, so you try another one.

In this example I found out it's Windows, and I don't care about the other OSes here.

### What we can read from this output

- An old 32-bit Windows 7
- It had one CPU
- `NtSystemRoot` shows the root: `C:\Windows`

### Saving the output

Before moving on to the other plugins, I save this output into a separate file. It's not required, but it's easier for us, because every time Volatility runs it first has to process the image, which affects the speed. It's also easier for searching: you can use `grep` or `less` on the saved file. If you don't want to save it, that's fine too.

I decided to do the TryHackMe rooms first and then come back to this, plus some CTF challenges, and maybe real-world stuff. I came this far just to get a picture of the topic.

---

## Artifacts

One concept I want to explain first: **Artifact**.

Everything you do on a computer leaves a trace behind. From browser searches to system logs to network traffic. These are artifacts.

Forensics means finding these artifacts on the system.

---

## TryHackMe: Introduction to DFIR

The Volatility Essentials room has two prerequisite rooms, so I'm doing those first. The first one is the DFIR intro.

DFIR means two things:

**Digital Forensics**: what I'm working on right now, and it shows up in a lot of CTFs. It asks:

- What exactly happened?
- Where did the attacker come from?
- Which file did they open?
- What did they take?

**Incident Response**: reacting to an incident. If a security problem happens, I should first be able to identify it, then respond to it, and bring the system back to normal. For example: identifying the attack, isolating the infected system from the network, cleaning up and recovering. It asks:

- What is going on right now?
- How do we stop the attack?
- How do we kick the attacker out of the network?

In the end, forensics helps IR, because until you understand how the attacker got in, you can't respond.

The room covers:

- Introduction to DFIR
- Some basic concepts used in the DFIR field
- The Incident Response processes used in the industry
- Some of the tools used for DFIR

### What is DFIR?

It's those two things I described above. The field helps security professionals identify the footprints attackers leave behind when a security incident happens.

The next part covers several topics:

### 1. Artifacts

I already talked about this above. Artifacts are pieces of evidence that point to an activity performed on a system. Anything left on the system that shows something was done can be an artifact.

Everything done on a computer is an artifact, and at the same time it can be evidence. Evidence of what? Evidence of what the hacker who got into the system did. The point here is that **an artifact is only valuable when it supports a hypothesis**. For example: I say the attacker got in and planted malware on the system. That's the hypothesis. The artifact is useful when I have evidence for it, like an unknown file in the Temp folder.

In short: I first ask what the attacker could have done. When I reach a possible conclusion, I use the artifacts to turn it into evidence, and I collect artifacts to answer that possible conclusion.

It's also better to say that an artifact is itself evidence. You put the evidence together to confirm your hypothesis and get to the answer. And of course your hypothesis may be wrong. If it is, you come up with another hypothesis and collect evidence again.

The new thing the text says is that artifacts are collected from 3 places:

- **File system**: the files and folders on the disk, like that unknown file in Temp
- **RAM / Memory**: things that are running right now
- **Network**: the traffic passing through the system

And this collection is done from two kinds of devices:

- **Endpoint**: the computer or laptop of a normal user, e.g. an employee's computer
- **Server**: a computer that provides a service, e.g. an email server

Most places and companies use operating systems like Linux and Windows.

### 2. Evidence Preservation

We have to accept that every forensic analysis contaminates the evidence. So first the evidence has to be kept write-protected. Then we work on a copy of it. This guarantees the original evidence isn't contaminated and stays safe during analysis. And even if the copy we have gets corrupted or has any kind of problem, we can go back and make another copy from the original.

- Take the original file
- Lock it
- Make a copy of the original and work on the copy

Most of the time a hash is used to check that the copy is no different from the original.

### 3. Chain of custody

A record of who had the evidence.

Once the evidence is collected, you have to make sure it's kept securely, and nobody unrelated should have access to it, otherwise the chain of custody is contaminated.

In short: a suspicious disk shouldn't be passed from hand to hand, because someone might delete or add something, and then you can't tell whose work is whose.

### 4. Order of volatility

Digital evidence is often volatile, meaning that if it isn't captured in time, it can be lost forever.

For example, RAM: after the system shuts down, everything is wiped. Others, like the hard disk, are more persistent, and the data isn't lost when the system shuts down, so they're less volatile than RAM.

In DFIR we have to pay attention to how volatile each source is.

### 5. Timeline creation

After collecting the artifacts and preserving their integrity, we have to present them in an understandable way so we can use all of their information.

For that I use a timeline, which puts all the events and activities in chronological order. It gives an overall view, and the different sources end up side by side.

For example, when I sort packets by time in a pcap file, I'm using a timeline.

There was a question at the end about which is more volatile, disk or RAM. The answer: RAM. There was also a challenge, but it was so simple there's nothing worth talking about.

### Tools

In this part I'm going to learn several tools for DFIR and go deeper. These are tools that help and speed up the DFIR process.

**Eric Zimmerman's tools**: he's a security researcher who wrote several tools to help with forensic analysis on the Windows platform. They help with registry analysis, file system analysis, timelines and a lot of other analysis.

**KAPE** (Kroll Artifact Parser and Extractor): automates collecting and parsing forensic artifacts and can help build a timeline of events.

*Why do we need tools?* A computer has thousands of artifacts and going through them by hand is very slow. These tools automate the repetitive work (collecting, reading, sorting) so you can spend your time thinking and analyzing.

**Autopsy**: an open-source platform with a GUI that helps analyze data from digital media like mobile devices, hard drives and removable drives. Its different plugins speed up the forensic process.

**Volatility**: the tool I'll use a lot for RAM analysis (memory analysis). We'll get to know it better in the future, because some rooms need it.

**Redline**: a tool that helps with reviewing collected information. From what I read online, it isn't used much anymore, but it doesn't hurt to know it.

**Velociraptor**: an advanced platform for monitoring, forensics and incident response on endpoints. It's open source but very powerful.

Even though all these tools are useful in DFIR, we have to understand the process we follow to reach the goal. The next task is about that.

### The Incident Response process

In this task I learned the incident response process and how forensics helps in it.

Reputable organizations like NIST have published standard methods for IR in the SP 800-61 guide. Other organizations like SANS published a handbook called the *Incident Handler's Handbook*. SANS gave its steps a short name so they're easier to remember, but both describe the same idea:

- **P**reparation
- **I**dentification
- **C**ontainment
- **E**radication
- **R**ecovery
- **L**essons Learned

That's the acronym **PICERL**, so it sticks in memory. Now let's explain each step.

**Preparation**: before an incident happens, you have to do things so everyone is ready to face it, like having people, technology, tools and so on. Since people are stressed during a hack, this helps everyone know where they are and what the next step is.

**Identification**: the incident is identified from some indicators. They have to be checked for False Positives, meaning whether the alert that came in is real or not.

**Containment**: the incident is contained and you try to limit its effects. Depending on the attack there are short-term and long-term containment measures, and you apply them based on the situation.

**Eradication**: the threat is removed from the system. This is where forensics comes in: it finds the way the attacker got into the system or network. That matters because if the entry point isn't closed, the attacker can come in again through the same way.

**Recovery**: once the threat is removed from the network, the services that were disrupted go back to how they were.

**Lessons Learned**: finally, the incident is reviewed and documented, and based on the findings you take actions so the team does better next time.

That challenge is done, so let's move on to the rest of the path and rooms.

---

## picoCTF: Sleuthkit Apprentice (Disk Image Forensics)

Before moving on to the next two rooms, I decided to put this picoCTF challenge that I solved in my write-ups. It was about a disk image and I found it interesting because I learned good things from it.

- **Challenge name:** Sleuthkit Apprentice
- **Category:** Disk image forensics
- **Difficulty:** Medium

They gave me a file `disk.flag.img.gz`. Honestly, at first I wanted to extract it with `unzip`, but then I remembered this:

```bash
gunzip disk.flag.img.gz
```

I extracted it. To be honest, I had solved a memory dump challenge before, but never a disk image. Since today's article is about this, we're learning together.

Also, I didn't solve this one easily. First I went to YouTube, and I also used AI for learning, which I think is the best and most useful way to use AI.

### First attempt: mount (failed)

I first wanted to mount it:

```bash
sudo mount -o loop disk.flag.img /tmp/picoctf_dist
```

It didn't work and gave this error:

```
mount: /tmp/picoctf_dist: wrong fs type, bad option, bad superblock on /dev/loop56, missing codepage or helper program, or other error.

       dmesg(1) may have more information after failed mount system call.
```

I searched and researched, and found out: Linux tries to treat the whole image as one filesystem. But a disk image is not a filesystem, it has several different partitions. So you have to tell the system where the filesystem you want starts in the image.

In simple words: you have to choose which of the partitions of the disk you want, and search inside it.

### Seeing the partitions

```bash
mmls disk.flag.img
```

`mmls` = Media Management List. It checks the structure of the disk and shows its partitions.

Output:

```
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000360447   0000153600   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000360448   0000614399   0000253952   Linux (0x83)
```

So this disk has 3 partitions:

1. First partition: Linux
2. Second partition: Swap
3. Third partition: Linux

### What is a sector?

You need this to understand the `mmls` output. A disk is made of small units called **sectors**. In the output it says:

```
Units are in 512-byte sectors
```

So each sector is 512 bytes. For example:

```
Start: 2048
Length: 204800
```

This means the first partition starts at this sector and is this long.

One more thing: when you want to use `fls`, you use the **Start** sector. You'll see what I mean in a moment.

If you want to calculate the size of a partition:

```
204800 × 512 = 104857600 bytes   (length × bytes per sector)
```

That's about 100 MiB.

### fls

`fls` = File and Directory Listing. It looks at the files and folders inside a filesystem. As I said, for `fls` we use the start sector, so we tell it that this partition starts at this number:

```bash
sudo fls -o 2048 disk.flag.img
```

This is the first partition, the Linux one:

```
d/d 11:	lost+found
r/r 12:	ldlinux.sys
r/r 13:	ldlinux.c32
r/r 15:	config-virt
r/r 16:	vmlinuz-virt
r/r 17:	initramfs-virt
l/l 18:	boot
r/r 20:	libutil.c32
r/r 19:	extlinux.conf
r/r 21:	libcom32.c32
r/r 22:	mboot.c32
r/r 23:	menu.c32
r/r 14:	System.map-virt
r/r 24:	vesamenu.c32
V/V 25585:	$OrphanFiles
```

Nothing interesting here in my opinion, because most of these files and folders are for the root of the Linux system. So, next one:

```bash
sudo fls -o 360448 disk.flag.img
```

```
d/d 451:	home
d/d 11:	lost+found
d/d 12:	boot
d/d 1985:	etc
d/d 1986:	proc
d/d 1987:	dev
d/d 1988:	tmp
d/d 1989:	lib
d/d 1990:	var
d/d 3969:	usr
d/d 3970:	bin
d/d 1991:	sbin
d/d 1992:	media
d/d 1993:	mnt
d/d 1994:	opt
d/d 1995:	root
d/d 1996:	run
d/d 1997:	srv
d/d 1998:	sys
d/d 2358:	swap
V/V 31745:	$OrphanFiles
```

This looks like the main structure of a Linux system.

To go into any folder you have to use the number before it, the **metadata address**. The structure is:

```bash
fls -o <partition_offset> -f ext4 disk.flag.img <metadata_address>
```

So filled in, it becomes:

```bash
fls -o 360448 disk.flag.img 451
```

I wanted to go into `home`, but there was nothing in it. I had tested some others before, but none had anything.

Then I went to `root` and found this:

```bash
sudo fls -o 360448 disk.flag.img 1995
```

```
r/r 2363:	.ash_history
d/d 3981:	my_folder
```

I went into `my_folder`, which looked more suspicious:

```bash
sudo fls -o 360448 disk.flag.img 3981
```

```
r/r * 2082(realloc):	flag.txt
r/r 2371:	flag.uni.txt
```

And yes, the flag file was there. At first I really didn't know how to get into it, but after a simple search I found out that `icat` can read the inside of a file in this kind of image. So I ran this:

```bash
icat -o 360448 disk.flag.img 2371
```

And I got the flag:

```
academy{by73_5urf3r_6395be48}
```

---

## What's next

This is already getting too long, so I'll continue in the next part. Coming up:

- Solve MemLabs 1
- Solve the room [Memory Analysis Introduction](https://tryhackme.com/room/memoryanalysisintroduction)
- Solve the room Volatility Essentials

Both rooms are on TryHackMe. And if I find a good CTF challenge, I'll solve it too and show it here.
