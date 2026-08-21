# 🚀 LEVEL UP AI | ROKETSAN Yapay Zeka Hackathonu

## AI Workshop — Otonom Sistemler, Sensör Füzyonu ve Kontrol

Bu repository, **LEVEL UP AI | ROKETSAN Yapay Zeka Hackathonu** kapsamında gerçekleştirilen **AI Workshop** sürecinde geliştirilen uygulama ve eğitim çalışmalarını içermektedir.

Çalışmanın temel amacı; otonom sistemlerde kullanılan **sensör verilerinin işlenmesi, durum kestirimi (state estimation), lokalizasyon, navigasyon, yol planlama ve kontrol** problemlerini Python tabanlı simülasyonlar üzerinden anlamak ve uygulamaktır.

Workshop boyunca teorik kavramlar yalnızca matematiksel olarak ele alınmamış; aynı zamanda **gürültülü sensör verileri, belirsizlik, koordinat dönüşümleri, filtreleme, sensör füzyonu ve kapalı çevrim kontrol** gibi gerçek dünya problemlerini yansıtan simülasyonlarla incelenmiştir.

---

## 🎯 Projenin Amacı

Otonom bir sistemin çevresini algılayabilmesi, nerede olduğunu belirleyebilmesi, güvenilir bir rota oluşturabilmesi ve bu rotayı takip edebilmesi için birden fazla algoritmanın birlikte çalışması gerekir.

Bu repository'de bu sürecin temel yapı taşları adım adım ele alınmaktadır:

**Sensör → Filtreleme → Durum Kestirimi → Lokalizasyon → Navigasyon → Yol Planlama → Kontrol**

Özellikle aşağıdaki sorulara uygulamalı olarak cevap aranmıştır:

* Gürültülü sensör verisinden güvenilir konum ve hız nasıl kestirilir?
* Radar verileri nasıl işlenir ve hedef takibi nasıl gerçekleştirilir?
* Farklı sensörlerin ölçümleri nasıl birleştirilir?
* GPS ve INS gibi birbirinin eksiklerini tamamlayan sistemler nasıl füze edilir?
* Bir aracın farklı koordinat sistemleri arasındaki konumu nasıl dönüştürülür?
* Bir hedefe ulaşmak için en uygun rota nasıl bulunur?
* Otonom bir araç hedef konumunu nasıl kararlı şekilde takip eder?
* Robot aynı anda hem kendi konumunu hem de çevresindeki nesneleri nasıl haritalandırabilir?

---

# 🧠 İçerik

Repository toplam **8 adet Jupyter Notebook** içermektedir.

| #  | Notebook                            | Ana Konu                                 |
| -- | ----------------------------------- | ---------------------------------------- |
| 01 | `Adım Adım Alfa Beta Filtresi`      | 1D durum kestirimi                       |
| 02 | `Navigasyon ve AlfaBeta Filtresi`   | 2D navigasyon ve heading                 |
| 03 | `Radar İz Takibi`                   | Radar simülasyonu, KF ve EKF             |
| 04 | `GPS INS Füzyonu - Lokalizasyon`    | GPS/INS sensör füzyonu                   |
| 05 | `Sensör ve Platform Body Eksenleri` | Koordinat ve eksen dönüşümleri           |
| 06 | `PID Kontrol`                       | Kapalı çevrim kontrol ve Ziegler-Nichols |
| 07 | `A Star Algoritması`                | Yol planlama ve optimum rota             |
| 08 | `1D EKF SLAM`                       | Eş zamanlı lokalizasyon ve haritalama    |

---

# 01 — Adım Adım Alfa-Beta Filtresi

📄 `01 - Adım Adım Alfa Beta Filtresi.ipynb`

Alfa-Beta filtresi, gürültülü ölçümlerden bir nesnenin **konum ve hızını kestirmek** için kullanılan, Kalman filtresinin temel mantığını anlamaya yardımcı olan basitleştirilmiş bir durum kestirim yaklaşımıdır.

Notebook içerisinde filtreleme süreci adım adım oluşturulmuştur:

1. Başlangıç durumunun tanımlanması
2. Durum tahmini (**Prediction**)
3. Sensör ölçümü ile tahmin arasındaki farkın hesaplanması
4. Durum güncellemesi (**Update / Correction**)
5. Gürültülü sensör verisi üzerinde simülasyon
6. `α` ve `β` katsayılarının filtre davranışına etkisinin incelenmesi

Özellikle **filtreleme kalitesi ile tepki hızı arasındaki trade-off** görselleştirilmiştir.

> **Temel kavramlar:** Prediction, Update, Residual, Noise Filtering, State Estimation

---

# 02 — 2D Navigasyon ve Alfa-Beta Filtresi

📄 `02 - Navigasyon ve AlfaBeta Filtresi.ipynb`

Bir boyutlu durum kestiriminden iki boyutlu navigasyon problemine geçiş yapılmaktadır.

Bu çalışmada matematiksel koordinat sistemi ile navigasyon sistemlerinde kullanılan **Heading** tanımının farklılıkları ele alınmaktadır.

### Ele alınan konular

* 2D Kartezyen koordinat sistemi
* Heading / yönelim
* Kuzey referanslı açı sistemi
* Hızın `x` ve `y` bileşenlerine ayrıştırılması
* 2D Alfa-Beta filtreleme
* Gürültülü GPS benzeri ölçümler
* Gerçek rota ve tahmin edilen rotanın karşılaştırılması

Bu çalışma, filtreleme algoritmalarının yalnızca tek eksenli hareketlerde değil, **iki boyutlu navigasyon problemlerinde** nasıl kullanılabileceğini göstermektedir.

---

# 03 — Radar İz Takibi

📄 `03 - Radar İz Takibi.ipynb`

Bu notebook, gürültülü radar ölçümlerinden hareket eden bir hedefin izinin çıkarılmasını ele almaktadır.

Radar modeli hedefi:

* **Range (Menzil)**
* **Azimuth (Yanca)**

ölçümleri üzerinden takip etmektedir.

Ölçümler Gauss gürültüsü içerirken, polar koordinatlardaki radar verileri Kartezyen koordinat sistemine dönüştürülerek hedef takibi gerçekleştirilmektedir.

### İncelenen konular

* Radar sensör modeli
* Polar → Kartezyen dönüşüm
* Gürültülü ölçüm üretimi
* Alfa-Beta Tracker
* Kalman Filtresi
* Extended Kalman Filter (EKF)
* Prediction / Update döngüsü
* Ölçüm ve model belirsizliği
* Kalman Gain
* Jacobian matrisi

Ayrıca **standart KF ile EKF arasındaki fark** radar ölçüm modeli üzerinden incelenmektedir.

Özellikle radar ölçümlerinin doğrusal olmayan yapısı nedeniyle EKF'nin neden gerekli olabileceği uygulamalı olarak gösterilmektedir.

---

# 04 — GPS + INS Sensör Füzyonu

📄 `04 - GPS INS Füzyonu - Lokalizasyon.ipynb`

Otonom araçlarda kritik öneme sahip **GPS + INS sensör füzyonu** problemi ele alınmaktadır.

Buradaki temel fikir:

* **INS:** Yüksek frekansta tahmin sağlar ancak zaman içerisinde drift üretir.
* **GPS:** Mutlak konum sağlar ancak daha düşük frekanslı ve gürültülüdür.
* **EKF:** İki sensörün avantajlarını birleştirerek daha güvenilir bir konum kestirimi oluşturur.

### Sistem

```text
             ┌─────────────┐
             │     INS     │
             │ High Rate   │
             └──────┬──────┘
                    │
                    ▼
               ┌─────────┐
               │   EKF   │
               │ Predict │
               └────┬────┘
                    │
       GPS ─────────┤
       Low Rate     │
                    ▼
               ┌─────────┐
               │ Update  │
               └────┬────┘
                    │
                    ▼
             Estimated State
```

### GPS-Denied Senaryosu

Çalışmanın önemli bölümlerinden biri GPS sinyalinin tamamen kesildiği senaryodur.

Örneğin:

* Tünel
* Kapalı otopark
* Urban canyon
* GPS karıştırılması

