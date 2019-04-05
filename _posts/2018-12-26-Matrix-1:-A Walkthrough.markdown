---
---

I came across the Matrix:1 VM hosted on VulnHub by Ajay Verma and decided to give it a whirl. With a difficulty level of Intermediate, one flag to find and content inspired by the Matrix trilogy, this was a fun machine to root. You can download this VM from [here](https://www.vulnhub.com/entry/matrix-1,259/) and stop reading if you want to complete the challenge yourself!

## Host Discovery

I started off with a network scan to find the target with netdiscover.

{% highlight code %}
netdiscover -r 192.168.83.0/24
{% endhighlight %}
![Netdiscover](images/matrix/Netdiscover.png)

There it is: `192.168.83.134`

## Information Gathering
My next step was to scan the machine with Nmap and discover the open ports.
![nmap](images/matrix/Nmap.png)
Nmap found 22/tcp, 80/tcp and 31337/tcp open. On accessing port 80, at the first glance, it seemed like there was nothing. Except, a message asking us to "Follow the White Rabbit". On closer inspection, I noticed a tiny rabbit on the bottom of that page and I decided to inspect it.
![port80](images/matrix/Port80.png)
I then saw that the name of that image was "p0rt_31337".
![whiterabbit](images/matrix/whiterabbit.png)
Taking the hint, I  decided to move on and take a look at the webpage on port 31337. Nothing caught my attention on the Cypher webpage. However, on inspecting the page source, I found a string that looked like it was Base64 encoded.
![port31337](images/matrix/port31337.png)
![source](images/matrix/viewsource.png)
Passing it through a decoder gave me: *echo "Then you'll see, that it is not the spoon that bends, it is only yourself." > Cypher.matrix*. This looked like a command in bash to save the string "Then you'll see, that it is not the spoon that bends, it is only yourself." into the file "Cypher.matrix".

On accessing the URL [http://192.168.83.134:31337/Cypher.matrix](http://192.168.83.134:31337/Cypher.matrix), I downloaded a file containing some junk (or what I thought was junk) characters. 
![brainfuck](images/matrix/brainfuck.png)

## Hmm..
This is where I got stuck for a while and I was not really sure of what to make of that file or its contents. Finally, after some Google-Fu, I found [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck), a programming language known for its minimalism. I used an online interpreter and deciphered the file:
![bfdecode](images/matrix/bfdecode.png)

So the file said that the password of the user "guest" starts with `k1ll0r` and we need to figure out the last two characters. I decided to use [Crunch](https://tools.kali.org/password-attacks/crunch) and create a wordlist, with all possible combinations, that I can use as a password list for bruteforcing the SSH service. 
![crunch](images/matrix/crunch.png)

After the wordlist was created I proceeded to bruteforce the SSH service using Medusa.
Success! The password is `k1ll0r7n`.
![passwordfound](images/matrix/passwordfound.png)


## Restricted Shell

After accessing the server via SSH, I found myself in a frustrating restricted shell - rbash.

![rbash](images/matrix/rbash.png)

I logged out and logged back in using the following command, making use of the fact that SSH executes appended commands upon login, and broke out of rbash.

{% highlight code %}
ssh guest@192.168.83.134 -t "bash --noprofile"
{% endhighlight %}
![breakrbash](images/matrix/breakrbash.png)

## Root!
On enumerating the machine, I checked the sudo configuration for the guest user and found that the user has permissions to execute all available commands:
![sudo](images/matrix/sudo.png)
It was then a matter of just switching to the root user!
![root](images/matrix/root.png)

## Flag!
Well, here is the flag!
![flag](images/matrix/flag.png)

