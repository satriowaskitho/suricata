# Suricata IDS 

Dikolaborasikan dengan ELK v6.x dan Filebeat di Ubuntu 18.04 LTS (VM VirtualBox), bismillah.

### Tabel of Content

```bash

```

### System Requirements
- OS	: Ubuntu 18.04 LTS
- RAM	: 4 GB (Recommended)
- CPU	: 2 Core
- Network : Bridged Adapter (VM VirtualBox)

## 1. Instalasi Ubuntu 18.04 LTS di VM VirtualBox
### 1.1 Persiapan Instalasi Ubuntu 18.04 LTS

- Download installer Ubuntu 18.04 LTS

	> [Ubuntu 18.04 LTS](http://releases.ubuntu.com/18.04.4/ubuntu-18.04.4-live-server-amd64.iso)

- Buat '_Machine_' baru di VirtualBox.
- Buka _Settings > Tab System_, ubah _Base Memory_ menjadi 4096 MB dan _Processor(s)_ menjadi 2 CPU.
- Buka _Settings > Tab Storage_, arahkan _Controller: IDE > Empty_
ke _Directory_ installer Ubuntu 18.04 LTS yang sudah diunduh.	
- Buka _Settings > Tab Network_, ubah _network adapter_ menjadi _Bridged Adapter_.
- Jalankan Ubuntu Server.

### 1.2 Proses Instalasi Ubuntu 18.04 LTS

- **_Select start-up disk_**, pilih installer yang telah diunduh. Tunggu proses _booting_ selesai.
- Pilih bahasa yang diinginkan. Pada tutorial ini, server menggunakan bahasa Inggris.
- (Opsional) Bila ada saran _Installer update available_, pilih _'Update to new installer'_ dan tunggu proses _updating_.
- **_Keyboard Configuration_**, bila dirasa sudah sesuai pilih _'Done'_.
- **_Network Connetion_**, akan muncul _network interface_ (ex. enp0s3) dan IP Server. Keduanya akan digunakan pada instalasi Suricata, jadi tolong <ins>diingat dan dicatat</ins>. Bila sudah pilih _'Done'_.

	![](https://github.com/satriowaskitho/suricata/blob/master/images/103.png)

- **_Configure Proxy_**, kosongkan saja dan langsung pilih _'Done'_.
- **_Configure Ubuntu archive mirror_**, tidak perlu diubah. Lalu pilih _'Done'_.
- **_Guided storage configuration_**, pastikan _'use an entire disk'_ telah terpilih (X). Lalu pilih _'Done'_.
- **_Storage configuration_**, pilih _'Done'_.
- **_Confirm destructive action_**, bila telah yakin, pilih _'Continue'_.
- **_Profile setup_**, tahap pengisian profil dan pembuatan _user_.
		- _Your name_, nama pembuat server.
		- _Your server's name_, nama host server yang nantinya digunakan oleh server.
		- _Username_ dan  _password_, digunakan untuk _login_ ke server.
		- Bila semuanya telah terisi, pilih _'Done'_.

- **_SSH Setup_**, pastikan _'Install OpenSSH server'_ telah terpilih (X). _'Import SSH Identity'_ dibiarkan terisi _'No'_. Lalu pilih _'Done'_.
- **_Featured Server Snaps_**, lewati tahap ini dan langsung pilih _'Done'_. Tunggu proses instalasi selesai.
- **_Installation Complete!_**, pilih _Reboot_. Tunggu proses _reboot_ selesai, bila telah selesai maka akan muncul _command_:

	```bash
	Ubuntu 18.04 LTS tty1

	[nama_host_server] login:
	```
	
- Silakan login menggunakan _username_ yang telah dibuat sebelumnya.

### 1.3 _Setting Up a Basic Firewall_

- Untuk memastikan _firewall_ mengizinkan koneksi SSH maka kita perlu mengizinkan OpenSSH. Jalankan _command_ berikut:

	```bash
	$ sudo ufw allow OpenSSH
	```
	
- Kemudian aktifkan _firewall_:

	```bash
	$ sudo ufw enable
	```

- Untuk memeriksa statusnya:

	```bash
	$ sudo ufw status
	```

	> Output:
	```bash
	Status: active

	To                         Action      From
	--                         ------      ----
	OpenSSH                    ALLOW       Anywhere
	OpenSSH (v6)               ALLOW       Anywhere (v6)
	```

- Untuk memudahkan proses kedepannya, silakan _login_ ke server melalui PuttY dengan mengakses <ins>Port 22</ins> dan <ins>IP Address server</ins> yang sebelumnya telah dicatat.
- Bila ingin memeriksa IP Address jalankan _command_:

	```bash
	$ ifconfig -a
	```
	
	![](https://github.com/satriowaskitho/suricata/blob/master/images/104.png)


	> IP Address berada di depan _'inet'_ pada tiap _network interface_ (ex. enp0s3).


## 2. Instalasi Suricata

### 2.1 Instal Dependensi Suricata

- Jalankan _command_ berikut. Tunggu sampai proses instalasi selesai.

	```bash
	sudo apt-get -y install libpcre3 libpcre3-dbg libpcre3-dev build-essential libpcap-dev   \
	                libnet1-dev libyaml-0-2 libyaml-dev pkg-config zlib1g zlib1g-dev \
	                libcap-ng-dev libcap-ng0 make libmagic-dev libjansson-dev        \
	                libnss3-dev libgeoip-dev liblua5.1-dev libhiredis-dev libevent-dev \
	                python-yaml rustc cargo
	```

### 2.2 Instal Suricata

- Jalankan _command_ berikut, tekan [Enter] saat ada perizinan:

	```bash
	sudo apt-get install software-properties-common
	sudo add-apt-repository ppa:oisf/suricata-stable
	sudo apt-get update	
	```

- Kemudian, instal  Suricata dan tunggu proses instalasi:

	```bash
	sudo apt-get -y install suricata
	```

### 2.3 Konfigurasi Suricata

- Sebelum itu pastikan kalian masih mengingat _network interface_ kalian. Untuk memeriksanya kembali jalankan _command_:

	```bash
	$ sudo nano /etc/netplan/50-cloud-init.yaml
	```
	> Output:
	```bash
	network: 
	  ethernets: 
	    enp0s3:
	      dhcp4: true
	  version: 2
	```
	
	> Pada output tersebut, <ins>enp0s3</ins> merupakan _network interface_.

- Kemudian, masuk ke file konfigurasi suricata dan **ubah/replace** semua `eth0` menjadi _network interface_ kalian.  Pada tutorial ini, `eth0` akan di-_replace_ menjadi `enp0s3`.

	```bash
	$ sudo nano /etc/suricata/suricata.yaml
	```

	> - Tekan, Alt+R. 
	> - **_Search to (Replace)_**, masukan 'eth0'.
	> - **_Replace with_**, masukan _network interface_ kalian (ex. enp0s3).
	> - Tekan, 'A' pada _keyboard_ untuk _replace all_.
	> - Kurang lebih akan ada 9 kata yang diubah.

- Masih pada file yang sama, ubah `HOME_NET` yang berada di `address-groups` menjadi _IP Address_ dari server kalian. Misalnya:

	```bash
	HOME_NET: "[192.168.100.11/24]"
	```

- Sekarang, masuk ke file konfigurasi Suricata ke-2 dan ubah _network interface_ seperti cara sebelumnya.

	```bash
	$ sudo nano /etc/default/suricata
	```

	> - Tekan, Alt+R. 
	> - **_Search to (Replace)_**, masukan 'eth0'.
	> - **_Replace with_**, masukan _network interface_ kalian (ex. enp0s3).
	> - Tekan 'A' pada _keyboard_ untuk _replace all_.
	> - Hanya akan ada 1 kata yang diubah.

### 2.4 Instal Suricata-Update
Suricata-update digunakan untuk memudahkan dalam pengelolaan _rules_ yang akan digunakan Suricata.

- Jalankan _command_ berikut dan tunggu hingga proses instalasi selesai.

	```bash
	sudo apt-get -y install python-pip
	sudo pip install pyyaml
	sudo pip install https://github.com/OISF/suricata-update/archive/master.zip
	```

- Setelah itu, _upgrade_ suricata-update.

	```bash
	sudo pip install --pre --upgrade suricata-update
	```

Suricata-update memerlukan _permission_ pada beberapa _directory_ tertentu. 

- Sekarang kita akan memberikan _permission_ pada _directory_ tersebut. 

	>Pertama, buat grup `suricata`.
	```bash
	sudo groupadd suricata
	```

	>Lalu, ubah grup pada _directory_ yang akan kita izinkan.
	```bash
	sudo chgrp -R suricata /etc/suricata
	```
	
	>Jalankan suricata-update untuk pertama kalinya dan tunggu sampai `Testing with suricata` _'Done'_.

	```bash
	sudo suricata-update
	```

	>Kemudian, ubah lagi grup pada _directory_ lainnya yang akan kita izinkan.
	```bash
	sudo chgrp -R suricata /var/lib/suricata/rules
	sudo chgrp -R suricata /var/lib/suricata/update
	```

	>Atur ketiga _directory_ tadi dengan _permission_ yang dibutuhkan oleh grup suricata.
	```bash
	sudo chmod -R g+r /etc/suricata/
	sudo chmod -R g+rw /var/lib/suricata/rules
	sudo chmod -R g+rw /var/lib/suricata/update
	```

	>Sekarang tambahan _username_ kalian ke grup tadi.
	```bash
	sudo usermod -a -G suricata [username_kalian]
	```

### 2.5 Mencari Sumber _Rules_ Lain Yang Tersedia

- Pertama, _update_  indeks sumber _rules_ dengan _command update-sources_.

	```bash
	sudo suricata-update update-sources
	```

- Untuk melihat seluruh sumber _rules_ yang tersedia, jalankan command ini:

	```bash
	sudo suricata-update list-sources
	```

	>Dari sekian banyak sumber _rules_, kita akan mengaktifkan 3 (tiga) sumber dengan _command_ berikut:
	```bash
	sudo suricata-update enable-source ptresearch/attackdetection
	sudo suricata-update enable-source oisf/trafficid
	sudo suricata-update enable-source sslbl/ssl-fp-blacklist
	```

- Lalu, _update rules_ lagi untuk mengunduh _rules_ paling baru dan menjalankan _rules_ yang baru kita aktifkan. Tunggu sampai proses selesai.

	```bash
	sudo suricata-update
	```

	>Untuk melihat sumber apa saja yang diaktifkan dapat menjalankan _command_ berikut:

	```bash
	sudo suricata-update list-enabled-sources
	```
