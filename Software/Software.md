# [sysinternals](https://learn.microsoft.com/en-us/sysinternals/)

https://github.com/Sysinternals

İçindeki bir takım araçlar kullanışlı duruyor.
Process Explorer aracı processin kullandığı dosyaları, bağımlılıkları ve alt processleri listeleyebiliyor.
Ayrıca bazı araçların Linux versiyonları da varmış.

<hr>

# microVMs
## [firecracker](https://firecracker-microvm.github.io/)

Yeni bir KVM tabanlı sanal makine yürütücü. Amazon 2018 yılından beri rust diliyle geliştiriyormuş. Amacı bulut sistemleri için küçük sanal makineler üretmek(~5mb, boot 5ms). Böylece yazılana göre ortalama bir server 150 kadar sanal makineyi çalıştırabiliyormuş. Şimdilik sadece Linux işletim sisteminde x64 intel, amd ve arm işlemcisiyle birlikte çalışabiliyormuş.

Qemu da benzer bir iş yapabiliyor fakat ikisinin özelleştikleri alanlar farklı olduğundan qemu bu kadar iyi iş çıkartamıyor. 

Container jailbreak gibi güvenlik açığı oluşturabildiğinden küçük sanal makineler daha güvenli olabilir denmiş. Atak alanını küçülterek olası saldırıları azaltıyorlarmış.

<hr>
