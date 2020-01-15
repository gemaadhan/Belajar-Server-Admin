# **KONSEP TERMINOLOGI DNS**

## DNS
Domain Name System, adalah sistem yang digunakan untuk memetakan domain ke ip address.

## Domain name
Domain Name adalah nama yang kita gunakan untuk mewakilkan sebuah resource di internet. Contoh nya `google.com, youtube.com, gemaadhan.com`.

## TLD (Top Level Domain)
Bagian paling kanan dari Domain Name, itu yang kita sebut dengan TLD. Contoh TLD yang biasanya digunakan adalah `com, net, gov, edu, io` dan lain lain. TLD ini dikelola oleh pihak pihak yang sudah ditunjuk oleh ICANN. Pihak pihak ini kemudian akan mendistribusikan domain domain (`google, youtube`) di bawah TLD(`com, net, gov`) melalui domain registrar(`rumah web, niagahoster, domanesia`).

## Host
Di dalam domain, pemilik domain bisa menetapkan sebuah host untuk ditunjuk dari sebuah domain. contohnya, `www` dari `www.gemaadhan.com` akan mewakili service atau komputer yang bisa diakses melalui domain `gemaadhan.com`. Pemilihan nama host name ini dibebaskan selama nama tersebut unik dari sisi domain kita.

## Subdomain
Sistem DNS bekerja menggunakan sistem hirarki. Sehingga TLD bisa punya banyak child atau subdomain di bawahnya, contoh nya, TLD `com` bisa membentuk `gemaadhan.com, youtube.com dan ubuntu.com`. Subdomain adalah domain yang menjadi bagian dari domain lain yang lebih besar. Dalam hal ini, `gemaadhan.com` bisa dikatakan adalah subdomain dari `com`.

setiap domain bisa mengontrol subdomain yang berada di bawah nya. ini juga kita sebut sebagai subdomain, Contohnya, kita bisa memiliki subdomain `blog` dari domain `www.blog.gemaadhan.com`, bagian `blog` itulah yang kita sebut subdomain.

## FQDN
Fully Qualified Domain Name adalah yang sebenarnya kita sebut dengan domain secara lengkap. Jadi `www.blog.gemaadhan.com.` bisa kita sebut dengan FQDN. Penulisan FQDN yang benar diakhiri dengan tanda dot. Tapi kadang software yang memanggil FQDN ga perlu dot di belakang, karena sebenarnya titik hanya dibutuhkan untuk memenuhi standar ICANN saja.

## Name server
Name Server adalah komputer yang bertugas memetakan domain ke ip address. Dia lah yang paling banyak bekerja pada sistem DNS. Terkadang karena jumlah domain yang perlu dipetakan terlalu banyak untuk dikerjakan oleh satu server, biasanya server tersebut akan mengarahkan request ke name server lain. Name server akan dikatakan authoritative, jika name server tersebut mempunyai wewenang akan domain tersebut.

## File Zone
Zone file adalah text file yang berisi pemetaan antara domain dan ip address. File zone berada di dalam name server yang biasanya memetakan subdomain atau zone tertentu.

## Records
Record disimpan dalam file zone. Record adalah single mapping antara resource dan nama resourcenya. Record bisa memetakan domain ke ip address, name server ke domain, mail server ke domain, dan lain lain.

## **How DNS Works**

### Root server
Karena sistem DNS bekerja menggunakan sistem hirarki, di ujung atas hirarki ada yang kita sebut dengan root server. Root server ini dikontrol oleh berbagai organisasi/pihak yang didelegasikan oleh ICANN.

Saat ini di seluruh dunia ada 13 root server. Tapi karena ada banyak jumlah domain yang perlu di resolve tiap menit, setiap server ini sebenarnya di mirrorkan ke server lain. Setiap server mirror dari satu root server akan memiliki ip address yang sama dengan root server tersebut. Ketika request ditujukan untuk root server tertentu, request akan diarahkan ke mirror dari root server yang terdekat dengan requester.

Jadi apa tugas root server ? Root server sebenarnya tidak mengetahui atau tidak bisa langsung menjawab jika ada request tentang suatu FQDN. Karena root server hanya memiliki record tentang TLD. Jadi root server hanya akan mengarahkan requester ke name server TLD yang menghandle TLD tertentu.

Misal jika ada request ke `www.gemaadhan.com` ke root server, root server akan mengecek file zone milikya apakah ada record untuk `www.gemaadhan.com` Root server hanya akan menemukan record untuk namserver TLD .com dan memberikan requester alamat dari name server TLD .com.

### TLD server
Requester kemudian mengirim request baru ke name server TLD .com.

Name server TLD .com akan membaca file zone nya, untuk mencari record untuk `www.gemaadhan.com.`. TLD server hanya akan menemukan record untuk nameserver `gemaadhan.com`. dan memberi requester alamat dari nameserver `gemaadhan.com`.

