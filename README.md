# Suricata IDS 

Dikolaborasikan dengan ELK v6.x dan Filebeat di Ubuntu 18.04 LTS (VM VirtualBox), bismillah.

### Tabel of Content
[System Requirements](https://github.com/satriowaskitho/suricata/blob/master/README.md#system-requirements)
1. [ Instalasi Ubuntu 18.04 LTS di VM VirtualBox ](https://github.com/satriowaskitho/suricata/blob/master/README.md#1-instalasi-ubuntu-1804-lts-di-vm-virtualbox)  

	1.1 [Persiapan Instalasi Ubuntu 18.04 LTS](https://github.com/satriowaskitho/suricata/blob/master/README.md#11-persiapan-instalasi-ubuntu-1804-lts) 
	
	1.2 [Proses Instalasi Ubuntu 18.04 LTS](https://github.com/satriowaskitho/suricata/blob/master/README.md#12-proses-instalasi-ubuntu-1804-lts)
	
	1.3  [Setting Up a Basic Firewall](https://github.com/satriowaskitho/suricata/blob/master/README.md#13-setting-up-a-basic-firewall)
	 
	1.4 [Configurasi  _Network Static IP Address_](https://github.com/satriowaskitho/suricata/blob/master/README.md#14--configurasi-network-static-ip-address)

2. [Instalasi Suricata](https://github.com/satriowaskitho/suricata/blob/master/README.md#2-instalasi-suricata) 

	2.1 [Instal Dependensi Suricata](https://github.com/satriowaskitho/suricata/blob/master/README.md#21-instal-dependensi-suricata)

	2.2 [Instal Suricata](https://github.com/satriowaskitho/suricata/blob/master/README.md#22-instal-suricata)

	2.3 [Konfigurasi Suricata](https://github.com/satriowaskitho/suricata/blob/master/README.md#23-konfigurasi-suricata)

	2.4 [Instal Suricata-Update](https://github.com/satriowaskitho/suricata/blob/master/README.md#24-instal-suricata-update)

	2.5 [Mencari Sumber Rules Lain Yang Tersedia](https://github.com/satriowaskitho/suricata/blob/master/README.md#25-mencari-sumber-rules-lain-yang-tersedia)

3. [Instalasi Java 8](https://github.com/satriowaskitho/suricata/blob/master/README.md#3-instalasi-java-8)

	3.1 [Instal Java 8](https://github.com/satriowaskitho/suricata/blob/master/README.md#31-instal-java-8)

	3.2 [Konfigurasi JAVA_HOME](https://github.com/satriowaskitho/suricata/blob/master/README.md#32-konfigurasi-java_home)

4. [Instalasi Elasticsearch](https://github.com/satriowaskitho/suricata/blob/master/README.md#4-instalasi-elasticsearch)

	4.1 [Instal Elasticsearch](https://github.com/satriowaskitho/suricata/blob/master/README.md#41-instal-elasticsearch)

	4.2 [Konfigurasi Elasticsearch](https://github.com/satriowaskitho/suricata/blob/master/README.md#42-konfigurasi-elasticsearch)

5. [Instalasi Kibana](https://github.com/satriowaskitho/suricata/blob/master/README.md#5-instalasi-kibana)

	5.1 [Instal Kibana](https://github.com/satriowaskitho/suricata/blob/master/README.md#51-instal-kibana)

	5.2 [Konfigurasi Kibana](https://github.com/satriowaskitho/suricata/blob/master/README.md#52-konfigurasi-kibana)

	5.3 [Instal Template Kibana 6](https://github.com/satriowaskitho/suricata/blob/master/README.md#53-instal-template-kibana-6)

6. [Instalasi Logstash](https://github.com/satriowaskitho/suricata/blob/master/README.md#6-instalasi-logstash)

	6.1 [Instal Logstash](https://github.com/satriowaskitho/suricata/blob/master/README.md#61-instal-logstash)

	6.2 [Download GeoLite2-City](https://github.com/satriowaskitho/suricata/blob/master/README.md#62-download-geolite2-city)

	6.3 [Konfigurasi Logstash](https://github.com/satriowaskitho/suricata/blob/master/README.md#63-konfigurasi-logstash)

7. [Tes Suricata](https://github.com/satriowaskitho/suricata/blob/master/README.md#7-tes-suricata)
8. [Eksplorasi Dashboard Kibana](https://github.com/satriowaskitho/suricata/blob/master/README.md#8-eksplorasi-dashboard-kibana)
9. [Instalasi Filebeat](https://github.com/satriowaskitho/suricata/blob/master/README.md#9-instalasi-filebeat)

### System Requirements
- OS	: Ubuntu 18.04 LTS
- RAM	: 4 GB (Recommended)
- CPU	: 2 Core
- Network : Bridged Adapter (VM VirtualBox)

## 1. Instalasi Ubuntu 18.04 LTS di VM VirtualBox
### 1.1 Persiapan Instalasi Ubuntu 18.04 LTS

- Download installer Ubuntu 18.04 LTS

	> [Ubuntu 18.04 LTS](http://releases.ubuntu.com/18.04.4/ubuntu-18.04.4-live-server-amd64.iso)

- Buat `Machine` baru di VirtualBox. Set `virtual hard disk` dengan ukuran 20 GB (Recommended).
- Buka _Settings > Tab System_, ubah `Base Memory` menjadi 4096 MB dan `Processor(s)` menjadi 2 CPU.
- Buka _Settings > Tab Storage_, arahkan _Controller: IDE > Empty_
ke _Directory_ installer Ubuntu 18.04 LTS yang sudah diunduh.	
- Buka _Settings > Tab Network_, ubah `network adapter` menjadi `Bridged Adapter`.
- Jalankan Ubuntu Server.

### 1.2 Proses Instalasi Ubuntu 18.04 LTS

- **_Select start-up disk_**, pilih installer yang telah diunduh. Tunggu proses _booting_ selesai.
- Pilih bahasa yang diinginkan. Pada tutorial ini, server menggunakan bahasa Inggris.
- (Opsional) Bila ada saran **_Installer update available_**, pilih `Update to new installer` dan tunggu proses _updating_.
- **_Keyboard Configuration_**, bila dirasa sudah sesuai, pilih `Done`.
- **_Network Connetion_**, akan muncul `network interface` (ex. enp0s3) dan `IP Address Server`. Keduanya akan digunakan pada instalasi Suricata, jadi tolong <ins>diingat</ins> ya folks. Bila sudah, pilih `Done`.

	![](https://github.com/satriowaskitho/suricata/blob/master/images/103.png)

- **_Configure Proxy_**, kosongkan saja dan langsung pilih `Done`.
- **_Configure Ubuntu archive mirror_**, tidak perlu diubah. Lalu pilih `Done`.
- **_Guided storage configuration_**, pastikan `use an entire disk` telah terpilih (X). Lalu pilih `Done`.
- **_Storage configuration_**, pilih `Done`.
- **_Confirm destructive action_**, bila telah yakin, pilih `Continue`.
- **_Profile setup_**, tahap pengisian profil dan pembuatan _user_.

	- `Your name`, nama pembuat server.

	- `Your server's name`, nama host server yang nantinya digunakan oleh server.

	- `Username` dan  `password`, digunakan untuk _login_ ke server.

	- Bila semuanya telah terisi, pilih `Done`.


- **_SSH Setup_**, pastikan `Install OpenSSH server` telah terpilih (X). `Import SSH Identity` dibiarkan terisi `No`. Lalu pilih `Done`.
- **_Featured Server Snaps_**, lewati tahap ini dan langsung pilih `Done`. Tunggu proses instalasi selesai.
- **_Installation Complete!_**, pilih `Reboot`. Tunggu proses _reboot_ selesai, bila telah selesai maka akan muncul _command_:

	```bash
	Ubuntu 18.04 LTS tty1

	[nama_host_server] login:
	```
	
- Silakan login menggunakan `username` yang telah dibuat sebelumnya.

### 1.3 _Setting Up a Basic Firewall_

- Untuk memastikan _firewall_ mengizinkan koneksi SSH, maka kita perlu mengizinkan OpenSSH. Jalankan _command_ berikut:

	```bash
	$ sudo ufw allow OpenSSH
	```
	
- Kemudian aktifkan _firewall_.

	```bash
	$ sudo ufw enable
	```

- Untuk memeriksa statusnya.

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

- Untuk memudahkan proses kedepannya, silakan _login_ ke server melalui PuTTY dengan mengakses `Port 22` dan `IP Address server` kalian.
- Bila ingin memeriksa `IP Address` jalankan _command_:

	```bash
	$ ifconfig -a
	```
	
	![](https://github.com/satriowaskitho/suricata/blob/master/images/104.png)


	> `IP Address` berada di depan `inet` pada tiap `network interface` (ex. <ins>192.168.100.11</ins>).

### 1.4  Configurasi _Network Static IP Address_
- Buat file baru bernama `01-netcfg.yaml` dengan _command_ berikut:

	```bash
	sudo nano /etc/netplan/01-netcfg.yaml
	```

	> Lalu isikan _file_ tersebut dengan konfigurasi di bawah. Ganti `IP Address` dan `gateway4` sesuai IP kalian. Tekan `Ctrl+S` untuk menyimpan dan `Ctrl+X` untuk keluar dari `nano`
		
	```bash
	network:
	  version: 2
	  renderer: networkd
	  ethernets:
	    enp0s3:
	      dhcp4: no
	      addresses: [192.168.100.11/24]
	      gateway4:  192.168.100.1
	      nameservers:
	        addresses: [8.8.8.8, 8.8.4.4]
	```

- Kemudian, terapkan konfigurasi tersebut dengan _command_ berikut.

	```bash
	sudo netplan apply
	```


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

- Jalankan _command_ berikut, tekan `[Enter]` saat ada perizinan:

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

- Sebelum itu pastikan kalian masih mengingat `network interface` kalian. Untuk memeriksanya kembali, jalankan _command_:

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

	> - Bila telah masuk ke dalam _file_, tekan `Alt+R`. 
	> - **_Search to (Replace)_**, masukan 'eth0'.
	> - **_Replace with_**, masukan _network interface_ kalian (ex. <ins>enp0s3</ins>).
	> - Tekan, 'A' pada _keyboard_ untuk _replace all_.
	> - Kurang lebih akan ada 9 kata yang diubah.
	> - Tekan `Ctrl+S` untuk simpan.

- Masih pada file yang sama, ubah `HOME_NET` yang berada di `address-groups` menjadi _IP Address_ dari server kalian. Misalnya:

	```bash
	HOME_NET: "[192.168.100.11/24]"
	```
	> Tekan `Ctrl+S` untuk simpan, lalu `Ctrl+X` untuk keluar dari `nano`.

- Sekarang, masuk ke file konfigurasi Suricata lainnya dan ubah _network interface_ seperti cara sebelumnya.

	```bash
	$ sudo nano /etc/default/suricata
	```

	> - Bila telah masuk ke dalam _file_, tekan `Alt+R`. 
	> - **_Search to (Replace)_**, masukan 'eth0'.
	> - **_Replace with_**, masukan _network interface_ kalian (ex. enp0s3).
	> - Tekan 'A' pada _keyboard_ untuk _replace all_.
	> - Hanya akan ada 1 kata yang diubah.
	> - Tekan `Ctrl+S` untuk simpan, lalu `Ctrl+X` untuk keluar.

### 2.4 Instal Suricata-Update
Suricata-update digunakan untuk memudahkan dalam pengelolaan _rules_ yang akan digunakan Suricata.

- Jalankan _command_ berikut dan tunggu hingga proses instalasi selesai.

	```bash
	sudo apt -y install python-pip
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
	
	>Jalankan suricata-update untuk pertama kalinya dan tunggu sampai `Testing with suricata` bertuliskan `Done`.

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

	>Sekarang tambahkan _username_ kalian ke grup tadi.
	```bash
	sudo usermod -a -G suricata [username_kalian]
	```

### 2.5 Mencari Sumber _Rules_ Lain Yang Tersedia

- Pertama, _update_  indeks sumber _rules_ dengan _command_ `update-sources`.

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

## 3. Instalasi Java 8
Elasticsearch dan logstash memerlukan `OpenJDK` yang tersedia di server. _Note: Java 9 is not supported_.

### 3.1 Instal Java 8
-	Jalankan _command_ berikut untuk menginstal java 8. Tunggu sampai proses instalasi selesai.

	```bash
	sudo apt update
	sudo apt install -y openjdk-8-jdk wget apt-transport-https
	```

- Cek java _version_.

	```bash
	$ java -version
	```
	>Output:
	```bash
	openjdk version "1.8.0_252"
	OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1~18.04-b09)
	OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
	```

### 3.2 Konfigurasi `JAVA_HOME`
- Masuk ke _directory_ di bawah.

	```bash
	sudo nano /etc/environment
	```
	>Tambahkan pada baris paling akhir, konfigurasi ini.

	```bash
	JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/"
	```

- _Reload_ menggunakan _command_ di bawah untuk menerapkan perubahan tersebut.

	```bash
	source /etc/environment
	```

- Cek apakah perubahan telah diterapkan.

	```bash
	echo $JAVA_HOME
	```
	>Output:
	```bash
	/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/
	```

## 4. Instalasi Elasticsearch
### 4.1 Instal Elasticsearch
- Dimulai dengan mengimport _Elasticsearch public GPG key_ ke dalam APT. Bila sukses akan muncul _feedback_ `OK`.

	```bash
	sudo wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
	```

- Lalu, menambahkan list _source_ dari Elastic ke `sources.list.d` _directory_, dimana APT akan melihat _source_ baru di sana. Tutorial ini akan menggunakan Elastic versi 6.x

	```bash
	echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
	```

- _Update_ daftar _package_ sehingga APT dapat membaca _source_ yang baru ditambahkan. Kemudian instal Elasticsearch.

	```bash
	sudo apt update
	sudo apt -y install elasticsearch
	```

### 4.2 Konfigurasi Elasticsearch
- Masuk ke konfigurasi _file_ Elasticsearch

	```bash
	sudo nano /etc/default/elasticsearch
	```
	>_Uncomment_ `JAVA_HOME`  dan masukan _directory_ ini.
	```bash
	JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/
	```

- Jalankan dan perbolehkan Elasticsearch _start_ otomatis ketika server dihidupkan.

	```bash
	sudo systemctl start elasticsearch
	sudo systemctl enable elasticsearch
	```

- Tes apakah Elasticsearch telah berjalan dengan baik.

	```bash
	$ curl -X GET "localhost:9200"
	```
	>Contoh output yang benar:
	```bash
	{
	  "name" : "KlUlyyn",
	  "cluster_name" : "elasticsearch",
	  "cluster_uuid" : "UkzubFEpRue6sQj-mCFHGQ",
	  "version" : {
	    "number" : "6.8.8",
	    "build_flavor" : "default",
	    "build_type" : "deb",
	    "build_hash" : "2f4c224",
	    "build_date" : "2020-03-18T23:22:18.622755Z",
	    "build_snapshot" : false,
	    "lucene_version" : "7.7.2",
	    "minimum_wire_compatibility_version" : "5.6.0",
	    "minimum_index_compatibility_version" : "5.0.0"
	  },
	  "tagline" : "You Know, for Search"
	}
	```

## 5. Instalasi Kibana
### 5.1 Instal Kibana
Instal Kibana. Tunggu sampai proses instalasi selesai.

```bash
sudo apt -y install kibana
```

### 5.2 Konfigurasi Kibana
Masuk ke _file_ konfigruasi Kibana.

```bash
sudo nano /etc/kibana/kibana.yml
```
>_Uncomment_  `server.port` dan ubah `server.host` sesuai _IP Address_ kalian.

```
server.port: 5601
server.host: "192.168.100.11"
```

![](https://github.com/satriowaskitho/suricata/blob/master/images/501.png)

- Jalankan dan perbolehkan Kibana _start_ otomatis ketika server dihidupkan.

	```bash
	sudo systemctl start kibana
	sudo systemctl enable kibana
	```

- Buka `port 5061` agar bisa diakses oleh _web browser_.

	```bash
	sudo ufw allow from any to any port 5601 proto tcp
	```

## 6. Instalasi Logstash
### 6.1 Instal Logstash
- Instal Logstash. Tunggu sampai proses instalasi selesai.

	```bash
	sudo apt -y install logstash
	```

- Pastikan Logstash dapat membaca _log file_ dengan menjalankan _command_ berikut.

	```bash
	sudo usermod -a -G adm logstash
	```

- _Update plugin_  Logstash dengan menjalankan _command_ di bawah. Proses akan cenderung lama. Tunggu sampai proses selesai.

	```bash
	sudo /usr/share/logstash/bin/logstash-plugin update
	```

### 6.2 Instal Template Kibana 6
- Masuk ke _directory_ `/etc/logstash` dengan _command_ di bawah.

	```bash
	cd /etc/logstash
	```
- Instal `git-core`, `clone` _template_ Kibana 6, dan masuk ke _folder_ hasil _clone_.

	```bash
	sudo apt-get install git-core
	sudo git clone https://github.com/StamusNetworks/KTS6.git
	cd KTS6
	```

- Masih di _directory_ KTS6. Muat _dashboard_ KTS6 dengan _command_ berikut. Tunggu sampai proses selesai.

	```bash
	./load.sh
	```

- Terakhir, pindahkan `elasticsearch6-template.json` ke _directory_ `/etc/logstash`.

	```bash
	sudo mv /etc/logstash/KTS6/es-template/elasticsearch6-template.json /etc/logstash
	```

### 6.3 Download GeoLite2-City
- Masuk ke _directory_ `/usr/share/GeoIP` dengan _command_ di bawah.

	```bash
	cd /usr/share/GeoIP
	```

- Lalu, unduh _database_ GeoLite2-City melalui link berikut.

	```bash
	sudo wget -N "https://download.maxmind.com/app/geoip_download?edition_id=GeoLite2-City&license_key=6spQbiHDzaMYOcdt&suffix=tar.gz"
	```

- Masih di _directory_ yang sama. Ubah nama _file_ agar dapat mudah dibaca.

	```bash
	sudo mv 'geoip_download?edition_id=GeoLite2-City&license_key=6spQbiHDzaMYOcdt&suffix=tar.gz' GeoLite2-City.tar.gz
	```

- Kemudian, ekstrak _file_ tersebut.

	```bash
	sudo tar -zxvf GeoLite2-City.tar.gz
	```

- Pindahkan _file_ `GeoLite-City.mmdb`  ke _directory_ `/usr/share/GeoIP`.

	```bash
	sudo mv /usr/share/GeoIP/GeoLite2-City_20200421/GeoLite2-City.mmdb /usr/share/GeoIP
	```

### 6.4 Konfigurasi Logstash
Tutorial hanya akan membuat 1 (satu) _file_ konfigurasi. _File_ tersebut berisi konfigurasi _input_ sekaligus _output_.

- Buat _file_ bernama `logstash.conf` dengan _command_ berikut.

	```bash
	sudo nano /etc/logstash/conf.d/logstash.conf
	```
	>Lalu, isi _file_ tersebut dengan konfigurasi di bawah ini. Tekan `Shift+Insert` untuk _paste_ ke `nano`.

	```bash
	input {
	  file {
	    path => ["/var/log/suricata/eve.json"]
	    sincedb_path => ["/var/lib/logstash/sincedb"]
	    codec => json
	    type => "SELKS"
	  }
	}

	filter {
	  if [type] == "SELKS" {
	    date {
	      match => [ "timestamp", "ISO8601" ]
	    }
	    ruby {
	      code => "
	        if event.get('[event_type]') == 'fileinfo'
	          event.set('[fileinfo][type]', event.get('[fileinfo][magic]').to_s.split(',')[0])
	        end
	      "
	    }
	    ruby {
	      code => "
	        if event.get('[event_type]') == 'alert'
	          sp = event.get('[alert][signature]').to_s.split(' group ')
	          if (sp.length == 2) and /\A\d+\z/.match(sp[1])
	            event.set('[alert][signature]', sp[0])
	          end
	        end
	      "
	    }
	    metrics {
	      meter => [ "eve_insert" ]
	      add_tag => "metric"
	      flush_interval => 30
	    }
	  }
	  if [http] {
	    useragent {
	      source => "[http][http_user_agent]"
	      target => "[http][user_agent]"
	    }
	  }
	  if [src_ip] {
	    geoip {
	      source => "src_ip"
	      target => "geoip"
	      database => "/usr/share/GeoIP/GeoLite2-City.mmdb"
	      add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
	      add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}" ]
	    }
	  }
	  if [dest_ip] {
	    geoip {
	      source => "dest_ip"
	      target => "geoip"
	      database => "/usr/share/GeoIP/GeoLite2-City.mmdb"
	      add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
	      add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}" ]
	    }
	  }
	}

	output {
	  if [event_type] and [event_type] != 'stats' {
	    elasticsearch {
	      hosts => localhost
	      index => "logstash-%{event_type}-%{+YYYY.MM.dd}"
	      template_overwrite => true
	      template => "/etc/logstash/elasticsearch6-template.json"
	    }
	  } else {
	    elasticsearch {
	      hosts => localhost
	      index => "logstash-%{+YYYY.MM.dd}"
	      template_overwrite => true
	      template => "/etc/logstash/elasticsearch6-template.json"
	    }
	  }
	}
	```

- Tes konfigurasi Logstash. Proses akan berlangsung agak lama. Tunggu sampai proses selesai.

	```bash
	sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
	```
	>Jika tidak ada _errror_, maka _output_ akan menampilkan `Configuration OK`

- Jalankan dan perbolehkan Logstash _start_ otomatis ketika server dihidupkan.

	```bash
	sudo systemctl start logstash
	sudo systemctl enable logstash
	```

## 7. Tes Suricata
- **_Turn Off Packages Offload_**, ubah sesuai _network interface_ masing-masing.

	```bash
	sudo ethtool -K enp0s3 tso off  
	sudo ethtool -K enp0s3 tx off  
	sudo ethtool -K enp0s3 gro off
	```

- Hapus `suricata.pid` bila ada.

	```bash
	sudo rm -R /var/run/suricata.pid
	```

- Jalankan Suricata. Pastikan _network interface_ sesuai dengan milik masing-masing.

	```bash
	sudo /usr/bin/suricata -D -c /etc/suricata/suricata.yaml -i enp0s3
	```
## 8. Eksplorasi Dashboard Kibana
Akses URL Kibana. Tunggu sampai _loading_ selesai.
>http://[IP_Address_Server]:5601

Masuk ke tab `Discover`, berada di kiri atas. Bila ini pertama kalinya kalian masuk ke dashboard Kibana. Maka, kalian akan diminta untuk memilih `default index`.

![](https://github.com/satriowaskitho/suricata/blob/master/images/901.png)

- Pilih `logstash-*`, tepat berada di bawah tombol `Create Index Pattern`.
- Lalu, buat indeks tersebut menjadi `default` dengan cara klik tombol `bintang` . Tombol tersebut berada di kanan atas, sejajar dengan tombol `refresh` dan `remove`.
- Setelah itu, kalian akan diarahkan ke tampilan tab `decover` yang sebenarnya. Bila terdapat @timestamp _barchart_ beserta tabel `time` dan `source` di bawahnya, maka log Suricata sudah berhasil diterima oleh Kibana. Yeayy.

Kemudian, akses tab `Dashboard`. Di sana akan banyak pilihan dashboard bila instalasi _template_ nya <ins>berhasil</ins> hehe. Karena baru pertama kali, kalian bisa mencoba dashboard `SN-OVERVIEW`. Dashbord tersebut menampilkan banyak grafik dari aktifitas server yang dibaca oleh Suricata.

![](https://github.com/satriowaskitho/suricata/blob/master/images/801.png)

Selanjutnya, kalian bisa mencoba dashboard tersebut satu per satu. Tidak semua dashboard dapat menvisualisasikan log karena tergantung jenis log yang masuk dan jenis rules yang dikonfigurasikan di Suricata.

Eurekaaa~ :D

## 9. Instalasi Filebeat
To be continue...

## 10. Simulasi Suricata
On planning...
