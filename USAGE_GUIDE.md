# 🚀 Sistem Kullanım Kılavuzu - Dış Ekipler İçin

## 📡 Port ve Endpoint Bilgileri

Sisteminiz **2 farklı port** üzerinden çalışıyor:

- **Port 8000**: Proxy Sunucu (Önerilen - Dashboard ve Chat Core için)
- **Port 9000**: Ana Sunucu (Direkt erişim - Test ve özel durumlar için)

---

## 👥 Kullanıcı Grupları ve Kullanım Senaryoları

### 1️⃣ **Dashboard Ekibi** (Agent Konfigürasyonu Yönetimi)

**Amaç:** Yeni agent'lar oluşturmak veya mevcut agent'ları güncellemek

#### 📍 Endpoint:
```
POST http://localhost:8000/send_json
```

#### 📤 Gönderilecek JSON:
```json
{
  "endpoint": "/agent_config",
  "agentId": "agent_8823_xyz",
  "persona_title": "Premium Müşteri Temsilcisi",
  "model_instructions": {
    "tone": "Resmi, Saygılı ve Çözüm Odaklı",
    "rules": [
      "Cevaplar 4 cümleyi geçmemelidir.",
      "Fiyatlar hakkında savunmacı değil, değer odaklı konuşulmalıdır.",
      "Tüm cevaplar 'Saygılarımla.' ile bitmelidir."
    ],
    "prohibited_topics": [
      "Rakiplerin fiyatları",
      "Siyasi görüşler"
    ]
  },
  "initial_context": {
    "company_slogan": "Kalite Asla Tesadüf Değildir.",
    "pricing_rationale": "Yüksek fiyatlandırmamız, birinci sınıf malzemeler ve kapsamlı garanti hizmetlerimizle ilişkilidir."
  }
}
```

#### 📥 Alacağınız Yanıt:
```json
{
  "status": "success",
  "main_status_code": 200,
  "main_response": {
    "status": "success",
    "agent_id": "agent_8823_xyz",
    "message": "Konfigürasyon kaydedildi"
  }
}
```

#### 💡 Önemli Notlar:
- `agentId` **zorunlu** - Unique olmalı
- `endpoint` her zaman `/agent_config` olmalı
- Aynı `agentId` ile tekrar gönderirseniz **güncelleme** yapar (UPSERT)

---

### 2️⃣ **Chat Core Ekibi** (Sohbet Mesajları)

**Amaç:** Kullanıcı mesajlarını AI'ya göndermek ve yanıt almak

#### 📍 Endpoint:
```
POST http://localhost:8000/send_json
```

#### 📤 Gönderilecek JSON (İlk Mesaj):
```json
{
  "endpoint": "/chat",
  "agent_id": "agent_8823_xyz",
  "session_id": "sess_user_999",
  "user_message": "Fiyatlarınız neden bu kadar yüksek?",
  "chat_history": []
}
```

#### 📤 Gönderilecek JSON (Geçmişli Mesaj):
```json
{
  "endpoint": "/chat",
  "agent_id": "agent_8823_xyz",
  "session_id": "sess_user_999",
  "user_message": "Garanti süresini uzatabilir misiniz?",
  "chat_history": [
    {
      "role": "user",
      "content": "Fiyatlarınız neden bu kadar yüksek?"
    },
    {
      "role": "assistant",
      "content": "Değerli müşterimiz, fiyatlandırmamız kalite standartlarımıza göre belirlenmiştir..."
    }
  ]
}
```

#### 📥 Alacağınız Yanıt:
```json
{
  "status": "success",
  "main_status_code": 200,
  "main_response": {
    "status": "success",
    "reply": "Değerli müşterimiz, fiyatlandırmamız kalite standartlarımıza göre belirlenmiştir. Birinci sınıf malzemeler ve kapsamlı garanti hizmetlerimiz bu fiyatlandırmanın temelini oluşturur. Size uzun vadeli memnuniyet sunmayı hedefliyoruz. Saygılarımla.",
    "metadata": {
      "topic_detected": "fiyat_itirazi",
      "tokens_used": 850,
      "blocked": false,
      "agent_id": "agent_8823_xyz",
      "session_id": "sess_user_999"
    }
  }
}
```

