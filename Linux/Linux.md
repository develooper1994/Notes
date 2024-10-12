# wsl app windows desktop shortcut
wsl üzerinden çalışan bir programın windows üzerinde bir kısayolu oluşturulabilir. Boş bir kısayol oluşturup komut yapıştırıldığında powershell komutu yürütecektir
(kötü amaçlı da kullanılabilir). Uygulama "wsl" ile çalışıyorsa ayrıca bir komut satırı ekranı açılıır, "wslg" ile çalışırsa açılmaz. İcon yanlış ise kısa yol 
ayarında değiştirilebilir. Uygulamayı başlat menüsünde görmek için kısa yolu 
`C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Ubuntu` dizinine kopyalayın.

Temel kalıp("/bin/bash -c  " ile başlanmak zorunda değildir):
`<wsl - wslg> -d <distribution> -- <command>`

"netbeans" ve "qtcreator" için örnekleri(tam dosya yolunu ile örneklendirdim):

netbeans:
```powershell
"C:\Windows\System32\wslg.exe" -d Ubuntu -- /bin/bash -c "export DISPLAY=:0; ~/netbeans-11.1/netbeans/bin/netbeans --jdkhome /usr/lib/jvm/java-8-openjdk-amd64"
```

qtcreator:
```powershell
"C:\Program Files\WSL\wslg.exe" -d Ubuntu --cd "~" -- qtcreator
```
<hr>

# vmware paylaşılan dizin mount olmazsa

> Vmware sanal makinede ayarlarında paylaşılan dizinlerin oluşturulduğu varsayılmaktadır.

> mount komutu kernel 4.0 ve üstünde çalışmıyormuş.

```bash
/usr/bin/vmhgfs-fuse .host:/ $HOME/shared -o subtype=vmhgfs-fuse,allow_other
```

## automount
```bash
sudo mkdir -p /mnt/hgfs/
```
```bash
sudo mkdir -p $HOME/shared
```

> /etc/fstab

```
.host:/    /mnt/hgfs/    fuse.vmhgfs-fuse    defaults,allow_other,uid=1000     0    0
/mnt/hgfs/ /home/<username>/shared none bind 0 0
```

``` bash
sudo mount -a
```
Eğer `mount` komut işe yaramazsa muhtemelen init sistemi systemd ile yönetiliyordur. O zaman aşağıdaki komutu deneyin.
``` bash
systemctl daemon-reload
```

ilk satır paylaşılan dizini mount eder. `/mnt/hgfs` dizininin hiçbir şekilde adının değişmediği ve silinmediği varsayılmaktadır! 
ikici satır ise paylaşılan dizin usb takmış gibi göstermek üzere $HOME dizine mount edilir. $HOME/shared dizini kullanıcı adına göre değişebilmektedir!
`/etc/fstab` dosyası statik okunduğunda daha kullanıcılar tanımlı olmadığından $HOME gibi değişkenler kullanılamaz! 

<hr>

# Bir sanal makine kurarken zorlanmadan önceki sanal makinedeki paketleri yüklemek için;

eski makinedeki paketlerin listesini çıkart:
```bash
dpkg --get-selections | grep -v deinstall | awk '{print $1}' > temiz_paket_listesi.txt
```

yeni makineye paketleri yükle:
```bash
xargs -a paket_listesi.txt sudo apt-get install -y
```

<hr>

# Framebuffer Temizle
dd if=/dev/zero of=/dev/fb

<hr>

# lddtree -> pax-utils
Bu utility sayesinde ldd aracından daha iyi bağımlılık ağacı elde edilebiliyor. Her bağımlılığın başka hangi bağımlılıkları varsa gösterilebiliyor.
!!!Dosya sistemi oluştururken çok işe yarayabilir!!!

```bash
$ lddtree $(which ssh)
/usr/bin/ssh (interpreter => /lib64/ld-linux-x86-64.so.2)
    libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1
        libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0
    libgssapi_krb5.so.2 => /lib/x86_64-linux-gnu/libgssapi_krb5.so.2
        libkrb5.so.3 => /lib/x86_64-linux-gnu/libkrb5.so.3
            libkeyutils.so.1 => /lib/x86_64-linux-gnu/libkeyutils.so.1
            libresolv.so.2 => /lib/x86_64-linux-gnu/libresolv.so.2
        libk5crypto.so.3 => /lib/x86_64-linux-gnu/libk5crypto.so.3
        libcom_err.so.2 => /lib/x86_64-linux-gnu/libcom_err.so.2
        libkrb5support.so.0 => /lib/x86_64-linux-gnu/libkrb5support.so.0
    libcrypto.so.3 => /lib/x86_64-linux-gnu/libcrypto.so.3
    libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1
    libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6
```
