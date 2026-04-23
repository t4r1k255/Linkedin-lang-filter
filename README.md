# 🌐 LinkedIn — Otomatik Dil Filtresi

LinkedIn iş ilanlarını ve feed gönderilerini **dil tespitine göre** otomatik olarak filtreleyen bir kullanıcı scripti (userscript). İzin verdiğin dillerin dışındaki içerikler ya gizlenir ya da etiketle işaretlenir.

---

## ✨ Özellikler

- **İş İlanı Filtresi** — `/jobs` sayfasındaki ilanları dile göre gizler veya etiketler
- **Feed Filtresi** — Ana akıştaki gönderileri dile göre filtreler
- **14 Dil Desteği** — TR, EN, DE, FR, ES, PT, IT, NL, RU, PL, ZH, JA, KO, AR
- **İki Mod** — `Gizle` (içeriği sayfadan kaldırır) veya `Etiketle` (dil kodu etiketi yapıştırır)
- **Bilinmeyen Dil Kontrolü** — Tespit edilemeyen içeriklere ne yapılacağını sen seçersin
- **Kalıcı Ayarlar** — Tercihler `localStorage`'a kaydedilir, sayfa yenilemede kaybolmaz
- **SPA Uyumlu** — LinkedIn'in React tabanlı yönlendirmesini (`pushState` / `popstate`) yakalar
- **Hafif Motor** — Büyük model yok; n-gram tabanlı ~5 KB profil verisiyle çalışır

---

## 🖥️ Kurulum

### 1. Userscript Yöneticisi Kur

Tarayıcına uygun eklentiyi yükle:

| Tarayıcı | Önerilen Eklenti |
|---|---|
| Chrome / Edge / Brave | [Tampermonkey](https://www.tampermonkey.net/) |
| Firefox | [Tampermonkey](https://www.tampermonkey.net/) veya [Greasemonkey](https://www.greasespot.net/) |
| Safari | [Userscripts](https://apps.apple.com/app/userscripts/id1463298887) |

### 2. Scripti Ekle

1. Tarayıcının araç çubuğunda Tampermonkey simgesine tıkla
2. **"Yeni Script Oluştur"** (veya *Create a new script*) seçeneğini seç
3. Açılan editördeki tüm içeriği sil
4. `linkedin-lang-filter.user.js` dosyasının içeriğini yapıştır
5. `Ctrl + S` ile kaydet

### 3. LinkedIn'i Aç

`https://www.linkedin.com/jobs` veya `https://www.linkedin.com/feed` adresine git. Sağ üst köşede **"Dil Filtresi"** rozeti belirirse kurulum tamamdır.

---

## 🎛️ Kullanım

### Rozet (Badge)

Sayfa açıkken sağ üst köşede küçük bir rozet görünür:

```
[ 12 filtrelendi ]  [ Dil Filtresi ]  [ ⚙ Ayarlar ]
```

- **Sayı** — Şu an gizlenen veya etiketlenen içerik adedi
- **"Dil Filtresi" butonu** — Tek tıkla filtreyi aç/kapat
- **"⚙ Ayarlar" butonu** — Yönetim panelini açar

### Ayarlar Paneli

| Ayar | Açıklama |
|---|---|
| **Filtre Modu** | `Gizle` → içeriği sayfadan kaldırır · `Etiketle` → dil kodu etiketi yapıştırır |
| **İzin Verilen Diller** | Görmek istediğin dilleri seç (çoklu seçim desteklenir) |
| **Bilinmeyen Dil** | Dili tespit edilemeyen içerikler gizlensin mi? |

Değişiklikleri uygulamak için **"Uygula"** butonuna tıkla. **"Sıfırla"** butonu tüm ayarları varsayılana döndürür.

---

## ⚙️ Varsayılan Ayarlar

```js
{
  enabled:       true,
  mode:          'hide',        // 'hide' | 'label'
  allowedLangs:  ['tr', 'en'],
  minWords:      6,             // dil tespiti için gereken minimum kelime
  unknownAction: 'show',        // 'show' | 'hide'
}
```

---

## 🔍 Dil Tespit Motoru

Script, harici API veya ağ isteği kullanmaz. Tüm tespit **tarayıcı içinde, çevrimdışı** çalışır:

- Her dil için en sık geçen **trigram (3-karakter n-gram) profilleri** tanımlıdır
- Metin normalize edilip n-gramlara bölünür; profildeki eşleşmeler puanlanır
- En yüksek puan belirlenen dili kazanır; iki dil arasındaki fark çok küçükse sonuç `unknown` döner
- **Güven eşiği:** ham skor `0.04` altındaysa veya birinci-ikinci arasındaki fark `0.02` altındaysa dil belirsiz sayılır
- Kısa metinlerde (< `minWords` kelime) tespit yapılmaz

> Tahmini doğruluk: uzun metinlerde **~%85+**, kısa başlıklarda daha düşük.

---

## 🌍 Desteklenen Diller

| Kod | Dil | Kod | Dil |
|---|---|---|---|
| `tr` | Türkçe | `nl` | Hollandaca |
| `en` | İngilizce | `ru` | Rusça |
| `de` | Almanca | `pl` | Lehçe |
| `fr` | Fransızca | `zh` | Çince |
| `es` | İspanyolca | `ja` | Japonca |
| `pt` | Portekizce | `ko` | Korece |
| `it` | İtalyanca | `ar` | Arapça |

---

## 📄 Lisans

MIT — Dilediğin gibi kullanabilir, değiştirebilir ve dağıtabilirsin.

---

## ⚠️ Yasal Uyarı

Bu script LinkedIn ile herhangi bir bağlantısı olmayan bağımsız bir topluluk aracıdır. LinkedIn'in [Kullanım Koşulları](https://www.linkedin.com/legal/user-agreement)'na uygun şekilde kullan.
