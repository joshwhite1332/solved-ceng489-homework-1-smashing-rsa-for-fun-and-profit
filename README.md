Download Link: https://assignmentchef.com/product/solved-ceng489-homework-1-smashing-rsa-for-fun-and-profit
<br>
Overview

In this assignment we will break the RSA encryption system. RSA, is a public-key cryptosystem which is commonly used in secure communication. Some examples of where you can find RSA implementations are Linux package managers(<em>e.g. apt,pacman</em>), SSL and digital signatures. In this homework we will explain and implement two different attacks on RSA.

First attack is one of the simplest attacks that can be performed on RSA and is extremely rare to happen in the real world. But it may be useful if you are interested in cryptography (and/or cybersecurity competitions). It is called ”common modulus attack” and with this attack we can get the plaintext from two different ciphertexts (of the same plaintext).

Second attack is a side channel attack (SCA). In SCAs we don’t try to break the cryptosystem directly, but we try exploit the implementation. We can use the extra information from time measurements, power consumption levels or even acoustic measurements along with many other side channels to find out the plaintext or cipher keys. In this assignment we will perform a power analysis side channel attack on RSA.

<h1>Attack 1: Common Modulus Attack</h1>

Consider the following scenairo:

The sysadmins of CENG issue each person in staff roster with an individual RSA key. To simplify the public key infrastructure (PKI) maintenance for the department, admins decide to generate a single modulus <em>N </em>and each person will be issued a personalized <em>public </em>encryption exponent <em>e</em>, and corresponding <em>private </em>decryption exponent <em>d</em>. Moreover, all key generations are performed by admins. This way the admins will also have a copy of each generated RSA key-set. They swear that they will not spy on us and propose the following reasons:

<ul>

 <li>When someone loses a key, admins can reissue the key. Possibly preventing data loss.</li>

 <li>In case of an internal security leak, admins can easily investigate it with all the keys available to them.</li>

</ul>

Further assume that to speed up the process of generating <em>e</em>, admins decide to select it from a set of pregenerated prime numbers. Therefore the greatest common divisor (gcd) of each generated <em>e </em>pair is 1 [<em>gcd</em>(<em>e<sub>i</sub>,e<sub>j</sub></em>) = 1].

Now suppose that a professor wants to send the exam questions, <em>m</em>, to two assistants of the course. He encrpyts the questions with each asistant’s respective public exponents, <em>e</em><sub>1 </sub>and <em>e</em><sub>2</sub>, to get <em>c</em><sub>1 </sub>= <em>m<sup>e</sup></em><sup>1 </sup>(mod <em>n</em>) and <em>c</em><sub>2 </sub>= <em>m<sup>e</sup></em><sup>2 </sup>(mod <em>n</em>). Later he sends <em>c</em><sub>1 </sub>and <em>c</em><sub>2 </sub>to the assistants.

In the above situation, an eavesdropper can recover the exam questions from <em>c</em><sub>1 </sub>and <em>c</em><sub>2 </sub>using the <em>common modulus </em>attack.

Recall that RSA performs encryption as follows:

<em>C </em>= <em>M<sup>e            </sup></em>(mod <em>N</em>)

We also need to remember <em>B´ezout’s identity</em>.

<strong>B´ezout’s identity. </strong><em>Let a and b be integers with greatest common divisor d. Then, there exist integers x and y such that ax </em>+ <em>by </em>= <em>d. More generally, the integers of the form ax </em>+ <em>by are exactly the multiples of d.</em>

Since we know that <em>e</em><sub>1 </sub>and <em>e</em><sub>2 </sub>are chosen from a set of primes, they are co-prime, that is <em>gcd</em>(<em>e</em><sub>1</sub><em>,e</em><sub>2</sub>) = 1. Then following the B´ezout’s identity, we know that there exists <em>x </em>and <em>y</em>, which satisfies <em>e</em><sub>1</sub><em>x </em>+ <em>e</em><sub>2</sub><em>y </em>= 1. And we can find <em>x </em>and <em>y </em>with the <a href="https://en.wikipedia.org/wiki/Extended_Euclidean_algorithm">Extended Euclidean Algorithm.</a> To sum up everything we know, we have the following:

<em>c</em>1 = <em>m</em><em>e</em>1            (mod <em>n</em>)(m encrpyted with <em>e</em><sub>1</sub>)                               <em>gcd</em>(<em>e</em><sub>1</sub><em>,e</em><sub>2</sub>) = 1

<em>c</em>2 = <em>m</em><em>e</em>2            (mod <em>n</em>)(m encrpyted with <em>e</em><sub>2</sub>)                                 <em>e</em><sub>1</sub><em>x </em>+ <em>e</em><sub>2</sub><em>y </em>= 1

Now onto the attack. Main idea is to take the <em>x</em>th and <em>y</em>th powers of <em>c</em><sub>1 </sub>and <em>c</em><sub>2 </sub>to cancel out the public exponents <em>e</em><sub>1 </sub>and <em>e</em><sub>2</sub>.

