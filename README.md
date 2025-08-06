# BTK HACKATHON 2025
### Tema : E-Ticaret
### Gemini ile güçlendirilmiş E-ticaret web sitesi

### Projede bulunan ai sistemleri :
1. Ürün sayfasında ürün hakkında soru cevap bölümü ai ile desteklenmiştir.
2. Akıllı semantic arama sistemi
3. Kombin öneri sistemi

# Akıllı Semantic Arama Sistemi

## Ürün Amacı
 Kullanıcıların serbest metinle ürünleri ararken sadece ilgili sonuçları döndüren **anlamsal (semantic) arama** altyapısı sağlamayı amaçlar. Geleneksel anahtar kelime eşleştirme yerine ürün açıklamaları, renk, kategori ve fiyat bilgilerini birleştirip vektör gömmelerine dönüştürerek, kullanıcının niyetine göre en alakalı ürünleri öne çıkarır.
## Ürünün Faydaları
- **Daha Yüksek Alaka (Relevance)**  
  Kullanıcının tam olarak yazdığı kelimeye değil, metnin genel anlamına odaklanır.(Bağlam bazlı arama yapsada %100 doğruluk sunmaz)
## Özellikler
1. **Metin Birleştirme (SearchableText)**  
   - Modeldeki `Name`, `Colour`, `Category`, `Price` ve `Description` sahaları tek bir metin olarak birleştirilir.Bu sayede UI ile etkileşime girmeden istenilen etiketler bağlamında arama yapılası sağlanır aynı zamanda.(Ürün özelliklerinin girileceği `description` kısmının arama kısmına etki edeceği için satıcıların ürün özellikleri kısmına daha çok özen göstermesine teşvik edeceği düşünmekteyiz)

2. **Embedding Service**  
   - `/Services/EmbeddingService.cs` dosyasında Gemini REST API (`embedContent`) kullanılarak tüm metinler vektöre dönüştürülür.  
   - Model adı ve API anahtarı `appsettings.json` üzerinden yönetilir.

3. **Vektör Veritabanı Entegrasyonu**  
   - `/Services/IndexingService.cs` ile SQL’den çekilen tüm ürünler Pinecone’a `Vector { Id, Values, Metadata }` formatında toplu olarak upsert edilir.  
   - Metadata: `category`, `price`, gerekirse `colour`, `name` gibi alanlar da kaydedilir.(daha iyi sonuçlar almak ve filtreleme için kullandık)

4. **Semantic Search API**  
   - `/Controllers/SearchController.cs` dosyasındaki `GET /api/search?query=...` endpoint’i:  
     1. Sorguyu embedding’e çevirir  
     2. Pinecone’da nearest-neighbor + metadata filtre uygular  (topk = 10)
     3. Dönen ID’leri SQL’den getirir, sıralar ve JSON olarak döner 

---


# Ürün Soru-Cevap Asistanı

- Bir kullanıcı ürün detay sayfasını ziyaret ettiğinde, ürün hakkında soru sorabilir. Soru, ürün bilgisiyle birlikte Gemini'ye gönderilir ve gelen cevap kullanıcıya gösterilir.

# AI Destekli Kombin Öneri Sistemi

- Bu kısımda, bir e-ticaret platformu için geliştirilen AI tabanlı kombin öneri sistemidir. Kullanıcılar, kıyafet görselleri yükleyerek kendi stillerine uygun ürün kombinleri alabilirler. Sistem, Google Gemini API ve Pinecone vektör veritabanı ile çalışmaktadır.


## Kullanılan Teknolojiler
- **.NET 8 & ASP.NET Core**  
- **Entity Framework Core**  
- **Google Generative Language API (Gemini)** 
- **Pinecone Vector DB**
- **Javascript**


Video link : https://www.youtube.com/watch?v=RjMfzGE1ipI