### Domain-level name server
Sampai disini, requester punya ip dari name server yang bertanggung jawab meresolve actual ip address of the resource yaitu name server dari `gemaadhan.com`. Sehingga requester akan mengirim request baru ke name server `gemaadhan.com` , name server `gemaadhan.com` akan melihat di zone file apakah dia punya record untuk `www.gemaadhan.com`.

Name Server `gemaadhan.com` akhirnya menemukan alamat host `www` dari `www.gemaadhan.com`.

### **Resolving Name Server**

Dari tadi kita menggunakan istilah `requester`. Apa sebenarnya requester itu ?

Di hampir semua kasus, requester adalah yang sebenarnya kita sebut dengan resolving name server. Dia adalah name server yang dikonfigurasi untuk melakukan request ke name server lainnya. Resolving name server adalah penghubung antara user dan nameserver2 lain dimana Resolving Name Server melakukan caching query yang pernah dibuat untuk meningkatkan kecepatan proses resolve sebuah domain. Requesting Name Server juga tahu alamat alamat root server agar bisa melakukan request ke root server. Dan melakukan proses yang tadi dijelaskan.

mungkin kita tidak sadar bahwa sebenarnya kita sudah sering menggunakan resolving name server, kalau kita sering menggunakan `8.8.8.8` atau `8.8.4.4` milik google atau resolving name server yang disediakan oleh ISP kita berarti kita sudah pernah menggunakan resolving name server.

ketika kita mengetik url di address bar, komputer kita pertama akan mengecek apakah dia bisa menemukan resource yang diminta secara lokal, dia akan mengecek file hosts pada komputer kita. Kalau tidak ada, komputer kita akan mengirim request ke resolving name server dan menunggu resolving name server mendapatkan ip address dari domain yang kita minta.

resolving name server juga memiliki cache, sebelum melakukan request ke root server dia akan mengecek cache miliknya apakah dia masih punya cache untuk domain yang diminta. Jika tidak ada, dia akan melakukan langkah langkah yang sudah dijelaskan tadi.

## Mengenal File Zone lebih dalam
File zone adalah cara name server menyimpan informasi tentang domain yang dia tahu. Namun kebanyakan request yang datang ke name server bukan lah request yang bisa langsung di jawab oleh name server, karena file zone name server biasanya hanya mengcover 1 domain saja.

jadi semakin banyak file zone yang dimiliki sebuah name server, makin banyak request yang bisa di jawab *authoritatively*.

file zone mendeskripsikan DNS zone, yang merupakan bagian dari keseluruhan sistem penamaan DNS. Pada umumnya digunakan untuk mengatur sebuah domain saja.

parameter $ORIGIN pada zone secara default adalah parameter yang sama dengan otoritas tertinggi. Jadi jika sebuah file zone digunakan untuk mengkonfigurasi gemaadhan.com. $ORIGIN akan di set ke gemaadhan.com.

parameter ini bisa dikonfigurasi di paling atas file zone atau di dikonfig di file konfigurasi DNS server yang mereferens ke file zone.

demikian juga, `$TTL` mengkonfigurasi time to live dari informasi yang disediakan. pada dasarnya `TTL`  adalah sebuah timer. sebuah caching name server bisa menggunakan hasil query sebelumnya untuk menjawab request sampai TTL value nya habis.

## **RECORD TYPES**

Dalam zone file kita bisa punya berbagai macam tipe record.

### SOA RECORD

`START OF AUTHORITY`, atau `SOA` merupakan record wajib yang harus ada di semua zone file. `SOA` harus menjadi record pertama di dalam file. `SOA` akan terlihat seperti ini :

```
gemaadhan.com.  IN SOA ns1.gemaadhan.com. halo.gemaadhan.com. (
                                            12083   ; serial number
                                            3h      ; refresh interval
                                            30m     ; retry interval
                                            3w      ; expiry period
                                            1h      ; negative TTL
)
```
#### Keterangan
**gemaadhan.com** : ini adalah root dari zone. menentukan file zone ini untuk domain gemaadhan.com. seringkali hanya ditulis dengan `@`. yang mana hanya sebuah placeholder yang merepresentasikan konten dari variable $ORIGIN yang kita pelajari sebelumnya.

**IN SOA** : `IN` berarti internet. sedangkan `SOA` adalah indikator bahwa ini adalah ```Start of Authority```.

**ns1.gemaadhan.com** :  menerangkan primary name server untuk domain `gemaadhan.com.` Name server bisa bertipe primary atau secondary.

