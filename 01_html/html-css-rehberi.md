# HTML & CSS — Backend Developer'ın Rehberi

> **Bu dosya ne için?**  
> Pixel-perfect tasarımcı yetiştirmek için değil. .NET Backend Developer olarak
> "okuyan, anlayan, yönlendiren" biri olmak için gereken minimum ama sağlam bilgi.
> Her bölümde: Ne işe yarar → Nerede kullanılır → Olmasa ne olur → Örnek

---

## İÇİNDEKİLER

1. [HTML Semantiği](#1-html-semantiği)
2. [Box Model](#2-box-model)
3. [Flexbox](#3-flexbox)
4. [Formlar](#4-formlar)
5. [CSS Specificity](#5-css-specificity)
6. [Hızlı Başvuru Kartı](#6-hızlı-başvuru-kartı)

---

## 1. HTML Semantiği

### Ne işe yarar?

HTML'de her şeyi `<div>` ile yapabilirsin. Ama `<div>` anlamsız bir kutudur.
"Semantic" taglar ise içeriğin ne olduğunu hem tarayıcıya hem insana söyler.

### Nerede kullanılır?

Her sayfanın iskelet yapısında. Bir web sayfası açtığında tarayıcı önce HTML'i okur,
kafasında bir ağaç (DOM) oluşturur. Semantic taglar bu ağacın anlamlı olmasını sağlar.

### Olmasa ne olur?

- Arama motorları sayfanı anlayamaz → SEO sıfır
- Ekran okuyucular (görme engelli kullanıcılar) sayfada kaybolur
- Başka bir developer koda baktığında "bu ne?" diye sorar
- Blazor / Razor Pages yazarken template'lerinde anlamsız div soup oluşur

### Örnek — Kötü vs İyi

```html
<!-- KÖTÜ: Anlamsız div soup -->
<div class="header">
  <div class="nav">
    <div class="link">Ana Sayfa</div>
    <div class="link">Hakkımızda</div>
  </div>
</div>
<div class="content">
  <div class="title"> Hoş Geldin</div>
  <div class="text">yemek aboneliği...</div>
</div>
<div class="footer">© 2025</div>


<!-- İYİ: Semantic HTML -->
<header>
  <nav>
    <a href="/">Ana Sayfa</a>
    <a href="/hakkimizda">Hakkımızda</a>
  </nav>
</header>

<main>
  <section>
    <h1> Hoş Geldin</h1>
    <p> yemek aboneliği...</p>
  </section>
</main>

<footer>© 2025</footer>
```

### Bilmen gereken taglar (hepsi bu)

| Tag | Ne demek | Ne zaman kullanılır |
|-----|----------|-------------------|
| `<header>` | Sayfa/bölüm başlığı | Logo, navbar, üst alan |
| `<nav>` | Navigasyon | Menü linkleri |
| `<main>` | Ana içerik | Sayfanın asıl konusu |
| `<section>` | Tematik bölüm | Birbirine benzer içerik grubu |
| `<article>` | Bağımsız içerik | Blog yazısı, ürün kartı |
| `<aside>` | Yan içerik | Sidebar, reklam, not |
| `<footer>` | Alt bilgi | Copyright, iletişim |
| `<h1>–<h6>` | Başlıklar | Hiyerarşik başlık yapısı |
| `<p>` | Paragraf | Normal metin |
| `<ul>` / `<ol>` | Liste | Sırasız / sıralı liste |
| `<li>` | Liste elemanı | `ul` veya `ol` içinde |
| `<a>` | Link | Sayfa geçişleri |
| `<img>` | Görsel | Resim gösterimi |
| `<div>` | Anlamsız kutu | Sadece layout için, başka çare yoksa |
| `<span>` | Anlamsız satır içi | Sadece stil vermek için |

---

## 2. Box Model

### Ne işe yarar?

CSS'teki HER element aslında bir kutudur. Bu kutunun 4 katmanı vardır.
Box model bu katmanları tanımlar. Bunu anlamadan hiçbir layout mantıklı görünmez.

```
┌─────────────────────────────────┐
│           MARGIN                │  ← Dışarıdaki boşluk (komşulardan uzaklık)
│   ┌─────────────────────────┐   │
│   │         BORDER          │   │  ← Kenarlık çizgisi
│   │   ┌─────────────────┐   │   │
│   │   │     PADDING     │   │   │  ← İçerideki boşluk (içerik ile border arası)
│   │   │  ┌───────────┐  │   │   │
│   │   │  │  CONTENT  │  │   │   │  ← Asıl içerik (text, görsel vs.)
│   │   │  └───────────┘  │   │   │
│   │   └─────────────────┘   │   │
│   └─────────────────────────┘   │
└─────────────────────────────────┘
```

### Nerede kullanılır?

Her yerde. Buton yaparken, kart yaparken, form alanı yaparken.
"Neden bu element diğerine yapışık?" veya "neden çok boşluk var?" sorularının cevabı
hep box model'de gizlidir.

### Olmasa ne olur?

Ekrandaki elemanlar neden o kadar yakın ya da uzak duruyor, anlayamazsın.
"Margin mi padding mi?" sorusunun farkı görünmez, layout kayar.

### Fark Ne? Margin vs Padding

```html
<!-- Örnek: İki buton yan yana -->

<!-- PADDING: Butonun kendi içindeki nefes -->
<button style="padding: 12px 24px;">
  Satın Al
</button>
<!-- Padding az olunca metin butona yapışır, çok olunca buton şişer -->


<!-- MARGIN: Butonlar arasındaki mesafe -->
<button style="margin-right: 16px;">İptal</button>
<button>Kaydet</button>
<!-- Margin olmazsa iki buton birbirine yapışır -->
```

### Gerçek Hayat Örneği

```html
<style>
  .kart {
    width: 300px;           /* Kartın genişliği */
    padding: 20px;          /* İçerik kenardan 20px uzakta */
    margin: 16px;           /* Kart, komşularından 16px uzakta */
    border: 1px solid #ccc; /* İnce gri kenarlık */
  }
</style>

<div class="kart">
  <h2>GymFuel - Haftalık Paket</h2>
  <p>5 günlük sağlıklı öğün</p>
  <button>Abone Ol</button>
</div>
```

### Kritik Tuzak: `box-sizing`

```css
/* Problem: Varsayılan davranış */
.kutu {
  width: 300px;
  padding: 20px;
  /* Gerçek genişlik: 300 + 20 + 20 = 340px */
  /* Element beklenenden büyük çıkar, layout bozulur */
}

/* Çözüm: Her projede bunu kullan */
* {
  box-sizing: border-box;
  /* Artık padding, width'in içine dahil */
  /* width: 300px dersen, gerçekten 300px olur */
}
```

> **Kural:** Her projeye `* { box-sizing: border-box; }` ile başla. Neden?
> Çünkü aksi halde elementlerin gerçek boyutunu hesaplamak zorunda kalırsın.
> Bütün modern projeler bu ayarla çalışır.

---

## 3. Flexbox

### Ne işe yarar?

Elementleri yatayda veya dikeyde hizalamak için. Navbar, kart dizisi,
buton grubu — bunların hepsi Flexbox ile yapılır.

### Nerede kullanılır?

"Şu iki şeyi yan yana koymak istiyorum" veya "bunu ortaya almak istiyorum"
dediğin her yerde. Next.js ile GymFuel frontend'ini yazarken her sayfada kullanacaksın.

### Olmasa ne olur?

Elementler doğal olarak alt alta dizilir. Yan yana koymak için `float` veya `position`
kullanmak zorunda kalırsın — ikisi de çok daha karmaşık ve hatalı.

### Temel Mantık

```css
/* Flex'i aktif etmek için ebeveyne (parent) verirsin */
.container {
  display: flex;
}

/* Çocuklar (children) artık flex item olur */
```

### Tek Bilmen Gereken 3 Property

```css
.container {
  display: flex;

  /* 1. Yatayda hizalama */
  justify-content: flex-start;   /* Sola yasla (varsayılan) */
  justify-content: center;       /* Ortala */
  justify-content: flex-end;     /* Sağa yasla */
  justify-content: space-between; /* Aralarına eşit boşluk */

  /* 2. Dikeyde hizalama */
  align-items: flex-start;  /* Üste yasla */
  align-items: center;      /* Ortala */
  align-items: flex-end;    /* Alta yasla */
  align-items: stretch;     /* Uzat (varsayılan) */

  /* 3. Yön */
  flex-direction: row;     /* Yatay (varsayılan) */
  flex-direction: column;  /* Dikey */
}
```

### Gerçek Hayat Örnekleri

```html
<style>

  /* Örnek 1: Navbar */
  .navbar {
    display: flex;
    justify-content: space-between; /* Logo solda, linkler sağda */
    align-items: center;            /* İkisi de dikey ortalı */
    padding: 0 24px;
    height: 60px;
    background: #1a1a1a;
  }

  /* Örnek 2: Bir şeyi tam ortaya alma */
  .sayfa {
    display: flex;
    justify-content: center;  /* Yatay merkez */
    align-items: center;      /* Dikey merkez */
    height: 100vh;            /* Tam ekran yüksekliği */
  }

  /* Örnek 3: Kart dizisi */
  .kart-listesi {
    display: flex;
    gap: 16px;         /* Kartlar arası boşluk */
    flex-wrap: wrap;   /* Sığmayanlar alt satıra geçsin */
  }

</style>

<!-- Navbar HTML -->
<nav class="navbar">
  <span>GymFuel 💪</span>
  <div>
    <a href="/">Ana Sayfa</a>
    <a href="/menu">Menü</a>
    <a href="/giris">Giriş Yap</a>
  </div>
</nav>

<!-- Ortaya alma -->
<div class="sayfa">
  <div class="login-kutusu">
    <h2>Giriş Yap</h2>
  </div>
</div>

<!-- Kart dizisi -->
<div class="kart-listesi">
  <div class="kart">Pazartesi Paketi</div>
  <div class="kart">Haftalık Paket</div>
  <div class="kart">Aylık Paket</div>
</div>
```

> **Pratik:** [flexboxfroggy.com](https://flexboxfroggy.com) — 30 dakika, 24 seviye.
> Bunu bir kez bitirirsen Flexbox sana yabancı gelmez.

---

## 4. Formlar

### Ne işe yarar?

Kullanıcıdan veri toplar. Login, kayıt, arama, sipariş — hepsi form.
Backend developer olarak formların nasıl çalıştığını bilmek zorundasın
çünkü bu veriler senin endpoint'ine geliyor.

### Nerede kullanılır?

Herhangi bir kullanıcı girişi alan sayfada. Login sayfası, kayıt formu,
ödeme sayfası, sipariş formu.

### Olmasa ne olur?

Kullanıcıdan veri alamazsın. Web uygulaması tek yönlü olur (sadece görüntüler, etkileşmez).

### Temel Form Anatomisi

```html
<form action="/api/login" method="POST">

  <!-- Her input bir label ile eşlenmeli -->
  <label for="email">E-posta</label>
  <input type="email" id="email" name="email" placeholder="ornek@mail.com" required>

  <label for="sifre">Şifre</label>
  <input type="password" id="sifre" name="sifre" required>

  <button type="submit">Giriş Yap</button>

</form>
```

**`action`** → Formun hangi URL'e gönderileceği (senin API endpoint'in)  
**`method`** → GET (URL'de görünür) veya POST (body'de gizli)  
**`name`** → Backend'in göreceği key ismi (`request.Form["email"]`)  
**`for` / `id`** eşleşmesi → Label'a tıklayınca input odaklanır

### Input Türleri — Bilmen Gerekenler

```html
<input type="text">      <!-- Düz metin -->
<input type="email">     <!-- Email formatı doğrular -->
<input type="password">  <!-- Metni gizler -->
<input type="number">    <!-- Sadece sayı -->
<input type="date">      <!-- Tarih seçici -->
<input type="checkbox">  <!-- Çoklu seçim -->
<input type="radio">     <!-- Tekli seçim -->
<input type="hidden">    <!-- Kullanıcı görmez, veri gönderir -->

<textarea rows="4">Büyük metin alanı</textarea>

<select>
  <option value="bronze">Bronze Paket</option>
  <option value="silver">Silver Paket</option>
  <option value="gold">Gold Paket</option>
</select>

<button type="submit">Gönder</button>
<button type="reset">Temizle</button>
<button type="button">Sadece tıklanır, form göndermez</button>
```

### Gerçek Hayat: GymFuel Login Formu

```html
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>GymFuel - Giriş</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: #f4f4f4;
    }

    .form-kutusu {
      background: white;
      padding: 40px;
      border-radius: 8px;
      width: 360px;
      box-shadow: 0 2px 12px rgba(0,0,0,0.1);
    }

    h2 {
      margin-bottom: 24px;
      text-align: center;
    }

    label {
      display: block;
      margin-bottom: 6px;
      font-size: 14px;
      font-weight: 600;
    }

    input {
      width: 100%;
      padding: 10px 14px;
      margin-bottom: 16px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 15px;
    }

    input:focus {
      outline: none;
      border-color: #2563eb;
    }

    button {
      width: 100%;
      padding: 12px;
      background: #2563eb;
      color: white;
      border: none;
      border-radius: 4px;
      font-size: 16px;
      cursor: pointer;
    }

    button:hover {
      background: #1d4ed8;
    }
  </style>
</head>
<body>

  <div class="form-kutusu">
    <h2>GymFuel 💪</h2>
    <form action="/api/auth/login" method="POST">

      <label for="email">E-posta</label>
      <input type="email" id="email" name="email"
             placeholder="sen@gymfuel.de" required>

      <label for="sifre">Şifre</label>
      <input type="password" id="sifre" name="password" required>

      <button type="submit">Giriş Yap</button>

    </form>
  </div>

</body>
</html>
```

> **Alıştırma:** Bu kodu bir `.html` dosyasına kopyala, tarayıcıda aç.
> DevTools'u aç (F12), Network sekmesine bak, formu gönder, isteği incele.
> Backend'de bu verinin nasıl göründüğünü anlamak için kritik.

---

## 5. CSS Specificity

### Ne işe yarar?

Bir elemente birden fazla CSS kuralı uygulandığında hangisinin kazanacağını belirler.
"Ben bu stili verdim ama neden uygulanmıyor?" sorusunun cevabı burada.

### Nerede kullanılır?

Hata ayıklarken. Bir stil neden çalışmıyor sorusunu çözerken.

### Olmasa ne olur?

Saatlerce nedenini anlamadan CSS ile boğuşursun. "!important" bayrağını
her yere yapıştırmaya başlarsın — ki bu kötü bir pratiktir.

### Güç Sırası (Düşükten Yükseğe)

```
Eleman (p, div, h1)     →  En zayıf
Sınıf (.kart, .buton)   →  Orta
ID (#login, #header)    →  Güçlü
Inline style=""         →  Çok güçlü
!important              →  Nükleer seçenek (kaçın)
```

### Somut Örnek

```html
<style>
  p {
    color: black;       /* Eleman seçici: zayıf */
  }

  .aciklama {
    color: blue;        /* Class seçici: orta — KAZANIR */
  }

  #ozet {
    color: red;         /* ID seçici: güçlü — KAZANIR */
  }
</style>

<!-- Bu p hangi renk olur? -->
<p class="aciklama">Bu mavi (class, elemandan güçlü)</p>

<!-- Bu p hangi renk olur? -->
<p id="ozet" class="aciklama">Bu kırmızı (ID, class'tan güçlü)</p>
```

### Gerçek Hayat Senaryosu

```css
/* Kütüphane/framework'ün yazdığı stil */
button {
  background: gray;
}

/* Senin yazdığın stil — aynı güçte, sonraki kazanır */
button {
  background: blue;   /* Kazanır çünkü sonra yazıldı */
}

/* Daha spesifik seçici yazarsan her zaman kazanırsın */
.form-kutusu button {
  background: green;  /* Kazanır çünkü daha spesifik */
}
```

### Kural: Hata Ayıklama Adımları

1. Chrome DevTools → Elements sekmesi → Elementi seç
2. Sağda "Styles" panelini gör
3. Üstü çizili stiller = override edilmiş, kazanmamış
4. Hangi seçicinin kazandığını gör
5. Kendi seçicini daha spesifik yap, `!important` kullanma

---

## 6. Hızlı Başvuru Kartı

### Sıfır'dan Sayfa Şablonu

```html
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sayfa Başlığı</title>
  <style>
    /* Her projeye bu ikisiyle başla */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: sans-serif; }
  </style>
</head>
<body>

  <header>
    <nav><!-- Navigasyon --></nav>
  </header>

  <main>
    <section>
      <h1>Ana Başlık</h1>
      <p>İçerik</p>
    </section>
  </main>

  <footer>
    <p>© 2025</p>
  </footer>

</body>
</html>
```

### En Çok Kullanılan CSS

```css
/* Layout */
display: flex;
justify-content: center;
align-items: center;
gap: 16px;
flex-wrap: wrap;

/* Boyut */
width: 100%;
max-width: 1200px;
height: 100vh;
padding: 16px 24px;   /* üst-alt  sağ-sol */
margin: 0 auto;        /* Blok elementi ortalar */

/* Görünüm */
background: #ffffff;
color: #1a1a1a;
border: 1px solid #ccc;
border-radius: 8px;
box-shadow: 0 2px 8px rgba(0,0,0,0.1);

/* Metin */
font-size: 16px;
font-weight: 600;
text-align: center;
line-height: 1.5;

/* Cursor */
cursor: pointer;    /* El işareti (butonlar için) */

/* Hover */
button:hover {
  opacity: 0.9;
}
```

### AI ile Çalışırken Ne Sorarsın

```
"Bir login formu yaz, flexbox ile ortaya al, responsive olsun"
"Bu navbar'da logo sola, linkler sağa gitsin, arası space-between olsun"
"Bu kartlar 3'lü grid şeklinde dizilsin, mobilde 1'li olsun"
```

AI kodu yazar, sen okuyup anladığını kontrol edersin.

---

## Önerilen Kaynaklar

| Kaynak | Kullanım |
|--------|---------|
| [MDN Web Docs](https://developer.mozilla.org) | Bir property'nin ne işe yaradığını araştır |
| [Flexbox Froggy](https://flexboxfroggy.com) | Flexbox'ı oynayarak öğren |
| [CSS Tricks - Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) | Flexbox görsel rehber |
| Chrome DevTools (F12) | Her şeyi canlı gör, dene, hata bul |

---

> **Son Not:** Bu dosyayı ezberlemek zorunda değilsin.  
> Bir şeye ihtiyaç duyduğunda aç, bak, kullan.  
> Kalanını AI yazar, sen anlarsın. Bu yeter.
