---
layout: post
title:  "ae27ff wargame challenges 6-10 Walkthrough"
date:   2019-3-26
excerpt: "a writeup of how to solve ae27ff wargame challenges 6-10"
image: "/images/2019-3-15-4-46-23.jpg"
category: writeup
tags: [ae27ff, wargame]
comments: false
---

## ae27ff  wargame challenges 6-10 sloutions


<p>Decided to write the sloutions i used to solve <a href="http://ae27ff.meme.tips/">ae27ff</a> wargame challenges.</p>
>  <a href="http://ae27ff.meme.tips/">ae27ff</a> is a set of levels that simulate challenges and puzzles that one may encounter during an ARG (Alternate Reality Game) including simple ciphers, steganography, different types of encodings, and familiarity with internet resources.
> Each level consists of some text, images, data, or files that is intended to lead you to the next page with some amount of investigation.
<br><br>

### #6 Defeated by brutes! [Crypto]:

> Hvs doggkcfr wg lobori

From the challenge name this is a crypto challenge, the given string looks like it was encoded with an <a href="https://en.wikipedia.org/wiki/ROT13">ROT cipher</a> so first i googled it and found <a href="http://hot-cross--puns.tumblr.com/post/132751979982/parsing-code-caesarshift-key-12">a post on tumblr</a> where someone decrypted a string that contains the word "doggkcfr" with ROT12 therefore i decided to try to decrypt my string using ROT12 with href="https://gchq.github.io/CyberChef/">CyperChef</a> and got:
~~~
Input:  Hvs doggkcfr wg lobori
Recipe: ROT12
Output: The password is xanadu
~~~
to level 7!.

### #7 xanadu [Crypto]:

> Qhr nhrq lrvhf fs uoulfbye. Ln oenlos fs. Eedfiy. V mhuk... tuaw'm qhr pdmpwbrg: blreiefb
> 
> The key has already been given to you.

Another crypto challenge I started with looking up the title "xanadu" and found the Wikipedia page on <a href="https://en.wikipedia.org/wiki/Project_Xanadu">project Xanadu</a>:
> Project Xanadu was the first hypertext project, founded in 1960 by Ted Nelson. Administrators of Project Xanadu have declared it an improvement over the World Wide Web, with mission statement: "Today's popular software simulates paper. The World Wide Web (another imitation of paper) trivialises our original hypertext model with one-way ever-breaking links and no management of version or contents."[1]
> 
> Wired magazine published an article called "The Curse of Xanadu", calling Project Xanadu "the longest-running vaporware story in the history of the computer industry".[2] The first attempt at implementation began in 1960, but it was not until 1998 that an incomplete implementation was released. A version described as "a working deliverable", OpenXanadu, was made available in 2014. 

While it was a fun read, it didn't help much though maybe there is a reference am missing. The given string seems to be an encrypted alphabetic text similar to Caesar cipher, but there are a password and it's given to us. At first, I thought the password was "blreiefb" because of the common format "text/email: password" and ended up wasting time then decided to make sure what kind of encryption is used and a found a handy tool called <a href="https://github.com/nccgroup/featherduster">FeatherDuster</a>:
> FeatherDuster is a tool for breaking crypto; It tries to make the process of identifying and exploiting weak cryptosystems as easy as possible. Cryptanalib is the moving parts behind FeatherDuster, and can be used independently of FeatherDuster.

 and decided to give it a try:
~~~
$: featherduster xanadu.txt                                                      (master|…) 6:38:34
Welcome to FeatherDuster!

To get started, use 'import' to load samples.
Then, use 'analyze' to analyze/decode samples and get attack recommendations.
Next, run the 'use' command to select an attack module.
Finally, use 'run' to run the attack and see its output.

For a command reference, press Enter on a blank line.


