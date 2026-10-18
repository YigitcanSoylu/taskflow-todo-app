# 🚀 TaskFlow — Görev Yönetim Uygulaması

TaskFlow, kullanıcıların günlük görevlerini düzenlemelerini, önceliklerini belirlemelerini ve üretkenliklerini takip etmelerini sağlayan modern ve duyarlı bir görev yönetim uygulamasıdır.

Bu proje, yazılım geliştirme eğitimi kapsamında modern web teknolojilerini uygulamalı olarak kullanmak, bileşen tabanlı mimariyi anlamak ve temel yazılım geliştirme becerilerini pekiştirmek amacıyla geliştirilmiştir.

## ✨ Özellikler

* **Görev Yönetimi (CRUD):** Görev ekleme, listeleme, güncelleme ve silme işlemleri.
* **Öncelik Yönetimi:** Görevleri düşük, orta ve yüksek öncelik seviyelerine göre sınıflandırma.
* **Kategori Yönetimi:** Görevleri farklı kategoriler altında düzenleme.
* **Son Tarih Takibi:** Görevlere son tarih atama ve geciken görevleri belirleme.
* **Arama ve Filtreleme:** Görevleri arama, durumlarına ve kategorilerine göre filtreleme.
* **Sürükle ve Bırak:** Görevlerin sıralamasını etkileşimli şekilde değiştirme.
* **İstatistik Paneli:** Görev tamamlama durumunu, öncelikleri, kategorileri ve son tarihleri analiz etme.
* **Koyu ve Açık Tema:** Kullanıcı tercihine göre arayüz temasını değiştirme.
* **Yerel Veri Saklama:** Tarayıcı yenilendiğinde görevlerin korunması.
* **Duyarlı Tasarım:** Farklı ekran boyutlarına uyum sağlayan kullanıcı arayüzü.
* **Bileşen Tabanlı Mimari:** Yeniden kullanılabilir ve düzenli Vue bileşenleri.

## 🛠️ Kullanılan Teknolojiler

| Teknoloji     | Kullanım Amacı                                  |
| ------------- | ----------------------------------------------- |
| Vue 3         | Kullanıcı arayüzü ve bileşen tabanlı geliştirme |
| JavaScript    | Uygulama mantığı ve işlevsellik                 |
| Vite          | Geliştirme sunucusu ve üretim derlemesi         |
| Pinia         | Merkezi durum yönetimi                          |
| Vue Router    | Sayfalar arası yönlendirme                      |
| Tailwind CSS  | Arayüz stillendirme                             |
| HTML5 ve CSS3 | Sayfa yapısı ve görsel tasarım                  |
| Local Storage | Görev verilerinin tarayıcıda saklanması         |

## 📸 Ekran Görüntüleri

Uygulamanın ekran görüntülerini `screenshots` klasörüne ekleyerek aşağıdaki bölümlerde gösterebilirsin.

### Ana Sayfa

![TaskFlow Ana Sayfa](screenshots/ana-sayfa.png)

### İstatistikler Sayfası

![TaskFlow İstatistikler Sayfası](screenshots/istatistikler.png)

### Hakkında Sayfası

![TaskFlow Hakkında Sayfası](screenshots/hakkinda.png)

## 🚀 Kurulum ve Çalıştırma

Projeyi kendi bilgisayarında çalıştırmak için aşağıdaki adımları izleyebilirsin.

### Gereksinimler

* Node.js
* npm
* Git

### 1. Projeyi klonla

GitHub deposunun adresini kullanarak projeyi bilgisayarına indir:

```bash
git clone GITHUB_DEPO_ADRESIN
```

`GITHUB_DEPO_ADRESIN` kısmını kendi GitHub depo adresinle değiştir.

### 2. Proje klasörüne gir

```bash
cd taskflow-todo-app
```

### 3. Bağımlılıkları yükle

```bash
npm install
```

### 4. Geliştirme sunucusunu başlat

```bash
npm run dev
```

Terminalde görüntülenen yerel adresi tarayıcıda açarak uygulamayı kullanabilirsin.

### 5. Üretim derlemesi oluştur

```bash
npm run build
```

Bu komut, uygulamanın dağıtıma hazır dosyalarını `dist` klasörüne oluşturur.

Üretim sürümünü yerel ortamda önizlemek için:

```bash
npm run preview
```

## 📁 Proje Dosya Yapısı

```text
todo_app/
├── public/
├── src/
│   ├── assets/
│   │   ├── main.css
│   │   └── base.css
│   ├── components/
│   │   ├── TodoForm.vue
│   │   └── TodoItem.vue
│   ├── interfaces/
│   │   └── todo.js
│   ├── router/
│   ├── stores/
│   │   └── todoStore.js
│   ├── views/
│   │   ├── TodoView.vue
│   │   ├── StatsView.vue
│   │   └── AboutView.vue
│   ├── App.vue
│   └── main.js
├── screenshots/
├── index.html
├── package.json
└── README.md
```

## 💾 Veri Saklama

TaskFlow, görev verilerini tarayıcının **Local Storage** özelliğini kullanarak saklar.

Bu sayede sayfa yenilendiğinde kayıtlı görevler korunur. Uygulama, görevleri saklamak için harici bir sunucuya veya veritabanına ihtiyaç duymaz.

**Not:** Veriler kullanıcının tarayıcısında saklanır. Farklı cihazlar veya tarayıcılar arasında otomatik veri senkronizasyonu yapılmaz.

## 🎯 Projenin Amaçları

Bu projenin temel amaçları şunlardır:

* Vue 3 ve modern JavaScript kullanımını uygulamalı olarak öğrenmek.
* CRUD işlemlerini gerçek bir uygulama üzerinde gerçekleştirmek.
* Yeniden kullanılabilir bileşenler geliştirmek.
* Pinia ile uygulama durumunu yönetmek.
* Vue Router ile sayfalar arası yönlendirme yapmak.
* Local Storage kullanarak verileri kalıcı olarak saklamak.
* Kullanıcı dostu ve duyarlı bir arayüz tasarlamak.
* Git ve GitHub kullanarak sürüm kontrolü süreçlerini uygulamak.
* Bir web uygulamasını internet üzerinden yayınlama sürecini öğrenmek.

## 🔮 Gelecekte Eklenebilecek Özellikler

* Kullanıcı kaydı ve giriş sistemi.
* Bulut tabanlı veri saklama ve cihazlar arası senkronizasyon.
* Görevler için bildirim ve hatırlatıcılar.
* Gelişmiş üretkenlik analizleri.
* Otomatik testler ve birim testleri.
* Erişilebilirlik iyileştirmeleri.



Vue 3 ve JavaScript ile geliştirilmiştir.
