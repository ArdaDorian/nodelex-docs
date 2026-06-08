# Nodelex Web Editör — Dokümantasyon

**Versiyon 1.0.0 · editor.nodelex.online**  
**Kurulum gerektirmez — tarayıcınızda çalışır**

---

## İçindekiler

1. [Genel Bakış](#1-genel-bakış)
2. [Başlangıç](#2-başlangıç)
3. [Projeler](#3-projeler)
4. [Diyalog Ağaçları](#4-diyalog-ağaçları)
5. [Node Tipleri](#5-node-tipleri)
6. [Karakterler](#6-karakterler)
7. [Eventler](#7-eventler)
8. [Canvas Kontrolleri](#8-canvas-kontrolleri)
9. [İçe ve Dışa Aktarma](#9-i̇çe-ve-dışa-aktarma)
10. [Klavye Kısayolları](#10-klavye-kısayolları)

---

## 1. Genel Bakış

Nodelex Web Editör, ücretsiz ve tarayıcı tabanlı bir diyalog ağacı editörüdür. Kayıt veya kurulum gerekmez — sayfayı açın ve hemen kullanmaya başlayın.

**Yapabilecekleriniz:**
- Birden fazla diyalog ağacı içeren projeler oluşturun ve yönetin
- Görsel node tabanlı bir canvas üzerinde dallanan konuşmalar tasarlayın
- Karakter ve oyun eventlerini paylaşılan bir kütüphanede düzenleyin
- Çalışmalarınızı JSON, CSV veya Excel olarak dışa aktarın

**Web editörde bulunmayanlar:**
- AI destekli diyalog üretimi (yalnızca masaüstü uygulaması)
- Unity / Unreal Engine doğrudan dışa aktarma (yalnızca masaüstü uygulaması)

**Verileriniz tarayıcınızda yerel olarak saklanır** (IndexedDB). Tarayıcı verilerini temizlemek projelerinizi siler.

---

## 2. Başlangıç

1. **editor.nodelex.online** adresini açın
2. İlk projenizi oluşturmak için **+ Yeni Proje** kartına tıklayın
3. Proje adını ve isteğe bağlı evren bağlamını girin
4. Proje ekranına yönlendirilirsiniz — **"Main Dialogues"** adlı ilk ağaç otomatik oluşturulur
5. Editörü açmak için ağaç kartına çift tıklayın

---

## 3. Projeler

### Proje Oluşturma
Proje Seçim ekranındaki **+ Yeni Proje** kartına tıklayın. Girin:
- **Ad** — proje adınız
- **Evren Bağlamı** *(isteğe bağlı)* — oyun dünyanızın kısa açıklaması. Ağaçlar arasında tutarlı bir diyalog oluşturmaya yardımcı olur.

### Proje Düzenleme
Proje kartındaki **⋯** menüsüne → **Düzenle**'ye tıklayın. Ad ve evren bağlamını güncelleyebilirsiniz.

### Proje Silme
**⋯** menüsü → **Sil**. Bu işlem projeyi ve içindeki tüm ağaç, karakter ve eventleri kalıcı olarak siler.

---

## 4. Diyalog Ağaçları

Diyalog ağaçları bir proje içinde yer alır. Her ağaç bağımsız bir dallanan konuşmadır.

### Ağaç Oluşturma
Proje ekranında (Ağaçlar sekmesi) **+ Yeni Ağaç**'a tıklayın. Bir ad girin ve dil seçin — bu ağaçtaki tüm diyaloglar seçilen dilde yazılacaktır.

### Ağaç Özelliklerini Düzenleme
Sağdaki Özellikler kenar çubuğunu açmak için bir ağaç seçin:
- **Ad** — satır içi düzenlenebilir, odak kaybında veya Enter'da kaydedilir
- **Dil** — bu ağacın hedef dilini değiştirin
- **Etiketler** — ağaçları düzenlemek ve filtrelemek için etiket atayın

### Etiketler
Özellikler kenar çubuğunda etiket oluşturun. Ağaçlara atayın ve üstteki filtre çubuğunu kullanarak belirli bir etikete sahip ağaçları görüntüleyin.

### Ağaç Açma
Ağaç kartına çift tıklayın veya seçip kenar çubuğundaki **Editörü Aç**'a tıklayın.

---

## 5. Node Tipleri

| Node | Giriş | Çıkış | Kullanım |
|---|---|---|---|
| **Starter** | Yok | Her seçenek için bir | Konuşmayı başlatır |
| **Dialogue** | 1 | Her seçenek için bir | Oyuncu seçimleriyle devam eder |
| **Statement** | 1 | 1 | Karakter konuşur, oyuncu yanıt vermez |
| **End** | Çok | Yok | Bir dalı kapatır |
| **Event** | 1 | 1 | Sessizce bir oyun eventi tetikler |

### Node Ekleme
- Sol kenar çubuğundan canvas üzerine **sürükleyin**
- Canvas üzerine **sağ tıklayın** → node tipini seçin

### Node Düzenleme
Her node şunları içerir:
- **Karakter** açılır menüsü — konuşan bir karakter atayın (veya doğrudan yeni ekleyin)
- **Diyalog / Satır** metin alanı — yazarken otomatik boyutlanır
- **Seçenekler** *(yalnızca Starter ve Dialogue)* — oyuncu yanıt seçenekleri ekleyin, her biri kendi çıkış pinine sahip olur

### Node Bağlama
Bir çıkış pininden başka bir nodun giriş pinine sürükleyin.

**Kurallar:**
- Her çıkış pini yalnızca bir hedefe bağlanır
- Starter, Dialogue ve Statement nodeları yalnızca bir gelen bağlantı kabul eder
- End nodeları birden fazla gelen bağlantı kabul eder

### Event Nodeları
Çalışma zamanında oyuncuya görünmez. Bir oyun eventi tetikler ve otomatik olarak ilerler. Açılır menüden bir event seçin — parametreler event tanımına göre dinamik olarak görünür.

---

## 6. Karakterler

Karakterler proje bazında saklanır ve tüm ağaçlarda paylaşılır.

### Karakter Oluşturma
**Karakterler** sekmesine gidin ve **+ Yeni Karakter**'e tıklayın. Ayarlayın:
- **Ad**
- **Kategori** — NPC, Oyuncu, Tüccar, Düşman vb.
- **Renk** — avatar dairesi için kullanılır

### Karakter Düzenleme
Özellikler kenar çubuğunu açmak için bir karakter kartına tıklayın:
- Ad, kategori, renk düzenleyin
- **Nitelik** ekleyin veya kaldırın (ör. samimi, saldırgan, gizemli)

### Nodelar'da Karakter Kullanma
Herhangi bir nodda **Karakter** açılır menüsüne tıklayın. Mevcut ağaçta kullanılan karakterler hızlı erişim için üstte görünür.

Editörden çıkmadan yeni karakter eklemek için **+ Karakter Ekle**'ye tıklayın.

---

## 7. Eventler

Eventler, diyalog sırasında tetiklenen oyun tarafı işlemlerini temsil eder — altın vermek, görev başlatmak, envanter güncellemek gibi.

### Event Oluşturma
**Eventler** sekmesine gidin ve **+ Yeni Event**'e tıklayın. Ayarlayın:
- **Ad** — ör. "Altın Kazan", "Görev Başlat"
- **Parametreler** — eventin taşıdığı veriyi tanımlayın

### Parametre Tipleri
| Tip | Kullanım |
|---|---|
| `text` | Metin değerleri — eşya adları, görev kimlikleri |
| `number` | Sayısal değerler — altın miktarı, adet |
| `boolean` | Doğru/yanlış bayrakları |

### Nodelar'da Event Kullanma
Canvas'a bir **Event** node ekleyin. Açılır menüden bir event seçin — parametreleri node içinde giriş alanları olarak görünür.

**+ Event Ekle**'ye tıklayarak yeni bir event doğrudan oluşturabilirsiniz.

---

## 8. Canvas Kontrolleri

### Navigasyon
| İşlem | Nasıl |
|---|---|
| Kaydırma | Orta fare düğmesiyle sürükleme |
| Kaydırma (alternatif) | Space + sol tık sürükleme |
| Yakınlaştırma | Kaydırma tekerleği |
| Görünüme sığdır | Ctrl + Shift + F |

### Seçim
| İşlem | Nasıl |
|---|---|
| Node seç | Sol tık |
| Kement seçim | Boş canvas üzerinde sol tık sürükleme |
| Seçime ekle | Shift + tık |
| Tümünü seç | Ctrl + A |
| Seçimi kaldır | Escape |

### Düzenleme
| İşlem | Nasıl |
|---|---|
| Seçileni sil | Delete veya Backspace |
| Çoğalt | Ctrl + D (fare konumunda oluşur) |
| Kopyala | Ctrl + C |
| Yapıştır | Ctrl + V (fare konumunda oluşur) |
| Geri al | Ctrl + Z |
| Yinele | Ctrl + Y veya Ctrl + Shift + Z |
| Kaydet | Ctrl + S |

### Bağlam Menüsü
**Boş canvas üzerine sağ tık:**
- İmleç konumuna Starter / Dialogue / Statement / End / Event node ekle

**Node üzerine sağ tık:**
- Node'u Çoğalt
- Node'u Sil

---

## 9. İçe ve Dışa Aktarma

Editör araç çubuğundaki **İçe Aktar** ve **Dışa Aktar** menülerinden erişin.

### İçe Aktarma
| Format | Kullanım |
|---|---|
| **JSON** | Daha önce dışa aktarılan bir diyalog ağacını geri yükle |
| **CSV** | Tablo formatından içe aktar |

İçe aktarma mevcut canvas'ı değiştirir. Kaydedilmemiş değişiklikleriniz varsa onay istenecektir.

### Dışa Aktarma
| Format | Kullanım |
|---|---|
| **JSON** | Kayıpsız — tüm node verisi, konumlar ve bağlantılar. Yedekleme veya projeler arası aktarım için kullanın. |
| **CSV** | Düz format — seçenek/dal başına bir satır. |

### Akış Dışa Aktarma
Diyalog akışını gösteren insan tarafından okunabilir çıktı:

| Format | Kullanım |
|---|---|
| **Excel** | Node tipine göre renk kodlu satırlar, hedef önizlemeleri |
| **JSON** | Yapılandırılmış akış verisi |
| **CSV** | Düz akış verisi |

---

## 10. Klavye Kısayolları

| Kısayol | İşlem |
|---|---|
| Ctrl + S | Ağacı kaydet |
| Ctrl + Z | Geri al |
| Ctrl + Y | Yinele |
| Ctrl + Shift + Z | Yinele (alternatif) |
| Ctrl + A | Tümünü seç |
| Ctrl + D | Seçili nodeları çoğalt |
| Ctrl + C | Seçili nodeları kopyala |
| Ctrl + V | Fare konumuna yapıştır |
| Ctrl + Shift + F | Görünüme sığdır |
| Delete / Backspace | Seçili node veya bağlantıları sil |
| Escape | Seçimi kaldır / bağlam menüsünü kapat |
| Space + sürükle | Canvas'ı kaydır |
