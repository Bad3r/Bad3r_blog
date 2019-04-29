---
layout: post
title:  "ae27ff wargame Walkthrough 1-5 Walkthrough"
date:   2019-3-25
excerpt: "a writeup of how to solve ae27ff wargame challenges 1-5"
image: "/images/2019-3-15-4-46-23.jpg"
category: writeup
tags: [ae27ff, wargame]
comments: false
---

## ae27ff wargame challenges 1-5 solutions


<p>Decided to write the solutions i used to solve <a href="http://ae27ff.meme.tips/">ae27ff</a> wargame challenges.</p>
>  <a href="http://ae27ff.meme.tips/">ae27ff</a> is a set of levels that simulate challenges and puzzles that one may encounter during an ARG (Alternate Reality Game) including simple ciphers, steganography, different types of encodings, and familiarity with internet resources.
> Each level consists of some text, images, data, or files that is intended to lead you to the next page with some amount of investigation.
<br><br>

### #1 Introductions [Basics]:

> Welcome to the candidate screening process.  Throughout this test you will be confronted with puzzles that will test your competence in a wide variety of subjects.
> Your goal is to approach each puzzle as a means to acquire a password. You should then use this password in a certain way to get to the next page.  All passwords should be entered as lowercase unless otherwise instructed.
> To give you a head-start, the password for this screen is "start". The password for the next screen is "thr3e".  From that you should be able to Address the question of how to advance.
<p>The password for the next level is given "thr3e", this more to make sure you understand how to navigate the site.
To solve this challenge click on the site icon top left then in the "Mobile password entry:"<sup id="fnref:1"><a href="#fn:1" class="footnote">1</a></sup> field write the password "thr3e" to proceed to the next level.</p>

<div class="footnotes">
  <ol>
    <li id="fn:1">
      <p> note that the password field is case sensitive. <a href="#fnref:1" class="reversefootnote">&#8617;</a></p>
    </li>
  </ol>
</div>


### #2 Human Error [Basics]:

> Apologies,
> It appears our developer entered the password for this level incorrectly.
> Maybe it was just a typo and the right screen still exists?

The password we got from level #1 was "thr3e" which is a typo for "three" I entered it and got to level #3.


### #3 DELiberation [Research]:

> Find the number of bits required to represent one character in the original ASCII encoding...
> Multiply it by 1702

1 char is stored in 1 byte = 8 bits. the US-ASCII uses 7 bits per character and the 8th bit is set to 0.
quick math in the terminal using bc:
~~~ bash
$: man bc
NAME:
       bc - An arbitrary precision calculator language
~~~

~~~ bash
$: bc
7 * 1702
11914
~~~

the password for the next level is "11914".


### #4 Popular with spiders [Data]:

> 155 141 156 151 164 157 142 141
> 
> ...if they counted with their legs

The hint is about legs, and the title of the challenge is "Popular with spiders" a spider has 8 legs the equivalent in computing is an octet because it has 8 bits
those numbers separated by a space are the octal values in the <a href="https://www.ascii-code.com/">ASCII table</a> where "155" = "m" and so on.
Instead of doing it manually I used <a href="https://gchq.github.io/CyberChef/">CyperChef</a> to convert from octal to text the output was "manitoba" and now I can move on to the next level.


### #5 Invested Capital [Examination]:

> We've been getting swamped with email spam this week along with another batch of "Nigerian Prince" letters, however the boys in the IT office are telling me we should take a second look at this email in particular.
> Let me know if you find anything interesting.
> >Pardon me, Admirable colleague,
> >
> >Suppose that you had enough money for life, Some might call me crazy - Who is to say whether that is actually the case but you yourself? on the Other hand
> >i am ready to make you a substantial *offer*. Regarding this offer, you only need to call 555-121-1709 and arrange a one-time payment and in 6-8 months
> >receive a substantial payout that will last you a lifetime. Don't you think it is time to fight for yourself? It is! So don't sit there sorry for yourself, call now!
> >
> >This offer is only good for a limited time.  All of the time people throw out one of the best offers - Little do they know what they are missing; Because it
> >looks like spam. Unless you're already rich, you'd be crazy not to take this offer!
> >
> >Thank you for your time.
> 
> (Do not call this phone number - it is fake)


I wasn't sure what to do with this one, but the title sent me in the right direction. The tile is "Invested Capital" made me think it might be related to the capitalized words in the given email, so I decided its time to practice some python and write a simple script that takes in the text and returns to me the words that start with a capital letter:

~~~ python
#! /usr/bin/python3
# script that prompts for a file  and return the words that start with 
# a capital letter in the given file.

def processString(str):

    words = str.split()
    lst = []
    output = ""
    for word in words:
        if word[0].isupper():
            lst.append(word)
            output += word[0]
    print("list of words that start with a capital letter:")
    print(lst)

    print("word combination:")
    print(output)

def main():
    print ("Enter a file name:")
    file = open('' + input().strip(), 'r')
    str = file.read()
    processString(str)
    file.close()



main()
~~~

I saved the email into a text file and ran the scipt:
~~~
$: python3 ./capital-letter-detect.py
Enter file name:
email.txt
list of words that start with a capital letter:
['Pardon', 'Admirable', 'Suppose', 'Some', 'Who', 'Other', 'Regarding', "Don't", 'It', 'So', 'This', 'All', 'Little', 'Because', 'Unless', 'Thank']
word combination:
PASSWORDISTALBUT
~~~
the script returns at the end the combination of those capitalized letters "PASSWORDISTALBUT", the password is "talbut".

It was fun solving those challenges, and I might keep going and write another post for higher level challenges.