FeatherDuster> analyze 
[+] Analyzing samples...
[+] Messages may be encrypted with a stream cipher or simple XOR.
[!] Individual messages have failed statistical tests for randomness.
[!] This suggests weak crypto is in use.
[!] Consider running single-byte or multi-byte XOR solvers.

[+] Suggested modules:
   alpha_shift          - A brute force attack against an alphabetic shift cipher. 
   base_n_solver        - A solver for silly base-N encoding obfuscation.          
   single_byte_xor      - A brute force attack against single-byte XOR encrypted ciphertext.
   multi_byte_xor       - A brute force attack against multi-byte XOR encrypted ciphertext.
   many_time_pad        - A statistical attack against keystream reuse in various stream ciphers
                          , and the one-time pad.
   vigenere             - A module to break vigenere ciphers using index of coincidence for
                          key length detection and frequency analysis.
~~~ 
I did not really need to use the tool but, it confirmed that the encryption used is <a href="https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher">Vigenère cipher</a> so I went back to <a href="https://gchq.github.io/CyberChef/">CyperChef</a> and used <a href="https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher">Vigenère Decode</a> and tried with the passphrase "blreiefb" but it was wrong my second guess was the name of the challenge "xanadu":
~~~
Input: Qhr nhrq lrvhf fs uoulfbye. Ln oenlos fs. Eedfiy. V mhuk... tuaw'm qhr pdmpwbrg: blreiefb
Recipe: Vigenère Decode, Key = "xanadu"
Output: The next level is horrible. It really is. Really. I mean... that's the password: horrible
~~~

found the password for the next level "horrible" bring it on.

### #8 Last-minute calculations [Math,Unknown]:

> It seems that one of our developers has been helping candidates cheat on the test; seems it was a mistake to transfer him from Finance. We found the following note being passed, but we don't know what it means.
> 
> Maybe you could figure it out for us?
<span class="image fit"><img src="{{ "/images/horrible.jpg" | absolute_url }}" alt="" /></span>

This is a math problem so I started by using bc:
~~~
┌ ~
└> % bc                                                                                       8:54:42
bc 1.07.1
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'. 
(2 * 3922138 - 18368) / 1101
7108
~~~
the calculated number is 7108 there is a hint in the picture that says “flip!” because this is a math problem, and the output is an integer I remembered how you could spell words in a calculator, but I didn’t have one next to me, so I wrote a python script that takes in a number and gets the letters equivalent to the digits and flips it:
~~~ python
#! /usr/bin/python3
'''
It's known that when digital digits are looked at upside down they resemble an English letter.
this script given a number it will convert it to English letters using the calculation:
Number  	 Letter
0       	 O/D
1       	 l
2       	 Z
3       	 E
4       	 h
5       	 S
6       	 g
7       	 L
8       	 B
9       	 G
example : 
    number  = 77345
    letters = LLESH
    word    = SHELL
'''
info = "\n" \
    " ▄▄▄▄    ▄▄▄      ▓█████▄ ▓█████  ██▀███  \n" \
    "▓█████▄ ▒████▄    ▒██▀ ██▌▓█   ▀ ▓██ ▒ ██▒\n" \
    "▒██▒ ▄██▒██  ▀█▄  ░██   █▌▒███   ▓██ ░▄█ ▒\n" \
    "▒██░█▀  ░██▄▄▄▄██ ░▓█▄   ▌▒▓█  ▄ ▒██▀▀█▄  \n" \
    "░▓█  ▀█▓ ▓█   ▓██▒░▒████▓ ░▒████▒░██▓ ▒██▒\n" \
    "░▒▓███▀▒ ▒▒   ▓▒█░ ▒▒▓  ▒ ░░ ▒░ ░░ ▒▓ ░▒▓░\n" \
    "▒░▒   ░   ▒   ▒▒ ░ ░ ▒  ▒  ░ ░  ░  ░▒ ░ ▒░\n" \
    " ░    ░   ░   ▒    ░ ░  ░    ░     ░░   ░ \n" \
    " ░            ░  ░   ░       ░  ░   ░     \n" \
    "      ░            ░                      \n" \
    " Github : https://github.com/bad3r         \n" \
    " Twitter: https://twitter.com/bd3r_       \n" \
    " Site   : https://bad3r.xyz/              \n"


