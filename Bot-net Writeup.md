Understanding Botnets

This is a write-up I put together for a task in my Security+ course. My approach was to first write down what I already knew from personal knowledge, then research from credible sources and see how much my understanding changed — and finally cover mitigation strategies.

What is a Botnet?

A botnet is a collection of devices — cameras, routers, or any device with an IP address and a vulnerability — controlled by an attacker without the owner's knowledge.

In simple terms: someone could be using your computer, or even a camera in your basement that you forgot even existed, after hacking into it (more on how that happens below). Once compromised, the device is used for further attacks without you ever knowing. A group of such devices — one belonging to me, one to a friend, one to you — together form a botnet.

A More Complete Definition

The attacker controlling a botnet is called the Bot Master (or Bot Herder). They need a way to get their bot malware running on a victim's system. Common infection vectors include:

Phishing
Vulnerability exploitation — abusing a bug in software or an internet-facing service
Drive-by download — a site that automatically downloads a (potentially malicious) file when visited
Weak or default credentials — especially common with cameras and routers, since most owners never change the factory password (a key factor in the Mirai botnet, discussed below)
Trojanized software — malware bundled with a cracked app or program

A compromised device is called a Bot or Zombie — zombie being, honestly, the cooler term.

How Bot Masters Control Zombies

There are two main architectures:

Centralized (C2 — Command & Control server): the attacker operates from a central server with direct access to every zombie, issuing commands from one place.
Decentralized (P2P botnet): zombies pass commands among themselves rather than relying on one central server. This makes the botnet much harder to take down, since there's no single point of failure.
What Botnets Are Used For

Once an attacker controls a botnet, it can be used for data theft, malware distribution, brute-force password attacks, and — most dangerously — DDoS attacks, which flood a target with traffic to take it offline.

The Interesting Part: Botnets Are Neutral

A botnet, as a structure, isn't inherently good or bad — it's just a model where one entity controls thousands of devices. It's essentially a form of distributed computing.

A good example is SETI@Home, a scientific project that searched for signals from space — a task requiring more processing power than a single computer could provide. The SETI server acted like a Bot Master: they built software, people voluntarily installed it, the server sent each participant's computer a chunk of data to process during idle time, and the results were sent back. This is a legitimate use of the botnet model: participants know their device is involved, and there's no malicious intent behind it.

A Famous Misuse: Mirai

Mirai (named after a Japanese anime, meaning "future" in Japanese) is one of the most well-known botnets. It worked by compromising one system and using it to attack others — mostly IoT devices like cameras and routers — via brute-force attacks against devices still using default credentials. First discovered in 2016 by a white-hat researcher, Mirai has been linked to some of the most severe DDoS attacks on record, including the attack on Dyn, a major DNS provider, which took down access to numerous major websites for several hours.

How Botnets Compromise Victims
Phishing employees or users into clicking a malicious message
Brute-force attacks
Exploiting unpatched vulnerabilities
Social engineering
Mitigation Strategies

Against unpatched vulnerabilities:

Structured software testing: unit testing → integration testing → system testing → acceptance testing → bug bounty programs
Continuous patching and hotfixing as issues are found
Identifying and patching known/legacy vulnerabilities
Keeping security tooling continuously updated

Against phishing:

Employee awareness training
Simulated phishing tests to gauge employee readiness
Clear reporting procedures for suspicious messages
Continuous monitoring via SIEM, alongside firewalls, IDS, and IPS
Filtering suspicious messages before they reach users
MFA, so a leaked password alone isn't enough for account access
Running suspicious files in an isolated environment (e.g., a VM) before touching the production system

Against weak/default credentials:

Changing default passwords
Enforcing strong password standards
Organization-wide password policies with a minimum complexity requirement
Storing passwords as hashes, never in plaintext

Against social engineering:

Training: teaching employees this threat exists and how to respond
Testing: running simulated scenarios so employees recognize real attempts
Technical: MFA, email filtering, minimizing exposed employee information
Physical: access control, surveillance
