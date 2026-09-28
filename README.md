# ARAŞTIRMA & RAPORLAMA ÖDEVİ (.NET)

## 1.Modern Yazılım Geliştirme Pratikleri

### Git ve GitHub

**Git:** projedeki geçmiş süreçleri arşivleyen bir yapıdır. Aynı projede birden fazla kişinin çalışmasına olanak sağlar. Planlı ve kontrollğ bir şekilde bir düzen içerisinde projeyi ilerletmemize ve süreçleri takip etmemize yardımcı olur.

**GitHub:** Git depolarını barındıran bir arayüz gibi düşünülebilir. Git komutlarının sonuçlarını gördüğümüz yerdir. Örneğin pull ya da push vb.

### Temel Git Komutları

**git init :** klasörde depo oluşturur.

**git clone :** uzaktaki depoyu yerel bilgisayara kopyalar. (git clone URL)

**git add :** değişiklikleri sahneye alır. Commit atmaya uygun hale getirir.

**git push :** yereldeki commit leri uzak depoya yollar.

**git pull :** uzak depoda ki son değişiklikleri getirir ve uygular

**git branch :** kaç tane branch(dal) olduğunu ve bizim hangisinde çalıştığımızı gösterir.

**git merge :** başka bir branch daki (daldaki) değişiklikleri mevcut branch ile birleştirir.

**Merge Conflict:** İki farklı branch deki değişikliklerin birbiri ile çakışması/ uyuşmaması durumudur.

git status ile uyuşmayan yerler bulunur ve farklı yerler tercihe göre seçilir. Ardından git add ve git commit yapılır.

**CI/CD (Sürekli Entegrasyon ve Sürekli Teslimat/Dağıtım):** CI, geliştiricilerin kod değişikliklerini ortak depoya eklediğinde projenin otomatik olarak derlenmesi ve test edilmesidir. Örneğin bir .NET Web API projesinde GitHub’a kod gönderildiğinde dotnet restore, dotnet build ve dotnet test komutları sırayla çalıştırılabilir. Böylece derleme hataları ve başarısız testler erken fark edilir.

CD ise testleri geçen sürümün yayımlanmaya hazırlanması veya sunucuya dağıtılması sürecidir. Sürekli teslimatta sürüm yayımlanmaya hazır tutulur ve son adım için insan onayı alınabilir. Sürekli dağıtımda ise gerekli kontrolleri geçen sürüm otomatik olarak yayımlanır. Bir test başarısız olursa süreç durdurulmalı ve hatalı sürüm dağıtılmamalıdır. Bu rapordaki GitHub Actions örneği yalnızca derleme ve test aşamalarını gösterir; sunucuya dağıtım yapmaz.

```yaml
# Dosya konumu: .github/workflows/dotnet.yml
name: dotnet-ci

on: [push, pull_request]

jobs:
 build-and-test:
  runs-on: ubuntu-latest
  steps:
   - uses: actions/checkout@v4
   - uses: actions/setup-dotnet@v4
    with:
      dotnet-version: '8.0.x'
   - run: dotnet restore
   - run: dotnet build --no-restore --configuration Release
   - run: dotnet test --no-build --configuration Release
```

| Satır | Basit anlamı |
|---|---|
| on: [push, pull_request] | Kod gönderilince veya pull request açılınca çalış. |
| runs-on: ubuntu-latest | İşlemleri GitHub’ın sağladığı bir Linux bilgisayarda yap. |
| checkout | Proje dosyalarını o bilgisayara indir. |
| setup-dotnet | .NET 8 araçlarını hazırla. |
| dotnet restore | Projenin ihtiyaç duyduğu NuGet paketlerini indir. |
| dotnet build | Kod derleniyor mu kontrol et. |
| dotnet test | Projedeki otomatik testleri çalıştır. |

**SDLC (Yazılım Geliştirme Yaşam Döngüsü),** bir yazılımın fikir aşamasından bakımına kadar geçtiği süreçtir. Örneğin bir aidat takip API’si geliştirildiğini varsayalım:

