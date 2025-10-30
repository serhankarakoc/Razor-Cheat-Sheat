# Razor Syntax & ASP.NET Core Cheatsheet

Bu rehber, ASP.NET Core'da Razor syntax'ı için kapsamlı bir referans sağlar.

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

## 🧩 Temel Razor Kuralları

| Tanım | Açıklama | Örnek |
|-------|----------|--------|
| `@` | C# kodu başlatır | `@DateTime.Now` |
| `@{ ... }` | Kod bloğu | `@{ var mesaj = "Merhaba"; }` |
| `@* ... *@` | Razor yorumu | `@* Görünmez *@` |
| `<!-- ... -->` | HTML yorumu | `<!-- Görünür -->` |
| `@( ... )` | Karmaşık ifade | `<p>@(Model.Fiyat + 5)</p>` |
| `@@` | @ karakterini literal basar | `@@username → @username` |

## ⚙️ C# Yapıları Razor İçinde

| Yapı | Örnek |
|------|--------|
| Değişken tanımı | `@{ var isim = "Eda"; }` |
| if / else | `@if(Model.Aktif){ <span>Aktif</span> } else { <span>Pasif</span> }` |
| for | `@for(int i=0;i<5;i++){ <li>@i</li> }` |
| foreach | `@foreach(var item in Model){ <p>@item.Ad</p> }` |
| switch | `@switch(Model.Tip){ case 1: <b>A</b>; break; }` |
| try/catch | `@try{ ... } @catch(Exception ex){ ... }` |

## 🧱 HTML + Razor Etkileşimi

| Amaç | Örnek |
|------|--------|
| Model değerini yazdırma | `<h2>@Model.Baslik</h2>` |
| Attribute içinde kullanma | `<input value="@Model.Ad" />` |
| Inline koşul | `<span>@(Model.Aktif ? "Açık" : "Kapalı")</span>` |
| Null kontrolü | `@(Model?.Ad ?? "Bilinmiyor")` |

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

## 💬 ViewComponent Kullanımı

| Kullanım | Açıklama | Örnek |
|----------|----------|--------|
| Component çağırma | `@await Component.InvokeAsync("KategoriListesi")` | |
| Parametreli | `@await Component.InvokeAsync("Banner", new { aktif = true })` | |
| Component sınıfı | `public class KategoriListesiViewComponent : ViewComponent { public IViewComponentResult Invoke() => View(); }` | |
| View konumu | `/Views/Shared/Components/KategoriListesi/Default.cshtml` | |

## 🧱 Razor Pages Özellikleri

| Özellik | Örnek |
|---------|--------|
| `@page` | `@page "/kayit"` |
| `asp-page` | `<a asp-page="/Index">Ana Sayfa</a>` |
| `asp-page-handler` | `<form asp-page-handler="Kaydet">` |
| `asp-route-*` | `<a asp-page="/Detay" asp-route-id="3">Detay</a>` |
| `@bind` | Two-way binding: `<input @bind="Ad" />` |
| OnGet, OnPost | `public void OnPostSave(){}` (PageModel) |

## 🔠 Veri Formatlama

| İşlem | Örnek |
|-------|--------|
| String format | `@String.Format("{0:C}", Model.Fiyat)` |
| Tarih format | `@Model.Tarih.ToString("dd.MM.yyyy")` |
| HTML encode kapatma | `@Html.Raw("<b>Kalın</b>")` |
| Null kontrol | `@(Model?.Ad ?? "Yok")` |

## 🧮 Form ve Model Binding

| Razor Tag Helper | Örnek |
|------------------|--------|
| `<form asp-action="Kaydet">` | Controller'a POST |
| `<input asp-for="Ad" />` | Model binding |
| `<textarea asp-for="Aciklama"></textarea>` | |
| `<select asp-for="KategoriId" asp-items="Model.Kategoriler"></select>` | |
| `<span asp-validation-for="Ad"></span>` | |

## 🧑‍💻 Html Helper Metotları

| Metot | Örnek |
|-------|--------|
| `@Html.DisplayFor` | `@Html.DisplayFor(m => m.Ad)` |
| `@Html.EditorFor` | `@Html.EditorFor(m => m.Email)` |
| `@Html.ValidationMessageFor` | `@Html.ValidationMessageFor(m => m.Ad)` |
| `@Url.Action` | `@Url.Action("Detay", "Urun")` |
| `@Url.Content` | `@Url.Content("~/images/logo.png")` |

