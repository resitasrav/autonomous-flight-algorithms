# Autonomous Flight Algorithms 🛰️

### 🛠️ Geliştirme Ekosistemi ve Standartlar

Bu projede kullanılan teknolojiler ve haberleşme protokolleri endüstri standartlarına dayanmaktadır:

- **Diller:** - [![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/) - *Scripting ve MAVSDK entegrasyonu.*
  - [![C++](https://img.shields.io/badge/C++-%2300599C.svg?style=flat&logo=c%2B%2B&logoColor=white)](https://isocpp.org/) - *Düşük seviyeli uçuş algoritmaları.*
  - [![XML](https://img.shields.io/badge/XML-FFA500?style=flat&logo=xml&logoColor=white)](https://en.wikipedia.org/wiki/XML) - *MAVLink mesaj tanımlamaları ve SDF modelleri.*

- **Otonom Sistem Katmanları:**
  - [![PX4 Autopilot](https://img.shields.io/badge/Platform-PX4_Autopilot-blue)](https://px4.io/) - *Uçuş kontrol yazılımı çekirdeği.*
  - [![ROS2](https://img.shields.io/badge/Middleware-ROS2-106699)](https://docs.ros.org/en/foxy/index.html) - *Robotik işletim sistemi katmanı.*
  - [![MAVLink](https://img.shields.io/badge/Protocol-MAVLink-blueviolet)](https://mavlink.io/en/) - *İHA-Yer İstasyonu haberleşme protokolü.*

- **Simülasyon ve Arayüz:**
  - [![Gazebo](https://img.shields.io/badge/Simulator-Gazebo-orange)](https://gazebosim.org/home) - *Fizik tabanlı 3D simülasyon ortamı.*
  - [![QGroundControl](https://img.shields.io/badge/GCS-QGroundControl-purple)](http://qgroundcontrol.com/) - *Yer kontrol istasyonu ve telemetri.*
  - [![SITL](https://img.shields.io/badge/Test-SITL-brightgreen)](https://docs.px4.io/main/en/simulation/) - *Software-in-the-Loop test metodolojisi.*
Bu proje; otonom İHA sistemleri için **PX4 Autopilot**, **Gazebo** ve **QGroundControl (QGC)** ekosistemlerinin entegrasyonu üzerine kurgulanmıştır. Geliştirilen algoritmalar, MAVLink protokolü üzerinden tam zamanlı veri alışverişi yaparak otonom görev icra etmektedir.

---

## 🛠️ Sistem Entegrasyon Mimarisi

Bir savunma sanayii projesinde sistemin nasıl haberleştiğini anlamak kritiktir. Bu projede kurduğum yapı şu şekildedir:

1.  **PX4 Autopilot (Beyin):** Uçuş kontrolcü yazılımı. Algoritmaların koşturulduğu ve uçuş kararlarının verildiği merkez.
2.  **Gazebo (Fiziksel Dünya):** İHA'nın fiziksel ağırlığı, rüzgar direnci ve sensör (Lidar, GPS, IMU) verilerinin simüle edildiği ortam.
3.  **QGroundControl (Komuta Merkezi):** MAVLink üzerinden telemetri verilerinin izlendiği, görevlerin (waypoint) harita üzerinden atandığı kullanıcı arayüzü.

> **Haberleşme Akışı:**
> `Gazebo (Sensör Verisi) -> PX4 (Algoritma İşleme) -> MAVLink -> QGroundControl (Görselleştirme/Komuta)`

---

## 📸 Sistem Ekosistemi Görünümü

<div align="center">
  <img src="assets/px4_gazebo_qgc_ekosistemi.png" alt="Sistem Haberleşmesi" width="900">
  <p><em>Şekil 1: Sağ tarafta Gazebo fiziksel simülasyonu ve Ubuntu 22.04 terminali, sol tarafta QGroundControl telemetri ekranı ve terminal üzerinde koşan PX4 SITL katmanı.</em></p>
</div>

| Otonom Görev İcrası | MAVLink Telemetri Akışı |
| :---: | :---: |
| <img src="assets/qgc_rota.png" width="400"> | <img src="assets/anlik_veri.png" width="400"> |
| *QGC üzerinden rota takibi* | *Hız, irtifa ve batarya verileri* |

---

## 📊 Simülasyon ve Sistem Testleri

Projenin Gazebo ve PX4 üzerindeki çalışma performansına dair ekran görüntüleri aşağıdadır. 

### 🛸 Multi-İHA ve Sistem Başlangıcı
<div align="center">
  <img src="assets/MULTI_IHA_gazebo_px4.png" alt="Multi İHA Gazebo PX4" width="850">
  <p><em>Şekil 1: PX4 ve Gazebo üzerinde çoklu İHA (Multi-UAV) sistem entegrasyonu ve SITL başlangıcı.</em></p>
</div>

---

### 🛠 Operasyonel Görünümler

| Sistem Başlangıcı | Kalkış Sekansı |
| :---: | :---: |
| <img src="assets/sistem_baslangici.png" width="400"> | <img src="assets/kalkis_gazebo.png" width="400"> |
| *Terminal üzerinden MAVLink bağlantısı* | *Otonom kalkış ve waypoint takibi* |

| Multi-İHA Yakın Plan | Alternatif Görünüm |
| :---: | :---: |
| <img src="assets/multi_iha_gazebo.png" width="400"> | <img src="assets/multi_iha_gazebo2.png" width="400"> |
| *Sürü algoritmaları test ortamı* | *Farklı kamera açılarından takip* |



---

## 🎯 Uygulanan Mühendislik Çözümleri
* SITL Simülasyonu: Geliştirme maliyetlerini düşürmek ve uçuş güvenliğini artırmak amacıyla, fiziksel prototip öncesi tüm senaryolar Gazebo SITL ortamında doğrulanmıştır.

* Haberleşme Katmanı: İHA ve Yer Kontrol İstasyonu (GCS) arasındaki veri akışı, endüstri standardı olan MAVLink protokolü ile optimize edilmiştir.

* Otonom Kontrol (Offboard): Standart uçuş modlarının ötesine geçilerek, MAVSDK üzerinden özel yörünge algoritmaları ve görev odaklı otonom komut seti entegre edilmiştir.
## 📁 Proje Klasör Yapısı

```text
.
├── src/                # Offboard kontrol scriptleri (MAVSDK/C++)
├── config/             # QGroundControl parametre dosyaları (.params)
├── worlds/             # Gazebo özel görev alanları (.world)
├── models/             # İHA ve sensör konfigürasyonları
└── docs/               # Sistem akış diyagramları


---

## ✍️ Geliştirici ve İletişim

Bu proje **Reşit Asrav** tarafından otonom sistemler ve savunma sanayii teknolojilerine katkı sunmak amacıyla geliştirilmiştir. Teknik detaylar, iş birliği veya projeye dair sorularınız için aşağıdaki kanallardan bana ulaşabilirsiniz:

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=flat&logo=linkedin&logoColor=white)](linkedin.com/in/reşit-asrav-94510b232) 
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:resitasrav@email.com)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/resitasrav)

> "Gelecek otonom sistemlerde, otonom sistemler ise doğru algoritmalarla şekillenir." 🚀