def getLetters(st):
    dict = {0: ')O/D(', 1: 'I', 2: 'Z', 3: 'E', 4: 'H', 5: 'S'
            , 6: 'G', 7: 'L', 8: 'B', 9: 'G'}

    letters = ""
    for num in map(int, st):
        letters += dict[num]

    return letters

def main():
    print(info)
    print("Enter a number:")
    letters = getLetters(input().strip())

    print("word = " + letters[::-1].lower())


if __name__ == '__main__':
    main()
~~~

then I ran the script:
~~~ bash
┌ ~
└> % python3 ./numToStringCalculator.py  
      
 Github : https://github.com/bad3r         
 Twitter: https://twitter.com/bd3r_       
 Site   : https://bad3r.xyz/              


Enter a number:
7108
word = b(d/o)il
~~~

The output is "b(d/o)il" the 2nd letter could be either 'd' or 'o' if it's 'o' the word would be "boil" tested it out and that was the password.


### #9 Darkest darkness [Steg]:

> <span class="image fit"><img src="{{ "/images/boil.png" | absolute_url }}" alt="" /></span>

All you get for this challenge is a black picture, and the title is "Darkest darkness" made me think that all I have to do is reset the color table; therefore, I opened Gimp and loaded the picture after that from the menu at the top:

Colors --> Auto --> equalize

and a word showed up on the screen "rosebud" and what do you know its the password for the next level :).
Note that there is other ways to solve this you can retrieve the message by restoring the color table in a graphics editor like Gimp (Colors → Map → Set Color Map) or by editing the data directly (change bytes 13, 14 and 15 of the file from 000000 to ffffff).

<span class="image fit"><img src="{{ "/images/boil-solved.png" | absolute_url }}" alt="" /></span>

### #10 Resource Locator [Web]:

> WhA7h8Qw   Q4USYkzu   PhVM3EzQ
> LDeRGXQL   L7NYT5a8   KtvBT5uZ
> GWsMqMzu   8JPtM9a3   hgcBHHWf
> Tk5rSPdj   eB44EE74   2uwUxyMK
> UyjHFuLP   jGJuVGiK   JMFpnqB7
> Kfdew6kp   ECfMJt4E   HFdwhM9T
> 7vDsdaNP   V2fEJ3Wu   YYbbiB2e
> 8GrEu7mH   k67HHw7d   aKbVhkaz
> pyWfzjBk   tTwnNqMR   7mF48BCT
> muMDb92e   wd6UZvtG   Xcdx25Mv
> AuFGZVDv   B3zJNaCV   EcVM7uxx

Some random strings, not much to go by. The title is “Resource Locator,” and it’s a web challenge which made me think of URL (Uniform Resource Locator) and that was the first hint. Decided to check the web page HTML using the <a href="https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Open_the_Inspector">inspect element tool</a> and found the second hint:
~~~ html
<!--c2 r5   ->   Pbin-->
<!--

optional hint:

150 157 167 40 151 163 40 164 150 145 40 151 156 146 157 162 155 141 164 151 157 156 40 157 156 40 164 150 145 40 160 141 147 145 40 141 162 162 141 156 147 145 144 77 40 167 150 141 164 40 164 145 162 155 163 40 144 145 163 143 162 151 142 145 40 164 150 151 163 40 141 162 162 141 156 147 145 155 145 156 164 77 40 144 157 145 163 40 164 150 141 164 40 143 157 162 162 145 163 160 157 156 144 40 164 157 40 171 157 165 162 40 150 151 156 164 77 12 122 145 155 145 155 142 145 162 40 164 150 141 164 40 171 157 165 40 143 141 156 40 146 151 156 144 40 164 151 160 163 40 150 145 162 145 72 40 150 164 164 160 72 57 57 160 141 163 164 145 142 151 156 56 143 157 155 57 124 164 147 151 156 62 166 115 40 50 146 162 157 155 40 154 157 147 151 156 40 160 141 147 145 51
-->
~~~
The critical thing to note here is that two hints are given above; the hint and the optional hint.
Started with decoding the optional hint using <a href="https://gchq.github.io/CyberChef/">CyperChef</a> it looked like octal data format when converted I got:
~~~
how is the information on the page arranged? what terms describe this arrangement? does that correspond to your hint?
Remember that you can find tips here: http://pastebin.com/Ttgin2vM (from login page)
~~~
The pastebin leads to the wargame rules/tools page but on pastebin which made me notice the resource on the pastebin URL “Ttgin2vM” that looks like the strings given to us in the challenge. And the hint mentions how the given data is arranged also given from the hint “c2 r5” that's column2 and raw5 in the matrix == “jGJuVGiK”.

next i went to https://pastebin.com/jGJuVGiK and found:
~~~
    6F 66 20 74 68 65 20 73 70 65 63 69 65 73 20 52 61 6E 67 69 66 65 72 20 74 61 72 61 6E 64 75 73
~~~
That looked like a hex code to me so I converted it in <a href="https://gchq.github.io/CyberChef/">CyperChef</a>:
~~~
of the species Rangifer tarandus
~~~
I googled "Rangifer tarandus" and it's another name for "reindeer" and that was the password for this level.

### #10 Bonus achievement (Leave No Stone Unturned): 

I decided to test the rest of the resources in challenge #10, but instead of doing it manually I wrote a script that takes in the data and test each resource and if it's valid then it will save the data from the pastebin into a test file:
~~~ python
import io
import urllib.request
from urllib.request import urlopen
from bs4 import BeautifulSoup
import gzip


'''

script that takes in a matrix of resources for pastebin.com example:
WhA7h8Qw   Q4USYkzu   PhVM3EzQ
LDeRGXQL   L7NYT5a8   KtvBT5uZ
GWsMqMzu   8JPtM9a3   hgcBHHWf
Tk5rSPdj   eB44EE74   2uwUxyMK
UyjHFuLP   jGJuVGiK   JMFpnqB7
Kfdew6kp   ECfMJt4E   HFdwhM9T
7vDsdaNP   V2fEJ3Wu   YYbbiB2e
8GrEu7mH   k67HHw7d   aKbVhkaz
pyWfzjBk   tTwnNqMR   7mF48BCT
muMDb92e   wd6UZvtG   Xcdx25Mv
AuFGZVDv   B3zJNaCV   EcVM7uxx

where each one of those could be a valid resource example:
http://pastebin.com/jGJuVGiK

the script test each URL and if its valid it will save the data to
the output file ./pastebinResoults.txt

'''
info = "\n" \

    " Github : https://github.com/bad3r         \n" \
    " Twitter: https://twitter.com/bd3r_       \n" \
    " Site   : https://bad3r.xyz/              \n"


def fetch_page(page):
    response = ""
    try:
        response = urlopen(page)
        if response.info().get('Content-Encoding') == 'gzip':
            response_buffer = io.StringIO(response.read())
            unzipped_content = gzip.GzipFile(fileobj=response_buffer)
            parsed_html = BeautifulSoup(unzipped_content.read(), "html.parser")

            return str(parsed_html)
        else:
            parsed_html = BeautifulSoup(response.read(), "html.parser")
            return str(parsed_html)

    except urllib.error.URLError:
        return response


