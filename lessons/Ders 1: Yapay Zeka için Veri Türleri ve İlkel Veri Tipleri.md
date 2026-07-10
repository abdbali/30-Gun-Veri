Ders 1: Yapay Zeka için Veri Türleri ve İlkel Veri Tipleri
1. GirişYapay zeka ve makine öğrenmesi modelleri, özünde karmaşık matematiksel fonksiyonlardan ve matris optimizasyonlarından oluşur. Bir yapay zeka modelinin mimarisine beslenen verilerin bilgisayar işlemcileri (CPU/GPU) ve tensör kütüphaneleri tarafından hatasız işlenebilmesi için girdilerin doğru programlama veri tiplerine (Primitive Data Types) dönüştürülmesi şarttır.Eğer sistemimize bir sayı metin (String) formatında gelirse, yapay zeka algoritması bu veri üzerinde matematiksel gradyan hesaplaması, mesafe ölçümü veya ağırlık çarpımı gerçekleştiremez; sistem çalışma zamanı (runtime) hatası fırlatır veya veriyi kategorik bir etiket olarak algılayıp yanlış eğitilir. Bu nedenle, veri ön işleme (data preprocessing) hattının ilk kapısı, ham girdilerin türünü dinamik olarak tespit etmek, doğrulamak ve onları yapay zekanın girdi katmanına uygun saf tiplere (Number, String, Boolean) eşlemektir.2. Kademeli Örnek Kod Yapısı (5 Seviye)Aşağıdaki kod blokları, basit bir veri türü algılamasından başlayarak, yapay zeka veri akış hatlarında (data pipeline) kullanılan kurumsal seviyede bir veri standardizasyon fonksiyonuna doğru kademeli olarak ilerlemektedir.Seviye 1: Temel Tür TespitiGelen ham girdinin programlama dilindeki en temel ilkel tipini dinamik olarak yakalıyoruz.JavaScriptfunction temelTurTespiti(girdi) {
    return typeof girdi;
}

console.log(temelTurTespiti(8.4));      // "number"
console.log(temelTurTespiti("Tensör"));  // "string"
Seviye 2: Koşullu Mantıksal YönlendirmeGirdinin veri tipine göre, yapay zeka hattında hangi alt modüle (Matematiksel Regresyon / NLP) aktarılacağını belirliyoruz.JavaScriptfunction hatYonlendirici(veri) {
    if (typeof veri === "number") {
        return "Sayısal Öznitelik: Regresyon/Sınıflandırma Hattı";
    } else if (typeof veri === "string") {
        return "Metinsel Öznitelik: Doğal Dil İşleme (NLP) Hattı";
    }
    return "Hata: Desteklenmeyen İlkel Tür";
}
Seviye 3: Metinsel Sayıları Sayısallaştırma ve Güvenli Hata YönetimiAPI'lerden veya web formlarından metin (string) olarak sızan sayısal öznitelikleri, NaN (Not a Number) kontrolü yaparak güvenli bir şekilde matematiksel tipe dönüştürüyoruz.JavaScriptfunction guvenliSayiDonusumu(hamVeri) {
    let donusturulen = Number(hamVeri);
    
    if (isNaN(donusturulen)) {
        return null; // Modelin hata vermemesi için eksik veri (null) olarak işaretleme
    }
    return donusturulen;
}