| Aşama | Pratikte ne yapılır? |
|---|---|
| Planlama | “Bu sistem neden gerekli, ne kadar sürede yapılacak?” soruları yanıtlanır. |
| Analiz | Kullanıcıların ne yapabileceği belirlenir. Örneğin sakin yalnızca kendi borcunu görebilmeli. |
| Geliştirme | Endpoint’ler, iş kuralları ve veritabanı kodlanır. |
| Test | “Bir sakin başka sakinin borcunu görebiliyor mu?” gibi senaryolar denenir. |
| Dağıtım | Uygulama sunucuya yüklenerek kullanıma açılır. |
| Bakım | Hatalar düzeltilir, performans iyileştirilir ve yeni ihtiyaçlar eklenir. |

Agile, Scrum ve Kanban ise bu işleri nasıl organize edeceğimizi anlatır:

- Agile: Küçük parçalar hâlinde çalışıp düzenli geri bildirim alma yaklaşımıdır.
- Scrum: İşleri genellikle belirli süreli sprint dönemlerine böler. Sprint başında işler seçilir, sonunda yapılanlar değerlendirilir.
- Kanban: İşleri “Yapılacak → Yapılıyor → Tamamlandı” gibi bir panoda takip eder. Aynı anda çok fazla işe başlanmasını sınırlamaya çalışır.

Kısacası SDLC, yazılımın geçtiği aşamaları; Scrum ve Kanban ise ekibin bu aşamalardaki işleri yönetme biçimini açıklar. Aşamalar mutlaka yalnızca bir kez ve katı bir sırayla yaşanmaz; bir özellik yayımlandıktan sonra geri bildirimle yeniden analiz ve geliştirme yapılabilir.

## 2. .NET Ekosistemi

### .NET nedir, ne işe yarar?

.NET bir programlama dili değil, yazılım geliştirme platformudur. C# (ayrıca F# ve Visual Basic) ile yazılan uygulamaları oluşturmak ve çalıştırmak için üç temel parça sağlar: SDK (proje oluşturma, derleme, test ve yayımlama araçları), çalışma zamanı/runtime (uygulama çalışırken kodu yürütme, bellek yönetimi gibi işler) ve sınıf kütüphaneleri (dosya, ağ, koleksiyon gibi hazır işlevler). Web API geliştirmek için bu platformun üzerine ASP.NET Core kullanılır. Örneğin C# ile yazdığım bir ürün API’sini dotnet build ile derler, .NET çalışma zamanı üzerinde çalıştırırım. .NET’i tercih etme nedenleri arasında güçlü kütüphaneler ve araçlar, C# ekosistemi, web servisleri geliştirme desteği ve uygun uygulamalarda farklı işletim sistemlerine dağıtabilme vardır.

### Kısa tarihçe ve isimler

- 2002-.NETFramework: Microsoft’unilk.NETuygulamaplatformu; Windows’a bağlı klasik uygulamalar ve ASP.NET için yaygınlaştı.
- 2016- .NET Core: Modern, açık kaynak ve platformlar arası ayrı bir uygulama platformu olarak yayımlandı. ASP.NET Core bununla birlikte modern web geliştirme çizgisini oluşturdu.
- 2020- .NET 5: .NET Core’un devamı .NET adıyla sürdürülmeye başlandı. Yani .NET 5, .NET Framework 4.8’in bir sonraki sürümü değildir. .NET 7, 8, 9 ve 10 bu modern çizginin sonraki sürümleridir.

| Konu | .NET Framework 4.x | .NET Core 1.0-3.1 | Modern .NET (5 ve sonrası) |
|---|---|---|---|
| Yerleri | İlk .NET uygulama ailesi | Yeniden tasarlanan devam çizgisi | .NET Core’un devamı; 7/8/9/10 bunun sürümleri |
| İşletim sistemi | Windows | Windows, Linux, macOS | Windows, Linux, macOS; uygulama türüne bağlı |
| Web geliştirme / Yeni backend projesi | Klasik ASP.NET / Eski Windows bağımlılığı gerektirirse | ASP.NET Core / Eski sürümler destek dışı | ASP.NET Core / Desteklenen bir sürüm seçilir |

