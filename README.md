# Enerji Şirketleri Misyon Vizyon Projesi

Bu proje, enerji şirketlerine ait misyon ve vizyon cümlelerinin analiz edilmesi için bir doğal dil işleme (NLP) modeli geliştirmeyi amaçlar. Proje kapsamında, veri temizleme, veri ön işleme, kelime gömme (word embedding) matrislerinin oluşturulması ve bir RNN (Recurrent Neural Network) tabanlı seq2seq modelinin eğitilmesi adımları yer almaktadır.


## İçindekiler
1. Proje Hakkında
2. Gereksinimler
3. Veri Seti ve Hazırlık
4. Veri Ön İşleme
5. Model Mimarisi
6. Model Eğitimi
7. Sonuçların İncelenmesi
8. Özelleştirme ve Geliştirme
9. Proje Yapısı


## Proje Hakkında

Bu proje, enerji şirketlerinin stratejik amaçlarını belirlemek için misyon ve vizyon cümlelerini analiz etmek amacıyla geliştirilmiştir. Proje, TensorFlow ve Python'ın çeşitli NLP kütüphanelerini kullanarak cümlelerin temizlenmesi, anlamlandırılması ve analiz edilmesini sağlar.


## Gereksinimler

Projeyi çalıştırmak için aşağıdaki Python kütüphaneleri gereklidir:
* `pandas`: Veri analizi ve veri işleme.
* `numpy`: Sayısal hesaplamalar ve dizi işlemleri
* `tensorflow.compat.v1`: Derin öğrenme modellerinin oluşturulması ve eğitilmesi.
* `re`: Düzenli ifadelerle metin işleme.
* `nltk`: Doğal dil işleme ve durma kelimelerinin temizlenmesi.
* `openpyxl`: Excel dosyalarının okunması ve yazılması.
* `time`: Zamanla ilgili işlemler.

Bu kütüphaneleri kurmak için:
```bash
pip install pandas numpy tensorflow=1.15.0 nltk openpyxl
```

## Veri Seti ve Hazırlık

Projede kullanılan veri seti, enerji şirketlerine ait misyon ve vizyon cümlelerini içerir ve bir Excel dosyasında saklanır. Veri seti, Türkçe ve İngilizce cümlelerin yanı sıra, bu cümlelerin `Misyon` veya `Vizyon` kategorisine ait olup olmadığını ve şirketin lokasyon bilgilerini de içermektedir.

Veri setinin yolu ve dosya formatı:
* Dosya Yolu: `/content/drive/MyDrive/Misyon Vizyon NLP/İşlenmiş Veri/ Enerji Şirketleri Misyon Vizyon.xlsx`
* Sütunlar:
  * `Cümleler`: Türkçe cümleler
  * `Cümlelerin İngilizceleri`: İngilizce cümleler
  *  `MV`: Misyon veya vizyon bilgisi (`Misyon`, `Vizyon`)
  *  `Lokasyon`: Şirketin yurtiçi veya yurtdışında bulunma durumu


## Veri Ön İşleme

Veri seti üzerinde gerçekleştirilen ön işleme adımları şunlardır:

1. **Büyük/Küçük Harf Dönüşümleri:** Tüm cümleler küçük harfe dönüştürülür.
2. **Stopwords Temizliği:** NLTK kütüphanesi kullanılarak durma kelimeleri (stop words) temizlenir.
3. **Kısaltmaların Açılması:** İngilizce cümlelerdeki kısaltmalar, uzun halleriyle değiştirilir.
4. **Noktalama İşaretlerinin Temizlenmesi:** Noktalama işaretleri ve özel karakterler cümlelerden çıkarılır.
5. **One-Hot Encoding:** Kategorik veriler (MV ve Lokasyon) sayısal verilere dönüştürülür.
6. **Sütunların Yeniden Adlandırılması:** Veri seti sütunları daha açıklayıcı hale getirilir (örneğin, `Cümlelerin İngilizceleri` sütunu `eng_sent` olarak yeniden adlandırılır).


## Model Mimarisi

Projede kullanılan model, bir seq2seq (sequence-to-sequence) modelidir. Bu model, RNN tabanlı olup, enerji şirketlerinin misyon ve vizyon cümlelerini analiz etmek için eğitilmiştir. Modelin mimarisi şu şekildedir:

* **Girdi Katmanı:** Model için gerekli olan veri girişlerini alır.
* **Encoder:** Girdi cümlelerini, RNN hücreleri kullanarak bir anlam temsilcisine dönüştürür.
* **Decoder:** Kodlayıcıdan alınan temsilci bilgileriyle çıktıları üretir.
* **Attention Mechanism:** Modelin önemli kısımlara odaklanmasını sağlar.
* **Çıkış Katmanı:** Sonuçları çıkarır ve tahmin edilen kelimeleri üretir.


## Model Eğitimi

Modelin eğitimi şu adımları içerir:

1. **Veri Setinin Bölünmesi:** Eğitim ve test veri setlerine bölünür.
2. **Modelin Tanımlanması:** TensorFlow kullanılarak modelin yapısı ve katmanları tanımlanır.
3. **Hiperparametrelerin Ayarlanması:** Modelin eğitimi için gereken parametreler belirlenir:
     * Epochs: Eğitim döngülerinin sayısı.
     * Batch Size: Her eğitim döngüsünde kullanılacak veri örneği sayısı.
     * RNN Size: RNN hücrelerinin büyüklüğü.
     * Num Layers: Modelde kullanılacak katman sayısı.
     * Learning Rate: Öğrenme oranı.
4. **Optimizasyon ve Kayıp Fonksiyonu:** Adam Optimizer kullanılarak modelin eğitimi gerçekleştirilir ve kayıp fonksiyonu minimize edilir.
5. **Modelin Eğitimi:** Eğitim verisi kullanılarak model eğitilir.



## Sonuçların İncelenmesi

Model eğitildikten sonra, kelime gömme matrisleri ve modelin kelimeleri nasıl anladığı incelenir. Modelin başarı oranı, kelimelerin kullanım sıklığı, ve modelin bilmediği kelimeleri nasıl ele aldığı değerlendirilir.
* **Kelime Gömme Matrisleri:** Her kelimeye karşılık gelen vektör değerleri içerir.
* **Kullanım Oranı:** Model tarafından kullanılan kelimelerin toplam kelimelere oranı.
* **Eksik Kelimeler:** Modelin eğitiminde kullanılan kelimelerden eksik olanlar ve bu kelimelerin yüzdesi.

## Özelleştirme ve Geliştirme

Proje, belirli durumlar için özelleştirilebilir ve geliştirilebilir:
* **Farklı Veri Setleri Kullanımı:** Projeyi farklı misyon ve vizyon veri setleri ile test edebilirsiniz.
* **Model Parametrelerinin Ayarlanması:** RNN büyüklüğü, katman sayısı gibi hiperparametreleri değiştirerek model performansını optimize edebilirsiniz.
* **Yeni Özelliklerin Eklenmesi:** Örneğin, başka NLP teknikleri veya veri temizleme adımları eklenebilir.

## Proje Yapısı

Proje dosyalarının genel yapısı şu şekildedir:

```bash
Enerji Şirketleri Misyon Vizyon Projesi/
│
├── veriler/
│   └── Enerji Şirketleri Misyon Vizyon.xlsx
│
├── kodlar/
│   ├── main.py
│
└── README.md
```

