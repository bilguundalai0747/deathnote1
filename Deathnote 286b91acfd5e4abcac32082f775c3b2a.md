# Deathnote

This is a machine from vulnhub boot2root.

Машинуудыг bridged network горимд тохируулсаны дараа “netdiscover” command ажилуулна.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled.png)

machine ip : 192.168.1.60

```
nmap -sV 192.168.1.60
```

```
Starting Nmap 7.94SVN ( [https://nmap.org](https://nmap.org/) ) at 2024-04-06 22:08 +08
Nmap scan report for 192.168.1.60
Host is up (0.00057s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
MAC Address: 08:00:27:5F:B2:12 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```
Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .
Nmap done: 1 IP address (1 host up) scanned in 6.99 seconds
```

80 дугаар порт нээлттэй lбайгаа учир browser оор хандая.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%201.png)

error  заасан ч, эндээс   wordpress гэсэн director ажиллаж байгааг мэдэж болно.

attacker machine ийн /etc/hosts дотор

```
192.168.1.60     deathnote.vuln
```

гэсэн мөрийг оруулснаар хандаж болно.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%202.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%203.png)

HINT 

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%204.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%205.png)

**enumeration**хийсэн боловч чухал director олоогүй

```

gobuster dir -u http://deathnote.vuln -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%206.png)

Зage source  дотороос  тодорхой path уудыг харж авсан. Үүнийг wp-scan ашиглаж ч гарган авч болно.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%207.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%208.png)

Зөвхөн 07 director дотор л файл байсан.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%209.png)

hint дээр байсан notes.txt  байна. Мөн  user.txt хэрэг болж магад.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2010.png)

notes.txt

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2011.png)

user.txt

Дахин enumerate хийхдээ wp-scan ашиглан kira гэх valid user байгааг мэдсэн. kira мөн users.txt дээр агуулагдаж байна.

```
wpscan --url 192.168.1.60/wordpress --enumerate
```

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2012.png)

21/ssh  port нээлттэй байгаа мөн бидэнд valid username байна. Дээрээс нь notes.txt дээр боломжит password ууд байна. What could go wrong?

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2013.png)

kira гэдэг user т тохирсон password notes.txt -д байсангүй. 

user.txt дотроос дахин brute force хийхээс.

```
hydra -L users.txt -P notes.txt ssh://192.168.1.60
```

hydra дээр олон утгуудаас brute force хийлгэхийн тулд -P, -L   option -оор тохируулсан комманд ажилуулна.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2014.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2015.png)

ls 

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2016.png)

fuckmybrain

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2017.png)

```
i think u got the shell , but you wont be able to kill me -kira
```

huh. 

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2018.png)

huuuuuuuuh.

Ухаж2 home/opt гэдэг directory дотроос хэрэг болох 2 director байгааг олсон.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2019.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2020.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2021.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2022.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2023.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2024.png)

kira user лүү шилжсэн.

su- Энэ нь хэрэглэгчдэд өөр хэрэглэгчийн хувьд тушаалуудыг гүйцэтгэх боломжийг олгодог. su-ийн хамгийн түгээмэл хэрэглээ бол супер хэрэглэгчийн эрхийг авах явдал юм. Үүнийг ихэвчлэн “super user" гэсэн товчлол гэж андуурдаг ч  “substitute user” гэсэн үгийн товчлол юм.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2025.png)

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2026.png)

just to make sure 😅

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2027.png)

эндээс хархад  /opt дотор аль хэдийн хийдгээ хийцэндээ.

/var ийг шалгая

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2028.png)

😢

zza

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2029.png)

sudo: Энэ команд нь өөрөө супер хэрэглэгчийн эрхтэй програмуудыг ажиллуулахад хэрэглэгддэг.
-l: Энэ нь sudo-д зориулсан туг бөгөөд таны зөвшөөрөгдөх sudo эрхүүдийг жагсаахыг хэлдэг.
sudo -l-г ажиллуулснаар таны нууц үг асуухгүй, гэхдээ энэ нь таны хэрэглэгчийн бүртгэлд хамаарах /etc/sudoers файлын (sudo зөвшөөрлийг хянадаг) мэдээллийг харуулах болно. Энэ файлд ихэвчлэн зөвхөн үндсэн хэрэглэгч хандах боломжтой тул та бүх тохиргоог харах боломжгүй болно.

(ALL: ALL) ALL
Энэ бол хамгийн чухал хэсэг юм. Энэ нь "kira"-д зориулсан бодит sudo эрхүүдийг тодорхойлдог. Үүнийг хэрхэн тайлбарлах вэ:

(ALL: ALL): Энэ хэсэг нь "kira" нь систем (ALL) дээр дурын хэрэглэгч (ALL) байдлаар дурын командыг (ALL) ажиллуулж чадна гэсэн үг юм.
ALL: Энэ нь өмнөх тодорхойлолт нь бүх тушаалд хамаатай гэдгийг онцолж байна.
Илүү энгийнээр хэлбэл:

Гаралт нь "kira" нь "deathnote" дээр хязгаарлалтгүй sudo хандалттай болохыг харуулж байна. Тэд sudo эрхтэй систем дээрх дурын хэрэглэгчийн хувьд дурын командыг ажиллуулж болно.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2030.png)

from the deployed machine.

![Untitled](Deathnote%20286b91acfd5e4abcac32082f775c3b2a/Untitled%2031.png)