**Platformlar arası çalışma ne demektir?** Aynı ASP.NET Core API kaynak kodu, uyumlu paketler ve yapılandırmayla Windows, Linux veya macOS üzerinde derlenip çalıştırılabilir; sunucu olarak Linux kullanmak mümkündür. Ancak her .NET uygulaması her işletim sisteminde çalışmaz. Örneğin WinForms ve WPF arayüzleri Windows’a özeldir; Windows’a özel bir kütüphane kullanan API de ek değişiklik olmadan Linux’ta çalışmayabilir

> ![dotnet --info örneği](assets/dotnet--info.png)

SDK satırı derleme araçlarının sürümünü; OS Platform ve RID çalışılan işletim sistemini/mimariyi; runtimes satırları uygulamayı çalıştıracak kurulu bileşenleri gösterir. xxx ve x yer tutucudur; bu satırlar gerçekten alınmış terminal çıktısı değildir.

### Senkron ve Asenkron Programlama

Senkron çağrı sonuca kadar yürütmeyi bekletebilir. Bir HTTP isteği veya veritabanı sorgusu gibi I/O ağırlıklı işte async/await, beklerken iş parçacığının başka isteklere hizmet etmesini sağlar; işlemi otomatik hızlandırmaz ve CPU ağırlıklı işi kendiliğinden paralelleştirmez. Task gelecekte tamamlanacak işlemin sonucunu temsil eder.

```csharp
public async Task GetProductAsync(int id, CancellationToken ct)
    => await _db.Products.FindAsync(new object[] { id }, ct);
```

## 3. Backend Geliştirme Temelleri

Frontend kullanıcı arayüzünü görüntüler ve etkileşimi toplar. Backend iş kurallarını, yetki kon trolünü ve kalıcı veriyi yönetir. Web sunucusu HTTP isteğini karşılar; ASP.NET Core’da Kestrel uygulama sunucusudur. API, yazılımlar arasında tanımlı etkileşim sözleşmesidir. Sık görülen API tarzları REST/HTTP, SOAP ve GraphQL; ayrıca RPC/gRPC de kullanılabilir.

HTTP, istemcinin istek ve sunucunun yanıt gönderdiği protokoldür. Metot, URL, başlıklar, gövde ve durum kodu birlikte davranışı tarif eder.

| Metot | Örnek | Beklenen Davranış |
|---|---|---|
| GET | GET /api/products/42 | Ürünü getirir; salt okumadır. |
| POST | POST /api/products | Yeni ürün oluşturur; çoğunlukla 201 Created. |
| PUT | PUT /api/products/42 | Kaynağı hedef sözleşmeye göre bütünüyle günceller; idempotent tasarlanır. |
| DELETE | DELETE /api/products/42 | Ürünü siler; tekrarda kalıcı son durum aynıdır. |

REST yaklaşımında kaynaklar URL ile temsil edilir, standart HTTP metotları kullanılır, sunucu her isteği gerekli bağlamıyla değerlendirir; durum kodları sonucu belirtir. GET için 200/404, doğrulama için 400, kimlik doğrulanmamışsa 401, yetki yoksa 403 tipik örneklerdir.

JSON, veri taşımak için hafif bir metin biçimidir:

```json
{"id":42,"name":"Kalem","price":15.50,"inStock":true}
```

idsayı, namemetin, pricesayı, inStockbooleandır. JSON’daözellikadları çift tırnak içinde olmalı; tarih için evrensel bir yerel tür yoktur, çoğunlukla ISO 8601 metni kullanılır.

