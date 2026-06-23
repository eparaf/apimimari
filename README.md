# Kaza & Tazminat Platformu — API Sözleşmesi

Üç istemcinin (Mobil, Web, Admin) tamamına hizmet veren tek **OpenAPI 3.1** sözleşmesi.
Spec: [`openapi.yaml`](./openapi.yaml) · `redocly lint` → **0 hata / 0 uyarı**.

## Tasarım kararları

- **Rota ayrımı:** Mobil ve Web aynı son kullanıcı olduğundan tekrarı önlemek için ortak `/app`
  segmentini paylaşır; yönetim farklı yetki sınırı nedeniyle `/admin` altında ayrılır; kimlik `/auth`'ta
  ortaktır. Amaç: spagettiden kaçınıp sade bir hiyerarşi.
- **Yeniden kullanılabilirlik:** `components/schemas` ile modüler; request ≠ response; ortak tipler
  (UUID, para=kuruş, telefon/TC/IBAN) paylaşılan şemalarla; hesaplama tipleri `oneOf` + `discriminator`.
- **PostgreSQL uyumu:** `uuid`, `bigint` (kuruş), `timestamptz`, `enum`, pattern'li `varchar` olarak eşlenir.
- **RESTful & ölçek:** Doğru metod/status kodları; versiyon, sayfalama, idempotency, optimistic locking
  (ETag/If-Match), rate-limit.

## Doğrulama & Görüntüleme

```bash
npx @redocly/cli lint openapi.yaml           # geçerlilik → 0 hata / 0 uyarı
npx @redocly/cli preview-docs openapi.yaml   # tarayıcıda interaktif doküman
```

Veya `api-docs.html`'i çift tıklayın, ya da `openapi.yaml`'ı [editor.swagger.io](https://editor.swagger.io)'a yapıştırın.