console.log(guvenliSayiDonusumu("42.15")); // 42.15 (Number)
console.log(guvenliSayiDonusumu("Veri"));   // null
Seviye 4: Mantıksal Sınıflandırma Etiketlerini StandartlaştırmaYaygın olarak kullanılan ikili sınıflandırma (Binary Classification) hedeflerini veya üyelik durumlarını, yapay zeka katmanlarının doğrudan kabul ettiği $1$ ve $0$ sinyallerine eşliyoruz.JavaScriptfunction ikiliEtiketStandartlastir(durum) {
    if (typeof durum === "boolean") {
        return durum ? 1 : 0;
    }
    if (typeof durum === "string") {
        let normalize = durum.trim().toLowerCase();
        if (normalize === "aktif" || normalize === "true" || normalize === "1") return 1;
    }
    if (durum === 1) return 1;
    
    return 0; // Kalan tüm durumlar negatif sınıf (0) kabul edilir
}
Seviye 5: Kurumsal Veri Standardizasyon ve Metaveri NesnesiFarklı veri kaynaklarından gelen karmaşık ve kirli ham veriyi çözer, tipini doğrular, temizler ve yapay zeka katmanına öznitelik türünü bildiren yapılandırılmış bir nesne mimarisi teslim eder.JavaScriptfunction yapayZekaGirdiKapsulleyici(hamGirdi) {
    // String temizliği ön hazırlığı
    let temizGirdi = typeof hamGirdi === "string" ? hamGirdi.trim() : hamGirdi;
    
    // Saf Sayı Kontrolü
    if (typeof temizGirdi === "number" && !isNaN(temizGirdi)) {
        return { durum: "SUCCESS", oznitelikTuru: "continuous", veri: temizGirdi };
    }
    
    // Metinleşmiş Sayı Kontrolü ve Dönüşümü
    if (typeof temizGirdi === "string" && temizGirdi !== "" && !isNaN(Number(temizGirdi))) {
        return { durum: "SUCCESS", oznitelikTuru: "continuous", veri: Number(temizGirdi) };
    }
    
    // Mantıksal Durum Kontrolü
    if (typeof temizGirdi === "boolean") {
        return { durum: "SUCCESS", oznitelikTuru: "categorical_binary", veri: temizGirdi ? 1 : 0 };
    }
    
    // Saf Metin Kontrolü
    if (typeof temizGirdi === "string" && temizGirdi !== "") {
        return { durum: "SUCCESS", oznitelikTuru: "text_raw", veri: temizGirdi };
    }
    
    return { durum: "ERROR", oznitelikTuru: "unknown", veri: null };
}
3. Study Case Öncesi Veri Seti ÇalışmasıAşağıda, bir finans kuruluşunun dijital kredi başvuru formundan toplanan 10 satırlık ham, kirli ve yapılandırılmamış bir veri seti simülasyonu yer almaktadır. Amacımız, bu verileri yapay zeka modeline kabul etmeden önce tiplerine göre analiz etmektir.Ham Veri Seti TablosuKayıt IDKullanıcı / API GirdisiGerçekte Temsil Ettiği ÖznitelikModel İçin İdeal Tip#0128Kullanıcı YaşıNumber#02"45"Kullanıcı Yaşı (String sızıntısı)Number#03"Mühendis"Meslek Bilgisi (Kategorik)String#04trueEv Sahibi Olma DurumuBoolean (veya Matriste 1)#05"false"Ev Sahibi Olma Durumu (String)Boolean (veya Matriste 0)#0675000.50Aylık Net GelirNumber#07"3200.75"Ek Gelir Bilgisi (Metinleşmiş)Number#08"Onaylandi"Geçmiş Kredi DurumuString#090Kara Liste Riski (Pasif)Number veya Boolean#10""Eksik / Boş Bırakılmış AlanFiltrelenmeli / nullVeri Dönüşüm AnaliziBu veri seti boru hattına girdiğinde ön işleme motoru şu aksiyonları alır:#02 ve #07 numaralı kayıtlardaki string tırnakları kaldırılır; "45" $\rightarrow$ 45 ve "3200.75" $\rightarrow$ 3200.75 sayısal değerlerine güvenle dönüştürülür.#05 kaydı içindeki metinsel "false" ifadesi mantıksal false tipine veya doğrudan yapay zeka ağırlık matrisleri için 0 değerine indirgenir.#10 numaralı boş string girdisi tespit edilerek temizlenir ve sisteme tanımsız girdi sinyali gönderilir.4. Study Case (Uygulamalı Görev)Senaryo ve BeklentiBir fintech şirketinde Yapay Zeka Veri Mühendisi olarak görev alıyorsunuz. Kredi skorlama modeline veri sağlayan harici bir API, sisteme sayıları ve metinleri karışık formatlarda göndermektedir. Sizden istenen, giriş kapısına (Gateway) yerleştirilecek bir Veri Tipi Doğrulama ve Standardizasyon Modülü yazmanızdır. Fonksiyon, gelen tek bir ham veriyi işleyip kurallara göre temizlenmiş çıktıyı döndürmelidir.İş KurallarıEğer gelen veri halihazırda saf bir sayı (number) ise, veriye hiç dokunmadan aynen geriye döndürün.Eğer gelen veri bir metin (string) ise ve bu metin sayısal bir değere dönüştürülebiliyorsa (Örn: "85" veya "1450.25"), bu değeri saf sayıya (number) çevirerek döndürün.Eğer gelen veri bir metin ise ve sayıya dönüştürülemiyorsa (Örn: "Reddedildi"), metni olduğu gibi geriye döndürün.Fonksiyonunuz her koşulda işlenmiş nihai veriyi return etmelidir.Canlı Simülasyon Arayüzü (ders1.html)Aşağıdaki kod bloğunu bilgisayarınızda ders1.html adıyla kaydedip tarayıcınızda açarak yazdığınız algoritmayı doğrudan test edebilirsiniz:
