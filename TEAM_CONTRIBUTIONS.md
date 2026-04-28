# 👥 Ekip Görev Dağılımı - AI Chatbot Projesi

## 📁 Mevcut Dosyalar ve Sorumluluklar

---

## 1️⃣ **Product Manager**
**Rol:** Proje yönetimi, gereksinim analizi, dokümantasyon koordinasyonu

### Sorumlu Olduğu Dosyalar:

#### `API_DOCUMENTATION.md` (40% - İş gereksinimleri)
- Proje özeti ve hedefler
- Kullanım senaryoları
- API endpoint spesifikasyonları (iş mantığı)
- Metadata formatı tanımı
- Test senaryoları

#### `README.md` (50% - Proje yönetimi)
- Proje özeti ve özellikler
- Hızlı başlangıç kılavuzu
- Kullanım senaryoları
- Roadmap ve versiyon bilgisi

#### `test_new_format.ps1` (20% - Test senaryoları)
- Test senaryolarının belirlenmesi
- Beklenen sonuçların tanımlanması

**Toplam Katkı:** ~3 dosya (koordinasyon ve planlama)

---

## 2️⃣ **Backend Developer**
**Rol:** API geliştirme, veritabanı tasarımı, iş mantığı

### Sorumlu Olduğu Dosyalar:

#### `main/main_receiver.py` (100% - Core backend)
- FastAPI sunucu implementasyonu
- `/agent_config` endpoint
- `/chat` endpoint (Gemini API entegrasyonu)
- `/persona` endpoint (legacy support)
- Database schema ve init
- CORS middleware
- Error handling
- UTF-8 encoding
- Token tracking
- Topic detection logic
- Prohibited topics filtering

#### `port-yönetimi/local_api_server.py` (100% - Proxy server)
- JSON routing proxy
- Dynamic endpoint forwarding
- Request/response handling

#### `main/.env` (50% - API configuration)
- Gemini API key setup
- Environment variables

#### `personas.db` (100% - Database)
- SQLite database schema
- agent_configurations table
- personas table
- chat_history table

#### `API_DOCUMENTATION.md` (30% - Teknik detaylar)
- API endpoint technical specs
- Database schema documentation
- Error codes
- Request/response formats

**Toplam Katkı:** ~5 dosya (core development)

---

## 3️⃣ **DevOps Engineer**
**Rol:** Deployment, environment setup, CI/CD, maintenance

### Sorumlu Olduğu Dosyalar:

#### `requirements.txt` (100% - Dependency management)
```
fastapi==0.109.0
uvicorn[standard]==0.27.0
google-generativeai==0.3.2
python-dotenv==1.0.0
requests==2.31.0
```

#### `main/.env` (50% - Environment setup)
- Environment variable template
- Configuration management

#### `.venv/` (100% - Virtual environment)
- Python virtual environment setup
- Package installation

#### `cleanup.ps1` (100% - Maintenance)
- Gereksiz dosya temizleme
- Project cleanup automation

#### `API_DOCUMENTATION.md` (10% - Deployment section)
- Kurulum adımları
- Server configuration
- Port management

**Toplam Katkı:** ~4 dosya (infrastructure)

---

## 4️⃣ **Frontend Developer**
**Rol:** Web arayüzü geliştirme, client-side logic

### Sorumlu Olduğu Dosyalar:

#### `web-ui/chatbot.html` (60% - JavaScript & HTML)
- HTML structure
- JavaScript logic:
  - API fetch calls
  - Chat history management
  - Message rendering
  - Event handlers
  - State management
  - Error handling
  - Typing indicator logic
- API integration
- DOM manipulation

#### `web-ui/README.md` (30% - Technical usage)
- Web UI kullanım kılavuzu (teknik kısım)
- API URL configuration
- JavaScript customization

**Toplam Katkı:** ~2 dosya (frontend development)

---

## 5️⃣ **QA Engineer**
**Rol:** Test stratejisi, otomatik testler, kalite güvence

### Sorumlu Olduğu Dosyalar:

#### `test_new_format.ps1` (80% - Test implementation)
- Automated test script
- Test assertions
- Response validation
- Metadata checks
- Error handling tests

#### `test_system.ps1` (100% - Legacy format tests)
- Backward compatibility tests
- Persona format validation

#### `API_DOCUMENTATION.md` (20% - Testing section)
- Test prosedürleri
- Test senaryoları
- Expected results
- Bug tracking

**Toplam Katkı:** ~3 dosya (quality assurance)

---

