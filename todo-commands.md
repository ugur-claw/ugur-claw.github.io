# Todo Sistemi

## Komutlar (bana mesaj atarak kullan)

### Ekle
- `"ekle: [başlık]"` - basit ekleme
- `"ekle: [başlık] | priority: urgent/high/medium/low"` - öncelikli ekleme
- `"ekle: [başlık] | deadline: [tarih]"` - deadline ile ekleme
- `"ekle: [başlık] | tags: [tag1,tag2]"` - etiketli ekleme

### Listele
- `"liste"` - tüm görevler
- `"liste urgent"` - sadece urgent olanlar
- `"liste bugün"` - bugün bitmesi gerekenler

### Güncelle
- `"yaptım [id]"` veya `"done [id]"` - tamamlandı işaretle
- `"sil [id]"` - görevi sil
- `"priority [id] urgent"` - öncelik güncelle

### Örnekler
```
ekle: Login sayfası bug fix | priority: urgent | deadline: 2026-03-21
ekle: Blog post taslağı yaz | tags: içerik
liste
yaptım 1
```

## Cron Haberleri
- **Sabah (09:00):** Günün görevleri + overdue check
- **Akşam (18:00):** Kalan görevler + tamamlanmayan hatırlatma