| Yaklaşım | İstek ve Şema | Güçlü Yanı | Dikkat Edilmesi Gereken |
|---|---|---|---|
| REST | Kaynak URL’leri ve HTTP; çoğunlukla JSON | Basit HTTP semantiği ve önbellek | Çok sayıda endpoint veya bazı ekranlarda fazla/eksik veri |
| SOAP | XML mesajı, WSDL sözleşmesi | Katı sözleşme ve WS-* ekosistemi | Daha ayrıntılı ve ağır mesajlar |
| GraphQL | Şemaya karşı sorgu; genellikle tek HTTP endpoint’i | İstemci gereken alanları seçer | Yetkilendirme, sorgu maliyeti ve önbellek tasarımı |

## 4. ASP.NET Core ve Mimari

ASP.NET,Microsoft’un klasik web teknolojileri ailesini de ifade eder. ASP.NET Core, modern .NET üzerinde çalışan, platformlar arası, modüler web uygulaması ve API çatısıdır. MVC, Model (veri/iş modeli), View (görünüm) ve Controller (isteği yönetir) sorumluluklarını ayırır. Yalnızca JSON dönen API’de View zorunlu değildir; controller veya Minimal API kullanılabilir.

Middleware, HTTP isteği/yanıtı boyunca çalışan ara bileşendir. Sıra önemlidir: istek sırayla ilerler, yanıt genellikle ters yönde döner. Dependency Injection, nesnenin bağımlılıklarını içeride new ile sabitlemek yerine dışarıdan verir. Bu, bir servisi testte sahte sürümüyle değiştirmeyi kolaylaştırır.

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
builder.Services.AddScoped();< IProductService, ProductService >();

builder.Services.AddAuthentication("Bearer").AddJwtBearer("Bearer", options =>
{
options.Authority = builder.Configuration["Auth:Authority"]; options.Audience =
builder.Configuration["Auth:Audience"];
});
builder.Services.AddAuthorization();
builder.Services.AddProblemDetails();

