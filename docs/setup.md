# GitHub Profil Metrics Kurulumu

Bu doküman, GitHub profil README depon için README’yi ve otomatik üretilen SVG metrics dosyalar?n? nas?l yay?nlayaca??n? anlat?r.

## 1) Profil deponu olu?tur

1. GitHub’da, **kullan?c? ad?nla birebir ayn?** isimde **public** bir depo olu?tur (örnek: `eyupcanbilgin`).
2. Depoyu lokaline clone’la ve ?u dosyalar? depoya ekle:
   - `README.md`
   - `.github/workflows/metrics.yml`
   - `assets/banner.svg`
   - `assets/qa-terminal.svg`

## 2) Personal Access Token (PAT) olu?tur (Classic gerekli)

Bu token sadece metrics verisi toplamak için olmal?. Asla ana ?ifreni kullanma ve token’? hiçbir zaman repoya commit etme.

Önemli: `lowlighter/metrics` GitHub GraphQL API kullan?r. GitHub ?u an fine-grained PAT ile GraphQL do?rulamay? desteklemedi?i için workflow hata verir. Bu yüzden **classic token** olu?turmal?s?n.

Classic PAT olu?turma:
1. `https://github.com/settings/tokens` sayfas?na git.
2. **Tokens (classic)** ? **Generate new token (classic)** seç.
3. Expiration belirle (ör. 30/60/90 gün).

Önerilen minimum kapsam (scopes):
- Public profil metri?i için genelde: **`public_repo`**
- Private repo aktiviteleri / private contribution görünürlü?ü gerekiyorsa: **`repo`**
- Organizasyon verileri gerekiyorsa (baz? plugin’ler): **`read:org`**

Not:
- En az yetkiyle ba?la; workflow log’u “permission denied” diyorsa sadece gereken scope’u ekle.
- Token’? olu?turduktan sonra **bir kez** gösterilir; güvenli bir yere kaydet.

## 3) `METRICS_TOKEN` secret’?n? ekle

1. GitHub’da deponu aç.
2. **Settings ? Secrets and variables ? Actions** yoluna git.
3. Yeni bir repository secret ekle:
   - **Name:** `METRICS_TOKEN`
   - **Value:** 2. ad?mda olu?turdu?un classic PAT

Güvenlik notlar?:
- Token’? **asla** `README.md` içine yazma.
- Token’? workflow dosyalar?nda **asla** hardcode etme.
- Token s?zarsa hemen **rotate/revoke** et.

## 4) GitHub Actions’? etkinle?tir

1. Profil deponun **Actions** sekmesine git.
2. Bu repo için workflow’lar?n çal??mas?na izin verildi?ini kontrol et.
3. Varsay?lan branch’inde `.github/workflows/metrics.yml` dosyas?n?n bulundu?unu do?rula.

## 5) Metrics workflow’unu manuel çal??t?r

1. **Actions ? Metrics** sayfas?n? aç.
2. **Run workflow** butonuna t?kla.
3. Varsay?lan branch’ini seçip çal??t?r.

Workflow ayr?ca ?u durumlarda otomatik çal??acak ?ekilde ayarl?:
- her gün (cron schedule),
- `main` veya `master` branch’ine `push` oldu?unda.

## 6) Üretilen dosyalar? do?rula

Workflow tamamland?ktan sonra, repo kök dizininde ?u dosyalar?n olu?tu?unu kontrol et:

- `metrics.profile.svg`
- `metrics.languages.svg`
- `metrics.calendar.svg`
- `metrics.activity.svg`
- `metrics.repositories.svg`

Dosyalar olu?mad?ysa:
- workflow loglar?n? kontrol et,
- `METRICS_TOKEN` geçerlili?ini ve kapsamlar?n? (scopes/permissions) do?rula,
- workflow’u tekrar çal??t?r.

## 7) Profil sayfas?nda görünümü kontrol et

1. GitHub profil sayfan? aç (`https://github.com/<kullanici-adin>`).
2. Banner, “command center” terminali ve tüm metrics görsellerinin do?ru render oldu?unu do?rula.
3. Hem desktop hem mobil görünümde okunabilirli?i kontrol et.

## 8) (Opsiyonel) Private contribution görünürlü?ü

Private contribution’lar?n profil contribution graph’?nda görünmesini istiyorsan:

1. GitHub profil ayarlar?n? aç.
2. **Include private contributions on my profile** seçene?ini etkinle?tir.
3. Token izinlerinin private veri eri?im ihtiyac?yla uyumlu oldu?undan emin ol.
