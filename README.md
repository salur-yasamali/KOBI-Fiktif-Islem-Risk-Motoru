# KOBİ Bankacılığında Murabaha Fiktif İşlem (Naylon Fatura) Tespit Motoru
**Rol:** Veri Odaklı Ürün Yöneticisi (Data-Driven PM)

## 🎯 Projenin Ticari Amacı (Business Case)
KOBİ bankacılığında tahsis edilen Murabaha kredilerinin, fonun amacı dışında (round-tripping, tabela şirketleri, danışıklı dövüş) kullanılmasını engellemek. Bu proje, operasyonel yükü hafifletmeyi, "False Positive" (yanlış alarm) oranlarını düşürerek gerçek KOBİ'leri mağdur etmemeyi ve bankanın kâr-zarar (P&L) tablosunu batık kredilerden korumayı amaçlayan veri odaklı bir karar destek sistemidir.

## ⚙️ Risk Motoru ve Puanlama Mantığı (Business Logic)
Sistemin kalbinde, sadece "Evet/Hayır" diyen statik bir kural seti değil; müşteri hareketlerini ticari ağırlıklarına göre oranlayan dinamik bir risk skorlaması yatmaktadır. Toplam 100 puanlık risk bütçesi şu şekilde dağıtılmıştır:

1. **EFT Dönüş Oranı (%25 Ağırlık):** Kredinin şahsi hesaplara geri dönmesi "Suçüstü" kabul edilir. Orana göre lineer puanlanır.
2. **Şirket Yaşı (%20 Ağırlık):** Yeni kurulan şirketlerin "Tabela Şirketi" olma riski 0-24 ay arasında kademeli olarak (Min-Max Scaling) puanlanır. 24 aydan büyük şirketlerde risk sıfırlanır.
3. **SGK Çalışan Sayısı (%20 Ağırlık):** 0 çalışanlı şirketler tam risk alırken, 10 çalışana kadar risk kademeli düşer.
4. **NACE Uyumu (%15 Ağırlık):** Fatura ile resmi sektör uyumsuzluğu anlık risk oluşturur.
5. **Ortak IP Adresi (%10 Ağırlık):** KOBİ ve Tedarikçinin aynı internet ağından bankacılığa girmesi organize işlemi işaret eder.
6. **Mernis Akrabalığı (%10 Ağırlık):** Ticaretin gerçek olma ihtimaline karşı "False Positive"i engellemek için ağırlığı %10'da tutulmuş, derecesine göre (1. derece / 2. derece) puanlanmıştır.

## 🖥️ Saha Operasyonu ve Şube Ekranı 
Makine öğrenmesi modelinin ürettiği bu fiktif işlem skoru salt bir arka plan verisi olarak kalmaz; şube gişe ekranlarına (front-end) doğrudan entegre bir karar destek mekanizmasına dönüşür. Risk puanı 70 eşiğini aşan KOBİ başvurularında sistem işlemi kilitler ve şube çalışanının ekranına otomatik bir kısıt pop-up'ı düşürür: 

> ⚠️ **Sistemsel Kısıt: Operasyonel Risk Kriteri** > *"Bu işlem, sistem tarafından otomatik olarak durdurulmuştur. Kredi kullandırım sürecine devam edilebilmesi için işlemin merkeze gönderilmesi gerekmektedir."*

**Aksiyon :** Bu kurgu sayesinde şube KOBİ portföy yöneticisi, müşteriyle karşı karşıya (papaz) kalmaz. İşlem tek bir butona basılarak iptal edilmez; aksine arka planda hesaplanan o 6 boyutlu detaylı risk skoru loglanarak, dosya doğrudan Merkez'deki **Teftiş ve Tahsis kurullarının** inceleme ekranına sevk edilir. Süreç, müşteri memnuniyetini zedelemeden ve personeli paniğe sevk etmeden otonom şekilde yönetilir.
