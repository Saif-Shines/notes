# Complete Intro to Linux and the Command-Line

[Complete Intro to Linux and the CLI](https://btholt.github.io/complete-intro-to-linux-and-the-cli/)

[btholt/complete-intro-to-linux-and-the-cli](https://github.com/btholt/complete-intro-to-linux-and-the-cli)

<aside>
💡 Hey, when writing scripts... People refer to `!`  as `bang`. I wondered why they call it like that. Turns out, it's because, in 1950s comic books referred them as 'bang' in shoot out scene.

</aside>

 to make a tar file without compressing

```bash
tar -cf archive.tar textfile.txt folder1

```

to make compressed tar file

```bash
tar -zcf archive.tar.gz textfile.txt folder1/
# tar -flags -nameOfZippedFile contentsOf filesAndFolders
```

extract tar

```bash
tar -xzf archive.tar.gz  -C dest #dest should be created already
```

create files using wildcards

```bash
touch file-{ny,in,us}.txt # creates 3 files as bash will give it as 3 arguments to touch cmd program

ls file-* # lists all the files starting with file-*

ubuntu@primary:~$ echo {a..z}
a b c d e f g h i j k l m n o p q r s t u v w x y z

ubuntu@primary:~$ echo {a..z}{1..4}
a1 a2 a3 a4 b1 b2 b3 b4 c1 c2 c3 c4 d1 d2 d3 d4 e1 e2 e3 e4 f1 f2 f3 f4 g1 g2 g3 g4 h1 h2 h3 h4 i1 i2 i3 i4 j1 j2 j3 j4 k1 k2 k3 k4 l1 l2 l3 l4 m1 m2 m3 m4 n1 n2 n3 n4 o1 o2 o3 o4 p1 p2 p3 p4 q1 q2 q3 q4 r1 r2 r3 r4 s1 s2 s3 s4 t1 t2 t3 t4 u1 u2 u3 u4 v1 v2 v3 v4 w1 w2 w3 w4 x1 x2 x3 x4 y1 y2 y3 y4 z1 z2 z3 z4
```

streams

```bash
cat newfile.txt 1>> thirdfile.txt  # appends
cat newfile.txt 1> anotherfile.txt # (re)writes
cat non-existant-file.txt 2> error.txt # it writs stderr to file which doesn't happen with 1>
ls -lash 1> ls.txt 2>ls-error.txt # if stdout goes ls.txt and stderr goes to ls-error.txt
ls -lash > ls.txt #writes stdout and stderr to same file
cat somefile.txt 2> /dev/null # /dev/null is a file that like black hole.
# in the above case, we are basically saying only pring 1> stdout to console and rest of 2> to black hole
```

pipe

```bash
yes y | rm -i file*

> # is for file
| # is for programs
```

- If you make changes to `.bashrc` and you don't want to wait until next terminal session to be run, then try running

```bash
source ~/.bashrc
```

<aside>
💡 The difference between `~/.bashrc` and `~/.bash_profile` is that `/.bash_profile` only run once as terminal session opens up.

</aside>

Move process foreground and background...

```bash
ubuntu@primary:~$ sleep 1000
^Z
[1]+  Stopped                 sleep 1000
ubuntu@primary:~$ jobs
[1]+  Stopped                 sleep 1000
ubuntu@primary:~$ bg 1
[1]+ sleep 1000 & # sends it to background
ubuntu@primary:~$ ps
    PID TTY          TIME CMD
  11135 pts/0    00:00:00 bash
  11247 pts/0    00:00:00 sleep
  11248 pts/0    00:00:00 ps
ubuntu@primary:~$ fg 1
sleep 1000
^Z
[1]+  Stopped                 sleep 1000
ubuntu@primary:~$ jobs -l
[1]+ 11247 Stopped                 sleep 1000
ubuntu@primary:~$ jobs
[1]+  Stopped                 sleep 1000
```

how to check if previous process has completed sucessfully. if `echo $?` print `0`then it successfully.

```bash
ubuntu@primary:~$ date
Sun Feb  7 16:18:22 IST 2021
ubuntu@primary:~$ echo $?
0
ubuntu@primary:~$
```

<aside>
💡 run a command on if one half of it succesfully complete

</aside>

```bash
ubuntu@primary:~$ ls
Home         extracted  folder2  less         snap            streams       yoyo
archive.tar  folder1    folder3  oldfile.txt  something-else  textfile.txt
ubuntu@primary:~$ touch status.txt && date >> status.txt && uptime >> status.txt
ubuntu@primary:~$ cat st
status.txt  streams/
ubuntu@primary:~$ cat status.txt
Sun Feb  7 16:22:20 IST 2021
 16:22:20 up  7:57,  1 user,  load average: 0.00, 0.00, 0.00
```

There is an `uptime command` which tells me how much time my computer is uptime so far.

```bash
ubuntu@primary:~$ uptime
 16:22:38 up  7:57,  1 user,  load average: 0.00, 0.00, 0.00
```

and n or

```bash
ubuntu@primary:~$ false && echo hi
ubuntu@primary:~$ true && echo hi
hi
ubuntu@primary:~$ false || echo hi
hi

ubuntu@primary:~$ true ; false ; echo hi # execute each no matter what

#subcommand any thing under $(command)
ubuntu@primary:~$ echo I think $(whoami) is a really cool user
I think ubuntu is a really cool user
```

have another vm created by multipass

```bash
multipass launch --name secondary
multipass shell secondary
ubuntu@secondary:~$ sudo useradd -s /bin/bash -m -g ubuntu saif
ubuntu@secondary:~$ sudo passwd saif # set a password
New password:
Retype new password:
passwd: password updated successfully
```

create private and public keys

```bash
ubuntu@primary:~$ ssh-keygen -t rsa # creates a ~/.ssh

# changing permissions of ~/.ssh direcotry
saif@secondary:~$ chmod 700 ~/.ssh
saif@secondary:~$ ls -lash
total 28K
4.0K drwxr-xr-x 3 saif ubuntu 4.0K Feb  7 16:46 .
4.0K drwxr-xr-x 4 root root   4.0K Feb  7 16:38 ..
4.0K -rw-r--r-- 1 saif ubuntu  220 Feb 25  2020 .bash_logout
4.0K -rw-r--r-- 1 saif ubuntu 3.7K Feb 25  2020 .bashrc
4.0K -rw-r--r-- 1 saif ubuntu  807 Feb 25  2020 .profile
4.0K drwx------ 2 saif ubuntu 4.0K Feb  7 16:46 .ssh
4.0K -rw------- 1 saif ubuntu  802 Feb  7 16:46 .viminfo
saif@secondary:~$ chmod 600 ~/.ssh/authorized_keys
```