var app = builder.Build();
app.UseExceptionHandler(); // Sonraki adımlardaki işlenmeyen hataları yakalar.
app.UseHttpsRedirection();
app.UseAuthentication();      // Kullanıcı kimliğini belirler.
app.UseAuthorization();        // Kimliğe göre erişimi denetler.
app.MapControllers();
app.Run();
```

Bu örnek iskeletin çalışması için uygun kimlik sağlayıcısı, doğru Authority/Audience, paketler ve yapılandırma gerekir. UseAuthentication her zaman UseAuthorization’dan önce gelir. Gerçek bir uygulamada CORS, yönlendirme veya statik dosya middleware’leri varsa konumları gereksinime göre ayarlanır.

DI (Dependency Injection), Türkçesiyle Bağımlılık Enjeksiyonu, bir sınıfın ihtiyaç duyduğu nesneyi kendi içinde oluşturmak yerine dışarıdan almasıdır.

Ürünleri getiren bir ProductsController düşün. Bu controller’ın ProductService adlı servise ihtiyacı var. DI kullandığında ASP.NET Core, servisi controller’a kendisi verir:

```csharp
// Program.cs: Hangi servisin kullanılacağını uygulamaya bildiriyoruz.
builder.Services.AddScoped<IProductService, ProductService>();
// Controller: Servisi dışarıdan alıyoruz.
public class ProductsController(IProductService service) : ControllerBase
{
   [HttpGet("api/products/{id:int}")]
   public async Task<IActionResult> Get(int id)
   {
     var product = await service.GetAsync(id);
     return product is null ? NotFound() : Ok(product);
   }
}
```

Dependency Injection (DI), bir sınıfın ihtiyaç duyduğu bağımlılıkların sınıf içinde oluşturulması yerine dışarıdan sağlanmasıdır. ASP.NET Core’da servisler Program.cs içinde kaydedilir ve gerektiğinde controller gibi sınıflara otomatik olarak verilir. Bu yaklaşım sınıflar arasındaki sıkı bağı azaltır, kodun test edilmesini ve bakımını kolaylaştırır.

Katmanlı mimaride API isteği alır, Service iş kurallarını uygular, Data Access veritabanıyla iletişim kurar. Repository, veri erişim işlemlerini düzenlemek için kullanılabilir; ancak EF Core kullanılan her projede zorunlu değildir.

Clean Architecture’da Domain temel iş kurallarını, Application yapılacak işlemleri, Infrastructure veritabanı ve dış servis bağlantılarını, API ise HTTP isteklerini yönetir. Kod bağımlılıkları iş kurallarına doğru yönelir; böylece Domain, kullanılan veritabanını veya API teknolojisini bilmek zorunda kalmaz.

Küçük projelerde basit bir katmanlı yapı yeterli olur. İş kuralları karmaşıklaştığında Clean Architecture kodun bakımını ve test edilmesini kolaylaştırır.

| Katmanlı mimari | Clean Architecture |
|---|---|
| **[![Katmanlı mimari diyagramı](assets/katmanli-mimari.png)]** | **[![Clean Architecture diyagramı](assets/clean-architecture.png)]** |

## 5. Veritabanı ve ORM

SQL, ilişkisel veritabanlarında veri eklemek, okumak, güncellemek ve silmek için kullanılan dildir. İlişkisel veritabanları verileri tablolar hâlinde saklar; tablolar birincil ve yabancı anahtarlarla ilişkilendirilebilir. İlişkisel olmayan veritabanları ise veriyi belge, anahtar-değer veya grafik gibi farklı yapılarda tutabilir. Örneğin ürünler ve siparişler arasında belirgin ilişkiler varsa ilişkisel veritabanı uygun bir seçim olabilir.

ORM (Object-Relational Mapping), uygulamadaki nesneler ile veritabanı tabloları arasında eşleme yapar. Entity Framework Core (EF Core), .NET uygulamalarında kullanılan bir ORM aracıdır. DbContext, EF Core’un veritabanıyla çalışmak için kullandığı sınıftır: sorguları yürütür ve nesnelerdeki değişiklikleri takip eder. Örneğin db.Products ürün kayıtlarına erişir; SaveChangesAsync() yapılan ekleme, güncelleme veya silme işlemlerini veritabanına kaydeder.

LINQ, C# içinde veriler üzerinde sorgu yazmayı sağlar. Sık kullanılan ifadeler: Where (filtreleme), Select (istenen alanları seçme), OrderBy (sıralama), Take (sonuç sayısını sınırlama), AnyAsync (eşleşen kayıt var mı?) ve CountAsync (kayıt sayısı). Örneğin:

```csharp
var products = await db.Products
  .Where(p => p.Price > 100)
  .OrderBy(p => p.Name)
  .Select(p => new { p.Id, p.Name })
  .Take(10)
  .ToListAsync();
