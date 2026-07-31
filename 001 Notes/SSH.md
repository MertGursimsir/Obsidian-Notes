authorized_keys: Bana kimler girebilir? (Gelen istemciyi doğrular)
    - sunucuda bulunur
    - şifresiz key ile girmek isteyenlerin, public key bilgilerini tutar
    - girebilecek davetliler listesi
    
    - public keyle bağlanmak istiyorsun
    - public key'in o dosyadaysa sunucu sana matematiksel bulmaca gönderir
    - private key ile bulmacayı çözüp gönderirsin

    - bu dosyada yoksan key ile bağlanamazsın, kullanıcı adı ve şifre ister

known_hosts: Ben doğru yere mi bağlanıyorum? (Gidilen sunucuyu doğrular)
    - clientta bulunur
    - daha önce bağlandığın ve güvenilen sunucuların dijital imzalarını (host key) kaydeder
    - bağlanmak istediğin sunucunun gerçekten gitmek istediğin sunucu olduğundan emin olmanı sağlar.
    
    - ilk kez bağlanırken, bu sunucu imzası ilk kez görülüyor güveniyor musun, diye sorar. yes dersen sunucunun benzersiz imzası known_hosts dosyasına yazılır
    - ikinciye bağlandığında sunucunun anlık imzası alınır ve known hoststakiyle karşılaştırılır, tutuyorsa bağlanılır

    - sunucunun dijital imzası için IP veya domain ismi yeter
    -> ssh-keyscan -t ed25519 10.40.10.40
    -> ssh-keyscan -H 10.40.10.40 >> ~/.ssh/known_hosts
      -H parametresi IP adresini dosyada hashler
    
    - Bu imza SSH servisleri ilk kurulduğunda sunucu için arka planda otomatik üretilir

    /etc/ssh/ssh_host_ed25519_key.pub (Dışarıya verilen imza)
    /etc/ssh/ssh_host_ed25519_key (Sunucunun bunu kanıtlamak için sakladığı gizli anahtar)
