# What brainpan.exe does?

I executed [brainpan.exe](https://github.com/cris-m/buffer_overflow_exploit_development/blob/main/assets/resources/brainpan.exe) file on windows machine just to understand what it is doing.

![alt Brainpan on windows machine](../images/brainpan1.png "TCP server")

It is not malicious but it is vulnerable, It is listening for income TCP connection on port ``9999``. From Linux machine, I try to connect to the port. It asked for the password, but I didn't have any password.

![alt Connection attempt](../images/brainpan2.png  "Try to connect to brainpan port")

I tried any password to connect, but I returns  ``ACCESS DENIED`` message. :flushed: :flushed: :flushed:

![alt Password Attempt](../images/brainpan3.png "Input random password")

Now I went on Windows side to see what was happening and, I saw based on printed message that the TCP server received my connection on port ``9999`` with my password. The password is copied to the buffer.

![alt Windows](../images/brainpan4.png "Check connection attempt")

I needed to find the password. After analyzing the binary in Linux using a tool called ``strings``, I saw a hard-coded string called ``shitstorm``. The string result I got can be found in [brainpan.txt](https://github.com/cris-m/buffer_overflow_exploit_development/blob/main/assets/resources/brainpan.txt).

![alt Brainpan analysis](../images/brainpan5.png "Analyse brainpan using strings")

I attempted to connect to the TCP server using ``shitstorm`` as password, I got ``ACCESS GRANTED`` message but the connection was terminated.

![alt Shitstorm](../images/brainpan6.png "Shitstorm password attempt")

Again I went on Windows to see what was happening. I realized that all the input I was sending to the TCP server was copied to the buffer.

![alt Shitstorm](../images/brainpan7.png "Shitstorm password attempt")

When I saw that data was copied to the buffer directly, buffer overflow concept come in my mind.

___

New to [Spiking](https://github.com/cris-m/buffer_overflow_exploit_development/blob/main/assets/documentations/spiking.md)