```

Bu LINQ sorgusu, fiyatı 100’den büyük ürünleri ada göre sıralar ve ilk 10 ürünün yalnızca ID ile adını getirir. SQL Server için karşılığı yaklaşık olarak şöyledir; EF Core’un ürettiği gerçek SQL kullanılan veritabanına göre değişebilir:

```sql
SELECT TOP (10) Id, Name
FROM Products
WHERE Price > 100
ORDER BY Name;
```

| Yaklaşım | Başlangıç noktası | Ne zaman tercih edilebilir? |
|---|---|---|
| Code-First | Önce C# sınıfları yazılır; veritabanı şeması migration’larla oluşturulur ve güncellenir. | Veritabanı uygulamayla birlikte geliştiriliyorsa. |
| Database-First | Önce mevcut veritabanı vardır; C# modelleri bu şemadan üretilir. | Var olan bir veritabanıyla çalışılıyorsa. |

Dört temel SQL işlemi aşağıdaki gibidir:

```sql
SELECT Id, Name FROM Products WHERE Price > 130;
INSERT INTO Products (Name, Price) VALUES ('Kalem', 16);
UPDATE Products SET Price = 16.00 WHERE Id = 42;
DELETE FROM Products WHERE Id = 58;
```

SELECT veri okur, INSERT yeni kayıt ekler, UPDATE mevcut kaydı değiştirir, DELETE kayıt siler. Özellikle UPDATE ve DELETE sorgularında WHERE koşulu unutulursa tablodaki tüm kayıtlar etkilenebilir.

## 6. Güvenlik ve Performans

Authentication (kimlik doğrulama), kullanıcının kim olduğunu belirler; örneğin giriş yaparken hesabını doğrular. Authorization (yetkilendirme) ise doğrulanan kullanıcının hangi işlemleri yapabileceğini kontrol eder. Giriş yapmış bir kullanıcının yalnızca kendi kayıtlarını görebilmesi yetkilendirmeye örnektir.

JWT (JSON Web Token), istemcinin API’ye kimlik ve yetki bilgisi taşımasında kullanılabilen bir token biçimidir. header.payload.signature olmak üzere üç bölümden oluşur. Header token türünü ve imza algoritmasını, payload kullanıcı ve süre gibi bilgileri, signature ise token’ın değiştirilip değiştirilmediğini doğrulamak için kullanılan imzayı içerir. Payload şifreli olmadığından içine parola gibi gizli bilgiler yazılmamalıdır. API; imzayı, geçerlilik süresini ve token’ın beklenen kaynaktan gelip gelmediğini kontrol etmelidir.

OAuth 2.0, bir uygulamaya belirli kaynaklara sınırlı erişim yetkisi verilmesini düzenler; tek başına giriş yapan kişinin kimliğini kanıtlayan bir protokol değildir. OpenID Connect (OIDC), OAuth 2.0 üzerine kimlik doğrulama katmanı ekler. OpenID, OIDC’den önce geliştirilmiş ayrı bir kimlik doğrulama yaklaşımıdır. OpenIddict ise .NET uygulamalarında OAuth 2.0 ve OIDC çözümleri geliştirmek için kullanılan bir yazılımdır.

### Performansı artırma yöntemleri

1. AsNoTracking(): EF Core’da yalnızca okunacak veriler için değişiklik takibini kapatarak gereksiz işlem maliyetini azaltabilir.
2. Sayfalama ve gerekli alanları seçme: Binlerce kaydı ve tüm sütunları tek seferde göndermek yerine yalnızca gereken sonuçları getirir.
3. Önbellekleme ve Redis: Sık istenen verileri geçici olarak saklayarak aynı veritabanı sorgusunun sürekli tekrarlanmasını önleyebilir. Veri değiştiğinde önbelleğin güncellenmesi de planlanmalıdır.
4. IAsyncEnumerable<T>: Uygun durumlarda sonuçları tamamını belleğe almadan parça parça işlemeye yardımcı olur.
5. Profiling (performans ölçümü): Yavaşlığın API’den, veritabanı sorgusundan veya başka bir işlemden kaynaklandığını ölçerek doğru noktayı iyileştirmeyi sağlar.

OWASP Top 10, web uygulamalarındaki önemli güvenlik risklerini gruplandırır. Aşağıdaki liste 2025 sürümüne aittir.

| Kategori | Risk ve örnek önlem |
|---|---|
| A01 – Broken Access Control | Kullanıcı yetkisi olmayan verilere erişebilir. ASP.NET Core’da [Authorize] kullanmanın yanında kaydın o kullanıcıya ait olup olmadığı da kontrol edilmelidir. |
| A02 – Security Misconfiguration | Hatalı ayarlar uygulamayı açığa çıkarabilir. Üretimde ayrıntılı hata sayfaları kapatılmalı ve gizli anahtarlar kodda tutulmamalıdır. |
| A03 – Software Supply Chain Failures | Kullanılan paketler veya derleme ve dağıtım süreci tehlikeye girebilir. Bağımlılıklar ve güncellemeleri düzenli kontrol edilmelidir. |
| A04 – Cryptographic Failures | Hassas veriler yetersiz korunabilir. HTTPS kullanılmalı; parolalar düz metin yerine güvenli parola hash’iyle saklanmalıdır. |
| A05 – Injection | Güvenilmeyen girdi SQL sorgusu veya başka bir komutun anlamını değiştirebilir. Parametreli sorgular kullanılmalı ve HTML çıktısı uygun biçimde kodlanmalıdır. |
| A06 – Insecure Design | İşleyiş daha tasarım aşamasında kötüye kullanıma açık olabilir. Yetki sınırları ve olası saldırı senaryoları geliştirmeden önce düşünülmelidir. |
| A07 – Authentication Failures | Hatalı giriş veya oturum yönetimi hesap ele geçirmeye yol açabilir. Token doğrulaması, güvenli parola politikası ve uygun hız sınırlaması uygulanmalıdır. |
| A08 – Software or Data Integrity Failures | Yazılımın veya verinin değiştirilip değiştirilmediği doğrulanmayabilir. Güvenilir paket ve dağıtım süreçleri kullanılmalıdır. |
| A09 – Security Logging and Alerting Failures | Şüpheli işlemler kaydedilmez veya fark edilmez. Önemli güvenlik olayları loglanmalı ve gerektiğinde uyarı üretilmelidir. |
| A10 – Mishandling of Exceptional Conditions | Beklenmeyen hatalar bilgi sızdırabilir veya güvenlik kontrollerini atlatabilir. Hatalar kontrollü yönetilmeli, kullanıcıya ayrıntılı sunucu bilgisi gösterilmemelidir. |

SQL Injection, kullanıcı girdisinin SQL sorgusuna güvenli olmayan biçimde eklenmesidir; parametreli sorgular bu riski azaltır. XSS, güvenilmeyen içeriğin tarayıcıda kod olarak çalışmasıdır; HTML çıktısı bağlamına uygun kodlanmalıdır. CSRF, tarayıcının kullanıcının oturum bilgilerini otomatik gönderdiği durumlarda, kullanıcı adına istenmeyen işlem yaptırılmasıdır; cookie tabanlı uygulamalarda antiforgery koruması kullanılabilir. Broken Auth ise giriş ve oturum yönetimindeki zayıflıkları ifade eder. Model doğrulama hatalı veriyi reddetmeye yardımcı olur, ancak tek başına bu güvenlik açıklarının hepsini önlemez.

## 7. Logging ve Hata Yönetimi

Loglama, uygulamada gerçekleşen olayları kaydetmektir. Bir isteğin neden başarısız olduğunu bulmak, olağandışı durumları izlemek ve sorunları gidermek için kullanılır. ASP.NET Core’da ILogger<T> ile log yazılabilir. Loglar konsola veya yapılandırılmış başka bir kayıt sistemine gönderilebilir. Parola ve token gibi gizli bilgiler loglara yazılmamalıdır.

| Log seviyesi | Ne zaman kullanılır? |
|---|---|
| Trace | Çok ayrıntılı işlem adımlarını incelemek için. |
| Debug | Geliştirme sırasında hata ayıklamak için. |
| Information | Normal bir işlemin gerçekleştiğini kaydetmek için. |
| Warning | İşlem devam etse de dikkat gerektiren bir durum için. |
| Error | Bir işlemin hata nedeniyle başarısız olması için. |
| Critical | Uygulamanın önemli bir bölümünün çalışamaması için. |

Global exception handling, uygulamanın farklı yerlerinde oluşan beklenmeyen hataları merkezi olarak yönetmektir. UseExceptionHandler() bu hataları yakalayan middleware’i ekler. AddProblemDetails() ise istemciye kontrollü bir hata yanıtı hazırlanmasına yardımcı olur:

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddProblemDetails();
var app = builder.Build();
pp.UseExceptionHandler();
app.MapGet("/products/{id:int}", (int id, ILogger<Program> logger) =>
{
  logger.LogInformation("Ürün istendi: {ProductId}", id);

 if (id != 42)
 {
    logger.LogWarning("Ürün bulunamadı: {ProductId}", id);
    return Results.NotFound();
 }

   return Results.Ok(new { Id = id, Name = "Kalem" });
});
app.Run();
```