gibi durumlarda GPS güncellemeleri durdurulmaktadır.

Bu durumda EKF yalnızca INS tahminine devam eder ve belirsizliğin zaman içerisinde nasıl arttığı gözlemlenir.

GPS tekrar kullanılabilir hale geldiğinde ise EKF ölçüm güncellemesi ile tahmini yeniden düzeltmektedir.

> **Temel kavramlar:** INS, GPS, Dead Reckoning, Drift, Sensor Fusion, EKF, GPS-Denied Navigation

---

# 05 — Sensör ve Platform Body Eksenleri

📄 `05 - Sensör ve Platform Body Eksenleri.ipynb`

Otonom sistemlerde sensörler çoğu zaman aracın merkezine veya ana eksenlerine hizalanmış değildir.

Bu nedenle sensörün kendi lokal koordinat sisteminde elde ettiği ölçümlerin, aracın **Body Frame** koordinat sistemine dönüştürülmesi gerekir.

Notebook'ta:

* Sensör lokal koordinat sistemi
* Platform Body koordinat sistemi
* Sensör montaj açısı
* Sensörün platform üzerindeki offset'i
* Rotation Matrix
* Translation Vector
* Polar → Kartezyen dönüşüm

ele alınmaktadır.

Genel dönüşüm mantığı:

```text
Sensor Frame
     │
     │ Rotation
     ▼
Rotated Sensor Frame
     │
     │ Translation
     ▼
Platform Body Frame
```

Bu çalışma, farklı sensörlerden gelen ölçümlerin ortak bir koordinat sisteminde birleştirilmesi için gerekli olan **frame transformation** yaklaşımının temelini oluşturmaktadır.

---

# 06 — PID Kontrol

📄 `06 - PID Kontrol.ipynb`

Bir otonom sistemin yalnızca hedefi algılaması yeterli değildir. Sistem aynı zamanda hedefe **kararlı, kontrollü ve mümkün olduğunca hızlı** şekilde ulaşabilmelidir.

Bu notebook'ta kapalı çevrim PID kontrol yaklaşımı incelenmektedir.

### PID bileşenleri

**P — Proportional**

Anlık hataya göre tepki verir.

**I — Integral**

Geçmişte biriken hatayı değerlendirerek kalıcı durum hatasını azaltır.

**D — Derivative**

Hatanın değişim hızını dikkate alarak aşım ve salınımları azaltmaya yardımcı olur.

### Karşılaştırılan kontrolcüler

* P
* PI
* PD
* PID

Aynı sistem üzerinde farklı kontrolcülerin davranışları simüle edilerek:

* Steady-State Error
* Overshoot
* Oscillation
* Settling
* Response Speed

gibi performans kriterleri gözlemlenmektedir.

---

## Ziegler-Nichols Otomatik PID Tuning

Notebook'un ileri bölümünde **Ziegler-Nichols Closed-Loop** yöntemi uygulanmaktadır.

Sistematik olarak:

1. Kritik kazanç `Ku` aranır.
2. Kritik periyot `Tu` belirlenir.
3. Elde edilen değerlerden PID parametreleri hesaplanır.
4. Hesaplanan parametreler sisteme uygulanır.
5. Sistem davranışı simülasyon üzerinden gözlemlenir.

Bu sayede PID parametrelerinin tamamen manuel deneme-yanılma yöntemiyle belirlenmesi yerine **sistematik bir tuning yaklaşımı** incelenmiştir.

---

# 07 — A* Yol Planlama Algoritması

📄 `07 - A Star Algoritması.ipynb`

A* algoritması, otonom navigasyonda kullanılan temel **path planning** algoritmalarından biridir.

Temel maliyet fonksiyonu:

$$
f(n) = g(n) + h(n)
$$

Burada:

* `g(n)` → Başlangıçtan mevcut düğüme kadar gerçek maliyet
* `h(n)` → Mevcut düğümden hedefe tahmini maliyet
* `f(n)` → Toplam tahmini maliyet

Notebook'ta 2D bir harita üzerinde engeller oluşturularak A* algoritmasının hedefe ulaşmak için düğümleri nasıl keşfettiği görselleştirilmektedir.

