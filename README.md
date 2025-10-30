# Razor-Cheatsheet
> 🧠 Razor Syntax & ASP.NET Core Cheatsheet (Tam Sürüm)
(.cshtml, Razor Pages, MVC, ViewComponents, Layouts, TagHelpers)

![banner](assets/razor-banner.png)

## 1. 1. Temel Razor Kuralları

Bu bölüm Razor sözdiziminin en temel öğelerini açıklar. `@` sembolü Razor içinde C# kodu başlatır. Razor, HTML ile C# arasında sorunsuz bir entegrasyon sağlar; aşağıdaki örnekler, literal ve blok formlarını ve yorum kullanımını gösterir.

Örnekler:
@ - C# kodunu inline başlatır:
    @DateTime.Now

@{ ... } - Kod bloğu (birden fazla satır C#):
    @{ var mesaj = "Merhaba"; var saat = DateTime.Now; }

@* ... *@ - Razor yorumudur (çıktıya hiç çıkmaz):
    @* Bu satır HTML çıktısında görünmez *@

<!-- ... --> - HTML yorumudur (kaynakta görünür):
    <!-- Bu yorum HTML kaynağında görünür -->

@( ... ) - Karmaşık ifadeleri inline kullanmak için parantez gerekir:
    <p>@(Model.Fiyat + 5)</p>

@@ - `@` karakterini literal olarak basmak için:
    @@username  => çıktıda @username olarak görünür.

İpucu: Razor içinde JavaScript ya da HTML yazarken `@` işaretinin C# başlangıcı olmadığından emin olmak için gerekirse `@@` veya `@( ... )` kullanın.


---

## 2. 2. C# Yapıları Razor İçinde

Razor sayfalarında C# kontrol yapıları aynen kullanılabilir. Eğer HTML ile birlikte kullanıyorsanız, blokların doğru bir şekilde açılıp kapandığından emin olun.

Örnekler:
Değişken tanımı:
    @{ var isim = "Eda"; }

if / else:
    @if (Model.Aktif) {
        <span>Aktif</span>
    } else {
        <span>Pasif</span>
    }

for döngüsü:
    @for (int i = 0; i < 5; i++) {
        <li>@i</li>
    }

foreach döngüsü:
    @foreach (var item in Model) {
        <p>@item.Ad</p>
    }

switch:
    @switch (Model.Tip) {
        case 1:
            <b>A</b>;
            break;
        default:
            <b>Diğer</b>;
            break;
    }

try/catch:
    @try {
        // riskli kod
    } @catch (Exception ex) {
        <p>@ex.Message</p>
    }

İpucu: Razor bloklarında C# kodu ve HTML iç içe geçtiğinde girintileme ve parantez dengesi okunabilirlik için kritiktir. Visual Studio/VSCode genellikle bu dengeyi korur.


---

## 3. 3. HTML + Razor Etkileşimi

Razor'ın gücü HTML ile C#'ı birleştirmesidir. Model verilerini doğrudan HTML içine yazabilir, attribute'lerde kullanabilir ve ternary (üçlü) operatörle inline mantık uygulayabilirsiniz.

Örnekler:
Model değerini yazdırma:
    <h2>@Model.Baslik</h2>

Attribute içinde kullanma:
    <input value="@Model.Ad" />

Inline koşul (ternary):
    <span>@(Model.Aktif ? "Açık" : "Kapalı")</span>

Null kontrolü (C# null-conditional + null-coalescing):
    @(Model?.Ad ?? "Bilinmiyor")

İpucu: Attribute içine karmaşık ifadeler koyacaksanız `@( ... )` kullanın: `<input value="@(@Model.Fiyat.ToString("C"))" />` gibi.


---

## 4. 4. Razor Direktifleri (Directives)

Directives, Razor sayfalarının davranışını derleme zamanında veya render sırasında etkileyen özel komutlardır. En sık kullanılanlar: @page, @model, @using, @inject, @layout, @section, @functions vb.

Örnekler ve açıklamalar:
@page - Razor Pages için sayfayı rota ile işaretler:
    @page "/urunler"

@model - View ya da PageModel tipini belirtir:
    @model MyApp.Models.UrunViewModel

@using - Namespace ekler, view içinde tipleri kullanmanızı sağlar:
    @using MyApp.Helpers

@inject - Dependency Injection ile servis injekte eder:
    @inject ILogger<Home> Logger
    // sonra: @Logger.LogInformation("...");

@inherits - View'ın base sınıfını belirler (nadir kullanılır):
    @inherits BaseViewPage

@functions - Sayfa içinde küçük yardımcı metotlar tanımlanabilir:
    @functions {
        string Selam() => "Merhaba";
    }

@layout / @RenderBody / @RenderSection - Layout ve bölümler için kullanılır:
    @layout "_Layout"  // veya _ViewStart'de tanımlanır
    // Layout içinde: @RenderBody(), @RenderSection("Scripts", required:false)

@addTagHelper / @removeTagHelper - TagHelper'ları dahil/çıkar:
    @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

@namespace - Razor view'lara varsayılan namespace ekler.
@attribute - Sayfa üzerinde derleme zamanlı attribute (ör. Authorize) ekler:
    @attribute [Authorize]

İpucu: `_ViewImports.cshtml` dosyasına ortak `@using` ve `@addTagHelper` ifadelerini koymak projeyi temiz tutar.


---

## 5. 5. Layout ve Partial Kullanımı

Layout'lar ortak sayfa şablonlarıdır (header/footer/ortak script). Partials ise sayfanın içinde yeniden kullanılabilir parçalardır.

Kullanımlar:
Layout belirtme (sayfa üstünde veya _ViewStart):
    @{ Layout = "_Layout"; }

İçerik gövdesi (layout içinde):
    @RenderBody()

Bölüm render (layout içinde belirlenir ve sayfada doldurulur):
    // Layout: @RenderSection("Scripts", required:false)
    // Page: @section Scripts { <script src="..."></script> }

Partial view (senkron):
    @Html.Partial("_Header")

Partial view (asenkron):
    @await Html.PartialAsync("_Header")

RenderPartial (void, doğrudan yazar):
    @{ Html.RenderPartial("_Footer"); }

RenderAction (eski MVC, controller action çağırır):
    @{ Html.RenderAction("Menu", "Home"); }

Editor Template / Display Template:
    @Html.EditorFor(m => m.Ad)  // /Views/Shared/EditorTemplates/Urun.cshtml

ViewComponent çağırma (daha güçlü, bağımsız):
    @await Component.InvokeAsync("Menu")
    @await Component.InvokeAsync("Banner", new { aktif = true })

İpucu: Performans için sık kullanılan küçük fragmentleri ViewComponent veya PartialCache ile cache'leyin.


---

## 6. 6. Tag Helpers

Tag Helper'lar HTML etiketleri gibi yazılan, ancak server-side davranış ekleyen güçlü bir özelliktir. ASP.NET Core ile birlikte gelmiştir ve Razor'da temiz, anlaşılır markup sağlar.

Örnekler:
<input asp-for="Ad" />  // model bağlama
<span asp-validation-for="Ad"></span>  // validation mesajı
<div asp-validation-summary="All"></div>
<a asp-action="Detay" asp-controller="Urun">Detay</a>
<a asp-route-id="@urun.Id">Detay</a>
<a asp-area="Admin" asp-controller="Panel">Panel</a>
<partial name="_Header" />
<form asp-action="Kaydet" method="post">...</form>
<select asp-for="KategoriId" asp-items="Model.Kategoriler"></select>

İpucu: TagHelper'ları aktif etmek için `_ViewImports.cshtml` içinde `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` olmalıdır.


---

## 7. 7. ViewComponent Kullanımı

ViewComponent'lar, PartialView'lara göre daha güçlüdır — bağımsız mantık içerir, DI kullanabilir ve kendi view'ını barındırır.

Çalışma örneği:
Component sınıfı:
    public class KategoriListesiViewComponent : ViewComponent
    {
        public IViewComponentResult Invoke()
        {
            var list = ...; // service ile liste al
            return View(list);
        }
    }

Çağırma (view içinde):
    @await Component.InvokeAsync("KategoriListesi")

View konumu:
    /Views/Shared/Components/KategoriListesi/Default.cshtml

Parametreli çağırma:
    @await Component.InvokeAsync("Banner", new { aktif = true })

İpucu: ViewComponent, bağımsız test ve yeniden kullanım sağlar; karmaşık UI mantıklarını ViewComponent içine koyun.


---

## 8. 8. Razor Pages Özellikleri

Razor Pages, PageModel ve .cshtml çiftleriyle daha "page-centric" bir geliştirme sağlar — controller katmanını soyutlar.

Örnekler:
Sayfa directive:
    @page "/kayit"

Yönlendirme linki:
    <a asp-page="/Index">Ana Sayfa</a>

Page handler form:
    <form asp-page-handler="Kaydet">...</form>
    // PageModel içinde: public IActionResult OnPostKaydet() { ... }

Route parametreleri:
    <a asp-page="/Detay" asp-route-id="3">Detay</a>

Two-way binding (Blazor değil, ama Razor Pages'da kullanılan tag helper'larda):
    <input @bind="Ad" />  // dikkat: sadece bazı Razor Page senaryolarında

PageModel eventleri:
    public void OnGet() { }
    public IActionResult OnPostSave() { ... }

İpucu: Razor Pages dosya hiyerarşisi URL ile birebir eşleşebilir; küçük projelerde çok hızlı geliştirmenizi sağlar.


---

## 9. 9. Veri Formatlama

Veri formatlama ve HTML encode işlemleri view tarafında sık kullanılır. Farklı formatlama yöntemlerini aşağıda görebilirsiniz.

Örnekler:
String format:
    @String.Format("{0:C}", Model.Fiyat)  // para birimi formatı

Tarih formatı:
    @Model.Tarih.ToString("dd.MM.yyyy")

HTML encode kapatma (dangerous - dikkatli kullanın):
    @Html.Raw("<b>Kalın</b>")  // içerik güvenliyse kullanın

Null kontrolü:
    @(Model?.Ad ?? "Yok")

İpucu: Kullanıcı girdilerini `Html.Raw` ile doğrudan basmak XSS riskine sebep olabilir; yalnızca sanitasyon yapılmış veri için kullanın.


---

## 10. 10. Form ve Model Binding

Formlar ve model binding, Razor ile veri gönderme/alma sürecinin merkezindedir. TagHelper'lar bu süreci oldukça basit hale getirir.

Örnekler:
<form asp-action="Kaydet" method="post">
    <input asp-for="Ad" />
    <textarea asp-for="Aciklama"></textarea>
    <select asp-for="KategoriId" asp-items="Model.Kategoriler"></select>
    <span asp-validation-for="Ad"></span>
    <button type="submit">Gönder</button>
</form>

Controller tarafı:
    [HttpPost]
    public IActionResult Kaydet(UrunModel model)
    {
        if (!ModelState.IsValid) return View(model);
        // kaydet
        return RedirectToAction("Index");
    }

İpucu: Model bindingde property isimleri form input name'leriyle eşleşmelidir. Complex tiplerde prefix kullanımı için `asp-for` çok yardımcıdır.


---

## 11. 11. Html Helper Metotları

Classic MVC'de HTML Helper'lar sık kullanılır. ASP.NET Core'da tag helper'lar daha modern bir alternatif olsa da helper'lar hâlâ yaygındır.

Örnekler:
@Html.DisplayFor(m => m.Ad)
@Html.EditorFor(m => m.Email)
@Html.ValidationMessageFor(m => m.Ad)
@Url.Action("Detay", "Urun")  // controller action url'si üretir
@Url.Content("~/images/logo.png")  // içerik yolu çözümlemesi

İpucu: EditorFor ve DisplayFor, model ile ilişkilendirilmiş template'leri (EditorTemplates/DisplayTemplates) kullanır.


---

## 12. 12. Kullanıcı ve Yetkilendirme

View içinde kullanıcı bilgilerine ve yetkilendirmeye erişmek kolaydır.

Örnekler:
Kullanıcı adı:
    @User.Identity.Name

Oturum kontrolü:
    @if (User.Identity.IsAuthenticated) { <p>Hoş geldiniz, @User.Identity.Name</p> }

Rol kontrolü:
    @if (User.IsInRole("Admin")) { <a href="/admin">Admin Panel</a> }

Policy attribute (sayfa düzeyinde):
    @attribute [Authorize(Policy = "Yonetici")]

İpucu: Yetkilendirme kontrollerini view'da yaparken mümkün olduğunca server-side kontrolleri (Controller/PageModel) da uygulayın; view sadece gösterim içindir.


---

## 13. 13. ViewImports ve ViewStart

Projede tekrar eden view ayarlarını merkezi hale getirmek için kullanılırlar.

_ViewImports.cshtml_ genellikle:
    @using MyApp.Models
    @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
    @namespace MyApp.Pages

_ViewStart.cshtml_ genelde layout tanımlar:
    @{ Layout = "_Layout"; }

İpucu: Common using ve tag helper'ların tek bir dosyada toplanması, her view'da tekrar yazmayı engeller.


---

## 14. 14. Editor/Display Template (Custom Render)

Editor ve Display template'leri ortak render davranışları için kullanışlıdır. Örneğin tarih formatı veya özel model görüntüleri.

Örnekler:
Display Template (Views/Shared/DisplayTemplates/Tarih.cshtml):
    @model DateTime?
    @Model?.ToString("dd.MM.yyyy")

Kullanım:
    @Html.DisplayFor(m => m.Tarih)

Editor Template (Views/Shared/EditorTemplates/Urun.cshtml):
    @model MyApp.Models.UrunModel
    <div class="urun-editor">
        @Html.TextBoxFor(m => m.Ad)
        @Html.TextAreaFor(m => m.Aciklama)
    </div>

İpucu: Template'ler tip bazlı çalışır; aynı tip için bir kez yazıp projede birçok yerde kullanabilirsiniz.


---

## 15. 15. Async ve Performans

Asenkron render ve doğru cache kullanımı sayfa performansını artırır.

Örnekler:
Partial async render:
    @await Html.PartialAsync("_Menu")

Component async render:
    @await Component.InvokeAsync("Menu")

Script section:
    @section Scripts { <script src="..."></script> }

İpucu: Ağ istekleri veya veri çeken component'leri asenkron yaparak thread havuzunu daha verimli kullanın. Caching: ResponseCache, MemoryCache ve OutputCache (yeni ASP.NET Core sürümlerinde) kullanın.


---

## 16. 16. Razor Special Syntax

Razor'ın bazı özel yardımcı sözdizimleri vardır — helper method'lar, JSON serileştirme ve encode işlemleri gibi.

Örnekler:
HTML encode kapatma:
    @Html.Raw("<b>test</b>")

Veriyi JSON'a çevirme (örnek):
    @using System.Text.Json
    <script>
        var model = @Html.Raw(JsonSerializer.Serialize(Model));
    </script>

C# helper method (page-level helper):
    @helper RenderText(string t) {
        <span>@t</span>
    }
    // çağırma: @RenderText("Merhaba")

İpucu: `@helper` yalnızca Razor view'larda desteklenir (cshtml) ve küçük yardımcılar için uygundur. Büyük logic'leri C# sınıflarında tutun.


---

## Assets
- `assets/razor-banner.png` — banner görseli (yer tutucu)
- `assets/layout-example.png` — layout örneği (yer tutucu)
- `assets/partialview-example.png` — partial view örneği (yer tutucu)

## Lisans
MIT © Bilginet Akademi
