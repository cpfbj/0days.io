---
title: "An Analysis of a shady USB to HDMI converter"
date: 2021-06-29T23:10:27+02:00
draft: false
---

Like many others, I have had the pleasure of working remotely for some time now.
That also means the right equipment, such as a triple-monitor setup is required for me. I do already have a triple-monitor setup, but I did not want to have to deal with moving HDMI cables from my desktop PC to my laptop every day. Therefore, dealing with a Lenovo T470 laptop and a ThinkPad Ultra docking station, I am somewhat limited in my monitor input- and outputs. I decided to look for a USB 3.0 to HDMI adapter online. Fortunately, I quickly found a dirt cheap converter online and decided to go for it. For 25 bucks you cannot go wrong.. Or can you?

Fast forward a few days and I hear a knock on my front door. The converter is here and It is finally time to set up my 3rd monitor for work purposes.

I plugged it straight into my workstation and immediately noticed something strange. It is registered as a regular USB memory stick. I clearly remember thinking this was not normal for a HDMI converter, but also thinking that it could be the drivers that they put on a memory stick inside the converter. Nonetheless, I did a quick antivirus scan on the USB stick to check for the most basic stuff. Nothing pops up, and Windows says it is clean. I would not expect otherwise from Windows, but then  I was at least 5% more confident that this is normal behavior and not something shady. 
I opened up the drive and found this. 
![](https://i.imgur.com/AdZVVIa.png)
Exactly as expected, there were three different drivers on the drive itself.Since I was running Windows 10 on my workstation I chose to execute the Windows7-Windows10_v1.0.0.1.exe file.


And the first thing I saw was this.
![](https://imgur.com/DdPI9TX.png)

Again, this is NOT what I wanted to see when installing drivers on my workstation. Chinese letters all over, and the driver is written by.. MS?

At this point, I decided to pull the device out of my workstation. To my great fear I saw yet another pop up as shown below. The text  translates to The device is ready and MS_Idd_Bus_02 is configured and ready to use.
![](https://imgur.com/oDYbWEC.png)


I did not install it, I exited the installer! Not knowing much about drivers in general, I did a quick search on this MS_Idd_Bus_02 and I saw only results from other people with the exact same USB 3.0 to HDMI driver. I was by now fairly concerned about exactly what I plugged into my workstation and I decided to shut it off completely. I took out the battery and called up my colleague, asking him to start inspecting logs from my PC.

While he was working on that, I booted up my regular everyday desktop PC and started a virtual Windows 10 machine. I plugged in the USB 3.0 to HDMI converter to my desktop and imported the shady .exe driver file to Joe Sandbox fearing the worst. 
If you do not know what Joe Sandbox is, I really cannot stress enough how good of a tool it is for analyzing suspicious files like this one. It runs the executable in a controlled environment and screenshots every step of the executable file for you to look through. Not only that, it also documents everything the file does in detail. Knowing this, I had to run my shady executable file through Joe Sandbox immediately.
![](https://i.imgur.com/PCRBqiY.png)


I waited for the results to come. After a few minutes it finished and gave the file a suspicious-score of 34/100. Knowing Joe Sandbox and having used it a few times beforehand I knew that Joe Sandbox quickly ramps up points for minor things, so I knew I had to do some more research before calling this one. A score of 34 is not a whole lot, but it is still worth looking into knowing what we know prior to these results.


Taking a look at the full report from Joe Sandbox we can see that the executable file gets flagged as suspicious and certainly does some suspicious things that I would not want a normal USB 3.0 to HDMI driver to do.   

One of the actions the executable file does is dropping a tmp file in the following path
C:\Program Files\HYC USB Display\is-S1KVH.tmp. This file was by Avira detected malicious with the label: TR/Crypt.XPACK.Gen2. This is a trojan infection that is used to drop other malware threats on a computer. It may carry the initial payload of the virus and uses an encode and decode process to drop it on the system.
Since the file is only marked dangerous by Avira and no other antivirus software, I cannot confirm or deny this for now with my limited malware analysis skills.

When taking a look at the long list of bad things this file does, you also find Creates an undocumented autostart registry key. Creating a new registry key is not bad in itself, but having it done undocumented is shady and generally a bad practice done on a commercial product bought world wide.
And did I mention it also changes security center settings? Source: C:\Windows\System32\svchost.exe
Key value created or modified: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Security Center cval
Again, I would not think of this as normal behavior for a USB 3.0 to HDMI converter.

One last action I wanted to write about is the long list of URLs that the installer is talking to during setup. It talks to 39 URLs including www.s.xboxlive.com, www.skygz.com and www.s.dnet.xboxlive.com etc.. For what purpose?!

I spoke to my colleague a few hours after I initially plugged the USB 3.0 to HDMI converter in, and he had not seen or detected any malicious or abnormal activity in general from my laptop or in our network in general.

So, I decided to contact the company that sold me the device. At first I gave them a call, explaining the situation and my findings. It was clear to me that they did not understand the situation, and after 10 minutes of me trying to explain the situation, they asked me to write them an email explaining it all.

After a long and very thoroughly written email explaining every step and warning, I got a response 30 minutes later, telling me that I got a full refund.
Well, that is great! But that was not what I wanted to achieve from this. My goal for contacting them and spending time on this was for them to remove it and stop selling it, but since the guy I spoke to on the phone said it was one of their best selling products, there was no way that they were going to take it off the website.

Oh well..
At least I got to write my findings down for you to read, and hopefully make sure that you do not buy too cheap stuff online like I did.

Is this the chinese government's way of tracking all of us? I highly doubt so, but it is nonetheless some shady code practice and I would not willingly continue to use this product knowing what I found.


For reference, you can see the full Joe Sandbox report in details here:
https://www.joesandbox.com/analysis/783924
