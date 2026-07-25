# 🛰️ EcoSmart: Yapay Zekâ Destekli Geri Dönüşüm Denetim Merkezi

**EcoSmart**, kamera görüntüsü üzerinden nesneleri gerçek zamanlı tespit eden ve bu nesnelerin doğru geri dönüşüm kutusuna atılıp atılmadığını dinamik olarak analiz eden **hibrit (YOLOv5 + HSV Segmentasyonu)** bir yapay zekâ denetim sistemidir.

---

## 📸 Uygulama Ekran Görüntüsü

<img width="1896" height="822" alt="Ekran görüntüsü 2026-07-26 021342" src="https://github.com/user-attachments/assets/4022e2b0-1c0f-4c39-bdc7-9ee445f47b65" />


---

### 🛠️ Sistem Nasıl Çalışır? (Çalışma Mantığı)

Proje, **iki farklı görüntü işleme tekniğini** aynı anda çalıştırarak akıllı bir karar mekanizması oluşturur:

* 🎥 **Canlı Kamera Görüntüsü (Web):** Görüntü anlık olarak alınır.
* 🤖 **1. YOLOv5 Nesne Tespiti:** Kağıt/Plastik nesnelerini tanır, koordinat ve merkez noktası bulur.
* 🎨 **2. HSV Renk Segmentasyonu:** Geri dönüşüm kutularını tespit eder, kutu merkezlerini hesaplar.
* 📐 **3. Akıllı Karar & Mesafe Kontrolü:** Ekranın %70 sınırını ve 280px Öklid mesafesini kontrol eder.
* 📊 **4. Streamlit Dashboard Arayüzü:** Ekranda ✅ DOĞRU ÜNİTE veya ❌ YANLIŞ ÜNİTE kararı gösterilir.

---

### 1. Dinamik Işık Dengesi (Gamma Correction)
Ortamdaki düşük ışık koşullarından etkilenmemek adına her bir kamera karesi matematiksel gamma düzeltmesinden (`adjust_gamma`) geçirilerek parlatılır.

### 2. Kutu Tespiti (HSV Renk Segmentasyonu)
Elde edilen özel HSV (Hue, Saturation, Value) aralıkları kullanılır. Kağıt ve plastik geri dönüşüm ünitelerinin ekrandaki konumları ve merkez noktaları anlık olarak tespit edilir.

### 3. Yapay Zekâ ile Nesne Tespiti (YOLOv5)
Eğitilmiş özel model (`best.pt`), ekrandaki nesneleri (Kağıt, Plastik, Çöp vb.) tespit eder. Yanlış tespitleri önlemek için **Non-Max Suppression (NMS)** ve güven eşiği (%20) uygulanır.

### 4. Akıllı Mühürleme & Karar Mantığı
* **Analiz Bölgesi:** Ekranın %70'inden yukarısı analiz alanı kabul edilir.
* **Mesafe Hesabı:** Nesne merkezi ile kutu merkezi arasındaki Öklid mesafesi ($d = \sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}$) hesaplanır.
* **Karar Kilitlenmesi:** Eğer nesne kutuya yeterince yakınsa ($<280\text{ px}$) sistem kararı kilitler ("✅ DOĞRU ÜNİTE" veya "❌ YANLIŞ ÜNİTE"). Nesne alandan ayrıldığında durum otomatik sıfırlanır.

---

## 🛠️ Kullanılan Teknolojiler

* **Arayüz (Dashboard):** Streamlit
* **Yapay Zekâ (AI):** PyTorch, YOLOv5
* **Görüntü İşleme:** OpenCV (`cv2`), NumPy
* **Programlama Dili:** Python 3.10+

---

## 🚀 Kurulum ve Çalıştırma

**1. Bağımlılıkları Yükleyin:**
`pip install -r yolov5/requirements.txt`

**2. Uygulamayı Başlatın:**
`streamlit run yolov5/app_dinamik.py`
