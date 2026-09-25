# WordPress + MySQL — Helm & Jenkins

A WordPress + MySQL stack packaged as a self-written Helm chart, with
a Jenkins pipeline that lints and validates the chart on every push.

## What I built

- Wrote a Helm chart from scratch (`Chart.yaml`, `values.yaml`,
  templates) for WordPress and MySQL
- Parameterized image names, replica count, and database credentials
  through `values.yaml` instead of hardcoding them in templates
- Connected WordPress to MySQL purely through the Service DNS name
  (`mysql:3306`), no hardcoded IPs
- Built a Jenkins pipeline that runs `helm lint` and `helm template`
  on every push to catch chart errors before deployment

## Debugging highlights

- **`helm install` created nothing** — the `templates/` folder had
  been accidentally deleted along with the boilerplate files; fixed
  by recreating the folder and re-adding the manifests
- **`helm lint` failed with "Chart.yaml not found"** — was running
  the command from inside the `templates/` folder instead of the
  project root; Helm commands must reference the chart folder from
  one level above it
- **Jenkins `helm: not found`** — the Jenkins container didn't have
  Helm installed; installed it directly into the container
- **`helm install --dry-run` failing on cluster auth** — even
  client-side dry runs try to reach a Kubernetes API; simplified the
  pipeline to rely on `lint` + `template` instead, which validate the
  chart without needing cluster access

## Tech stack

Helm · Kubernetes · Jenkins · Docker


# WordPress + MySQL — Helm & Jenkins

WordPress + MySQL yığınının, sıfırdan yazılmış bir Helm chart olarak 
paketlenmesi; her push'ta chart'ı doğrulayan bir Jenkins pipeline'ı 
ile birlikte.

## Neler yaptım

- WordPress ve MySQL için, sıfırdan bir Helm chart yazdım (`Chart.yaml`, 
  `values.yaml`, template'ler)
- Image isimlerini, replica sayısını ve veritabanı kimlik bilgilerini, 
  template'lere sabit yazmak yerine `values.yaml` üzerinden 
  parametrelendirdim
- WordPress'i MySQL'e, tamamen Service DNS ismi (`mysql:3306`) 
  üzerinden bağladım, hiç sabit IP kullanmadım
- Her push'ta `helm lint` ve `helm template` çalıştıran bir Jenkins 
  pipeline'ı kurdum, deploy'dan önce chart hatalarını yakalamak için

## Karşılaştığım ve çözdüğüm gerçek sorunlar

- **`helm install` hiçbir şey oluşturmadı** — `templates/` klasörü, 
  boilerplate dosyalarla birlikte yanlışlıkla silinmişti; klasörü 
  yeniden oluşturup manifestleri tekrar ekleyerek çözdüm
- **`helm lint`, "Chart.yaml bulunamadı" hatası verdi** — komutu, 
  proje kökü yerine `templates/` klasörünün içinden çalıştırıyordum; 
  Helm komutları, chart klasörünü, bir üst dizinden referans almalı
- **Jenkins'te `helm: not found`** — Jenkins container'ında Helm 
  kurulu değildi; doğrudan container'ın içine kurdum
- **`helm install --dry-run`, cluster kimlik doğrulamasında 
  başarısız oldu** — client-side dry-run bile, bir Kubernetes API'sine 
  ulaşmaya çalışıyor; pipeline'ı, cluster erişimi gerektirmeyen 
  `lint` + `template` kombinasyonuna göre sadeleştirdim

## Kullanılan teknolojiler

Helm · Kubernetes · Jenkins · Docker