(<em>c</em>1)<em>x </em>∗ (<em>c</em>2)<em>y </em>= (<em>m</em><em>e</em>1)<em>x </em>∗ (<em>m</em><em>e</em>2)<em>y                                                                                                      </em>(mod <em>n</em>)

= <em>m</em><em>e</em><sup>1</sup><em>x </em>∗ <em>m</em><em>e</em><sup>2</sup><em>y                                                                                                                 </em>(mod <em>n</em>)

= <em><sub>m</sub></em><em>e</em><sup>1</sup><em>x</em>+<em>e</em><sup>2</sup><em>y                                                                                                                          </em>(mod <em>n</em>)

= <em>m</em><sup>1 </sup>= <em>m                                                                                  </em>(mod <em>n</em>)

That’s it. We broke the RSA. We also now have the exam questions!

Your task is to write a program which finds out the plaintext from the given two ciphertexts, public exponents and the common modulus.

<h2>Program Specifications</h2>

<ul>

 <li>You can write your programs in any language you choose as long as they can be run on inek machines. Python is highly recommended because it makes dealing with huge numbers easy.</li>

 <li>Since you can use any programming language you will have to provide a Makefile containing commands to compile (if necessary) and run your programs. make run command will be run in the directory containing your Makefile during evaluation.</li>

 <li>Your program should read a csv file called csv, which will be placed to the same path with your program, for the values of <em>c</em><sub>1</sub>, <em>c</em><sub>2</sub>, <em>e</em><sub>1</sub>, <em>e</em><sub>2 </sub>and <em>n</em>. First field will be the name of the parameter (i.e. one of the following: c1, c2, e1, e2 or n). Second field will be the value of the parameter in hexadecimal format.</li>

 <li>You should output the plaintext of the message to the stdout and you should output nothing else.</li>

 <li>Submit your programs as a zip file named zip, where XXXXXXX is your student id.</li>

</ul>

<strong>Note: </strong>A sample input/output will be provided.

<h1>Attack 2: Power Analysis SCA</h1>

In this side channel attack we will monitor the power consumption of the chip performing the RSA encryption or decryption to find out the key. Instead of attacking the RSA algorithm itself, we are trying to exploit the implementation of the algorithm. We will focus on one specific implementation error. There also may be other implementation errors which allow different side channels to be exploited.

RSA utilizes modular exponentiation with big numbers. Since we deal with huge numbers exponentiation takes time. So we need an efficient algorithm for this. Luckly we have such an algorithm called <a href="https://en.wikipedia.org/wiki/Exponentiation_by_squaring">exponentiation by squaring</a> (or square and multiply). Psuedo code looks like this:

<strong>function </strong>ModPower(<em>b</em>, <em>exp</em>) <em>r </em>← 1

<strong>for </strong><em>bit </em>in <em>exp </em><strong>do </strong><em>r </em>← <em>r </em>∗<em>r </em>mod <em>n </em>(Squaring step) <strong>if </strong><em>bit </em>== 1 <strong>then </strong><em>r </em>= <em>b</em>∗<em>r </em>mod <em>n </em>(Multiplication step)

<strong>end if</strong>

<strong>end for return </strong><em>r</em>

<strong>end function</strong>

Important thing to take away from this function is that we go over the exponent bit by bit. And remember, we are using keys as exponents to encrypt or decrypt the messages in RSA. You see where this is going?

If you were given a list of squaring and multiplication operations during the execution of this function you can find out the exponent (in our context, the key). For example if the performed operations are [Square, Multiply, Square, Multiply, Square, Square, Square, Multiply] we can easily say that the exponent is 11001:

Square       Multiply     Square       Multiply     Square     Square     Square       Multiply

1                                         1                              0                 0                            1

Now think about the power consumption of the processor during this process. For simplicity assume that the processor only consumes power during squaring and multiplication.

Figure 1: A Sample power trace with performed operations annotated

In the power trace if the power consumption is increased for a short duration bit is 0, and if it is increased for a longer duration bit is 1.

As you can see it is quite straightforward to get the key from a power trace. The program you will write will take in a power trace and a ciphertext and will output the plaintext.

<h2>Program Specifications</h2>

<ul>

 <li>You can write your prorams in any language of your choosing as long as they can be run on inek machines. Python is highy recommended because it makes dealing with huge numbers easy.</li>

 <li>Since you can use any programming language you will have to provide a Makefile containing commands to compile (if necessary) and run your programs. make run command will be run in the directory containing your Makefile during evaluation.</li>

 <li>Your program should read the power trace from a file called trc, which will be placed to the same path with your program. This file will contain the power measurements of a chip during decryption. Each line in the file will be a single power measurement value between 0 and 15 sampled in regular intervals.</li>

 <li>Your program will read a ciphertext encrypted with a public key and the value of <em>n </em>from stdin as a hex string separated by a newline character and will output the corresponding plaintext to stdout. It shouldn’t output anything other than the plaintext.</li>

 <li>Submit your programs as a zip file named zip, where XXXXXXX is your student id.</li>

</ul>

<strong>Note: A sample input/output will be provided.</strong>