**halo.gemaadhan.com** : ini adalah email address admin dari zone ini. `@` yang biasanya ada pada email digantikan dengan `dot`. Jika ada dot di dalam email. dot akan digantikan dengan backslash contohnya `halo.bro@gemaadhan.com` menjadi `halo\bro.gemaadhan.com`.

**12083** : ini adalah serial number dari zone file. setiap kali kita edit zone file, kita harus menambahkan angka ini. secondary server akan mengecek apakah serial number primary server untuk zona ini lebih besar daripada serial number zone miliknya. jika iya, secondary server akan merequest file zone baru, jika tidak secondary name server tetap menggunakan file original miliknya.

**3h** : ini adalah refresh interval untuk zone ini. ini adalah jumlah waktu yang secondary server akan tunggu sebelum polling primary zone untuk perubahan pada file.


**30m** : ini adalah retry interval untuk zone ini. jika secondary server tidak bisa terhubung ke primary ketika refresh period habis, dia akan menunggu dalam rentan waktu ini dan retry untuk poll ke primary.

**3w** : ini adalah expire period. jika secondary name server tidak dapat menghubungi primary server dalam rentan waktu ini, dia tidak akan lagi merespon request sebagai authoritative source for this zone.

**1h**
ini adalah jumlah waktu dimana name server akan men cache sebuah name error jika dia tidak bisa menemukan requested name di dalam file ini.

### Record A dan AAAA
kedua record ini memetakan host ke ip address. A untuk ipv4 dan AAAA untuk ipv6.

format A DAN AAAA record akan terlihat seperti ini :

```
host     IN      A       IPv4_address
host     IN      AAAA    IPv6_address
```

karena `SOA` record kita memanggil primary server di `ns1.gemaadhan.com`. record nya akan terlihat seperti ini :

```
ns1     IN  A       111.222.111.222
```

kita tidak harus memasukan full name. kita hanya perlu memasukkan host tanpa FQDN dan DNS server akan fill the rest dengan $ORIGIN value. tapi kita bisa juga menggunakan FQDN jika mau.

```
ns1.gemaadhan.com.     IN  A       111.222.111.222
```
kita juga biasanya menggunakan web server, record nya akan terlihat seperti ini
```
www     IN  A       222.222.222.222
```

kita juga biasanya memberi tahu kemana base domain kita akan ke resolve.

```
gemaadhan.com.     IN  A       222.222.222.222
```

atau untuk merefer ke base domain cukup gunakan  `@`

```
@       IN  A       222.222.222.222
```

kita juga bisa meresolve apapun yang tidak didefine secara explicit dengan menggunakan wildcard

```
*       IN  A       222.222.222.222
```

semua contoh di atas work juga untuk record AAAA.

### CNAME Records

CNAME record digunakan sebagai alias untuk A atau AAAA records.

contoh kita punya A record untuk host server1. kemudian kita bisa menggunakan `www` sebagai alias untuk server1.


```
server1     IN  A       111.111.111.111
www         IN  CNAME   server1
```

be aware alias ini terkadang menimbulkan performance losses karena membutuhkan query tambahan ke server. sebenarnya dengan menggunakan A atau AAAA record saja hasil yang sama juga bisa di dapat.

penggunaan CNAME direkomendasikan untuk menyediakan alias ke resource yang berada diluar zone.

### MX Records
MX record digunakan untuk mail exchanges. ini yang membuat email kita tiba di mail server dengan benar.

tidak seperti tipe record yang lain, mail records umumnya tidak memetakan host ke something, karena MX berlakuk untuk satu zone.

```
IN  MX  10   mail.gemaadhan.com.
```

tidak ada host name pada di awal penulisan MX records. namun ada penambahan nomor disana. nomor tersebut adalah preference number yang mmebantu komputer memutuskan server mana yang harus mengirim email jika ada multiple mail server. makin kecil angka nya makin tinggi prioritasnya.

MX record harus menunjuk ke A atau AAAA record dan bukan yang ditunjuk oleh CNAME.

katakan kita punya 2 mail server. record nya akan seperti ini

```
IN  MX  10  mail1.gemaadhan.com.
IN  MX  50  mail2.gemaadhan.com.
mail1   IN  A       111.111.111.111
mail2   IN  A       222.222.222.222
```

kita juga bisa menulisnya seperti Ini
```
IN  MX  10  mail1
IN  MX  50  mail2
mail1   IN  A       111.111.111.111
mail2   IN  A       222.222.222.222
```

### NS records
digunakan untuk mendefine name server untuk zone ini.

seperti MX record, record ini berlaku untuk satu zone, sehingga tidak perlu menggunakan host.

```
IN  NS     ns1.gemaadhan.com.
IN  NS     ns2.gemaadhan.com.
```

kita paling nggak harus punya 2 name server pada setiap zone untuk kerepluan redudansi. kebanyakan DNS server software menganggap zone file invalid jika cuma ada single name server.
