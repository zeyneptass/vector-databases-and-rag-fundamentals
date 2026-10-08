# Vector Databases and RAG Fundamentals

Bu repo, **Pinecone** ve **Sentence Transformers** kullanarak vektör veritabanlarıyla uygulama geliştirmenin temellerini öğrendiğim eğitim serisinin notebook'larını içerir. Her notebook, farklı bir vektör veritabanı uygulamasını sıfırdan inşa eder.

## 📚 İçerik

| # | Notebook | Konu | Açıklama |
|---|----------|------|----------|
| 1 | `SemanticSearchDemo.ipynb` | Anlamsal Arama | Metin benzerliği araması ile temel semantik search |
| 2 | `SemanticSearchDemo2.ipynb` | Anlamsal Arama (v2) | Farklı veri seti ile semantik search denemesi |
| 3 | `RAG_Demo.ipynb` | RAG | Retrieval Augmented Generation ile LLM'e bağlam sağlama |
| 4 | `RecommenderSystems.ipynb` | Öneri Sistemleri | İçerik tabanlı öneri sistemi |
| 5 | `hybrid_search.ipynb` | Hibrit Arama | Sparse (BM25) + Dense (CLIP) vektörlerle hibrit arama |
| 6 | `anomaly_detection.ipynb` | Anomali Tespiti | Cisco ASA log dosyalarında anomali tespiti |
| 7 | *(yakında)* | Yüz Benzerliği | DeepFace ile yüz benzerliği araması |

## 🛠️ Kullanılan Teknolojiler

- **Python 3.10+**
- **Pinecone** — Vektör veritabanı (serverless)
- **Sentence Transformers** — Metin ve görsel gömmeleri (embedding)
- **OpenAI / DeepSeek API** — RAG için LLM
- **BM25** — Sparse vektörler (hibrit arama)
- **CLIP** — Görsel-metin gömmeleri
- **DeepFace** — Yüz tanıma ve yüz gömmeleri
- **Pandas, NumPy, Matplotlib** — Veri işleme ve görselleştirme
- **scikit-learn** — PCA ve t-SNE (boyut indirgeme)
- **Google Colab** — Geliştirme ortamı

## 🚀 Kurulum

Bu notebook'lar **Google Colab** üzerinde çalıştırılmak üzere tasarlanmıştır.

### 1. Repoyu klonlayın
```bash
git clone https://github.com/zeyneptass/vector-databases-and-rag-fundamentals.git
cd vector-databases-and-rag-fundamentals
```

### 2. Gerekli kütüphaneleri kurun

Her notebook'un ilk hücresinde gerekli `pip install` komutları bulunur. Örnek:

```python
!pip install -q pinecone sentence-transformers openai tqdm pandas
```

### 3. API anahtarlarını ayarlayın

Colab kullanıyorsanız **Secrets** (🔑) bölümüne şu anahtarları ekleyin:

| Secret Adı | Nereden Alınır |
|------------|----------------|
| `PINECONE_API_KEY` | Pinecone Console |
| `OPENAI_API_KEY` | OpenAI Platform |
| `DEEPSEEK_API_KEY` | DeepSeek Platform |

Ardından notebook'ta şu şekilde çağırın:

```python
from google.colab import userdata

PINECONE_API_KEY = userdata.get('PINECONE_API_KEY')
```

## 📖 Her Notebook Ne Yapar?

### 1. Semantic Search
Metinleri vektörlere çevirip Pinecone'a yükler. Bir sorgu verildiğinde, anlamca en yakın metinleri getirir. Klasik anahtar kelime aramasından farklı olarak eşanlamlıları da yakalar.

**Akış:**
1. Quora/AG News veri setini yükle
2. Sentence Transformer ile gömmeleri üret
3. Pinecone'a yükle
4. Sorgu yap ve en benzer sonuçları getir

---

### 2. RAG (Retrieval Augmented Generation)
Pinecone'dan ilgili belgeleri çeker, bunları bir prompt içine yerleştirir ve OpenAI/DeepSeek gibi bir LLM'e gönderir. LLM, bu bağlama dayanarak özetlenmiş, okunabilir bir yanıt üretir.

**Akış:**
1. Wikipedia makalelerini Pinecone'a yükle
2. Kullanıcı sorusunu vektöre çevir
3. Pinecone'dan ilgili belgeleri getir
4. Belgeleri prompt içine yerleştir
5. LLM'den yanıt al

---

### 3. Recommender Systems
Haber makaleleri veya ürün açıklamaları üzerinden içerik tabanlı öneri sistemi kurar. Bir sorguya en benzer öğeleri önerir.

**Akış:**
1. Haber makalelerini yükle
2. Başlık veya içerik bazlı gömmeler üret
3. Pinecone'a yükle
4. "Obama" gibi bir sorguyla en benzer makaleleri öner

---

### 4. Hybrid Search
Aynı anda sparse (BM25) ve dense (CLIP) vektörleri kullanır. `alpha` parametresi ile ikisinin ağırlığını ayarlayarak hibrit arama yapar. Moda ürünleri üzerinde gösterilmiştir.

**Akış:**
1. Moda ürün veri setini yükle
2. BM25 ile sparse, CLIP ile dense gömmeler üret
3. İkisini aynı satırda Pinecone'a yükle
4. `alpha` parametresiyle hibrit arama yap

**Alpha Parametresi:**
- `alpha = 0` → Tamamen sparse (kelime eşleşmesi)
- `alpha = 1` → Tamamen dense (anlamsal)
- `alpha = 0.5` → Dengeli

---

### 5. Anomaly Detection
Cisco ASA log dosyalarında normal logları öğrenip, bunlara benzemeyen anormal logları tespit eder. Denetimli öğrenme ile küçük bir model eğitilir.

**Akış:**
1. Etiketli log verisiyle küçük bir model eğit
2. Örnek log dosyasını gömmele
3. Pinecone'a yükle
4. İyi bir logu referans alıp en düşük skorlu sonucu bul (anomali)
