---
layout: default
---

## Arch Linux EBYS Tübitak e-imza Kurulumu

Bizde **ACS ACR38T Beyaz** kart okuyucu var.

Öncelikle `pcsclite` dosyasını ve <!--more--> aur’dan `akia` dosyalarını yükledim.

EBYS üzerinden imza atmak istediğimde site benden **envision** **istemcisini** kurmamı istedi. İnen **deb** uzantılı dosyayı **debtap** ile Arch’a kurulacak şekilde dönüştürdüm. Dönüştürme sırasında herhangi bir değişiklik yapmadım.

Envision istemcisi kuruldu ama kurulum sonrası hatası verdi. Bu sorunun açıklaması şöyle:

Envision istemcisi systemd’ye **socket** adında bir modül ekliyor ancak modül çalıştırılmıyor. Bunun iki nedeni var: Birincisi `pyton2` eksikliği, ikincisi /usr/CBKSoft dizininin sahibinin **root** olması.

python2’nin kurulumu kolay. `pacman -S python2`

İkincisine gelince: Dosya izinlerinden çok anlamadığım için acemi usulü Dolphin’i root olarak çalıştırdım, CBKSoft dizininin sahiplerine kendi kullanıcımı ekledim ve bunu tüm alt dizinlere ve dosyalara uygula dedim. Bunu yaptıktan sonra

`sudo systemctl enable --now socket.service` deyince socket çalışmaya başladı.

Bu arada java 18 falan çalışmayabilir. Bu yüzden aur’dan **Oracle’ın java 8**‘ini kurmam gerekti. Şu andaki adı `jre8`. Kurduktan sonra java 8’i şu şekilde varsayılan yaptım:

java sürümlerini listelemek için `archlinux-java status` dedim. Ardından

`sudo archlinux-java set java-8-jre/jre` komutuyla işi tamamladım.

Yetti mi? Yetmeyebilir… Bunun için
`/usr/CBKSoft/enVision.Client.Service/SignerCMSTubitak/`
dizinin içindeki iki dosyanın özelliklerine baktım. Birlikte açmak için varsayılan uygulamanın java 8 olduğundan emin olmalıydım.

Bütün bunları yapınca çalışır gibi oldu ama bir sorun daha çıkardı. İmzamı girince
`cannot encode tr.gov.tubitak.uekae.esya.asn.cms.SignedData@1`
hatası çıktı.

[Tübitak](https://kamusm.bilgem.tubitak.gov.tr/depo/sertifikalar/depo.jsp)‘ın sitesinden en alttaki Sertifikadeposu.svt’yi indirip kullanıcı dizinimde gizli bulanan .sertifikadeposu dizininin içine attım. Burada önceden aynı isimde bir dosya vardı ama sanırım bozuktu. Onunla değiştir deyince sorun düzeldi.
