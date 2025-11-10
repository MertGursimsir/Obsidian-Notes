nmcli connection show
nmcli connection show "Wired connection 1"
nmcli connection modify "Wired connection 1" ipv4.dns "%%DNS_IP%%"
nmcli connection modify "Wired connection 1" ipv4.dns-search "%%DOMAIN_NAME%%"
nmcli connection modify "Wired connection 1" ipv4.ignore-auto-dns yes
	DHCP’den gelen DNS ayarlarını **yok sayar**
nmcli connection down "Wired connection 1" && nmcli connection up "Wired connection 1"

resolvectl status ens34
resolvectl query host1.mert.net --> alan adı çözümlemesi veya direkt ping


`resolvectl` ile yaptığın ayarlar **geçici**dir. Çünkü bunlar doğrudan `systemd-resolved` üzerinden RAM’de tutulur. Yeniden başlatıldığında ya sıfırlanır ya da `NetworkManager` ya da `systemd-networkd` gibi bir servis tarafından üzerine yazılır.
	resolvectl dns ens34 %%DNS_IP%%
	resolvectl domain ens34 %%DOMAIN_NAME%%

---
`systemd-resolved`, **Linux sistemlerinde DNS çözümlemesini (isimden IP adresi bulma)** yapan bir servistir. DNS işleriyle ilgilenen arka planda çalışan sistem hizmetidir.

---
Ağ arayüzü:
- `eth0`, `ens34`: Ethernet (kablolu) ağ kartı
    
- `wlan0`, `wlp3s0`: Wi-Fi (kablosuz) ağ kartı
    
- `lo`: Localhost (kendi kendine bağlantı)
---

`resolvectl`, `systemd-resolved` servisini kontrol etmek için kullanılan **komut satırı aracıdır**.

---

- İnternete çıkmak isteyen bir uygulama (örneğin tarayıcı), alan adını çözümler.
    
- `systemd-resolved` devreye girer ve tanımlı DNS sunucusunu kullanarak IP adresini bulur.
    
- Bu DNS bilgileri, ilgili **ağ arayüzü** (`ens34` gibi) üzerinde atanmış olabilir.

---


`nmcli`, Linux’ta ağ bağlantılarını yönetmek için kullanılan bir **komut satırı aracıdır**.

> Açılımı: **NetworkManager Command Line Interface**

Bu araç, arka planda çalışan **NetworkManager** adlı servisi kontrol eder. Yani grafik arayüze (GUI) ihtiyaç duymadan ağ bağlantılarını yapılandırmanı sağlar.