### Özellikler

* 2D grid haritası
* Engel oluşturma
* Başlangıç ve hedef noktası
* Priority Queue
* Heuristic function
* Optimum yol arama
* Canlı algoritma animasyonu
* Rastgele harita üretimi
* Rastgele başlangıç / hedef
* Çözülebilir harita kontrolü

Bu sayede yalnızca algoritmanın sonucunu değil, **arama sürecinin kendisini** de görselleştirmek amaçlanmıştır.

---

# 08 — 1D EKF-SLAM

📄 `08 - 1D EKF SLAM.ipynb`

Çalışmanın ileri seviye konularından biri olan **SLAM (Simultaneous Localization and Mapping)** problemi, kavramların daha kolay anlaşılabilmesi amacıyla 1D ortamda ele alınmıştır.

SLAM'in temel amacı:

> Robotun kendi konumunu tahmin ederken aynı anda çevresindeki bilinmeyen landmark'ların konumlarını da keşfetmesidir.

Durum vektörü örneğin iki landmark için:

$$
\mathbf{x} =
\begin{bmatrix}
x_r \
m_1 \
m_2
\end{bmatrix}
$$

şeklinde tanımlanmaktadır.

### İncelenen EKF-SLAM adımları

1. Sistem ve dünya modelinin oluşturulması
2. State Vector tanımlanması
3. Prediction / Motion Update
4. Motion Noise modellenmesi
5. Landmark Initialization
6. Measurement Update
7. Innovation hesaplanması
8. Jacobian oluşturulması
9. Kalman Gain hesaplanması
10. Covariance güncellemesi
11. Robot ve landmark belirsizliklerinin incelenmesi

Özellikle robot bir landmark'ı ilk kez gördüğünde haritaya eklenmesi ve sonraki gözlemlerle belirsizliğin azalması görselleştirilmiştir.

Ayrıca landmark gözlemlerinin kaybolduğu durumda **dead reckoning drift** ve belirsizlik artışı gözlemlenmektedir.

---

# 🛠️ Kullanılan Teknolojiler

Çalışmalar ağırlıklı olarak Python ekosistemi kullanılarak hazırlanmıştır.

* 🐍 **Python**
* 📓 **Jupyter Notebook**
* 🔢 **NumPy**
* 📊 **Matplotlib**
* 🧮 Lineer cebir ve olasılıksal modelleme
* 🤖 Otonom sistemler ve robotik algoritmaları

---

# 🧩 Öğrenilen Temel Kavramlar

Bu workshop kapsamında aşağıdaki teknik başlıklar uygulamalı olarak ele alınmıştır:

### State Estimation

* Alfa-Beta Filter
* Kalman Filter
* Extended Kalman Filter
* Prediction / Update
* Innovation / Residual
* Kalman Gain
* Covariance

### Sensor Fusion

* GPS + INS
* Radar
* Sensör gürültüsü
* Process Noise
* Measurement Noise
* GPS-Denied Navigation

### Localization & Mapping

* Dead Reckoning
* Coordinate Frames
* Sensor-to-Body Transformation
* EKF-SLAM
* Landmark Mapping

### Navigation & Planning

* Heading
* 2D Navigation
* A* Path Planning
* Heuristic Search

### Control

* Feedback Control
* P / PI / PD / PID
* Steady-State Error
* Overshoot
* Oscillation
* Ziegler-Nichols Tuning

---

# 📈 Genel Sistem Perspektifi

Notebook'lar birbirinden bağımsız örnekler olarak görülebileceği gibi, birlikte düşünüldüğünde bir **otonom sistemin temel yazılım mimarisini** de temsil etmektedir:

```text
                 ┌─────────────────────┐
                 │       SENSÖRLER     │
                 │                     │
                 │ GPS / INS / Radar   │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   SENSOR FUSION     │
                 │                     │
                 │ KF / EKF / α-β      │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    LOCALIZATION     │
                 │                     │
                 │ Position / Velocity │
                 │ EKF-SLAM            │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │     NAVIGATION      │
                 │                     │
                 │ Heading / A*        │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │      CONTROL        │
                 │                     │
                 │ P / PI / PD / PID   │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   AUTONOMOUS SYSTEM │
                 └─────────────────────┘
```