## 6️⃣ **UI Designer + UX Writer**
**Rol:** Arayüz tasarımı, kullanıcı deneyimi, metin yazımı

### Sorumlu Olduğu Dosyalar:

#### `web-ui/chatbot.html` (40% - CSS & UX)
**UI Designer (25%):**
- CSS styling:
  - Color scheme (gradient purple)
  - Layout design
  - Typography (Segoe UI)
  - Animations (fadeIn, typing)
  - Message bubbles
  - Responsive design
  - Metadata badges
  - Button styles

**UX Writer (15%):**
- User-facing text:
  - "Size nasıl yardımcı olabilirim?"
  - "Mesajınızı yazın..."
  - Error messages
  - Placeholder text
  - Header copy

#### `web-ui/README.md` (70% - User guide)
- Kullanım kılavuzu
- Özellikler açıklaması
- Ekran görüntüsü açıklamaları
- User-friendly documentation

#### `README.md` (50% - User-facing content)
- Proje açıklaması (user-friendly)
- Özellikler listesi
- Hızlı başlangıç (kullanıcı perspektifi)
- Ekran görüntüsü

**Toplam Katkı:** ~3 dosya (design & content)

---

## 📊 Dosya Bazında Detaylı Sahiplik

### `main/main_receiver.py`
```
Backend Developer: 100%
```

### `port-yönetimi/local_api_server.py`
```
Backend Developer: 100%
```

### `web-ui/chatbot.html`
```
Frontend Developer: 60% (HTML + JavaScript)
UI Designer: 25% (CSS + Animations)
UX Writer: 15% (Copy + Messages)
```

### `requirements.txt`
```
DevOps: 100%
```

### `API_DOCUMENTATION.md`
```
Product Manager: 40% (Business requirements)
Backend Developer: 30% (Technical specs)
QA Engineer: 20% (Testing section)
DevOps: 10% (Deployment section)
```

### `README.md`
```
Product Manager: 50% (Overview, features)
UX Writer: 50% (User-facing content)
```

### `web-ui/README.md`
```
UX Writer: 70% (User guide)
Frontend Developer: 30% (Technical details)
```

### `test_new_format.ps1`
```
QA Engineer: 80% (Test implementation)
Product Manager: 20% (Test scenarios)
```

### `test_system.ps1`
```
QA Engineer: 100%
```

### `cleanup.ps1`
```
DevOps: 100%
```

### `main/.env`
```
DevOps: 50% (Environment setup)
Backend Developer: 50% (API configuration)
```

### `personas.db`
```
Backend Developer: 100%
```

---

## 📈 Katkı Yüzdesi (Dosya Sayısı Bazında)

| Rol | Dosya Sayısı | Ana Sorumluluk |
|-----|--------------|----------------|
| **Backend Developer** | 5 dosya | Core development |
| **DevOps** | 4 dosya | Infrastructure |
| **QA Engineer** | 3 dosya | Testing |
| **Product Manager** | 3 dosya | Planning & docs |
| **UI/UX (Designer + Writer)** | 3 dosya | Design & content |
| **Frontend Developer** | 2 dosya | Web interface |

---

## 🔄 Sprint Workflow (Gerçekçi Senaryo)

### Sprint 1: Foundation
- **Backend Developer**: `main_receiver.py`, `local_api_server.py`, database
- **DevOps**: `requirements.txt`, `.env`, virtual environment

### Sprint 2: Integration
- **Backend Developer**: Gemini API integration, endpoints
- **Frontend Developer**: `chatbot.html` (HTML + JS)

### Sprint 3: Design & Testing
- **UI Designer**: `chatbot.html` (CSS)
- **UX Writer**: Copy, messages
- **QA Engineer**: `test_new_format.ps1`, `test_system.ps1`

### Sprint 4: Documentation & Deployment
- **Product Manager**: `README.md`, `API_DOCUMENTATION.md`
- **UX Writer**: `web-ui/README.md`
- **DevOps**: `cleanup.ps1`, deployment

---

## 🎯 Gerçek Dünya Senaryosu

Bu dağılım gerçek bir startup/şirket ortamını yansıtıyor:

1. **Backend Developer** - En fazla kod yazan kişi (core logic)
2. **DevOps** - Infrastructure ve deployment
3. **QA Engineer** - Test automation
4. **Frontend Developer** - Web UI implementation
5. **Product Manager** - Koordinasyon ve dokümantasyon
6. **UI/UX** - Design ve kullanıcı deneyimi

Her kişi kendi uzmanlık alanında çalışmış ve birlikte entegre bir sistem oluşturmuş! 🚀
