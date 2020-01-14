# Terminologi dns

## Tld
Paling kanan dari bagian domain yang dipisahkan oleh titik. Common tld are com,net,gov,edu,io. Pihak tertentu dikasih control terhadap tld oleh icann. Sehingga pihak pihak ini bisa mendistribusikan domain name under tld, biasanya melalui domain registrar.

## Host
`www` dari `www.example.com` merujuk pada service atau komputer yang bisa diakses melalui domain. Pemilihan nama host name can be arbitrary selama nama tersebut unik untuk domain kita.

## Subdomain
Dns bekerja pada sebuah hirarki. Sehingga tld bisa punya banyak domain, contoh nya tld com bisa punya google.com, youtube.com dan ubuntu.com di bawah nya. Subdomain adalah domain yan gmenjadi bagian dari domain lain yang lebih besar. Dalam hal ini google.com bisa dikatakan adalah subdomain dari tld com.

setiap domain bisa mengontrol subdomain yang berada dibawah nya. Ini biasanya yang kita sebut subdomain. Contohnya, kita bisa memiliki subdomain fakultas dari domain domain kampus. Misal www.teknik.undip.ac.id, bagian `teknik` itulah yang kita sebut subdomain.
perbedaan antara host dan subdomain adalah bahwa host merujuk pada service/resource nya, sedangkan subdomain adalah ekstensi dari parent domain nya.

## Fully qualified domain name
Fqdn adalah yang sebenarnya kita sebut dengan domain secara lengkap. Dari contoh sebelumnya “www.teknik.undip.ac.id” merupakan fqdn. Proper fqdn biasanya diakhiri dengan titik, mengindikasikan rott dari hirarki dns. Contoh “www.teknik.undip.ac.id.” Terkadang software yang memanggil fqdn ga perlu titik dibelakang, karena sebenarnya titik hanya dibutuhkan untuk memenuhi standar icann aja.

## Name server
Adalah computer yang bertugas mentranslate domain ke ip. Dia lah yang paling banyak bekerja pada sistem dns. Terkadang karena jumlah domain yang perlu di translate terlalu banyak untuk dikerjakan oleh satu server, biasanya server tersebut akan mengarahkan request ke name server lain atau mendelegasikan tanggung jawab bagian dari subdomain ke name server tersebut.  Name server dikatakan authoritative, maksudnya bahwa name server tersebut memberikan jawaban queries tentang domain langsung di bawah control mereka.

## Zone file
Zone file adalah text file yang berisi mapping antara domain dan ip. Zone file berada di dalam name server dan pada umumnya menyediakan resource untuk domain tertentu.

## Records
Record disimpan dalam zone file. Record adalah mapping antara domain dan ip. Record bisa nge mampping name ke ip , name server ke domain, mail server ke domain. Dll.

## How dns works

### Root server
Dns merupakan sistem hirarki. Di paling ujung atas ada yang kita sebut dengan root server. Server2 ini dikontrol oleh berbagai organisasi yang dilegasikan oleh icann.

Saat ini adalah 13 root server di seluruh dunia. Tapi karena ada banyak jumlah domain yang perlu di resolve tiap menit, setiap server ini sebenarnya di mirrorkan. Yang menarik setiap mirror dari satu root server berbagi ip address yang sama. Ketika request ditujukan untuk root server tertentu, request akan diarahkan ke mirror dari root server yang terdekat.

Jadi root server ngapain ? Root server sebenarnya ga tahu kalau ada query tentang fqdn. Jadi root server akan mengarahkan requester ke name server yang menghandle tld tertentu.

Misal jika ada request ke www.google.com untuk root server, dia akan mengecek zone file untuk www.googl.ecom, dan ga akan ketemu. Dia cuma akan nemu record untuk tld .com dan memberikan requester, alamat dari name server yang bertanggung jawab menghandle tld .com.

### Tld server
Requester kemudian mengirim request baru  ke alamat/ip address yang tadi dikasih root server. Dimana ip tersebut bertanggun hawa untuk tld .com.

Name server dari tld .com akan membaca zone file nya, apakah ada record untuk www.google.com. Dan lagi lagi ga ketemu. Tapi dia tahu kok name server yang bertanggung jawab untuk menghablde domain google.com. Selanjutnya si requester akan dikasih ip name server google.com.

### Domain-level name server
Sampai disini, requester punya ip dari name server yang bertangguna jawab meresolve actual ip address of the resource yaitu name server google.com. Sehingga requester akan mengirim request baru ke name server , name server google.com akan melihat di zone file apakah dia tahu www.google.com.

Name server google.com mengecek zone file dan menenmukan record untuk host www. Dan name server akan meberikan final address / ip address dari host tersebut ke requester.

### Apa itu resolving name server ?

Di paragraph sebelumnya, kita merefe ke sebuah kata “requester”. Apa itu ?

In almost all cases, requesteradalah yang kita sebut dengan resolving name server. Dia adalah name server yang dikonfigurasi untuk bertanya kepada name server lain. Pada dasarnya resolving name server adalah penghubung antara user dimana  dia melakukan caching query yang pernah dibuat untuk meningkatkan kecepatan proses resolve dan tahu alamat2 root server untuk bisa mresolve request yang belum diketahui.