#### 💡 Önemli Notlar:
- `agent_id` **zorunlu** - Daha önce Dashboard'dan kaydedilmiş olmalı
- `session_id` **zorunlu** - Her kullanıcı için unique
- `user_message` **zorunlu** - Kullanıcının mesajı
- `chat_history` **opsiyonel** - Boş array gönderebilirsiniz
- `endpoint` her zaman `/chat` olmalı

---

### 3️⃣ **Test/Demo Kullanıcıları** (Direkt Erişim)

**Amaç:** Sistemi test etmek veya demo yapmak

#### Seçenek 1: Web Arayüzü (Önerilen)
```
Tarayıcıda aç: web-ui/chatbot.html
```
- Otomatik olarak Port 9000'e bağlanır
- Görsel arayüz ile test
- Metadata görüntüleme

#### Seçenek 2: Direkt API Çağrısı
```
POST http://localhost:9000/chat
```

**JSON:**
```json
{
  "agent_id": "agent_8823_xyz",
  "session_id": "test_session_001",
  "user_message": "Merhaba!",
  "chat_history": []
}
```

---

## 🔄 Veri Akışı Diyagramı

### Dashboard → Sistem
```
Dashboard Ekibi
    ↓
POST http://localhost:8000/send_json
    ↓
{
  "endpoint": "/agent_config",
  "agentId": "...",
  "persona_title": "...",
  ...
}
    ↓
Proxy Sunucu (Port 8000)
    ↓
Ana Sunucu (Port 9000) /agent_config
    ↓
Database (SQLite)
    ↓
Yanıt: {"status": "success", "agent_id": "..."}
```

### Chat Core → Sistem → AI
```
Chat Core Ekibi
    ↓
POST http://localhost:8000/send_json
    ↓
{
  "endpoint": "/chat",
  "agent_id": "...",
  "user_message": "...",
  "chat_history": [...]
}
    ↓
Proxy Sunucu (Port 8000)
    ↓
Ana Sunucu (Port 9000) /chat
    ↓
Database (Agent config çek)
    ↓
Google Gemini API
    ↓
Yanıt: {"reply": "...", "metadata": {...}}
```

---

## 📋 Alan Açıklamaları

### Agent Config İçin:

| Alan | Tip | Zorunlu | Açıklama |
|------|-----|---------|----------|
| `endpoint` | string | ✅ | Her zaman `/agent_config` |
| `agentId` | string | ✅ | Unique agent ID |
| `persona_title` | string | ✅ | Agent'ın görünen adı |
| `model_instructions.tone` | string | ✅ | Konuşma tonu |
| `model_instructions.rules` | array | ✅ | Kurallar listesi |
| `model_instructions.prohibited_topics` | array | ❌ | Yasaklı konular |
| `initial_context` | object | ❌ | Başlangıç bağlamı |

### Chat İçin:

| Alan | Tip | Zorunlu | Açıklama |
|------|-----|---------|----------|
| `endpoint` | string | ✅ | Her zaman `/chat` |
| `agent_id` | string | ✅ | Kayıtlı agent ID |
| `session_id` | string | ✅ | Kullanıcı session ID |
| `user_message` | string | ✅ | Kullanıcı mesajı |
| `chat_history` | array | ❌ | Önceki mesajlar (boş olabilir) |

### Metadata Yanıtı:

| Alan | Tip | Açıklama |
|------|-----|----------|
| `topic_detected` | string | Tespit edilen konu (fiyat_itirazi, garanti_sorgusu, urun_bilgisi, genel) |
| `tokens_used` | integer | Kullanılan token sayısı (maliyet takibi) |
| `blocked` | boolean | Yasaklı konu tespit edildiyse `true` |
| `agent_id` | string | Kullanılan agent ID |
| `session_id` | string | Session ID |

---

## 🧪 Test Örnekleri

