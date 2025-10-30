# Razor Syntax & ASP.NET Core Cheatsheet 🚀

Bu rehber, ASP.NET Core'da Razor syntax'ı için kapsamlı bir referans sağlar. Güncel ASP.NET Core versiyonları (6.0+) ile uyumludur.

## 📋 İçindekiler

- [Temel Razor Kuralları](#-temel-razor-kuralları)
- [C# Yapıları Razor İçinde](#-c-yapıları-razor-i̇çinde)
- [HTML + Razor Etkileşimi](#-html--razor-etkileşimi)
- [Razor Direktifleri](#-razor-direktifleri)
- [Layout ve Partial Kullanımı](#-layout-ve-partial-kullanımı)
- [Tag Helpers](#-tag-helpers)
- [ViewComponent Kullanımı](#-viewcomponent-kullanımı)
- [Razor Pages Özellikleri](#-razor-pages-özellikleri)
- [Veri Formatlama](#-veri-formatlama)
- [Form ve Model Binding](#-form-ve-model-binding)
- [Html Helper Metotları](#-html-helper-metotları)
- [Kullanıcı ve Yetkilendirme](#-kullanıcı-ve-yetkilendirme)
- [ViewImports ve ViewStart](#-viewimports-ve-viewstart)
- [Editor/Display Template](#-editordisplay-template)
- [Async ve Performans](#-async-ve-performans)
- [Razor Special Syntax](#-razor-special-syntax)
- [Pratik Örnekler](#-pratik-örnekler)
- [Best Practices](#-best-practices)
- [Sık Kullanılan Snippet'lar](#-sık-kullanılan-snippetlar)

## 🧩 Temel Razor Kuralları

| Tanım | Açıklama | Örnek |
|-------|----------|--------|
| `@` | C# kodu başlatır | `@DateTime.Now` |
| `@{ ... }` | Kod bloğu | `@{ var mesaj = "Merhaba"; }` |
| `@* ... *@` | Razor yorumu | `@* Görünmez *@` |
| `<!-- ... -->` | HTML yorumu | `<!-- Görünür -->` |
| `@( ... )` | Karmaşık ifade | `<p>@(Model.Fiyat + 5)</p>` |
| `@@` | @ karakterini literal basar | `@@username → @username` |
| `@:` | Satır içi HTML çıktısı | `@:Bu bir <strong>HTML</strong> çıktısı` |

**Örnek Kullanım:**
```csharp
@{
    // C# kodu
    var kullaniciAdi = "Ahmet";
    var bugun = DateTime.Today;
}
<p>Merhaba @kullaniciAdi, bugün @bugun.ToString("dd.MM.yyyy")</p>
```

## ⚙️ C# Yapıları Razor İçinde

| Yapı | Örnek |
|------|--------|
| Değişken tanımı | `@{ var isim = "Eda"; }` |
| if / else | `@if(Model.Aktif){ <span>Aktif</span> } else { <span>Pasif</span> }` |
| for | `@for(int i=0;i<5;i++){ <li>@i</li> }` |
| foreach | `@foreach(var item in Model){ <p>@item.Ad</p> }` |
| switch | `@switch(Model.Tip){ case 1: <b>A</b>; break; }` |
| try/catch | `@try{ ... } @catch(Exception ex){ ... }` |
| while | `@{ int i=0; while(i<3){ <span>@i</span> i++; } }` |
| using | `@using (var stream = new MemoryStream()) { ... }` |

**Detaylı Örnekler:**
```csharp
@* if/else *@
@if (User.Identity.IsAuthenticated)
{
    <p>Hoş geldiniz, @User.Identity.Name!</p>
}
else
{
    <a href="/login">Giriş Yap</a>
}

@* foreach *@
<ul>
@foreach (var urun in Model.Urunler)
{
    <li>@urun.Ad - @urun.Fiyat.ToString("C")</li>
}
</ul>

@* switch *@
@switch (Model.Durum)
{
    case Durum.Aktif:
        <span class="badge bg-success">Aktif</span>
        break;
    case Durum.Pasif:
        <span class="badge bg-danger">Pasif</span>
        break;
    default:
        <span class="badge bg-secondary">Bilinmiyor</span>
        break;
}
```

## 🧱 HTML + Razor Etkileşimi

| Amaç | Örnek |
|------|--------|
| Model değerini yazdırma | `<h2>@Model.Baslik</h2>` |
| Attribute içinde kullanma | `<input value="@Model.Ad" class="@(Model.Hata ? "error" : "")" />` |
| Inline koşul | `<span>@(Model.Aktif ? "Açık" : "Kapalı")</span>` |
| Null kontrolü | `@(Model?.Ad ?? "Bilinmiyor")` |
| JavaScript içinde kullanım | `<script>var userId = '@Model.UserId';</script>` |
| CSS class dinamik | `<div class="card @(Model.Onemli ? "border-warning" : "")">` |

**Örnekler:**
```csharp
@* Attribute içinde Razor *@
<div class="alert @(Model.Basarili ? "alert-success" : "alert-danger")">
    @Model.Mesaj
</div>

@* Data attribute'ları *@
<button data-id="@Model.Id" data-action="sil" class="btn btn-danger">
    Sil
</button>

@* Style dinamik *@
<div style="color: @(Model.Acil ? "red" : "black"); font-weight: @(Model.Kalın ? "bold" : "normal")">
    @Model.Icerik
</div>
```

## 🧭 Razor Direktifleri (Directives)

| Direktif | Açıklama | Örnek |
|----------|----------|--------|
| `@page` | Sayfayı bir Razor Page olarak işaretler | `@page "/urunler"` |
| `@model` | Model tipini tanımlar | `@model MyApp.Models.Urun` |
| `@using` | Namespace ekler | `@using MyApp.Helpers` |
| `@inject` | Servis ekler | `@inject ILogger<Home> Logger` |
| `@inherits` | Base class belirler | `@inherits BaseViewPage` |
| `@functions` | C# metotlarını tanımlar | `@functions { string Selam() => "Hi!"; }` |
| `@layout` | Layout sayfası belirler | `@layout "_Layout"` |
| `@section` | Layout'a özel içerik tanımlar | `@section Scripts { <script>...</script> }` |
| `@RenderBody()` | Layout içeriğini çağırır | _Layout.cshtml içinde |
| `@RenderSection("Scripts")` | Bölüm çağırma | _Layout.cshtml içinde |
| `@addTagHelper` | TagHelper'ı aktif eder | `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` |
| `@removeTagHelper` | Devre dışı bırakır | `@removeTagHelper *, MyApp.TagHelpers` |
| `@namespace` | Varsayılan namespace tanımlar | `@namespace MyApp.Pages` |
| `@attribute` | Derleme zamanında attribute ekler | `@attribute [Authorize]` |

**Kapsamlı Örnek:**
```csharp
@page "/urun/detay/{id:int}"
@model UrunDetayModel
@using MyApp.Data
@inject IUrunService UrunService
@{
    ViewData["Title"] = Model.Urun.Ad;
    Layout = "_Layout";
}

@section Styles {
    <style>
        .urun-detay { max-width: 600px; }
    </style>
}

<h1>@Model.Urun.Ad</h1>
```

## 🧩 Layout ve Partial Kullanımı

| Kullanım | Açıklama | Örnek |
|----------|----------|--------|
| Layout belirtme | `@layout "_Layout"` | |
| İçerik gövdesi | `@RenderBody()` | |
| Bölüm render | `@RenderSection("Scripts", required:false)` | |
| Partial view (senkron) | `@Html.Partial("_Header")` | |
| Partial view (asenkron) | `@await Html.PartialAsync("_Header")` | |
| RenderPartial | `@{ Html.RenderPartial("_Footer"); }` | |
| RenderAction (eski MVC) | `@{ Html.RenderAction("Menu", "Home"); }` | |
| Editor Template | `@Html.EditorFor(m => m.Ad)` | |
| Display Template | `@Html.DisplayFor(m => m.Tarih)` | |
| Component çağırma | `@await Component.InvokeAsync("Menu")` | |
| Component parametreli | `@await Component.InvokeAsync("Banner", new { aktif = true })` | |

**Layout Örneği (_Layout.cshtml):**
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - MyApp</title>
    <link rel="stylesheet" href="~/css/site.css" />
    @await RenderSectionAsync("Styles", required: false)
</head>
<body>
    <header>
        @await Html.PartialAsync("_Navbar")
    </header>

    <div class="container">
        <main role="main">
            @RenderBody()
        </main>
    </div>

    <footer class="border-top footer text-muted">
        @{ Html.RenderPartial("_Footer"); }
    </footer>

    <script src="~/lib/jquery/dist/jquery.min.js"></script>
    <script src="~/js/site.js"></script>
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

**View Kullanımı:**
```csharp
@{
    Layout = "_Layout";
    ViewData["Title"] = "Ana Sayfa";
}

@section Styles {
    <link rel="stylesheet" href="~/css/home.css" />
}

<div class="text-center">
    <h1>Hoş Geldiniz</h1>
    <partial name="_WelcomeMessage" model="Model" />
</div>

@section Scripts {
    <script>
        $(document).ready(function() {
            console.log("Sayfa yüklendi");
        });
    </script>
}
```

## 🧰 Tag Helpers

| Tag Helper | Açıklama | Örnek |
|------------|----------|--------|
| `asp-for` | Model özelliğine bağlar | `<input asp-for="Ad" />` |
| `asp-validation-for` | Validation mesajı | `<span asp-validation-for="Ad"></span>` |
| `asp-validation-summary` | Hata özetini gösterir | `<div asp-validation-summary="All"></div>` |
| `asp-action` | Controller Action yönlendirmesi | `<a asp-action="Detay">Detay</a>` |
| `asp-controller` | Controller belirtir | `<a asp-controller="Urun">Git</a>` |
| `asp-route-*` | Route parametresi ekler | `<a asp-route-id="@urun.Id">Detay</a>` |
| `asp-area` | Area belirtir | `<a asp-area="Admin" asp-controller="Panel">Panel</a>` |
| `partial` | Parça render eder | `<partial name="_Header" />` |
| `form` | `<form asp-action="Kaydet" method="post">` | |
| `select` | `<select asp-for="KategoriId" asp-items="Model.Kategoriler"></select>` | |
| `asp-route` | Route adı belirtir | `<a asp-route="urun_detay">Detay</a>` |
| `asp-fragment` | Sayfa içi bölüm | `<a asp-fragment="yorumlar">Yorumlar</a>` |
| `asp-append-version` | Cache busting | `<script asp-append-version src="~/js/site.js"></script>` |

**Kapsamlı Form Örneği:**
```html
<form asp-action="Kaydet" asp-controller="Urun" method="post" class="needs-validation" novalidate>
    <div asp-validation-summary="ModelOnly" class="text-danger"></div>
    
    <input type="hidden" asp-for="Id" />
    
    <div class="form-group">
        <label asp-for="Ad" class="form-label"></label>
        <input asp-for="Ad" class="form-control" />
        <span asp-validation-for="Ad" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label asp-for="Aciklama" class="form-label"></label>
        <textarea asp-for="Aciklama" class="form-control" rows="3"></textarea>
        <span asp-validation-for="Aciklama" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label asp-for="KategoriId" class="form-label"></label>
        <select asp-for="KategoriId" asp-items="Model.KategoriListesi" class="form-select">
            <option value="">-- Kategori Seçin --</option>
        </select>
        <span asp-validation-for="KategoriId" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label asp-for="Fiyat" class="form-label"></label>
        <input asp-for="Fiyat" class="form-control" type="number" step="0.01" />
        <span asp-validation-for="Fiyat" class="text-danger"></span>
    </div>
    
    <div class="form-group form-check">
        <input asp-for="Aktif" class="form-check-input" />
        <label asp-for="Aktif" class="form-check-label"></label>
    </div>
    
    <button type="submit" class="btn btn-primary">Kaydet</button>
    <a asp-action="Index" class="btn btn-secondary">İptal</a>
</form>
```

## 💬 ViewComponent Kullanımı

| Kullanım | Açıklama | Örnek |
|----------|----------|--------|
| Component çağırma | `@await Component.InvokeAsync("KategoriListesi")` | |
| Parametreli | `@await Component.InvokeAsync("Banner", new { aktif = true })` | |
| Tag Helper | `<vc:kategori-listesi></vc:kategori-listesi>` | |
| Component sınıfı | `public class KategoriListesiViewComponent : ViewComponent { public IViewComponentResult Invoke() => View(); }` | |
| View konumu | `/Views/Shared/Components/KategoriListesi/Default.cshtml` | |

**ViewComponent Örneği:**
```csharp
// Components/SonUrunlerViewComponent.cs
public class SonUrunlerViewComponent : ViewComponent
{
    private readonly IUrunService _urunService;
    
    public SonUrunlerViewComponent(IUrunService urunService)
    {
        _urunService = urunService;
    }
    
    public async Task<IViewComponentResult> InvokeAsync(int adet = 5)
    {
        var urunler = await _urunService.GetSonUrunlerAsync(adet);
        return View(urunler);
    }
}
```

**View Kullanımı:**
```html
@* Yöntem 1: InvokeAsync *@
<div class="sidebar">
    @await Component.InvokeAsync("SonUrunler", new { adet = 3 })
</div>

@* Yöntem 2: Tag Helper *@
<div class="sidebar">
    <vc:son-urunler adet="3"></vc:son-urunler>
</div>
```

**Component View (Default.cshtml):**
```html
@model IEnumerable<Urun>

<h4>Son Ürünler</h4>
<div class="list-group">
    @foreach (var urun in Model)
    {
        <a href="#" class="list-group-item list-group-item-action">
            <div class="d-flex w-100 justify-content-between">
                <h6 class="mb-1">@urun.Ad</h6>
                <small>@urun.Fiyat.ToString("C")</small>
            </div>
            <small class="text-muted">@urun.Kategori.Ad</small>
        </a>
    }
</div>
```

## 🧱 Razor Pages Özellikleri

| Özellik | Örnek |
|---------|--------|
| `@page` | `@page "/kayit"` |
| `asp-page` | `<a asp-page="/Index">Ana Sayfa</a>` |
| `asp-page-handler` | `<form asp-page-handler="Kaydet">` |
| `asp-route-*` | `<a asp-page="/Detay" asp-route-id="3">Detay</a>` |
| `@bind` | Two-way binding: `<input @bind="Ad" />` |
| `@bind:event` | Binding event: `<input @bind="Ara" @bind:event="oninput" />` |
| OnGet, OnPost | `public void OnPostSave(){}` (PageModel) |
| Handler metotları | `public IActionResult OnPostDelete(int id)` |

**Razor Page Örneği:**
```csharp
@page "/urun/ekle"
@model UrunEkleModel

<h2>Ürün Ekle</h2>

<form method="post">
    <div class="form-group">
        <label asp-for="Urun.Ad"></label>
        <input asp-for="Urun.Ad" class="form-control" />
        <span asp-validation-for="Urun.Ad" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label asp-for="Urun.Fiyat"></label>
        <input asp-for="Urun.Fiyat" class="form-control" />
        <span asp-validation-for="Urun.Fiyat" class="text-danger"></span>
    </div>
    
    <button type="submit" class="btn btn-primary">Kaydet</button>
    <a asp-page="./Index" class="btn btn-secondary">İptal</a>
</form>

@functions {
    public class UrunEkleModel : PageModel
    {
        private readonly IUrunService _urunService;
        
        [BindProperty]
        public Urun Urun { get; set; }
        
        public UrunEkleModel(IUrunService urunService)
        {
            _urunService = urunService;
        }
        
        public void OnGet()
        {
            Urun = new Urun();
        }
        
        public async Task<IActionResult> OnPostAsync()
        {
            if (!ModelState.IsValid)
            {
                return Page();
            }
            
            await _urunService.EkleAsync(Urun);
            return RedirectToPage("./Index");
        }
    }
}
```

## 🔠 Veri Formatlama

| İşlem | Örnek |
|-------|--------|
| String format | `@String.Format("{0:C}", Model.Fiyat)` |
| Tarih format | `@Model.Tarih.ToString("dd.MM.yyyy")` |
| HTML encode kapatma | `@Html.Raw("<b>Kalın</b>")` |
| Null kontrol | `@(Model?.Ad ?? "Yok")` |
| Currency format | `@Model.Fiyat.ToString("C2")` |
| Percentage format | `@Model.Indirim.ToString("P1")` |
| Custom format | `@Model.Telefon.ToString("(###) ### ## ##")` |

**Formatlama Örnekleri:**
```csharp
@* Para birimi *@
<p>Fiyat: @Model.Fiyat.ToString("C")</p> @* ₺123,45 *@
<p>Fiyat: @Model.Fiyat.ToString("C0")</p> @* ₺123 *@
<p>Fiyat: @Model.Fiyat.ToString("C2", new CultureInfo("en-US"))</p> @* $123.45 *@

@* Tarih formatları *@
<p>Bugün: @DateTime.Now.ToString("d")</p> @* 31.12.2023 *@
<p>Bugün: @DateTime.Now.ToString("D")</p> @* 31 Aralık 2023 Pazar *@
<p>Bugün: @DateTime.Now.ToString("yyyy-MM-dd")</p> @* 2023-12-31 *@

@* Özel format *@
<p>Telefon: @Model.Telefon.ToString("(###) ### ## ##")</p>
<p>Yüzde: @(Model.Indirim / 100m).ToString("P1")</p> @* %15,0 *@

@* HTML içerik *@
<div>
    @Html.Raw(Model.Aciklama)
</div>
```

## 🧮 Form ve Model Binding

| Razor Tag Helper | Örnek |
|------------------|--------|
| `<form asp-action="Kaydet">` | Controller'a POST |
| `<input asp-for="Ad" />` | Model binding |
| `<textarea asp-for="Aciklama"></textarea>` | |
| `<select asp-for="KategoriId" asp-items="Model.Kategoriler"></select>` | |
| `<span asp-validation-for="Ad"></span>` | |
| `<input type="checkbox" asp-for="Aktif" />` | |
| `<input type="radio" asp-for="Cinsiyet" value="Erkek" />` | |

**Gelişmiş Form Örneği:**
```html
@model KullaniciViewModel

<form asp-action="ProfilGuncelle" asp-antiforgery="true" enctype="multipart/form-data">
    @Html.AntiForgeryToken()
    
    <div class="row">
        <div class="col-md-6">
            <div class="form-group">
                <label asp-for="Ad"></label>
                <input asp-for="Ad" class="form-control" placeholder="Adınızı girin" />
                <span asp-validation-for="Ad" class="text-danger"></span>
            </div>
        </div>
        <div class="col-md-6">
            <div class="form-group">
                <label asp-for="Soyad"></label>
                <input asp-for="Soyad" class="form-control" placeholder="Soyadınızı girin" />
                <span asp-validation-for="Soyad" class="text-danger"></span>
            </div>
        </div>
    </div>
    
    <div class="form-group">
        <label asp-for="Email"></label>
        <input asp-for="Email" class="form-control" type="email" />
        <span asp-validation-for="Email" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label asp-for="DogumTarihi"></label>
        <input asp-for="DogumTarihi" class="form-control" type="date" />
        <span asp-validation-for="DogumTarihi" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label asp-for="Ulke"></label>
        <select asp-for="Ulke" asp-items="Model.Ulkeler" class="form-select">
            <option value="">-- Ülke Seçin --</option>
        </select>
        <span asp-validation-for="Ulke" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label>Cinsiyet</label>
        <div>
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" asp-for="Cinsiyet" value="Erkek" />
                <label class="form-check-label" asp-for="Cinsiyet">Erkek</label>
            </div>
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" asp-for="Cinsiyet" value="Kadın" />
                <label class="form-check-label" asp-for="Cinsiyet">Kadın</label>
            </div>
        </div>
        <span asp-validation-for="Cinsiyet" class="text-danger"></span>
    </div>
    
    <div class="form-group form-check">
        <input asp-for="Bildirimler" class="form-check-input" />
        <label asp-for="Bildirimler" class="form-check-label">Bildirimleri aç</label>
    </div>
    
    <div class="form-group">
        <label asp-for="ProfilResmi"></label>
        <input asp-for="ProfilResmi" class="form-control" type="file" accept="image/*" />
        <span asp-validation-for="ProfilResmi" class="text-danger"></span>
    </div>
    
    <button type="submit" class="btn btn-success">Güncelle</button>
    <a asp-action="Index" class="btn btn-outline-secondary">Vazgeç</a>
</form>
```

## 🧑‍💻 Html Helper Metotları

| Metot | Örnek |
|-------|--------|
| `@Html.DisplayFor` | `@Html.DisplayFor(m => m.Ad)` |
| `@Html.EditorFor` | `@Html.EditorFor(m => m.Email)` |
| `@Html.ValidationMessageFor` | `@Html.ValidationMessageFor(m => m.Ad)` |
| `@Url.Action` | `@Url.Action("Detay", "Urun")` |
| `@Url.Content` | `@Url.Content("~/images/logo.png")` |
| `@Html.ActionLink` | `@Html.ActionLink("Detay", "Urun", new { id = 1 })` |
| `@Html.HiddenFor` | `@Html.HiddenFor(m => m.Id)` |
| `@Html.LabelFor` | `@Html.LabelFor(m => m.Ad)` |
| `@Html.TextAreaFor` | `@Html.TextAreaFor(m => m.Aciklama, new { @class = "form-control" })` |

**Html Helper Örnekleri:**
```csharp
@* Form elemanları *@
@using (Html.BeginForm("Kaydet", "Urun", FormMethod.Post, new { @class = "form-horizontal" }))
{
    @Html.AntiForgeryToken()
    @Html.ValidationSummary(true, "", new { @class = "text-danger" })
    
    @Html.HiddenFor(m => m.Id)
    
    <div class="form-group">
        @Html.LabelFor(m => m.Ad, new { @class = "control-label" })
        @Html.TextBoxFor(m => m.Ad, new { @class = "form-control", placeholder = "Ürün adı" })
        @Html.ValidationMessageFor(m => m.Ad, "", new { @class = "text-danger" })
    </div>
    
    <div class="form-group">
        @Html.LabelFor(m => m.Aciklama, new { @class = "control-label" })
        @Html.TextAreaFor(m => m.Aciklama, new { @class = "form-control", rows = 4 })
        @Html.ValidationMessageFor(m => m.Aciklama, "", new { @class = "text-danger" })
    </div>
    
    <button type="submit" class="btn btn-primary">Kaydet</button>
    @Html.ActionLink("İptal", "Index", null, new { @class = "btn btn-secondary" })
}

@* Link oluşturma *@
<p>
    @Html.ActionLink("Ürün Detay", "Detay", "Urun", new { id = Model.Id }, new { @class = "btn btn-info" })
</p>

@* Resim yolu *@
<img src="@Url.Content("~/images/products/" + Model.ResimYolu)" alt="@Model.Ad" />

@* Script ve CSS *@
@Styles.Render("~/Content/css")
@Scripts.Render("~/bundles/jquery")
```

## 🔒 Kullanıcı ve Yetkilendirme

| Kullanım | Örnek |
|----------|--------|
| Kullanıcı adı | `@User.Identity.Name` |
| Oturum kontrolü | `@if(User.Identity.IsAuthenticated)` |
| Rol kontrolü | `@if(User.IsInRole("Admin"))` |
| Policy attribute | `@attribute [Authorize(Policy="Yonetici")]` |
| Claim kontrolü | `@User.HasClaim("Permission", "Edit")` |
| Authorization Tag Helper | `<a asp-action="Edit" asp-authorize>Düzenle</a>` |

**Yetkilendirme Örnekleri:**
```csharp
@* Kullanıcı bilgileri *@
@if (User.Identity.IsAuthenticated)
{
    <div class="user-info">
        <span>Hoş geldiniz, @User.Identity.Name</span>
        
        @if (User.IsInRole("Admin"))
        {
            <span class="badge bg-danger">Admin</span>
        }
        
        @if (User.HasClaim("Permission", "Edit"))
        {
            <button class="btn btn-sm btn-outline-primary">Düzenle</button>
        }
        
        <a href="/logout" class="btn btn-sm btn-outline-secondary">Çıkış</a>
    </div>
}
else
{
    <div>
        <a href="/login" class="btn btn-outline-primary">Giriş Yap</a>
        <a href="/register" class="btn btn-primary">Kayıt Ol</a>
    </div>
}

@* Yetkiye göre içerik gösterme *@
@if (User.IsInRole("Admin") || User.IsInRole("Editor"))
{
    <div class="admin-panel">
        <h4>Yönetici Paneli</h4>
        <a asp-action="Create" class="btn btn-success">Yeni Ekle</a>
        <a asp-action="Reports" class="btn btn-info">Raporlar</a>
    </div>
}

@* Policy bazlı yetkilendirme *@
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

@{
    var canEdit = await AuthorizationService.AuthorizeAsync(User, "EditPolicy");
}

@if (canEdit.Succeeded)
{
    <a asp-action="Edit" asp-route-id="@Model.Id" class="btn btn-warning">Düzenle</a>
}
```

## 🧹 ViewImports ve ViewStart

| Dosya | Açıklama |
|-------|----------|
| `_ViewImports.cshtml` | Ortak `@using`, `@addTagHelper`, `@inject` tanımları |
| `_ViewStart.cshtml` | Ortak `@layout` tanımı (`_Layout.cshtml`) |

**_ViewImports.cshtml:**
```csharp
@using MyApp
@using MyApp.Models
@using MyApp.ViewModels
@using MyApp.Services
@using Microsoft.AspNetCore.Identity
@using Microsoft.AspNetCore.Authorization

@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper *, MyApp

@inject IConfiguration Configuration
@inject IViewLocalizer Localizer
```

**_ViewStart.cshtml:**
```csharp
@{
    Layout = "_Layout";
}
```

**Area ViewImports Örneği:**
```csharp
@* Areas/Admin/Views/_ViewImports.cshtml *@
@using MyApp.Areas.Admin
@using MyApp.Areas.Admin.Models
@using MyApp.Areas.Admin.Services

@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

@namespace MyApp.Areas.Admin.Views
```

## 🧠 Editor/Display Template (Custom Render)

| Tür | Yol | Örnek |
|-----|-----|--------|
| Display Template | `/Views/Shared/DisplayTemplates/Tarih.cshtml` | `@Html.DisplayFor(m => m.Tarih)` |
| Editor Template | `/Views/Shared/EditorTemplates/Urun.cshtml` | `@Html.EditorFor(m => m.Urun)` |
| Custom Template | `/Views/Shared/DisplayTemplates/Address.cshtml` | `@Html.DisplayFor(m => m.Adres)` |

**Display Template Örneği (Tarih.cshtml):**
```html
@model DateTime?

@if (Model.HasValue)
{
    <span class="date-display" title="@Model.Value.ToString("F")">
        @Model.Value.ToString("dd MMMM yyyy")
    </span>
}
else
{
    <span class="text-muted">Belirtilmemiş</span>
}
```

**Editor Template Örneği (Decimal.cshtml):**
```html
@model decimal?

<div class="input-group">
    <input type="number" 
           step="0.01" 
           value="@Model" 
           name="@ViewData.TemplateInfo.GetFullHtmlFieldName("")"
           id="@ViewData.TemplateInfo.GetFullHtmlFieldId("")"
           class="form-control" />
    <span class="input-group-text">₺</span>
</div>
```

**Kullanım:**
```csharp
@* Display Template *@
<dt>Tarih:</dt>
<dd>@Html.DisplayFor(m => m.OlusturmaTarihi)</dd>

@* Editor Template *@
<div class="form-group">
    <label asp-for="Fiyat"></label>
    @Html.EditorFor(m => m.Fiyat, new { htmlAttributes = new { @class = "form-control" } })
    <span asp-validation-for="Fiyat" class="text-danger"></span>
</div>
```

## ⚡ Async ve Performans

| İşlem | Örnek |
|-------|--------|
| Partial async render | `@await Html.PartialAsync("_Menu")` |
| Component async render | `@await Component.InvokeAsync("Menu")` |
| Script Section | `@section Scripts { <script src="..."></script> }` |
| Environment Tag Helper | `<environment include="Development">...</environment>` |
| Cache Tag Helper | `<cache expires-after="@TimeSpan.FromMinutes(10)">...</cache>` |

**Performans Optimizasyonları:**
```html
@* Environment bazlı içerik *@
<environment include="Development">
    <div class="alert alert-warning">
        Geliştirme ortamındasınız
    </div>
</environment>

<environment exclude="Development">
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>

@* Cache kullanımı *@
<cache expires-after="@TimeSpan.FromMinutes(30)" vary-by-user="true">
    <div class="sidebar">
        @await Component.InvokeAsync("KategoriMenu")
    </div>
</cache>

<cache expires-on="@DateTime.Today.AddDays(1)">
    <div class="footer">
        @await Html.PartialAsync("_Footer")
    </div>
</cache>

@* Lazy loading *@
<div id="yorumlar" data-url="@Url.Action("Yorumlar", "Urun", new { id = Model.Id })">
    Yükleniyor...
</div>

<script>
    document.addEventListener('DOMContentLoaded', function() {
        // Lazy load yorumlar
        fetch(document.getElementById('yorumlar').dataset.url)
            .then(response => response.text())
            .then(html => {
                document.getElementById('yorumlar').innerHTML = html;
            });
    });
</script>
```

## 🧩 Razor Special Syntax

| Kullanım | Açıklama | Örnek |
|----------|----------|--------|
| HTML encode kapatma | `@Html.Raw()` | `@Html.Raw("<b>test</b>")` |
| Veriyi JSON'a çevirme | `@Json.Serialize(Model)` | |
| C# helper method | `@helper RenderText(string t) { <span>@t</span> }` | |
| Localization | `@Localizer["Hello"]` | |
| ViewData kullanımı | `@ViewData["Title"]` | |

**Özel Syntax Örnekleri:**
```csharp
@* HTML Raw *@
@{
    var htmlContent = "<div class='alert'><strong>Önemli!</strong> Mesaj</div>";
}
@Html.Raw(htmlContent)

@* JSON Serialize *@
<script>
    var model = @Html.Raw(Json.Serialize(Model));
    console.log(model);
</script>

@* Helper metotlar *@
@helper FormatPara(decimal fiyat) {
    <span class="price">@fiyat.ToString("C")</span>
}

@helper Badge(string text, string type = "primary") {
    <span class="badge bg-@type">@text</span>
}

@* Kullanım *@
<p>Fiyat: @FormatPara(Model.Fiyat)</p>
<p>Durum: @Badge(Model.Durum, "success")</p>

@* Localization *@
<h1>@Localizer["Welcome"]</h1>
<p>@Localizer["HelloMessage", User.Identity.Name]</p>

@* ViewData/ViewBag *@
@{
    ViewData["Title"] = "Ürün Listesi";
    ViewBag.PageTitle = "Tüm Ürünler";
}

<h1>@ViewBag.PageTitle</h1>
<title>@ViewData["Title"] - MyApp</title>
```

## 🎯 Pratik Örnekler

### Tam Sayfa Örneği (Index.cshtml)
```html
@page
@model IndexModel
@{
    ViewData["Title"] = "Ana Sayfa";
}

<div class="container">
    <div class="row">
        <div class="col-md-8">
            <h1>@ViewData["Title"]</h1>
            
            @* Arama Formu *@
            <form method="get" class="mb-4">
                <div class="input-group">
                    <input type="text" name="q" class="form-control" placeholder="Ara..." value="@Model.AramaKelimesi" />
                    <button type="submit" class="btn btn-primary">Ara</button>
                </div>
            </form>

            @* Ürün Listesi *@
            <div class="row">
                @foreach (var urun in Model.Urunler)
                {
                    <div class="col-md-6 col-lg-4 mb-4">
                        <div class="card h-100">
                            <img src="@Url.Content(urun.ResimYolu)" class="card-img-top" alt="@urun.Ad">
                            <div class="card-body">
                                <h5 class="card-title">@urun.Ad</h5>
                                <p class="card-text">@urun.KisaAciklama</p>
                                <div class="d-flex justify-content-between align-items-center">
                                    <span class="h5 text-primary">@urun.Fiyat.ToString("C")</span>
                                    <a asp-page="/Urun/Detay" asp-route-id="@urun.Id" 
                                       class="btn btn-outline-primary">İncele</a>
                                </div>
                            </div>
                            <div class="card-footer">
                                <small class="text-muted">
                                    @if (urun.Stok > 0)
                                    {
                                        <span class="text-success">Stokta var</span>
                                    }
                                    else
                                    {
                                        <span class="text-danger">Stokta yok</span>
                                    }
                                </small>
                            </div>
                        </div>
                    </div>
                }
            </div>

            @* Sayfalama *@
            @if (Model.ToplamSayfa > 1)
            {
                <nav aria-label="Sayfalama">
                    <ul class="pagination justify-content-center">
                        @for (int i = 1; i <= Model.ToplamSayfa; i++)
                        {
                            <li class="page-item @(i == Model.MevcutSayfa ? "active" : "")">
                                <a class="page-link" href="?sayfa=@i&q=@Model.AramaKelimesi">@i</a>
                            </li>
                        }
                    </ul>
                </nav>
            }
        </div>

        <div class="col-md-4">
            @* Sidebar *@
            <div class="sidebar">
                @await Component.InvokeAsync("KategoriMenu")
                @await Html.PartialAsync("_PromosyonBanner")
                <vc:son-bakilan-urunler></vc:son-bakilan-urunler>
            </div>
        </div>
    </div>
</div>

@section Styles {
    <style>
        .card-img-top {
            height: 200px;
            object-fit: cover;
        }
        .sidebar {
            position: sticky;
            top: 20px;
        }
    </style>
}

@section Scripts {
    <script>
        // Sayfa yüklendiğinde çalışacak kodlar
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Ana sayfa yüklendi');
            
            // Kartlara hover efekti
            const cards = document.querySelectorAll('.card');
            cards.forEach(card => {
                card.addEventListener('mouseenter', function() {
                    this.style.transform = 'translateY(-5px)';
                    this.style.transition = 'transform 0.3s ease';
                });
                
                card.addEventListener('mouseleave', function() {
                    this.style.transform = 'translateY(0)';
                });
            });
        });
    </script>
}
```

### Dashboard Örneği
```html
@page
@model DashboardModel
@{
    ViewData["Title"] = "Dashboard";
}

<div class="container-fluid">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
        <h1 class="h2">@ViewData["Title"]</h1>
        <div class="btn-toolbar mb-2 mb-md-0">
            <div class="btn-group me-2">
                <button type="button" class="btn btn-sm btn-outline-secondary">Paylaş</button>
                <button type="button" class="btn btn-sm btn-outline-secondary">İhracat</button>
            </div>
            <button type="button" class="btn btn-sm btn-outline-secondary dropdown-toggle">
                <span data-feather="calendar"></span>
                Bu hafta
            </button>
        </div>
    </div>

    @* İstatistik Kartları *@
    <div class="row">
        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-primary shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-primary text-uppercase mb-1">
                                Toplam Satış
                            </div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">
                                @Model.ToplamSatis.ToString("C")
                            </div>
                        </div>
                        <div class="col-auto">
                            <i class="fas fa-dollar-sign fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-success shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-success text-uppercase mb-1">
                                Yeni Siparişler
                            </div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">@Model.YeniSiparisler</div>
                        </div>
                        <div class="col-auto">
                            <i class="fas fa-shopping-cart fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-info shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-info text-uppercase mb-1">
                                Dönüşüm Oranı
                            </div>
                            <div class="row no-gutters align-items-center">
                                <div class="col-auto">
                                    <div class="h5 mb-0 mr-3 font-weight-bold text-gray-800">
                                        @Model.DonusumOrani.ToString("P1")
                                    </div>
                                </div>
                                <div class="col">
                                    <div class="progress progress-sm mr-2">
                                        <div class="progress-bar bg-info" role="progressbar" 
                                             style="width: @((int)(Model.DonusumOrani * 100))%"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="col-auto">
                            <i class="fas fa-chart-line fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-warning shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-warning text-uppercase mb-1">
                                Bekleyen Müşteriler
                            </div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">@Model.BekleyenMusteriler</div>
                        </div>
                        <div class="col-auto">
                            <i class="fas fa-comments fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    @* Grafik ve Tablolar *@
    <div class="row">
        <div class="col-xl-8 col-lg-7">
            <div class="card shadow mb-4">
                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                    <h6 class="m-0 font-weight-bold text-primary">Satış Özeti</h6>
                </div>
                <div class="card-body">
                    <div class="chart-area">
                        <canvas id="satisGrafik"></canvas>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-xl-4 col-lg-5">
            <div class="card shadow mb-4">
                <div class="card-header py-3">
                    <h6 class="m-0 font-weight-bold text-primary">Son Siparişler</h6>
                </div>
                <div class="card-body">
                    <div class="list-group list-group-flush">
                        @foreach (var siparis in Model.SonSiparisler)
                        {
                            <div class="list-group-item d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="mb-1">@siparis.MusteriAdi</h6>
                                    <small class="text-muted">@siparis.UrunAdi</small>
                                </div>
                                <span class="badge bg-@siparis.DurumRenk rounded-pill">
                                    @siparis.Tutar.ToString("C")
                                </span>
                            </div>
                        }
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

@section Scripts {
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Satış grafiği
        const ctx = document.getElementById('satisGrafik').getContext('2d');
        const satisGrafik = new Chart(ctx, {
            type: 'line',
            data: {
                labels: @Html.Raw(Json.Serialize(Model.GrafikAylar)),
                datasets: [{
                    label: 'Satışlar',
                    data: @Html.Raw(Json.Serialize(Model.GrafikSatislar)),
                    borderColor: 'rgb(78, 115, 223)',
                    backgroundColor: 'rgba(78, 115, 223, 0.1)',
                    tension: 0.4
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        display: false
                    }
                }
            }
        });
    </script>
}
```

## 💡 Best Practices

### 1. **Kod Organizasyonu**
```csharp
@* İYİ *@
@{
    // Başlangıç kodu
    var pageTitle = Model.Ad + " - Detay";
    ViewData["Title"] = pageTitle;
}

@* KÖTÜ *@
@{
    // HTML içinde karmaşık C# kodu
}
```

### 2. **Model Kullanımı**
```csharp
@* İYİ *@
@model UrunDetayViewModel

<h2>@Model.Urun.Ad</h2>
<p>@Model.Urun.Aciklama</p>

@* KÖTÜ *@
@{
    var urun = ViewBag.Urun as Urun;
}
<h2>@urun.Ad</h2>
```

### 3. **Tag Helper vs Html Helper**
```csharp
@* İYİ - Tag Helper *@
<input asp-for="Email" class="form-control" />

@* KÖTÜ - Html Helper *@
@Html.TextBoxFor(m => m.Email, new { @class = "form-control" })
```

### 4. **Partial View Kullanımı**
```csharp
@* İYİ *@
<partial name="_UserCard" model="user" />

@* KÖTÜ *@
@Html.Partial("_UserCard", user)
```

### 5. **Async Kullanımı**
```csharp
@* İYİ *@
@await Component.InvokeAsync("RecentPosts")

@* KÖTÜ *@
@Component.Invoke("RecentPosts")
```

## 🔧 Sık Kullanılan Snippet'lar

### Pagination Snippet
```html
@if (Model.TotalPages > 1)
{
    <nav aria-label="Sayfalama">
        <ul class="pagination">
            <li class="page-item @(Model.CurrentPage == 1 ? "disabled" : "")">
                <a class="page-link" href="?page=@(Model.CurrentPage - 1)">Önceki</a>
            </li>
            
            @for (int i = 1; i <= Model.TotalPages; i++)
            {
                <li class="page-item @(i == Model.CurrentPage ? "active" : "")">
                    <a class="page-link" href="?page=@i">@i</a>
                </li>
            }
            
            <li class="page-item @(Model.CurrentPage == Model.TotalPages ? "disabled" : "")">
                <a class="page-link" href="?page=@(Model.CurrentPage + 1)">Sonraki</a>
            </li>
        </ul>
    </nav>
}
```

### Alert Snippet
```html
@if (TempData["Success"] != null)
{
    <div class="alert alert-success alert-dismissible fade show" role="alert">
        @TempData["Success"]
        <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    </div>
}

@if (TempData["Error"] != null)
{
    <div class="alert alert-danger alert-dismissible fade show" role="alert">
        @TempData["Error"]
        <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    </div>
}
```

### Breadcrumb Snippet
```html
<nav aria-label="breadcrumb">
    <ol class="breadcrumb">
        <li class="breadcrumb-item"><a asp-controller="Home" asp-action="Index">Ana Sayfa</a></li>
        <li class="breadcrumb-item"><a asp-controller="Urun" asp-action="Index">Ürünler</a></li>
        <li class="breadcrumb-item active" aria-current="page">@Model.Ad</li>
    </ol>
</nav>
```

---

## 📚 Ek Kaynaklar

- [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core)
- [Razor Syntax Reference](https://docs.microsoft.com/aspnet/core/mvc/views/razor)
- [Tag Helpers in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/intro)
- [View Components](https://docs.microsoft.com/aspnet/core/mvc/views/view-components)

## 🎉 Katkıda Bulunma

Bu cheatsheet'i geliştirmek için:
1. Fork'layın
2. Değişikliklerinizi yapın
3. Pull Request gönderin

---

**⭐ Bu rehber faydalı olduysa yıldız vermeyi unutmayın!**