User biasanya memiliki beberapa resovling name server yang dikonfigurasi di komputer nya. Biasnaya resolving name server disediakan oleh isp atau other organization. Contohnya google menyediakan resolver dns server yang kita bisa gunakan.

ketika kita mengetik url di address bar, computer kita pertama ngecek apakah dia bisa menemukan rosource yang diminta secara local, dia akan mengecek file hosts pada komputer kita. Kalau ga ada, komputer kita akan menrim request ke resolving name server dan menunggu ip address of the resource.

resolving name server kemudian akan mengecek, apakah dia punya cache untuk request tersebut. Jika ngga ada, dia akan melakukan langkah langkah yang sudah dijelaskan tadi.

## Mengenal Zone file lebih dalam
zone file adalah cara name server menyimpan informasi tentagn domain yang dia tahu. namun kebanyakan request yang datang ke name server bukan lah request yang bisa dihandle langsung oleh name server, karena mereka ga punya zone file nya.

makin banyak zone file yang dimilih sebuah name server, makin banyak request yang bisa di jawab *authoritatively*.

zone file mendeskripsikan tentang DNS zone, umumnya digunakna untuk mengkonfigurasi single domain.

$ORIGIN adalah parameter yang menunjuka pada zona higest level of authority secara default. jadi jika sebuah zone file digunakan untuk mengkonfigurasi example.com. $ORIGIN akan di set ke example.com.

ini bisa dikonfigurasi di top of zone file atau bisa juga di define di file configuration yang mereference ke zone file.

demikian juga `$TTL` mengconfigurasi time to live dari informasi yang disediakan. pada dasarnya `TTL`  adalah sebuah timer. sebuah caching name server bisa menggunakan hasil query sebelumnya untuk menjawab request sampai TTL value nya habis.

## RECORD TYPES

Dalam zone file kita bisa punya berbagai macam tipe record.

### SOA RECORD

`START OF AUTHORITY`, atau `SOA` merupakan record wajib yang harus ada di semua zone file. `SOA` harus menjadi record pertama di dalam file (walapun $ORIGIN atau $TTL bisa saja diletakkan di atas). `SOA` akan terlihat seperti ini :

```
gemaadhan.com.  IN SOA ns1.gemaadhan.com. halo.gemaadhan.com. (
                                            12083   ; serial number
                                            3h      ; refresh interval
                                            30m     ; retry interval
                                            3w      ; expiry period
                                            1h      ; negative TTL
)
```
#### gemaadhan.com.
ini adalah root dari zone. menentukan bahwa file zone ini untuk domain gemaadhan.com. seringkali hanya ditulis dengan `@`. yang mana hanya sebuah placeholder yang merepresentasikan konten dari variable $ORIGIN yang kita pelajari sebelumnya.

#### IN SOA
`IN` berarti internet. sedangkan `SOA` adalah indikator bahwa ini adalah record SOA.

#### ns1.gemaadhan.com.
menjelaskan primary name server untuk domain `gemaadhan.com.` Name server bisa bertipe primary atau secondary,

#### halo.gemaadhan.com.
ini adalah email address admin dari zone ini. `@` yang biasanya ada pada email digantikan dengan `.` pada record di atas. jika email dot di dalamnya. dot akan digantikan dengan backslash contohnya `halo.bro@gemaadhan.com` menjadi `halo\bro.gemaadhan.com`

#### 12083
ini adalah serial number dari zone file. setiap kali kita edit zone file, kita harus menambahkan angka ini. secondary server akan mengecek apakah serial number primary server untuk zona ini lebih besar daripada serial number zone miliknya. jika iya, secondary server akan merequest file zone baru, jika tidak secondary name server tetap menggunakan file yang original.

#### 3h
ini adalah refresh interval untuk sebuah zone. adalah jumlah waktu yang secondary server akan tunggu sebelum melihat perubahan pada primay zone file.


#### 30m
ini adalah retry interval untuk sebuah zone. jika secondary server ga bisa terubung ke primary ketika refresh period tiba, dia akan menunggu dalam rentan waktu ini dan retry untuk poll the primary.

#### 3w
ini adalah expire period. jika secondary name server tidak dapat mengcontact primary server dalam rentan waktu ini, dia tidak akan lagi mrespon request sebagai authoritative source for this zone.

#### 1h
ini adalah jumlah waktu dimana name server akan men cache sebuah name error jika dia tidak bisa menemukan requested name di dalam file ini.

### A dan AAAA
kedua record ini memetakan host ke ip address. A untuk ipv4 dan AAAA untuk ipv6.

A DAN AAAA record akan terlihat seperti ini :

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
kita juga biasanya menggunakan web server record nya akan terlihat seperti ini
```
www     IN  A       222.222.222.222
```

kita juga biasanya memberikan record kemana base domain kita akan ke record.

```
gemaadhan.com.     IN  A       222.222.222.222
```

atau untuk merefer ke base domain cukup gunakan  `@`

```
@       IN  A       222.222.222.222
```

kita juga bisa meresolve apapun yang tidak teresolve secara explicit dengan menggunakan wildcard

```
*       IN  A       222.222.222.222
```

semua contoh di atas work juga untuk record AAAA.

### CNAME Records

CNAME record digunakan sebagai alias untuk A dan AAAA records.

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