def main():
    print(info)
    print("Enter file name:")
    fp = open(input().strip(), 'r')
    data = fp.read()
    resources = data.split()
    raw_url = 'http://pastebin.com/raw/'
    output = open('pastebinResults.txt', 'w+')

    print("Output will be saved to the file ./pastebinResults.txt")

    for res in resources:
        print("testing URL: " + raw_url + res)
        url = raw_url + res
        txt = fetch_page(url)
        if len(txt) > 0:
            output.write("####################################################################\n")
            output.write('URL: ' + url + '\n')
            output.write("####################################################################\n")
            output.write(txt + '\n')

    print("Done!")
    fp.close()
    output.close()
    with open('pastebinResults.txt', 'r') as out:
        print(out.read())

if __name__ == '__main__':
    main()
~~~
ran the script with the given data:
~~~ bash
┌ ~
└> % python3 ./pastebinResourceLocator.py    

 Github : https://github.com/bad3r         
 Twitter: https://twitter.com/bd3r_       
 Site   : https://bad3r.xyz/              

Enter file name:
resources
Output will be saved to the file ./pastebinResults.txt
testing URL: http://pastebin.com/raw/WhA7h8Qw
testing URL: http://pastebin.com/raw/Q4USYkzu
testing URL: http://pastebin.com/raw/PhVM3EzQ
testing URL: http://pastebin.com/raw/LDeRGXQL
testing URL: http://pastebin.com/raw/L7NYT5a8
testing URL: http://pastebin.com/raw/KtvBT5uZ
testing URL: http://pastebin.com/raw/GWsMqMzu
testing URL: http://pastebin.com/raw/8JPtM9a3
testing URL: http://pastebin.com/raw/hgcBHHWf
testing URL: http://pastebin.com/raw/Tk5rSPdj
testing URL: http://pastebin.com/raw/eB44EE74
testing URL: http://pastebin.com/raw/2uwUxyMK
testing URL: http://pastebin.com/raw/UyjHFuLP
testing URL: http://pastebin.com/raw/jGJuVGiK
testing URL: http://pastebin.com/raw/JMFpnqB7
testing URL: http://pastebin.com/raw/Kfdew6kp
testing URL: http://pastebin.com/raw/ECfMJt4E
testing URL: http://pastebin.com/raw/HFdwhM9T
testing URL: http://pastebin.com/raw/7vDsdaNP
testing URL: http://pastebin.com/raw/V2fEJ3Wu
testing URL: http://pastebin.com/raw/YYbbiB2e
testing URL: http://pastebin.com/raw/8GrEu7mH
testing URL: http://pastebin.com/raw/k67HHw7d
testing URL: http://pastebin.com/raw/aKbVhkaz
testing URL: http://pastebin.com/raw/pyWfzjBk
testing URL: http://pastebin.com/raw/tTwnNqMR
testing URL: http://pastebin.com/raw/7mF48BCT
testing URL: http://pastebin.com/raw/muMDb92e
testing URL: http://pastebin.com/raw/wd6UZvtG
testing URL: http://pastebin.com/raw/Xcdx25Mv
testing URL: http://pastebin.com/raw/AuFGZVDv
testing URL: http://pastebin.com/raw/B3zJNaCV
testing URL: http://pastebin.com/raw/EcVM7uxx
Done!
####################################################################
URL: http://pastebin.com/raw/jGJuVGiK
####################################################################
6F 66 20 74 68 65 20 73 70 65 63 69 65 73 20 52 61 6E 67 69 66 65 72 20 74 61 72 61 6E 64 75 73
####################################################################
URL: http://pastebin.com/raw/YYbbiB2e
####################################################################
Death twitches my ear;
'Live,' he says...
'I'm coming.'
~~~
The first link is the sloution for challenge #10 but the second link contains a quote:

> Death twitches my ear;
> 'Live,' he says...
> 'I'm coming.'

With some googling Found that this is a quote by virgil tried that as a passphrase "virgil" and unlocked the achievement "Leave No Stone Unturned".

 > This was not the answer, however you've unlocked an achievement:
 >
 > Leave No Stone Unturned