---
---

After four years in InfoSec, a CEH and an ECSA certification, I was looking at courses that would push me to my limits and validate my practical skill-set. I wanted an in-depth understanding of vulnerabilities with minimum hand-holding and OSCP looked like the perfect fit.

## OSCP? What's that?

Offensive Security Certified Professional (OSCP) is the certification for Offensive Security's "Penetration Testing with Kali Linux (PWK)" course which focuses on hands-on exploitative information security skills. What's cool about this certification is that there are no multiple choice questions and the passing criteria for the exam is to successfully compromise 5 machines in 24 hours.

## The Prep:

The nature of the exam requires some heavy preparation and sharpening of skills. After scanning through the [course syllabus](https://www.offensive-security.com/documentation/penetration-testing-with-kali.pdf) I settled on a two month learning plan before I signed up, where I brushed up on:

  * Operating System Basics (Linux and Windows commands)
  * Bash, Batch and Python Scripting
  * Networking basics like TCP/IP, ports and protocols
  * Web application attacks
  * Metasploit Framework payloads and modules
  * Privilege escalation techniques

I also found the book ["Penetration Testing: A Hands-on Introduction to Hacking"](https://www.amazon.com/Penetration-Testing-Hands-Introduction-Hacking/dp/1593275641/) by Georgia Weidman extremely handy, as it covers almost all concepts of pentesting that OSCP entails. Finally, I worked on some machines from [vulnhub.com](https://www.vulnhub.com) to test the [pentesting] waters, so to speak. A few I highly recommend are:
  * [Kioptrix Level 1](https://www.vulnhub.com/entry/kioptrix-level-1-1,22/)
  * [Kioptrix Level 1.1](https://www.vulnhub.com/entry/kioptrix-level-11-2,23/)
  * [Kioptrix Level 1.2](https://www.vulnhub.com/entry/kioptrix-level-12-3,24/)
  * [pWnOS v2.0](https://www.vulnhub.com/entry/pwnos-20-pre-release,34/)
  * [SickOs 1.1](https://www.vulnhub.com/entry/sickos-11,132/)
  * [SickOS 1.2](https://www.vulnhub.com/entry/sickos-12,144/)
  * [Tr0ll 1](https://www.vulnhub.com/entry/tr0ll-1,100/)
  * [Tr0ll 2](https://www.vulnhub.com/entry/tr0ll-2,107/)
  * [LordOfTheRoot](https://www.vulnhub.com/entry/lord-of-the-root-101,129/)

## The Course:

The course did not start immediately after registering for it; I had a 20 day wait before I received the lab access and the manuals. I purchased the course with 90 days of lab access, since I have a family and a full time job and couldn't dedicate all my time to studying. I decided that I would try and put in about 6 hours a day (Yes, I was very sleep-deprived!) and try to hack atleast 40 machines in 70 days.


![So It Begins](images/soitbegins.jpg)

## Around the labs in 90 days!

The first 15 days flew past going through the course material (a 300 odd page PDF and instruction videos), working on exercises and before I knew it, I was scanning my very first victim machine. The labs contain 50 odd machines, some of which are in the public network and some, well, other networks ([This](https://www.offensive-security.com/images/pwk-lab-net-intro1.png) was the setup!) and I didn't know where to start. Knocking off systems in the order of IPs made sense to me, which is what I did. After rooting a machine, the suggestion is to check for interesting files, passwords, documents, and anything else that looks relevant (even love letters!). Whenever I encountered a machine that I was stuck on for too long, I moved on to the next. I had decided to look at the forums only if I was stuck for longer than 5 hours and looking back, that was the right decision. The lab environment was fantastic. There were all kinds of servers from XP to Solaris and everything in-between. There were a range of software and services running on these machines, which allowed me to experiment with various vulnerabilities and exploits. Towards the end of one month, I had an attack-plan in hand (enumerate, research everything, enumerate again, decide possible attack vectors and keep Metasploit usage to the minimum!)and the number of owned machines was soon in double digits. The progress was steady and I learned tons of new attack vectors and privilege escalation techniques.

Have you ever laughed and cried at the same time and wondered whether you are going insane? Well, some machines in the labs might do that to you. Having grabbed some of the 'low hanging fruit' machines, the picture of me cruising through the labs was flashing before my eyes, when BOOM! I reached some of the dreaded boxes (Pain, Sufferance etc.). It took a hellacious tenacity, extensive research and some serious hard-work to crack some systems and I loved and hated every minute of it. 

![coffee](images/coffee.gif)

Toward the end of the second month, I made significant progress. I discovered the IT, Development and Admin subnet, which can only be accessed via certain hosts that were dual-homed. I pivoted into those networks and owned as many hosts as I could in the limited timeframe I had. There are also systems which can only be compromised through other machines, which is what makes the OSCP labs so..life-like! In 65 days, I had cracked 43 machines, completed the buffer overflow exercises and was geared up for the exam.

![exam](images/exam.jpg)

## The Exam:

To keep myself motivated, I booked the exam date 70 days into my lab time. And the dreaded Friday had arrived! I received an e-mail with the connection details at 1630 sharp and I was off! I decided to start with a low hanging fruit to keep my spirits up. In an hour, I had rooted the first machine and grabbed 25 points! I moved on and in the next 9 hours I had exploited an additional 3 machines and had accumulated enough points to pass the exam. At that stage, I knew I needed to catch up on some much needed sleep. After a 5 hour break, I was rested enough to tackle the last machine. It was frustrating and extremely tricky with a lot of rabbit-holes. With 2 hours left for the 24 hours to run out, I finally cracked the last machine! I spent the remaining time double-checking all my screenshots and exam notes before tackling the report. At 1615, my VPN connection dropped. With enough breaks and sleep, I completed the report by early Sunday morning and submitted it.

Two days later, I got a confirmation e-mail from OffSec that I had fulfilled all criteria necessary for the certification and I was now OSCP certified!

![trex](images/trex.jpg)

## Tips and Tricks:

A few things that I gathered during both the course and the exam:

  * **Holler**: Ensure your family, friends and significant other understand the dedication required to complete this course.
  * **Enumerate**: All forums and blogs talk about this, and I would like to re-hash how important this is! It will save you a ton of time in both the labs and the exam.
  * **Stay Organized**: Use a good note-keeping application and keep clear and concise notes (I used Evernote). Re-organize and clear up your Kali workspace and maintain a good folder structure for exploits and shells. This will really speed you up during your exam!
  * **Keep it Manual**: Keep your Metasploit, Meterpreter and scanner use to a minimum
  * **Manage your Time**: Have a clear time-line for your labs and try to stick to it. Planning ahead will allow you to maximize utilization of the lab time. During the exams, map out how long you would want to spend on each machine and move on to the next if stuck.
  * **Work Hard**: There is no need to spell this out :)
  * **Take a Day Off**: Kick back and relax a day before your exam. Your brain needs the time off, trust me!
  * **Have Fun**: This course is extremely unique. It can be frustrating and fulfilling at the same time but the satisfaction is incomparable! You will really miss the labs at the end of your certification and you should really make the most of it. Hack as many machines as possible in as many ways as you can; you will not come across a playground like this often.

## In Conclusion:

This certification is unbelievably rewarding, albeit rather demanding. Like the OffSec motto, you will really need to "Try harder". The course completely changes your perspective of security and approach to pentesting. The OSCP journey is full of highs and lows and I enjoyed every bit of it!


*-TinyRex*
