# INTODUCTION

(1)
Halo teman teman semua, ditulisan kali ini saya akan ngomongin tentang iptables, iptables biasanya digunakan untuk paket filtering dan mengelola nat rule. 

(2)
iptables  sendiri sebenarnya terdiri dari bebeberapa tabel, dan masing masing tabel akan mengandung beberapa chain, sedangkan chain akan mengandung beberapa rules.  

iptables memiliki 4 buah tabel. (3)
 
 1. Filter table
 2. NAT table
 3. Mangle table
 4. Raw table
 
(4) 
ditulisan ini kita hanya akan ngomongin tentang filter table dulu. untuk tabel tabel lainnya akan kita bahas di tulisan berikutnya. oke kita masuk ke filter table. 

(5)
 filter tabel memiliki 3 buah chain. 
	1. INPUT chain -> digunakan untuk paket yang memang ditujukan untuk server kita. 
	2. FORWARD chain -> digunakan untuk paket yang ditujukan ke komputer/server lain yang melewati server kita. 
	3. OUTPUT chain -> sedangkan output chain digunakan untuk paket yang dihasilkan oleh server kita.

(6)
oke saya ulangi ya, INPUT chain digunakan untuk paket yang tujuannya adalah server kita, misalkan server kita memiliki service sshserver, ketika ada permintaan untuk login ke server kita, permintaan tersebut akan di handle oleh input chain.
  
kemudian untuk forward chain cara kerja mirip seperti router, misalkan ada client yang ingin mengakses google.com. paket dari client akan dikirimkan ke server google melewati router kita, atau dalam hal ini server kita, nah paket paket ini akan di handle oleh forward chain. 

nah yang terakhir adalah output chain, output chain adalah kebalikan dari input chain, kalo input akan menghandle paket yang ditujukan ke server kita sedangkan output adalah menghandle paket yang keluar yang dibuat oleh server kita. 

(7)
lanjut... 

sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o eth0 -j ACCEPT