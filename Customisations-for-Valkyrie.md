The following are customisations of the link, zelda and vis machines ontop of Ubuntu 14.04 installation

####SSH Login without password
http://www.linuxproblem.org/art_9.html

First log in on A as user a and generate a pair of authentication keys. Do not enter a passphrase:

````
a@A:~> ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/a/.ssh/id_rsa): 
Created directory '/home/a/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/a/.ssh/id_rsa.
Your public key has been saved in /home/a/.ssh/id_rsa.pub.
The key fingerprint is:
3e:4f:05:79:3a:9f:96:7c:3b:ad:e9:58:37:bc:37:e4 a@A
```

Now use ssh to create a directory ~/.ssh as user b on B. (The directory may already exist, which is fine):

```
a@A:~> ssh b@B mkdir -p .ssh
b@B's password: 
```

Finally append a's new public key to b@B:.ssh/authorized_keys and enter b's password one last time:

```
a@A:~> cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'
b@B's password: 
```

From now on you can log into B as b from A as a without password:

```
a@A:~> ssh b@B
```

#### SSH alisaes
Add contents like this to .ssh/config
```
Host link
        User val
        HostName 10.185.0.40

Host zelda
        User val
        HostName 10.185.0.41
```


### Management of eth0 interface
Add this contents to /etc/network/interfaces

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
# interfaces(5) file used by ifup(8) and ifdown(8)

# The loopback network interface
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
        address 10.185.0.104
        netmask 255.255.255.0
        gateway 10.185.0.1
        dns-nameservers 129.215.33.247 129.215.91.244
```