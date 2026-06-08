# Nodelex Unity Package — Dokümantasyon

**Versiyon 1.0.0 · Unity 6+**  
**Bağımlılık:** `com.unity.nuget.newtonsoft-json`

---

## İçindekiler

1. [Genel Bakış](#1-genel-bakış)
2. [Kurulum](#2-kurulum)
3. [Hızlı Başlangıç](#3-hızlı-başlangıç)
4. [Diyalog Ağacı İçe Aktarma](#4-diyalog-ağacı-içe-aktarma)
5. [DialogueRunner](#5-dialoguerunner)
6. [UI Bağlantısı](#6-ui-bağlantısı)
7. [Oyun Event'lerini Yönetme](#7-oyun-eventlerini-yönetme)
8. [Birden Fazla Diyalog Ağacı](#8-birden-fazla-diyalog-ağacı)
9. [IDialogueUI Arayüzü](#9-idialogueui-arayüzü)
10. [Veri Referansı](#10-veri-referansı)
11. [Sorun Giderme](#11-sorun-giderme)

---

## 1. Genel Bakış

Nodelex Unity Package, **Nodelex** diyalog ağacı editörü ile Unity oyununuz arasındaki köprüdür. Diyalog ağaçlarınızı Nodelex'te görsel olarak tasarlayın, tek tuşla export edin — package geri kalanını halleder.

**Package'ın yaptıkları:**
- Nodelex'ten gelen `.unity.json` dosyalarını Unity ScriptableObject asset'lerine çevirir
- Diyalog ağaçlarını runtime'da `DialogueRunner` ile çalıştırır
- Nodelex'te tanımlanan oyun event'lerini (quest, envanter, para birimi vb.) tetikler
- Event'leri temiz bir C# API üzerinden oyun kodunuza yönlendirir

**Sizin yaptıklarınız:**
- Diyalog UI'ını tasarlamak (metin kutusu, seçenek butonları, konuşmacı adı)
- Oyun event'lerine tepki vermek (altın vermek, quest başlatmak vb.)
- Oyuncu NPC ile etkileşime girince `StartDialogue()` çağırmak

---

## 2. Kurulum

### Adım 1 — Scriptleri Ekle

`Nodelex/` klasörünü projenizin `Assets/` dizinine kopyalayın:

```
Assets/
└── Nodelex/
    ├── Editor/
    └── Runtime/
```

### Adım 2 — Newtonsoft.Json Yükle

1. **Window → Package Manager** menüsünü açın
2. **+** → **Add package by name** seçin
3. Şunu girin: `com.unity.nuget.newtonsoft-json`
4. **Add**'e tıklayın

### Adım 3 — Doğrula

**Window → Nodelex → Import Dialogue Tree** penceresini açın. Hatasız açılıyorsa kurulum tamamdır.

---

## 3. Hızlı Başlangıç

Sıfırdan çalışan bir diyaloga en hızlı yol. 2D sahne, NPC etkileşimi, HUD ve oyun event'lerini içeren tam çalışan bir örnek için pakete dahil **Sample Project**'e göz atın.

**Adım 1 — Nodelex'ten export et**

Nodelex uygulamasında Editor menüsünden **Export to Unity**'ye tıklayın. `export/` klasörüne iki dosya yazılır:

```
export/
├── agac-adiniz.unity.json
└── characters.json
```

**Adım 2 — Unity'ye import et**

**Window → Nodelex → Import Dialogue Tree**'yi açın, `.unity.json` dosyanızı seçin ve **Import**'a tıklayın. Asset'ler `Assets/DialogueTrees/` altında oluşur.

**Adım 3 — NPC'yi hazırla**

NPC GameObject'inize `DialogueRunner` component'i ekleyin. Oluşan `DialogueTreeSO` asset'ini **Dialogue Tree** alanına sürükleyin.

**Adım 4 — Diyalogu başlat**

```csharp
private DialogueRunner _runner;

private void Awake()
{
    _runner = GetComponent<DialogueRunner>();
}

public void Interact()
{
    _runner.StartDialogue();
}
```

**Adım 5 — Node ve seçenekleri dinle**

```csharp
_runner.OnNodeEntered      += node    => Debug.Log(node.dialogueText);
_runner.OnOptionsPresented += options => Debug.Log($"{options.Count} seçenek");
_runner.OnDialogueEnded    += ()      => Debug.Log("Diyalog bitti");
```

Minimum kurulum bu kadar. UI entegrasyonu, event yönetimi ve gelişmiş kullanım için sonraki bölümlere bakın.

---

## 4. Diyalog Ağacı İçe Aktarma

### 4.1 Nodelex'ten Export Et

Nodelex Editor menüsünde **Export to Unity**'ye tıklayın. Şunlar üretilir:

- **`agac-adiniz.unity.json`** — diyalog ağacı
- **`characters.json`** — Nodelex projesindeki tüm karakterler

Import sırasında bu iki dosyanın aynı klasörde olması gerekir.

### 4.2 Manuel Import — Editor Penceresi

1. **Window → Nodelex → Import Dialogue Tree**'yi açın
2. **Browse…**'a tıklayın ve `.unity.json` dosyanızı seçin
3. Aynı klasörde `characters.json` varsa `✓ characters.json detected` yazar
4. **Output Folder**'ı ayarlayın (varsayılan: `Assets/DialogueTrees`)
5. **Characters Folder**'ı ayarlayın — tüm karakterler buraya kaydedilir, tüm ağaçlar arasında paylaşılır (varsayılan: `Assets/DialogueTrees/Characters`)
6. **Import**'a tıklayın

### 4.3 Otomatik Import — Sürükle & Bırak

`.unity.json` dosyasını doğrudan Unity Project penceresine sürükleyin. Package otomatik algılar ve import'u başlatır. `characters.json` aynı klasördeyse otomatik yüklenir.

### 4.4 Yeniden Import

Aynı ağacı tekrar import etmek mevcut asset'leri siler ve yeniden oluşturmaz — **günceller**. Sahne referansları ve Inspector atamaları korunur. Nodelex'te güncelleme yaptıktan sonra `DialogueTreeSO`'yu tekrar atamanıza gerek yoktur.

### 4.5 Import Sonrası Asset Yapısı

```
Assets/DialogueTrees/
├── Characters/             ← tüm ağaçlar arasında paylaşılır
│   ├── Josh.asset
│   └── Muhafiz.asset
├── TavernScene/
│   ├── TavernScene.asset   ← bunu DialogueRunner'a atayın
│   └── Nodes/
│       ├── node_abc.asset
│       └── node_def.asset
└── DungeonScene/
    ├── DungeonScene.asset
    └── Nodes/
```

---

## 5. DialogueRunner

`DialogueRunner`, runtime'da `DialogueTreeSO`'yu yürüten MonoBehaviour'dur. NPC GameObject'inize ekleyin.

### 5.1 Inspector Alanları

| Alan | Tip | Açıklama |
|---|---|---|
| **Dialogue Tree** | `DialogueTreeSO` | Çalıştırılacak ağaç. Koddan da atanabilir. |
| **Dialogue UI Component** | `MonoBehaviour` | Opsiyonel. `IDialogueUI` implement eden UI scripti. |
| **Auto Advance Statements** | `bool` | Aktifse Statement node'lar belirli süre sonra otomatik ilerler. |
| **Statement Auto Delay** | `float` | Otomatik ilerleme öncesi bekleme süresi (saniye). Varsayılan: `2`. |

### 5.2 Unity Events (Inspector'dan bağlanabilir)

| Event | Ne zaman tetiklenir |
|---|---|
| `onDialogueStarted` | `StartDialogue()` çağrıldığında |
| `onDialogueEnded` | End node'a ulaşıldığında veya `StopDialogue()` çağrıldığında |
| `onSpeakerChanged` | Konuşmacı karakteri değiştiğinde — karakter adını `string` olarak iletir |

### 5.3 C# Event'leri

```csharp
// Görünür bir node aktif olduğunda tetiklenir (Starter, Dialogue, Statement)
runner.OnNodeEntered += (DialogueNodeSO node) => { };

// Seçenekler hazır olduğunda tetiklenir
// Yalnızca Starter ve Dialogue node'larında — Statement ve End'de tetiklenmez
runner.OnOptionsPresented += (IReadOnlyList<DialogueOption> options) => { };

// Bir Event node geçildiğinde tetiklenir
runner.OnDialogueEvent += (DialogueEventPayload payload) => { };

// Diyalog sona erdiğinde tetiklenir
runner.OnDialogueEnded += () => { };
```

### 5.4 Public API

```csharp
// Inspector'da atanan ağaçla başlat
runner.StartDialogue();

// Belirli bir ağaçla başlat
runner.StartDialogue(DialogueTreeSO tree);

// Oyuncu seçeneği seç — 0'dan başlayan index
runner.SelectOption(int index);

// Statement node'u ilerlet
// Yalnızca mevcut node'un seçeneği yoksa geçerlidir
runner.Advance();

// Diyalogu anında durdur
runner.StopDialogue();

// Mevcut durum
bool           runner.IsRunning;
DialogueNodeSO runner.CurrentNode;
DialogueTreeSO runner.ActiveTree;
```

### 5.5 Runtime'da Node Davranışları

| Node Tipi | Ne yapar | Nasıl ilerler |
|---|---|---|
| **Starter** | İlk node. Diyalog metni ve seçenekler içerir. | `SelectOption(index)` |
| **Dialogue** | Bir veya daha fazla oyuncu yanıtı olan diyalog satırı. | `SelectOption(index)` |
| **Statement** | Oyuncu yanıtı olmayan diyalog satırı (monolog, anlatı). | `Advance()` veya otomatik ilerleme |
| **Event** | Oyuncuya görünmez. Bir oyun event'i tetikler ve otomatik geçer. | Otomatik |
| **End** | Diyalogu sonlandırır. `OnDialogueEnded`'ı tetikler. | Otomatik |

**Event node şeffaflığı:** Oyuncu bir seçenek seçtiğinde, sonraki görünür node'a ulaşmadan önce bir veya daha fazla Event node geçiliyorsa tüm event'ler sırayla tetiklenir. Oyuncu hiçbir kesinti görmez — A → Event → B ile A → B aynı görünür.

---

## 6. UI Bağlantısı

Önerilen yaklaşım, UI scriptinizden `DialogueRunner`'ın C# event'lerine doğrudan subscribe olmaktır. Bu, UI'ınızı Nodelex package'ından bağımsız tutar.

### 6.1 Önerilen Pattern

```csharp
public class DialoguePanel : MonoBehaviour
{
    [SerializeField] private TMP_Text   speakerNameText;
    [SerializeField] private TMP_Text   dialogueBodyText;
    [SerializeField] private Transform  optionsContainer;
    [SerializeField] private GameObject optionButtonPrefab;
    [SerializeField] private Button     continueButton;

    private DialogueRunner _runner;

    public void Initialize(DialogueRunner runner)
    {
        _runner = runner;
        _runner.OnNodeEntered      += HandleNodeEntered;
        _runner.OnOptionsPresented += HandleOptions;
        _runner.OnDialogueEnded    += HandleEnd;

        continueButton.gameObject.SetActive(false);
        continueButton.onClick.AddListener(() => _runner.Advance());
    }

    private void OnDestroy()
    {
        if (_runner == null) return;
        _runner.OnNodeEntered      -= HandleNodeEntered;
        _runner.OnOptionsPresented -= HandleOptions;
        _runner.OnDialogueEnded    -= HandleEnd;
    }

    private void HandleNodeEntered(DialogueNodeSO node)
    {
        speakerNameText.text  = node.character != null ? node.character.characterName : string.Empty;
        dialogueBodyText.text = node.dialogueText;
        continueButton.gameObject.SetActive(node.IsStatementNode);

        foreach (Transform child in optionsContainer)
            Destroy(child.gameObject);
    }

    private void HandleOptions(IReadOnlyList<DialogueOption> options)
    {
        foreach (Transform child in optionsContainer)
            Destroy(child.gameObject);

        for (int i = 0; i < options.Count; i++)
        {
            int capturedIndex = i;
            var obj = Instantiate(optionButtonPrefab, optionsContainer);
            obj.GetComponentInChildren<TMP_Text>().text = options[i].text;
            obj.GetComponent<Button>().onClick.AddListener(() =>
                _runner.SelectOption(capturedIndex));
        }
    }

    private void HandleEnd()
    {
        // Son node metni görünsün diye panel anında kapanmıyor
        // Continue butonuna basınca kapanır
        continueButton.gameObject.SetActive(true);
        continueButton.onClick.RemoveAllListeners();
        continueButton.onClick.AddListener(() => Destroy(gameObject));
    }
}
```

### 6.2 Önemli: OnDestroy'da Unsubscribe Et

UI paneli destroy edilmeden önce runner event'lerinden mutlaka unsubscribe olun. Unsubscribe edilmeden destroy edilirse, bir sonraki diyalogda runner yok edilmiş nesneye erişmeye çalışır ve `MissingReferenceException` fırlatır. Bölüm 11'e bakın.

### 6.3 Paneli Spawn Etmek

Yaygın bir pattern: diyalog başladığında paneli instantiate et, bittiğinde destroy et.

```csharp
// NPC veya manager scriptinizde:
private void OnInteract()
{
    var panel = Instantiate(dialoguePanelPrefab, hudTransform);
    panel.GetComponent<DialoguePanel>().Initialize(_runner);
    _runner.StartDialogue();
}
```

### 6.4 Koşullu Advance

`Advance()` public'tir ve herhangi bir yerden çağrılabilir. Bu sayede Statement node'ları bir butona, animasyona, coroutine'e veya herhangi bir oyun koşuluna bağlayabilirsiniz:

```csharp
// Buton basışı
continueButton.onClick.AddListener(() => _runner.Advance());

// Animasyon tamamlanınca
private IEnumerator AnimasyonBitinceDEvam()
{
    yield return animator.PlayAnimation("giris");
    _runner.Advance();
}

// Belirli süre sonra
private IEnumerator BekleyipIlerle(float saniye)
{
    yield return new WaitForSeconds(saniye);
    _runner.Advance();
}

// Koşul sağlanınca
if (oyuncudaEsyaVar)
    _runner.Advance();
```

---

## 7. Oyun Event'lerini Yönetme

Diyalog Event node'ları oyun tarafı mantığı tetikler — para ödülü, quest başlatma, envanter güncelleme, ses çalma, kamera hareketi.

### 7.1 DialogueEventListener (Önerilen)

`DialogueEventListener`'dan bir subclass oluşturun ve `DialogueRunner` ile **aynı GameObject'e** ekleyin. Her event için `Awake`'te bir handler kaydedin.

```csharp
using Nodelex;

public class TavernEventHandler : DialogueEventListener
{
    protected override void Awake()
    {
        base.Awake(); // Zorunlu — ilk satır olmalı

        Register("evt_gold_acquired", OnGoldAcquired);
        Register("evt_quest_start",   OnQuestStarted);
        Register("evt_camera_zoom",   OnCameraZoom);
    }

    private void OnGoldAcquired(DialogueEventPayload payload)
    {
        float miktar = payload.Get<float>("amount");
        GameManager.Instance.AltinEkle(miktar);
    }

    private void OnQuestStarted(DialogueEventPayload payload)
    {
        QuestManager.Instance.QuestBaslat("ana_gorev");
    }

    private void OnCameraZoom(DialogueEventPayload payload)
    {
        StartCoroutine(KameraManager.ZoomYap());
    }
}
```

**Event adıyla register edin, ID ile değil.** Nodelex'teki event ID'leri otomatik üretilen UUID'lerdir. Bunun yerine Nodelex'te verdiğiniz event adını kullanın (ör. `"evt_gold_acquired"`).

#### Tüm event'leri yakalamak

`OnEventReceived`'ı override ederek tek bir handler'dan tüm event'leri dinleyebilirsiniz:

```csharp
protected override void OnEventReceived(DialogueEventPayload payload)
{
    Analytics.Kaydet("diyalog_event", payload.EventName);
}
```

#### Kayıtsız event'leri yakalamak

`OnUnhandledEvent`'i override ederek handler'ı olmayan event'leri yönetebilirsiniz:

```csharp
protected override void OnUnhandledEvent(DialogueEventPayload payload)
{
    Debug.LogWarning($"Handler bulunamadı: {payload.EventName}");
}
```

### 7.2 Doğrudan Subscribe

Daha basit durumlarda `OnDialogueEvent`'e doğrudan subscribe olabilirsiniz:

```csharp
_runner.OnDialogueEvent += payload =>
{
    if (payload.EventName == "evt_gold_acquired")
        GameManager.Instance.AltinEkle(payload.Get<float>("amount"));
};
```

### 7.3 DialogueEventPayload

```csharp
payload.EventId     // string — Nodelex'in otomatik ürettiği UUID
payload.EventName   // string — Nodelex'te verdiğiniz event adı
payload.Parameters  // IReadOnlyDictionary<string, object>

// Parametre adına göre tip-güvenli erişim
float  miktar  = payload.Get<float>("amount");
string kaynak  = payload.Get<string>("source");
bool   aktif   = payload.Get<bool>("isActive");
```

---

## 8. Birden Fazla Diyalog Ağacı

Bir NPC birden fazla ağaca sahip olabilir — ilk karşılaşma için biri, quest sonrası için başkası. Runtime'da aralarında geçiş yapın:

```csharp
public class Maren : NPC
{
    [SerializeField] private DialogueTreeSO ilkKarsilasmaAgaci;
    [SerializeField] private DialogueTreeSO questSonrasiAgac;

    private bool _questBasladi;

    public override void Interact(GameObject instigator)
    {
        var agac = _questBasladi ? questSonrasiAgac : ilkKarsilasmaAgaci;
        _runner.StartDialogue(agac);
    }

    // Event handler'dan çağırın
    public void QuestBasladiginda()
    {
        _questBasladi = true;
    }
}
```

Her ağaç ayrı bir `DialogueTreeSO` asset'i olarak import edilir. Hepsini Inspector'da atayın, geçişi kodda yönetin.

---

## 9. IDialogueUI Arayüzü

`IDialogueUI`, C# event'lerine subscribe olmaya alternatif opsiyonel bir yaklaşımdır. UI MonoBehaviour'unuza implement edin ve `DialogueRunner`'ın **Dialogue UI Component** alanına atayın. Runner durum değiştikçe metodları otomatik çağırır.

```csharp
public interface IDialogueUI
{
    // Starter ve Dialogue node'larında çağrılır
    void ShowDialogue(string speakerName, string text, IReadOnlyList<DialogueOption> options);

    // Statement node'larında çağrılır
    // Oyuncu hazır olduğunda runner.Advance() çağırın
    void ShowStatement(string speakerName, string text);

    // Diyalog sona erdiğinde çağrılır
    void HideDialogue();
}
```

Temiz bir interface sözleşmesi istiyorsanız `IDialogueUI` kullanın. Daha fazla kontrol veya tam bağımsızlık istiyorsanız C# event'leri (Bölüm 6) kullanın.

---

## 10. Veri Referansı

### DialogueTreeSO

| Alan | Tip | Açıklama |
|---|---|---|
| `treeName` | `string` | Ağacın adı |
| `sourceFile` | `string` | Orijinal export dosyasının adı |
| `nodes` | `List<DialogueNodeSO>` | Tüm node'lar |
| `edges` | `List<DialogueEdge>` | Tüm bağlantılar |
| `characters` | `List<CharacterSO>` | Bu ağaçta referans alınan karakterler |

### DialogueNodeSO

| Alan | Tip | Açıklama |
|---|---|---|
| `nodeId` | `string` | Benzersiz tanımlayıcı |
| `nodeType` | `NodeType` | Starter / Dialogue / Statement / Event / End |
| `character` | `CharacterSO` | Konuşan karakter, `null` olabilir |
| `dialogueText` | `string` | Diyalog satırı |
| `options` | `List<DialogueOption>` | Oyuncu seçenekleri |
| `eventId` | `string` | UUID (yalnızca Event node'larda) |
| `eventName` | `string` | Gösterim adı (yalnızca Event node'larda) |
| `eventParameters` | `List<EventParameter>` | Parametreler (yalnızca Event node'larda) |
| `IsStatementNode` | `bool` | Yardımcı property |
| `IsEventNode` | `bool` | Yardımcı property |
| `IsTerminal` | `bool` | End node'sa `true` |
| `HasOptions` | `bool` | Seçenek listesi doluysa `true` |

### CharacterSO

| Alan | Tip | Açıklama |
|---|---|---|
| `characterId` | `string` | Nodelex ile eşleşen benzersiz tanımlayıcı |
| `characterName` | `string` | Gösterim adı |
| `category` | `string` | ör. `"NPC"`, `"Player"` |
| `avatar` | `Sprite` | Opsiyonel — Unity'de elle atanır |

### DialogueOption

| Alan | Tip | Açıklama |
|---|---|---|
| `id` | `string` | Edge sistemi tarafından kullanılan dahili ID |
| `text` | `string` | Oyuncuya gösterilen metin |

---

## 11. Sorun Giderme

**İkinci diyalogda MissingReferenceException**  
UI paneliniz event'lerden unsubscribe olmadan destroy edildi. UI scriptinize `OnDestroy` ekleyin ve tüm handler'ları kaldırın. Bölüm 6.2'ye bakın.

**"Starter node bulunamadı" hatası**  
Import edilen ağaçta Starter node yok. Nodelex'te ağacınızda tam olarak bir Starter node olduğunu kontrol edin ve yeniden export edin.

**Import sonrası karakterler boş**  
Import sırasında `characters.json` bulunamadı. `.unity.json` dosyanızla aynı klasörde olduğundan emin olun.

**`base.Awake()` eksik — event'ler tetiklenmiyor**  
`DialogueEventListener`'ı subclass'layıp `Awake`'i override ediyorsanız `base.Awake()` **ilk satır** olmalı. Olmadan runner bağlantısı hiç kurulmaz ve tüm `Register` çağrılarının etkisi olmaz.

**Seçenek butonu index'i her zaman aynı**  
Loop değişkenini doğrudan capture ediyorsunuz. Loop içinde yerel bir değişken kullanın:
```csharp
int capturedIndex = i; // doğru
btn.onClick.AddListener(() => _runner.SelectOption(capturedIndex));
```

**Event handler çağrılmıyor**  
Event ID (UUID) ile register ediyorsunuz. Bunun yerine event adıyla register edin — Nodelex'te verdiğiniz isim:
```csharp
Register("evt_gold_acquired", OnGoldAcquired); // doğru
```

**Diyalog son node'u göstermeden bitiyor**  
End node `OnDialogueEnded`'ı anında tetikler. `HandleEnd`'de bir continue butonu gösterin, paneli ancak butona basılınca destroy edin. Bölüm 6.1'e bakın.

**`payload.Get<T>` KeyNotFoundException fırlatıyor**  
Parametre adı eşleşmiyor. Key'ler Nodelex'te tanımladığınız parametre adlarıdır (ör. `"amount"`), UUID veya sayısal ID değil.

**Diyalog başlıyor ama ekranda hiçbir şey görünmüyor**  
UI scriptinizin `StartDialogue()` çağrılmadan **önce** runner event'lerine subscribe olduğundan emin olun. `Initialize`, `StartDialogue`'dan sonra çağrılırsa ilk `OnNodeEntered` event'i zaten tetiklenmiş olur ve UI onu kaçırır.
