---
layout: post
title: What went down in Kudankulam
date: 2019-11-01 15:09:00
description: Our initial thoughts and understanding on the cyber attack at Kudankulam
---
This theme implements a built-in Jekyll feature, the use of Pygments, for sytanx highlighting. It supports more than 100 languages. This example is in C++. All you have to do is wrap your code in a liquid tag:

{% raw  %}
{% highlight c++ %}  <br/> code code code <br/> {% endhighlight %}
{% endraw %}

Produces something like this:

{% highlight c++ %}

int main(int argc, char const \*argv[])
{
    string myString;

    cout << "input a string: ";
    getline(cin, myString);
    int length = myString.length();

    char charArray = new char * [length];

    charArray = myString;
    for(int i = 0; i < length; ++i){
        cout << charArray[i] << " ";
    }

    return 0;
}

{% endhighlight %}

There has been a lot that has been said about the cyber attack on the Kudankulam Nuclear Power Plant which came to light on Oct 30. While very less is known regarding the attack itself, a lot of speculation has blown what is known out of proportion. In this post we wish to separate the hype from the fact and explore (given the publicly disclosed data) what the attack means and what the future holds.

what we know about the current malware (RAT?)
Analysis of the malware sample (now on VirusTotal) shows that it is a modification of a previously known and used malware, Dtrack, which has been used in the past to attack financial institutions in India. It is a Remote Access Trojan, which means that it is malware that looks like a legitimate file (PDB in this case?) while allowing a remote user to command a machine. From publicly available analysis of the malware, we see the following functions have been used on a Windows NT machine:

1. There is a CollectHistory function which recovers internet search history from the browser installed on the machine; there is search queries for both Google Chrome and Mozilla Firefox history files. This information is then written into temp files which can be extracted to a remote server (command and control or C2) by the attacker.

2. A function for collecting local OS registry information such as registered owner, registered organization, install date and current user. This is also stored in a separate temp file.

3. A function to collect network information and stores them in temp files for extraction. This has three commands:
    3.1 netstat -naop tcp: 
        n- display active tcp connections
        a- all active connections and port numbers
        o- pid of active connections, basically which proces is using which connection
        p- protocol in this case tcp

    3.2 ipconfig all: this gives the IP configuration of the current machine including the hardware address, gateway and subnet details as well as all the interfaces on the machine.

    3.3 netsh interface ip show config: shows IP configuration. seems redundant to ipconfig all

4. A function that uses the commant tasklist to return a list of currently active processes on the machine.

All of these functions point to one thing: reconnaisanse. There is no displayed understanding of the control system or of specific processes in the operational environment. However, there are a few hardcoded local IP addresses which might point some prior recon effort of the IT environment.

How did we know that Kudankulam was attacked? An analysis of the malware sample shows one specific command that contains the username KKNPP, which most likely belonged to the power plant. This is what pushed the security community to worry about it.

IT vs OT
In order to understand cyber attacks on critical infrastructures, one concept is important to decipher: IT vs OT. Organizations that operate critical infrastructures have two distinct cyber networks: an Information Technology (IT) network and an Operational Technology (OT) Network. This is essentially for separation of concerns. The IT network of an organization is for organizational management, things like payroll, employee management or even the laptops that employees use. This network is almost always internet connected, although firewalls are commonly used to restrict some access. The OT network on the other hand is what is the nervous system of the process. In the case of Kudankulam, the OT network has machines that run control algorithms that manage power generation and also machines that store historical data for analysis. 

On the matter of air gaps
An air gap essentially means that no path exists from the internet to the isolated computer i.e. neither is the isolated machine connected to the internet nor is any other machine that can talk to the isolated machine. It is very common for control systems (OT) that control critical infrastructure to be completely air gapped from the IT systems of the same organization. It is important to understand the air gap to delve into possible attacks on the OT. A true air gap is one where the machine or network of machines is physically disconnected from the internet i.e. the only way data can pass into or out of the network is when someone connects an external storage device (like a USB stick). More often, the air gap is a software defined using a firewall. The firewall contains rules to prevent any external connections from being established with the isolated computer. The most common reason for having a firewall based air gap is to allow for controlled software update of a control system device. While this might jump to you as an exploitable foothold, let us suffice to say that no air gap (including a physical one) is impossible to surpass. It has been done multiple times with critical infrastructure attacks in the recent past.  

What harm can be caused
IF an attacker (note the very big if) is able to get access to the OT network and then is able to escalate their privilege to a level where they can send control commands to the control system then they MAY be able to do nefarious things. Why do we say may? Because for a successful attack on a control system the attacker must understand the process they are controlling. It takes months of staying within the OT network to gather enough intel to get a vague understanding of the process and how to control it. Even if this is done, the attacker must then be able to disable all possible safety mechanisms, which in the case of nuclear power plants is well.. a lot. Given attacker can perform all these steps, they can cause large scale damage, ranging from destroying the turbines to taking human lives.

So what happened in Kudankulam? All actions point towards continued recon of the IT network. The attackers were probably using this as a stepping stone and were trying to gather any information that they can about the control system: the browser history could for example  show if certain vendor websites were visited often for update downloads. 

There have, however, been process failures that have been experienced at KKNP in the last few months. While it might be a stretch to correlate the two, it is important to note that a successful cyber attacker can exhibit similar behavior once the OT network is compromised.

Attribution of the attack
Attribution of an attack to a nation state or a group of individuals is an extremely difficult task. The common practice is to look at similarities between malware samples (think of things like coding principles and reused code) and decide with some confidence that they came from the same source. In the case of the malware found in Kudankulam, similarities have been found to the Dtrack and ATMDtrack attacks, which in turn looked similar to the 2013 DarkSeoul attack campaign attributed to a threat actor known as Lazarus. Note that this is not the actual name of a group, this is just pseudonym assigned by the security community. Again, no one knows what nation funds or houses the group but Kaspersky has found IP addresses that can be traced to North Korea, thereby making it a possible suspect. 

An attack on the critical infrastructure of any nation may be construed as an act of war. What makes retaliation in cyber-space extremely hard is imperfect knowledge of the adversary. In threats of the scale of Kudankulam , it is important to ask what motive the adversary has. When Lazarus was attributed with the Sony hack, we might infer the motive to be revenge (for the satirical potrayal of the North Korean dictator) or when the same group launched attacks against South Korea the motive could be wide spread disruption against an unfriendly neighbor. Which raises the question: what would motivate Pyongyang (if attribution were to be believed) to launch an attack on Indian critical infrastructure? And what is the right way to retaliate?

So what should we do? Looks like nothing bad happened, should we move on? Absolutely not. First we must thank our stars that the attack was caught early. Second, we should stop pushing the snooze button on cybersecurity for critical infrastructures and wake up to the realization that the technical and organizational processes are not in place t