### PowerShell ile Test (Dashboard):
```powershell
$config = @{
    endpoint = "/agent_config"
    agentId = "test_agent_001"
    persona_title = "Test Asistan"
    model_instructions = @{
        tone = "Arkadaş canlısı"
        rules = @("Kısa cevaplar ver")
        prohibited_topics = @()
    }
} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Uri "http://localhost:8000/send_json" -Method Post -Body $config -ContentType "application/json"
```

### PowerShell ile Test (Chat):
```powershell
$chat = @{
    endpoint = "/chat"
    agent_id = "test_agent_001"
    session_id = "test_session"
    user_message = "Merhaba!"
    chat_history = @()
} | ConvertTo-Json -Depth 3

Invoke-RestMethod -Uri "http://localhost:8000/send_json" -Method Post -Body $chat -ContentType "application/json"
```

### cURL ile Test (Linux/Mac):
```bash
# Agent Config
curl -X POST http://localhost:8000/send_json \
  -H "Content-Type: application/json" \
  -d '{
    "endpoint": "/agent_config",
    "agentId": "test_agent_001",
    "persona_title": "Test Asistan",
    "model_instructions": {
      "tone": "Arkadaş canlısı",
      "rules": ["Kısa cevaplar ver"]
    }
  }'

# Chat
curl -X POST http://localhost:8000/send_json \
  -H "Content-Type: application/json" \
  -d '{
    "endpoint": "/chat",
    "agent_id": "test_agent_001",
    "session_id": "test_session",
    "user_message": "Merhaba!",
    "chat_history": []
  }'
```

---

## ❌ Hata Durumları

### 1. Agent Bulunamadı
```json
{
  "status": "error",
  "detail": "Agent bulunamadı: agent_xyz"
}
```
**Çözüm:** Önce Dashboard'dan agent'ı kaydedin.

### 2. Eksik Alan
```json
{
  "status": "error",
  "detail": "agent_id ve user_message zorunlu"
}
```
**Çözüm:** Zorunlu alanları kontrol edin.

### 3. API Anahtarı Hatası
```json
{
  "status": "error",
  "detail": "GEMINI_API_KEY .env'de yok"
}
```
**Çözüm:** Sistem yöneticisine bildirin.

### 4. Yasaklı Konu
```json
{
  "status": "success",
  "reply": "Üzgünüm, bu konu hakkında bilgi veremiyorum. Başka nasıl yardımcı olabilirim?",
  "metadata": {
    "blocked": true,
    ...
  }
}
```
**Durum:** Normal - Yasaklı konu tespit edildi.

---

## 📞 Destek

### Sık Sorulan Sorular:

**S: Hangi portu kullanmalıyım?**  
C: Dashboard ve Chat Core için **Port 8000** (proxy) kullanın. Test için Port 9000 kullanabilirsiniz.

**S: Chat history nasıl yönetilmeli?**  
C: Her mesaj sonrası aldığınız yanıtı history'ye ekleyin. Son 10 mesajı göndermek yeterli.

**S: Agent ID'yi nereden alacağım?**  
C: Dashboard ekibi agent oluştururken `agentId` belirler. Aynı ID'yi chat'te kullanın.

**S: Session ID unique olmalı mı?**  
C: Evet, her kullanıcı için farklı bir session ID kullanın.

**S: Metadata ne işe yarar?**  
C: Analytics, maliyet takibi ve konu bazlı raporlama için kullanabilirsiniz.

---

## 🎯 Özet Tablo

| Ekip | Port | Endpoint | Amaç |
|------|------|----------|------|
| **Dashboard** | 8000 | `/send_json` → `/agent_config` | Agent oluştur/güncelle |
| **Chat Core** | 8000 | `/send_json` → `/chat` | Mesaj gönder, yanıt al |
| **Test/Demo** | 9000 | `/chat` (direkt) | Test ve demo |
| **Web UI** | 9000 | `/chat` (otomatik) | Görsel test |

---

**Sistem Hazır! Kullanmaya başlayabilirsiniz!** 🚀
