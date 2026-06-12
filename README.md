# 📰 AANewsCrawler - Data Mining System

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg?style=for-the-badge&logo=python)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Scraping Target](https://img.shields.io/badge/Target-Anadolu%20Agency-orange.svg?style=for-the-badge)](https://www.aa.com.tr/)
[![Framework](https://img.shields.io/badge/Lib-PyQuery%20%26%20Requests-green.svg?style=for-the-badge)](https://pypi.org/project/pyquery/)

Bu proje, **Anadolu Ajansı (AA)** haber sitesi üzerinde belirtilen ID aralıklarında otomatik tarama yapan, haber başlığı, özeti, içeriği, kategorisi, dili ve görselleri gibi veri madenciliği bileşenlerini ayıklayıp yerel veritabanı formatında (`JSON` & `CSV`) saklayan profesyonel bir **Web Crawler** ve **Veri Madenciliği** aracıdır.

---

## 🚀 Öne Çıkan Özellikler

*   **🔄 Kaldığı Yerden Devam Etme (Resumable State):** Crawler başlatıldığında otomatik olarak mevcut `news.json` dosyasını kontrol eder. En son işlenen haber ID'sini tespit edip, kaldığı ID'den güvenli bir şekilde taramaya devam eder.
*   **🛡️ Hata Toleransı & Otomatik Yeniden Deneme (Resilience):** Bağlantı kopmaları, sunucu zaman aşımları (`Timeout`) veya geçersiz URL durumlarında çökmeyi önleyen dinamik istisna yönetimi (Exception Handling) ve yeniden deneme mekanizması.
*   **⌨️ Acil Durum Durdurma (Graceful Stop):** Çalışma esnasında `ESC` tuşuna basıldığında crawler işlemleri güvenli bir şekilde sonlandırır, o ana kadar çekilen tüm verileri diske yazar ve veri kaybını önler.
*   **📁 Hiyerarşik Görsel İndirici (Media Downloader):** Haberlere ait büyük boyutlu kapak fotoğraflarını otomatik olarak indirir ve diskte klasör kirliliğini önlemek amacıyla **yüzlük ID gruplarına göre** (`./images/3332000/`, `./images/3332100/` vb.) klasörler halinde organize eder.
*   **📝 Görsel Tanım Günlüğü (Metadata CSV):** İndirilen görsellerin `alt` (açıklama) etiketlerini haber ID'si ile eşleştirerek `./images/image_definitions.csv` dosyasına UTF-8 kodlaması ile kaydeder.
*   **⏱️ Sunucu Dostu Tarama (Polite Crawling):** İki istek arasında dinamik bekleme süreleri eklenerek Anadolu Ajansı sunucularına aşırı yük bindirilmesi (DDoS algısı) ve IP bloklanması engellenir.

---

## 📁 Proje Yapısı

```bash
├── main.py            # Uygulama giriş noktası, tarama döngüsü ve parametre kontrolü
├── crawler.py         # HTTP istekleri, PyQuery ayrıştırma mantığı ve dosya kaydetme işlemleri
├── data.py            # OOP standardına uygun News (Haber) veri modeli ve JSON dönüştürücü
├── news.json          # Toplanan tüm haberlerin yapılandırılmış verisini saklayan ana JSON veri kümesi
├── README.md          # Proje dökümantasyonu
└── images/            # İndirilen görsellerin ve CSV eşleşmelerinin saklandığı dizin
    ├── image_definitions.csv  # Görsel açıklamaları ve ID eşleşmeleri (CSV formatında)
    └── [ID_GRUP]/     # Haber ID'sine göre gruplanmış alt klasörler (Örn: 3332000/)
```

---

## 📦 Kurulum ve Gereksinimler

Projenin çalıştırılması için Python 3.8+ yüklü olmalıdır. Gerekli kütüphaneleri yüklemek için aşağıdaki adımları izleyin:

### 1. Depoyu Klonlayın

```bash
git clone https://github.com/emirtdede/AANewsCrawler-DataMining.git
cd AANewsCrawler-DataMining
```

### 2. Bağımlılıkları Yükleyin

Proje HTTP istekleri için `requests`, HTML ayrıştırma için `pyquery` ve klavye dinleme için `keyboard` kütüphanelerini kullanmaktadır:

```bash
pip install requests pyquery keyboard
```

> 💡 **Not (Windows/Linux):** `keyboard` kütüphanesi tuş vuruşlarını global olarak dinlediği için Linux üzerinde root yetkileri, Windows üzerinde ise bazı terminal yapılandırmalarında yönetici izinleri gerektirebilir.

---

## 🛠️ Kullanım ve Yapılandırma

Crawler'ın taranacağı haber aralığını `main.py` dosyasından düzenleyebilirsiniz:

```python
# main.py içerisindeki hedef ID aralığını belirleyin:
start_id = 3_332_000
end_id = 3_338_000
```

Ardından uygulamayı başlatın:

```bash
python main.py
```

### Taramayı Durdurma
Tarama işlemi devam ederken durdurmak isterseniz klavyenizden **`ESC`** tuşuna basmanız yeterlidir. Crawler döngüden güvenli bir şekilde çıkıp verileri diske kaydedecektir.

---

## 📊 Veri Şeması (JSON)

Toplanan her bir haber verisi `news.json` dosyasına aşağıdaki şemada kaydedilir:

```json
[
  {
    "ID": 3332001,
    "Title": "Haber Başlığı",
    "Link": "https://www.aa.com.tr/tr/gundem/haber-detay-url-adresi/3332001",
    "Summary": "Haberin kısa özet veya spot başlık bilgisi.",
    "Date": "24.01.2025",
    "Language": "tr",
    "Category": "gundem",
    "Body": "Haberin detaylı paragraf içeriklerinin birleştirilmiş metin hali..."
  }
]
```

---

## 🛡️ Lisans

Bu proje **MIT Lisansı** altında lisanslanmıştır. Detaylar için lisans dökümanını inceleyebilirsiniz.

---

## 🧑‍💻 Geliştirici

*   **Emir Tarık Dede** - [GitHub Profili](https://github.com/emirtdede)