## 🔒 Kullanıcı ve Yetkilendirme

| Kullanım | Örnek |
|----------|--------|
| Kullanıcı adı | `@User.Identity.Name` |
| Oturum kontrolü | `@if(User.Identity.IsAuthenticated)` |
| Rol kontrolü | `@if(User.IsInRole("Admin"))` |
| Policy attribute | `@attribute [Authorize(Policy="Yonetici")]` |

## 🧹 ViewImports ve ViewStart

| Dosya | Açıklama |
|-------|----------|
| `_ViewImports.cshtml` | Ortak `@using`, `@addTagHelper`, `@inject` tanımları |
| `_ViewStart.cshtml` | Ortak `@layout` tanımı (`_Layout.cshtml`) |

**Örnek:**
```cshtml
@using MyApp.Models
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@layout "_Layout"
```

## 🧠 Editor/Display Template (Custom Render)

| Tür | Yol | Örnek |
|-----|-----|--------|
| Display Template | `/Views/Shared/DisplayTemplates/Tarih.cshtml` | `@Html.DisplayFor(m => m.Tarih)` |
| Editor Template | `/Views/Shared/EditorTemplates/Urun.cshtml` | `@Html.EditorFor(m => m.Urun)` |

## ⚡ Async ve Performans

| İşlem | Örnek |
|-------|--------|
| Partial async render | `@await Html.PartialAsync("_Menu")` |
| Component async render | `@await Component.InvokeAsync("Menu")` |
| Script Section | `@section Scripts { <script src="..."></script> }` |

## 🧩 Razor Special Syntax

| Kullanım | Açıklama | Örnek |
|----------|----------|--------|
| HTML encode kapatma | `@Html.Raw()` | `@Html.Raw("<b>test</b>")` |
| Veriyi JSON'a çevirme | `@Json.Serialize(Model)` | |
| C# helper method | `@helper RenderText(string t) { <span>@t</span> }` | |

---

## 🎯 Pratik Örnekler

### Temel View Örneği
```cshtml
@page
@model UrunModel

<h2>@Model.UrunAdi</h2>
<p>Fiyat: @Model.Fiyat.ToString("C")</p>

@if (Model.Stok > 0)
{
    <span class="text-success">Stokta var</span>
}
else
{
    <span class="text-danger">Stokta yok</span>
}
```

### Form Örneği
```cshtml
<form asp-action="Kaydet" method="post">
    <div class="form-group">
        <label asp-for="Ad"></label>
        <input asp-for="Ad" class="form-control" />
        <span asp-validation-for="Ad" class="text-danger"></span>
    </div>
    
    <div class="form-group">
        <label asp-for="Email"></label>
        <input asp-for="Email" class="form-control" />
        <span asp-validation-for="Email" class="text-danger"></span>
    </div>
    
    <button type="submit" class="btn btn-primary">Kaydet</button>
</form>
```

### Layout Kullanımı
**_Layout.cshtml:**
```cshtml
<!DOCTYPE html>
<html>
<head>
    <title>@ViewData["Title"] - MyApp</title>
</head>
<body>
    <header>
        <partial name="_Header" />
    </header>
    
    <main>
        @RenderBody()
    </main>
    
    <footer>
        <partial name="_Footer" />
    </footer>
    
    @RenderSection("Scripts", required: false)
</body>
</html>
```

**View:**
```cshtml
@{
    ViewData["Title"] = "Ana Sayfa";
    Layout = "_Layout";
}

@section Scripts {
    <script>
        console.log("Sayfa yüklendi");
    </script>
}
```

---

**Not:** Bu cheatsheet ASP.NET Core 6.0 ve üzeri versiyonlar için günceldir.

## 📚 Ek Kaynaklar

- [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core)
- [Razor Syntax Reference](https://docs.microsoft.com/aspnet/core/mvc/views/razor)
- [Tag Helpers in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/intro)

---

*⭐ Bu rehber faydalı olduysa yıldız vermeyi unutmayın!*
