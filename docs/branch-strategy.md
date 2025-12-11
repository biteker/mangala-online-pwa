# Branch Strategy (GitHub Flow + Conventional Commits)

Bu doküman Mangala Online PWA projesinin tüm Git süreçlerini, branch yapısını, PR kurallarını ve merge stratejilerini tanımlar. Tüm geliştiriciler bu standartlara uymakla yükümlüdür.

---

# 1) GENEL PRENSİP

Proje **GitHub Flow** kullanır:

* `main` her zaman **deployable** olmalıdır.
* Tüm geliştirmeler **feature branch** üzerinden yapılır.
* Değişiklikler Pull Request (PR) üzerinden merge edilir.
* Merge yöntemi **Squash & Merge**’dür.
* `main` dalına **doğrudan push yasaktır**.

---

# 2) BRANCH İSİMLENDİRME

Format:

```
feature/<taskId>-<kısa-açıklama>
```

Örnekler:

```
feature/BE-06-rule-engine-distribute
feature/BE-10-turn-timer
feature/FE-03-game-board-ui
feature/DO-02-backend-dockerfile
```

Kısa açıklama **kebab-case** olmalıdır.

---

# 3) COMMIT MESAJ STANDARTI (Conventional Commits)

Commit mesajı formatı:

```
<type>: <açıklama>
```

## Türler:

* `feat:` Yeni özellik
* `fix:` Hata düzeltme
* `refactor:` Davranışı değiştirmeyen kod iyileştirmesi
* `chore:` Yapılandırma, script
* `docs:` Döküman değişiklikleri
* `test:` Test ekleme/düzenleme

## Örnek Commitler:

```
feat: add turn timer service
fix: incorrect capture rule calculation
refactor: optimize board rendering
chore: update docker-compose config
```

---

# 4) PULL REQUEST KURALLARI

Her PR **zorunlu olarak** şu bölümleri içerir:

### PR Template

```
## Summary
Bu PR ne yapıyor? Kısa açıklama.

## Related Task
- BE-06
- FE-03

## Changes
- Rule engine distribute.ts dosyası eklendi
- turn-timer entegre edildi

## Testing
- Manuel test yapıldı
- Unit test eklendi (varsa)

## Checklist
- [ ] Kod lint hatası içermiyor
- [ ] Unit testler geçiyor
- [ ] Mimariye uygun
- [ ] Gereksiz dosya yok
```

---

# 5) MERGE STRATEJİSİ

Tüm PR’lar şu yöntemle birleştirilir:

### ✔ **Squash & Merge**

* Tüm commitler tek commit haline gelir.
* `main` dalı temiz ve okunabilir kalır.
* Release notları oluşturmak kolaylaşır.

### ❌ Merge Commit **YASAK**

Neden? Commit geçmişi kirlenir.

### ❌ Rebase & Merge **YASAK**

Ekip çalışmasında karışıklık yaratır.

---

# 6) MAIN BRANCH KORUMA KURALLARI

GitHub’da şu ayarlar zorunludur:

* ✔ `main` dalına **direct push** kapalı
* ✔ PR reviews: **minimum 1 onay**
* ✔ Status checks: **testlerin geçmesi zorunlu**
* ✔ Squash & Merge tek seçenek olarak aktif
* ✔ PR açılmadan merge yapılamaz

---

# 7) RELEASE STRATEJİSİ

Henüz MVP aşamasında olduğumuz için release etiketi isteğe bağlıdır.
İleride kullanılacak format:

```
v1.0.0
v1.1.0
v2.0.0
```

Semver (Semantic Versioning) standartlarına uyulur.

---

# 8) ÖZET

Bu döküman tüm yazılım ekibi ve VSCode agent'ları için **tek geçerli Git standardıdır**.
Uygulanması zorunludur ve ihlal edilmesi durumunda PR geri çevrilebilir.