Bu yapı; **algılama → kestirim → karar → planlama → kontrol** döngüsünün temel prensiplerini ortaya koymaktadır.

---

# 🚀 Çalışmanın Kazanımları

Workshop sonunda yalnızca algoritmaların teorik yapısı değil, algoritmaların gerçek bir otonom sistem içerisinde neden ve nerede kullanıldığı üzerinde de çalışılmıştır.

Öne çıkan kazanımlar:

* Gürültülü sensör verilerinin filtrelenmesi
* Durum ve belirsizlik kavramlarının anlaşılması
* Kalman ve Extended Kalman Filter mantığının kavranması
* Farklı sensörlerin sensör füzyonu ile birleştirilmesi
* GPS/INS tabanlı lokalizasyon
* Koordinat sistemleri ve frame dönüşümleri
* Radar tabanlı hedef takibi
* GPS-Denied ortamların modellenmesi
* Path planning algoritmalarının uygulanması
* SLAM probleminin temel mantığının anlaşılması
* Kapalı çevrim kontrol sistemlerinin modellenmesi
* PID parametrelerinin otomatik tuning yaklaşımının incelenmesi
* Algoritmaların simülasyon ve görselleştirme ile analiz edilmesi

---

# 📂 Repository Yapısı

```text
.
├── 01 - Adım Adım Alfa Beta Filtresi.ipynb
├── 02 - Navigasyon ve AlfaBeta Filtresi.ipynb
├── 03 - Radar İz Takibi.ipynb
├── 04 - GPS INS Füzyonu - Lokalizasyon.ipynb
├── 05 - Sensör ve Platform Body Eksenleri.ipynb
├── 06 - PID Kontrol.ipynb
├── 07 - A Star Algoritması.ipynb
└── 08 - 1D EKF SLAM.ipynb
```

---

# ▶️ Çalıştırma

Projeyi lokal ortamınızda çalıştırmak için Python ve Jupyter Notebook kurulumu yeterlidir.

```bash
git clone <repository-url>
cd <repository-directory>
jupyter notebook
```

Ardından ilgili `.ipynb` dosyası Jupyter Notebook veya JupyterLab üzerinden açılarak hücreler sırasıyla çalıştırılabilir.

Gerekli temel Python paketleri:

```bash
pip install numpy matplotlib jupyter
```

---

# 📌 Not

Bu repository'deki çalışmaların temel amacı, otonom sistemlerde kullanılan algoritmaları **pedagojik ve simülasyon tabanlı bir yaklaşımla** incelemektir.

Simülasyonlar gerçek bir araç veya sensör sisteminin tüm fiziksel özelliklerini birebir modellememektedir. Bunun yerine algoritmaların çalışma prensiplerini, avantajlarını, sınırlamalarını ve birbirleriyle olan ilişkilerini anlaşılır şekilde ortaya koymayı hedeflemektedir.

---

# 🏆 LEVEL UP AI | ROKETSAN

Bu çalışma, **LEVEL UP AI | ROKETSAN Yapay Zeka Hackathonu — AI Workshop** kapsamında gerçekleştirilmiştir.

Workshop sürecinde edinilen teorik bilgilerin Python tabanlı uygulamalar, simülasyonlar ve görselleştirmeler aracılığıyla pratiğe aktarılması hedeflenmiştir.

**Otonom sistemler perspektifinden; algılama, kestirim, lokalizasyon, navigasyon, planlama ve kontrol süreçlerinin uçtan uca incelendiği bir çalışma olarak hazırlanmıştır.**

---

## 📜 License

Bu repository'nin kullanım ve lisans koşulları için repository içerisindeki lisans dosyasını inceleyiniz.

---

<div align="center">

### 🚀 From Sensors to Autonomy

**LEVEL UP AI | ROKETSAN — AI Workshop**

*State Estimation • Sensor Fusion • Localization • Navigation • Planning • Control*

</div>