Bu örnekte ILogger<Program> istenen ürünün ID’sini loglar. Ürün bulunamazsa 404 Not Found döner; bu beklenen bir sonuçtur, beklenmeyen bir sistem hatası değildir. Sonraki işlemlerde beklenmeyen bir exception oluşursa UseExceptionHandler() bunu merkezi olarak ele alır. Middleware bu işlemlerden önce eklendiği için hatayı yakalayabilir. Üretim ortamında kullanıcıya ayrıntılı hata mesajı veya stack trace gösterilmemelidir.

## 8. Yazılım Geliştirme Prensipleri

### SOLID

| İlke | Anlamı | Kısa örnek |
|---|---|---|
| S – Single Responsibility | Bir sınıf tek bir temel işten sorumlu olmalıdır. | InvoiceService fatura hesaplar; e-posta gönderimini EmailService yapar. |
| O – Open/Closed | Yeni özellik eklenirken mevcut kodu mümkün olduğunca değiştirmemek gerekir. | Yeni ödeme yöntemi, mevcut ödeme sınıflarını değiştirmek yerine IPaymentMethod arayüzünü uygular. |
| L – Liskov Substitution | Bir alt sınıf, yerine geçtiği üst sınıfın beklenen davranışını bozmamalıdır. | SaveAsync metodu başarı döndürüyorsa veriyi gerçekten kaydetmelidir. |
| I – Interface Segregation | Sınıflar kullanmadıkları metotları uygulamak zorunda kalmamalıdır. | Okuma ve yazma işlemleri için ayrı IReader ve IWriter arayüzleri tanımlanır. |
| D – Dependency Inversion | İş kuralları doğrudan belirli bir altyapı sınıfına bağlı olmamalıdır. | Sipariş servisi doğrudan SmtpEmailSender yerine IEmailSender arayüzünü kullanır. |

