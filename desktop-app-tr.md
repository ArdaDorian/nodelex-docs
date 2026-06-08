# Nodelex Masaüstü Uygulaması — Dokümantasyon

**Versiyon 0.1.0 · Windows / macOS / Linux**  
**Gereksinim:** Python backend (uygulama ile birlikte gelir)

---

## İçindekiler

1. [Genel Bakış](#1-genel-bakış)
2. [Kurulum](#2-kurulum)
3. [Başlangıç](#3-başlangıç)
4. [Projeler](#4-projeler)
5. [Diyalog Ağaçları](#5-diyalog-ağaçları)
6. [Node Tipleri](#6-node-tipleri)
7. [Karakterler](#7-karakterler)
8. [Eventler](#8-eventler)
9. [Canvas Kontrolleri](#9-canvas-kontrolleri)
10. [İçe ve Dışa Aktarma](#10-i̇çe-ve-dışa-aktarma)
11. [Unity Dışa Aktarma](#11-unity-dışa-aktarma)
12. [AI Özellikleri](#12-ai-özellikleri)
13. [Klavye Kısayolları](#13-klavye-kısayolları)

---

## 1. Genel Bakış

Nodelex Masaüstü, tam özellikli NPC diyalog ağacı editörüdür. Web editördeki her şeye ek olarak AI destekli diyalog üretimi, Unity motor dışa aktarma ve yerel veri kalıcılığı sunar.

**Yapabilecekleriniz:**
- Birden fazla diyalog ağacı ve karakter içeren projeler oluşturun ve yönetin
- Görsel olarak dallanan konuşmalar tasarlayın
- Nodeları doldurmak veya tam diyalog ağaçları oluşturmak için AI kullanın
- Doğrudan Unity'ye dışa aktarın

**Verileriniz yerel olarak** makinenizdeki bir SQLite veritabanında saklanır.

---

## 2. Kurulum

1. **nodelex.online** adresinden en son sürümü indirin
2. Yükleyiciyi çalıştırın
3. Uygulamalar klasöründen **Nodelex**'i başlatın

Python backend, uygulama açıldığında otomatik olarak başlar ve kapattığınızda durur.

---

## 3. Başlangıç

1. Uygulamayı başlatın
2. İlk projenizi oluşturmak için **+ Yeni Proje**'ye tıklayın
3. Ad ve isteğe bağlı evren bağlamı girin
4. İlk ağaç **"Main Dialogues"** otomatik oluşturulur
5. Editörü açmak için ağaç kartına çift tıklayın

---

## 4. Projeler

### Proje Oluşturma
**+ Yeni Proje**'ye tıklayın. Girin:
- **Ad**
- **Evren Bağlamı** *(isteğe bağlı)* — oyun dünyanızı tanımlayın. Zaman dilimi,派派派派派派派派派派派派派派派 派派派派派派派派派派派派派派派 派派派派派派派派派派派派派派派 派派派派派派派派派派派派派派派 Bu bağlam, diyalog oluştururken AI'ya gönderilir.

### Proje Düzenleme
Proje kartında **⋯** → **Düzenle**.

### Proje Silme
**⋯** → **Sil**. Projeyi ve tüm verilerini kalıcı olarak siler.

### Unity Proje Yolu
Editörde **Editör menüsü → Unity'ye Aktar → Unity Projesi Bul** seçeneğiyle bir Unity proje klasörü bağlayın. Ayarlandıktan sonra dosya iletişim kutusu olmadan doğrudan dışa aktarmak için **Commit** kullanın.

---

## 5. Diyalog Ağaçları

### Ağaç Oluşturma
Proje ekranında (Ağaçlar sekmesi) **+ Yeni Ağaç**'a tıklayın. Ad ve dil belirleyin.

### Ağaç Özellikleri
Özellikler kenar çubuğunu açmak için bir ağaç seçin:
- **Ad** — satır içi düzenlenebilir
- **Dil** — AI bu dilde diyalog üretir
- **Etiketler** — filtreleme için

### Etiketler
Özellikler kenar çubuğunda oluşturun ve atayın. Filtre çubuğuyla ağaçları filtreleyin.

### Ağaç Açma
Ağaç kartına çift tıklayın veya kenar çubuğunda **Editörü Aç**'a tıklayın.

---

## 6. Node Tipleri

| Node | Giriş | Çıkış | Kullanım |
|---|---|---|---|
| **Starter** | Yok | Her seçenek için bir | Konuşmayı başlatır |
| **Dialogue** | 1 | Her seçenek için bir | Oyuncu seçimleriyle devam eder |
| **Statement** | 1 | 1 | Karakter konuşur, oyuncu yanıt vermez |
| **End** | Çok | Yok | Bir dalı kapatır |
| **Event** | 1 | 1 | Sessizce bir oyun eventi tetikler |

### Node Ekleme
- Sol kenar çubuğundan **sürükleyin**
- Canvas üzerine **sağ tıklayın** → node tipi seçin

### Node Bağlama
Bir çıkış pininden giriş pinine sürükleyin.

**Kenar kuralları:**
- Her çıkış pini → tam olarak bir hedef
- Starter, Dialogue, Statement, Event nodeları → maksimum bir gelen bağlantı
- End nodeları → birden fazla gelen bağlantı kabul edilir

### Event Nodeları
Çalışma zamanında oyuncuya görünmez. Bir oyun eventi tetikler ve otomatik olarak ilerler. Dışa aktarmalarda şeffaftır — bağlantı, event node yokmuş gibi köprülenir.

---

## 7. Karakterler

Karakterler proje bazında saklanır ve tüm ağaçlarda paylaşılır.

### Karakter Oluşturma
Karakterler sekmesi → **+ Yeni Karakter**:
- **Ad**
- **Kategori** — NPC, Oyuncu, Tüccar, Düşman, Müttefik, Görev Verici, Diğer
- **Renk** — avatar rengi

### Karakter Özellikleri
Kenar çubuğunu açmak için bir karaktere tıklayın:
- Ad, kategori, renk düzenleyin
- **Nitelik** ekleyin / kaldırın — bu nitelikler, bu karakter için diyalog üretilirken AI'ya gönderilir

### Nodelar'da Karakter Kullanma
Herhangi bir nodda **Karakter** açılır menüsünden seçin. Mevcut ağaçta kullanılan karakterler üstte görünür. Editörden çıkmadan oluşturmak için **+ Karakter Ekle**'ye tıklayın.

---

## 8. Eventler

Eventler, diyalog sırasında tetiklenen oyun tarafı işlemlerini temsil eder — para vermek, görev başlatmak, envanter güncellemek.

### Event Oluşturma
Eventler sekmesi → **+ Yeni Event**:
- **Ad** — ör. "Altın Kazan"
- **Parametreler** — eventin taşıdığı tipli alanlar

### Parametre Tipleri
| Tip | Açıklama |
|---|---|
| `text` | Metin — eşya adı, görev kimliği |
| `number` | Sayısal — altın miktarı |
| `boolean` | Doğru / yanlış |

### Nodelar'da Event Kullanma
Bir **Event** node ekleyin. Açılır menüden event seçin — parametreler giriş alanları olarak görünür. Yeni event oluşturmak için **+ Event Ekle**'ye tıklayın.

---

## 9. Canvas Kontrolleri

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
| Çoğalt | Ctrl + D |
| Kopyala | Ctrl + C |
| Yapıştır | Ctrl + V |
| Geri al | Ctrl + Z |
| Yinele | Ctrl + Y veya Ctrl + Shift + Z |
| Kaydet | Ctrl + S |

### Bağlam Menüsü — Canvas
Boş canvas üzerine sağ tık:
- Starter / Dialogue / Statement / End / Event node ekle
- **✦ AI ile Oluştur** — tam bir diyalog ağacı üret

### Bağlam Menüsü — Node
Node üzerine sağ tık:
- **✦ AI ile Doldur** — bu nodu AI kullanarak doldur
- Node'u Çoğalt
- Node'u Sil

---

## 10. İçe ve Dışa Aktarma

Başlık çubuğundaki **Editör** menüsünden erişin.

### Transfer (İçe / Dışa Aktarma)
Kayıpsız format — tüm node verisi, konumlar ve bağlantıları korur. Yedekleme veya projeler arası taşıma için kullanın.

| Format | Açıklama |
|---|---|
| JSON | Tam ağaç verisi |
| CSV | Seçenek başına düz satır formatı |

### Akış Dışa Aktarma
Diyalog akışını gösteren insan tarafından okunabilir çıktı:

| Format | Açıklama |
|---|---|
| Excel | Renk kodlu satırlar, hedef önizlemeleri |
| JSON | Yapılandırılmış akış verisi |
| CSV | Düz akış verisi |

---

## 11. Unity Dışa Aktarma

Diyalog ağacınızı doğrudan bir Unity projesine aktarın.

### Kurulum
1. Bir ağaç için editörü açın
2. **Editör menüsü → Unity'ye Aktar → Unity Projesi Bul**
3. Unity proje kök klasörünü seçin (`Assets/` klasörünü içeren klasör)
4. Nodelex `Assets/Export/` klasörünü otomatik olarak oluşturur

### Commit
Kurulumdan sonra, iletişim kutusu olmadan anında dosya yazmak için **Unity'ye Aktar → Commit** kullanın.

Her commit iki dosya yazar:
- `{AgaçAdı}.unity.json` — diyalog ağacı
- `characters.json` — projenizdeki tüm karakterler

### Manuel Dışa Aktarma
Özel bir kayıt konumu seçmek için **Unity'ye Aktar → Dışa Aktar...** kullanın.

### Unity'de
Bu dosyaları nasıl içe aktaracağınız ve kullanacağınız için **[Unity Paketi Dokümantasyonuna](#)** bakın.

---

## 12. AI Özellikleri

AI özellikleri, desteklenen bir sağlayıcıdan API anahtarı gerektirir.

### Kurulum
1. **Ayarlar menüsü → AI Ayarları**
2. Bir sağlayıcı seçin (Gemini, Claude veya OpenAI)
3. Bir model seçin
4. API anahtarınızı girin ve **Anahtarı Kaydet**'e tıklayın

### AI ile Doldur
Node üzerine sağ tık → **✦ AI ile Doldur**

Node'un diyalog metnini ve seçeneklerini şunlara göre doldurur:
- Karakterin adı, kategorisi ve nitelikleri
- Üst akış konuşma bağlamı (en fazla 10 önceki node)
- Talimatınız

Bu özelliği kullanmadan önce node'a bir karakter atanmış olmalıdır.

### AI ile Oluştur
Canvas üzerine sağ tık → **✦ AI ile Oluştur**

Şunlara göre sıfırdan tam bir diyalog ağacı oluşturur:
- Seçilen karakterler ve nitelikleri
- Sahne bağlamı açıklamanız
- Ağacın dil ayarı
- Projenin evren bağlamı

Oluşturulan nodelar canvas'a eklenir. İnceleyin ve hazır olduğunuzda kaydedin.

### Desteklenen Sağlayıcılar

| Sağlayıcı | Ücretsiz Katman | Notlar |
|---|---|---|
| **Gemini Flash** | ✓ Evet | Çoğu kullanıcı için önerilir |
| **Claude** | Sınırlı | Daha yüksek kaliteli çıktı |
| **OpenAI** | Hayır | GPT-4o ve varyantları |

---

## 13. Klavye Kısayolları

| Kısayol | İşlem |
|---|---|
| Ctrl + S | Ağacı kaydet |
| Ctrl + Z | Geri al |
| Ctrl + Y | Yinele |
| Ctrl + Shift + Z | Yinele (alternatif) |
| Ctrl + A | Tümünü seç |
| Ctrl + D | Seçilileri çoğalt |
| Ctrl + C | Seçilileri kopyala |
| Ctrl + V | Fare konumuna yapıştır |
| Ctrl + Shift + F | Görünüme sığdır |
| Delete / Backspace | Seçilileri sil |
| Escape | Seçimi kaldır / menüyü kapat |
| Space + sürükle | Canvas'ı kaydır |
