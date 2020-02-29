---
layout: post
title: What went down in Kudankulam
date: 2019-11-01 15:09:00
description: Our initial thoughts and understanding on the cyber attack at Kudankulam
---

##The Attack
Kudankulam Nuclear Power Plant (KKNPP) near Chennai, India, has been confirmed to have been the target of a cyberattack. Since the news came out on October 30th, a lot has been said about it. As researchers in the field of cybersecurity for critical infrastructures, we wish to describe how these systems are usually secured and to separate the hype from the fact. 

###IT vs. OT
To understand cyber attacks on critical infrastructures, we need to understand one concept: IT vs. OT. Organizations that operate critical infrastructures have two distinct cyber networks: an Information Technology (IT) network and an Operational Technology (OT) Network. This is mainly for the separation of concerns. The IT network of an organization is for organizational management, things like payroll, employee management, or even the laptops that employees use. This network is almost always internet-connected, although firewalls are commonly used to restrict some access. The OT network, on the other hand, is what is the nervous system of the process. In the case of Kudankulam, the OT network has machines that run control algorithms that manage power generation and also machines that store historical data for analysis.

An air gap essentially means that no path exists from the Internet to the isolated computer, i.e., neither is the isolated machine connected to the Internet nor is any other machine that can talk to the isolated machine. It is widespread for control systems (OT) that control critical infrastructure to be air-gapped from the IT systems of the same organization.

An actual air gap is one where the machine or network of machines is physically disconnected from the Internet, i.e., the only way data can pass into or out of the network is when someone connects an external storage device (like a USB stick). More often, the air gap is a software-defined using a firewall. The firewall contains rules to prevent any external connections from being established with the isolated computer. The most common reason for having a firewall-based air gap is to allow for a controlled software update of a control system device. While this might jump to you as an exploitable foothold, let us suffice to say that no air gap (including a physical one) is impossible to surpass. It has been done multiple times with critical infrastructure attacks in the recent past. 

The Plant released a statement stating that, “...KKNPP and other Indian Nuclear Power Plant Control Systems are stand-alone and not connected to an outside cyber network and the Internet. Any Cyberattack on the Nuclear Power Plant Control System is not possible.” This essentially is an oversimplification of what we just described.


##What harm can be caused
If an attacker can get access to the OT network and then can escalate their privileges to a level where they can send control commands to the control system, then they may be able to do evil things. Why do we say may? Because for a successful attack on a control system, the attacker must understand the process they are controlling. It takes months of staying within the OT network to gather enough intel to get a vague understanding of the process and how to control it.

Stuxnet is a famous case where attackers were able to target nuclear centrifuges. Security researchers who reverse-engineered the code in the malware found that it was extremely focused and meant to hit centrifuges in one of Iran’s nuclear power plants. The attackers had knowledge of all the equipment used in the plants, their make, models, as well as software versions. The attack was initiated by malware that was present in USBs. This is the level of prior knowledge, and reconnaissance needed to attack a nuclear power plant.

Even if an attacker can understand how the network functions, the attacker must then be able to disable all possible safety mechanisms which are fairly extensive in nuclear power plants. If an attacker can perform these steps, they can cause large-scale damage, ranging from destroying the turbines to taking human lives.

The Ukraine Attack was yet another case similar to Stuxnet. Attackers disabled power distribution stations in Ukraine, causing a widespread blackout. The attack started several months before the blackout actually occurred. The attack began when attackers spear-fished several employees of the companies and embedded Microsoft Word documents with malware. They gained access to the IT networks this way. 

Later, they collected credentials to the OT network from the network and gained access to the OT network. They even attempted to overwrite firmware on the power grid protection equipment to render them useless. They erased boot-records in the control systems servers and went on to perform a denial-of-service attack on the call centers to prevent them from notifying any customers. 

Power Plants in Ukraine, as well as Iran, were secured in a way similar to that of KKNPP. Attackers still found ways to attack the Power Plants and were able to make the jump from IT to OT. Hence, the argument the KKNPP authorities make in their press release is not entirely valid.

So what really happened in Kudankulam? 

###What we know about the current malware
Analysis of the malware sample (now on VirusTotal) shows that it is a modification of a previously known and used malware, Dtrack, which has been used in the past to attack financial institutions in India. It is a Remote Access Trojan, which means that it is malware that looks like a legitimate file while allowing a remote user to command a machine. Analysis of the malware can give insights into the goals of the attacker. First, there is a CollectHistory function that recovers internet search history from the browser installed on the machine; there are search queries for both Google Chrome and Mozilla Firefox history files. Second, the malware attempts to collect local operating system registry information such as registered owner, registered organization, install date, and current user. Thirdly, the malware gathers a list of currently active processes on the machine. Finally, it scans for information regarding the network the affected machine is on. All of this information is then written into temporary files that can be extracted to a remote server (command and control or C2) by the attacker.

How did we know that Kudankulam was attacked? An analysis of the malware sample (after it was posted online) shows one specific command that contains the username KKNPP, which most likely belonged to the power plant. This is what pushed the security community to worry about it.

All of the malware functions point to one thing: reconnaissance. There is no displayed understanding of the control system or of specific processes in the operational environment. However, there are a few hardcoded local IP addresses that might point some prior recon effort of the IT environment. The attackers were probably using this as a stepping stone and were trying to gather any information that they can about the control system: the browser history could, for example, show if certain control equipment vendor websites are visited often for software updates.

There have, however, been process failures that have been experienced at KKNPP in the last few months. While it might be a stretch to correlate the two, it is essential to note that a successful cyber attacker can exhibit similar behavior once the OT network is compromised.

##Course Correction
Attribution of an attack to a nation-state or a group of individuals is a challenging task. The common practice is to look at similarities between malware samples (think of things like coding principles and reused code) and decide with some confidence that they came from the same source. In the case of the malware found in Kudankulam, similarities have been found to the Dtrack and ATMDtrack attacks, which in turn looked similar to the 2013 DarkSeoul attack campaign attributed to a threat actor known as Lazarus. Note that this is not the actual name of a group, this is just a pseudonym assigned by the security community. Again, no one knows what nation funds or houses the group, but Kaspersky has found IP addresses that can be traced to North Korea, thereby making it a possible suspect.

KKNPP was, in fact, hacked, but unlike what a lot of the initial (social) media posts suggested, the attack wasn’t on life-threatening critical infrastructures. The IT vs. OT air-gap in KKNPP seems to have served its purpose. And while power plants are prepared for such vulnerabilities in the IT network, it should still sound an alarm for the government to impose cybersecurity regulations and mandate that these power plants hire penetration testers and auditors to expose and fix any holes that a foreign state might leverage to gain control over nation critical processes.
