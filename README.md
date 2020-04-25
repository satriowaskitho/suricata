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
- **_Select start-up disk_**, pilih installer yang telah diunduh.
- Tunggu proses _booting_ selesai.
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
- **_Featured Server Snaps_**, lewati tahap ini dan langsung pilih _'Done'_.
- Tunggu proses instalasi selesai.
- **_Installation Complete!_**, pilih _Reboot_.
- Tunggu proses _reboot_ selesai, bila telah selesai maka akan muncul _command_:
	```bash
	Ubuntu 18.04 LTS tty1

	[nama_host_server] login:
	```
- Silakan login menggunakan _username_ yang telah dibuat sebelumnya.

### 1.3 Konfigurasi _Network Static IP Address_ di Ubuntu 18.04 LTS
- Untuk memudahkan proses kedepannya, silakan _login_ ke server melalui PuttY dengan mengakses <ins>Port 22</ins> dan <ins>IP Address server</ins> yang sebelumnya telah dicatat.
- Bila ingin memeriksa IP Address jalankan _command_:
	```bash
	$ ifconfig -a
	```
	![](https://github.com/satriowaskitho/suricata/blob/master/images/104.png)


	> IP Address berada di depan _'inet'_ pada tiap _network interface_ (ex. enp0s3).
-

## 2. Instalasi Suricata

### 2.1 Instal Dependensi Suricata
```bash
sudo apt-get install libpcre3 libpcre3-dbg libpcre3-dev build-essential libpcap-dev   \
			libnet1-dev libyaml-0-2 libyaml-dev pkg-config zlib1g zlib1g-dev \
			libcap-ng-dev libcap-ng0 make libmagic-dev libjansson-dev        \
			libnss3-dev libgeoip-dev liblua5.1-dev libhiredis-dev libevent-dev \
			python-yaml rustc cargo
```
