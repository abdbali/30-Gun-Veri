Senden "Veri ve Yapay Zeka Simülasyon Serisi" için yeni bir interaktif eğitim sayfası tasarlamanı istiyorum. Sayfa, kod yazmayı ilk defa gören öğrencilere uygun, rehberlik edici ama aynı zamanda genişletilmiş bir kod yazma deneyimi sunmalıdır. 

Lütfen aşağıdaki mimari ve stil kurallarına kesin olarak uy:

1. ARAYÜZ STİLİ VE KURALLAR:
- Kesinlikle emoji, ikon veya çocuksu görsel öğeler KULLANMA. Tasarım tamamen sade, kurumsal ve profesyonel olmalıdır.
- Tailwind CSS (@tailwindcss/browser@4 cdn sürümü) kullanılacaktır.
- Sayfa sol, orta ve sağ olmak üzere 3 ana kolondan (lg:col-span-12 düzeninde 4-5-3 şeklinde) oluşmalıdır.

2. PANEL İÇERİKLERİ VE İŞLEVLERİ:
- SOL PANEL (Rehber): Gerçek bir iş senaryosu içermeli. "Kodlama Yol Haritası" başlığı altında öğrencinin orta paneldeki editöre yazması gereken mantık adım adım (1, 2, 3 diye) Türkçe açıklanmalıdır. Hemen altına öğrencilerin kopya çekebileceği minik bir syntax/sözdizimi örneği (Sözde Kod) eklenmelidir.
- ORTA PANEL (Kod Editörü): Koyu arka planlı (bg-gray-900/bg-gray-950) bir kod editörü olmalıdır. İçinde geniş bir <textarea> yer almalı, öğrenci buraya mantığı tamamen sıfırdan veya geniş bir blok halinde yazabilmelidir. Altında bir "İpucu Ekle" bir de "Derle ve Testleri Başlat" butonu yer almalıdır.
- SAĞ PANEL (Sistem Çıktısı): En üstte test edilecek örnek ham veri parametreleri listelenmeli. Altında ise "Veri Ambarı Konsolu" başlığı altında, öğrenci kodu çalıştırdığında her bir test verisi için tek tek "[OK]" veya "[HATA]" basan bir canlı log akışı bulunmalıdır.

3. TEKNİK ALTYAPI (JavaScript):
- Öğrencinin yazdığı kod, `new Function` yapısı kullanılarak dinamik olarak derlenmeli ve bir test array'indeki (en az 5-6 farklı senaryo ve bir adet uç/hatalı durum içeren veri grubu) veriler bu fonksiyondan geçirilerek test edilmelidir.
- Kodda hata olması durumunda catch bloğu çalışmalı ve tarayıcının orijinal hata mesajı konsol kutusuna kırmızı renkte basılmalıdır.
- Başarı durumuna göre sol panelin altında yeşil veya turuncu geri bildirim kutuları (feedback) çıkmalıdır.

Lütfen bu kurallara göre sadece tek bir parça halinde, doğrudan kopyalanıp çalıştırılabilecek tam HTML kodunu üret.

Şimdi üreteceğin dersin konusu: [BURAYA YENİ DERSİN KONUSUNU YAZIN, ÖRN: DERS 2 - KOD İLE EKSİK VERİLERİ TEMİZLEME]
