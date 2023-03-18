**KULLANIM KILAVUZU**


ilk olarak “WireGuard-VPN-Docker” dizinine giriyoruz. docker-compose.yml içine giriyoruz burada sizin kendizie göre editlemeniz gereken bölümler var.

```bash
İlk olarak kullandınız text editör ile;
PEERS= bölümüne kaç kullanıcınız varsa o sayıyı yazmalısınız.
SERVERURL=YOUR-SERVER-İP bölümüne sunucuzun ip adresini yazmalısınız.
```

Dosyayı düzenletikten sonra dokcer  dosyamızı ayağa kaldırıyoruz.

```bash
$docker-compose up -d
#####
Running 0/8
 ⠋ wireguard Pulling                                                                             7.3s
   ⠦ 736b83edfa49 Download complete                                                              5.6s
   ⠦ 75348e6f9b96 Download complete                                                              5.6s
   ⠦ d954eeff28c6 Download complete                                                              5.6s
   ⠦ 1e8b9c7c7dfc Downloading    [==================================>    ]  9.272MB/13.62MB      5.6s
   ⠦ afc822f73ebf Download complete                                                              5.6s
   ⠦ cc8b896dcae6 Downloading    [>                                      ]   5.94MB/322.9MB      5.6s
   ⠦ ca56eae2867d Pulling fs layer                                                               5.6s
```

Eğer bir hata almazsanız en sonunda Runing kısmın altına bu kısım eklenecek.

```bash
[+] Running 2/2
 ⠿ Network wireguard-vpn-docker_default  Created
 ⠿ Container wireguard                   Started
```

Containerımızın çalıştığını kontrol etmek için “docker ps" komutunu kullanıyoruz. Bu komutu bir kaç kere belirli sürelerde çalıştırıp “STATUS” bölümünden Up time izleyebiliriz.

```bash
docker ps
CONTAINER ID   IMAGE                   COMMAND   CREATED         STATUS         PORTS                                           NAMES
a892c7ddb976   linuxserver/wireguard   "/init"   3 minutes ago   Up 3 minutes   0.0.0.0:51820->51820/udp, :::51820->51820/udp   wireguard
```

Herşey yolunda gittiyse diziminizn için de “config” dizini oluşacaktır. Buranın içinde kaç adet “PEERS=” belirtiyseniz o sayıda dizin oluşur ve bunların içinde ayrı ayrı conf dosyaları ve qr kod resimleri vardır.

```bash
root@debian:~/WireGuard-VPN-Docker/config ls -all

drwxr-xr-x  2 lehisa lehisa 4096 Mar 18 11:58 coredns
-rw-------  1 lehisa lehisa  187 Mar 18 11:58 .donoteditthisfile
drwx------  2 lehisa lehisa 4096 Mar 18 11:58 peer1
drwx------  2 lehisa lehisa 4096 Mar 18 11:58 peer2
drwx------  2 lehisa lehisa 4096 Mar 18 11:58 peer3
drwx------  2 lehisa lehisa 4096 Mar 18 11:58 peer4
drwx------  2 lehisa lehisa 4096 Mar 18 11:58 peer5
drwxr-xr-x  2 lehisa lehisa 4096 Mar 18 11:58 server
drwxr-xr-x  2 lehisa lehisa 4096 Mar 18 11:58 templates
-rw-------  1 lehisa lehisa 1185 Mar 18 11:58 wg0.conf
```

Peer1 dizinin içerisi;

```bash
root@debian:~/WireGuard-VPN-Docker/config/peer1 ls -all

-rw-------  1 lehisa lehisa  309 Mar 18 11:58 peer1.conf
-rw-------  1 lehisa lehisa 1127 Mar 18 11:58 peer1.png
-rw-------  1 lehisa lehisa   45 Mar 18 11:58 presharedkey-peer1
-rw-------  1 lehisa lehisa   45 Mar 18 11:58 privatekey-peer1
-rw-------  1 lehisa lehisa   45 Mar 18 11:58 publickey-peer1
```

Burada bizim işimize yarayacak şey peer1.conf dosyası burada ki dosyanın içinde ki bilgileri kullanacamız clientler de WireGuard uygulamasına ekliyoruz.

Ayrıca bu peer dosyalarının bir kopyasını kendi bilgisayarınıza almak için; “SCP” komutunu kullanacağız.

```bash
scp -r root@ip-adres:/root/WireGuard-VPN-Docker/ /home/mobaxterm/
```

Böylece sistemde her bir kişi için oluşturduğumuz wireguard config dosyaları kendi sistemimize çektik. Bunları dilerseniz bir exel dosyası oluşturup kimin hangi peer kullandığını hangilerinin boşta, hangilerinin kullanımda olduğunu takip edebilirsiniz.