### Design Patterns

Singleton, uygulama boyunca tek bir nesne örneğinin kullanılmasını sağlar. Paylaşılan ve eşzamanlı kullanıma uygun servislerde işe yarayabilir; istek başına kullanılan DbContext için uygun değildir.

Repository, veritabanı erişim işlemlerini bir yerde toplar. Örneğin ProductRepository, ürün arama ve kaydetme işlemlerini yönetebilir. EF Core kullanırken her tabloya ayrıca Repository yazmak zorunlu değildir.

Factory, hangi nesnenin oluşturulacağına karar verir. Örneğin ödeme türü “kredi kartı” ise kart, “havale” ise havale işlemini yapan sınıfı oluşturabilir.

Clean Code, kodun amacının kolay anlaşılması ve güvenle değiştirilebilmesidir. Açıklayıcı isimler kullanmak, uzun metotları anlamlı parçalara ayırmak ve aynı kodu gereksiz yere tekrarlamamak buna yardımcı olur. Örneğin Check(id) yerine HasOverdueDebt(apartmentId) adı metodun ne yaptığını daha açık gösterir.

### Yazılım mimarileri

| Mimari | Ne zaman tercih edilebilir? |
|---|---|
| Layered | API, iş kuralları ve veri erişiminin ayrı tutulduğu basit veya orta ölçekli uygulamalarda. |
| Clean Architecture | İş kuralları karmaşıksa ve veritabanından bağımsız tutulmak isteniyorsa. |
| Microservices | Sistemin farklı bölümleri bağımsız geliştirme, dağıtım veya ölçekleme gerektiriyorsa. |
| Event-Driven | “Sipariş oluşturuldu” gibi olaylara farklı bileşenlerin tepki vermesi gerekiyorsa. |
| Hexagonal (Ports & Adapters) | Veritabanı veya dış servisler değişse bile temel iş kurallarının korunması isteniyorsa. |

Küçük bir projede anlaşılır bir katmanlı yapıyla başlamak yeterli olabilir. Proje büyüdükçe ihtiyaçlara göre daha belirgin mimari sınırlar oluşturulabilir.
