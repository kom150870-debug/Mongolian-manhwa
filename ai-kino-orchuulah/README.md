# KINO AI — AI Монгол хадмал

Cloudflare Pages, Functions, OpenAI, QPay, AdSense бүхий Ad + Pay-per-use + VIP MVP.

## Функцууд
- Үнэгүй: SRT / VTT импорт, редактор, SRT таталт; зөвшөөрөлтэй үед AdSense зар.
- 4,900₮: нэг 25 MB хүртэл AI орчуулга.
- 29,900₮ VIP: 30 хоногт 10 файл, заргүй, автоматаар сунгагдахгүй.
- QPay invoice, server-side verification, anonymous session cookie, Cloudflare KV storage.

## Cloudflare Pages deployment
1. GitHub repo: kom150870-debug/Mongolian-manhwa
2. Root directory: ai-kino-orchuulah
3. Framework preset: None; build command хоосон; build output directory: .
4. Cloudflare KV namespace үүсгэж KINO_DB binding нэм (Production болон Preview).
5. Cloudflare Pages → Settings → Variables and Secrets хэсэгт дараах утгуудыг тохируул:
   - OPENAI_API_KEY: encrypted secret
   - QPAY_BASE_URL: https://merchant-sandbox.qpay.mn (эхлээд sandbox), бодит үед https://merchant.qpay.mn
   - QPAY_CLIENT_ID: encrypted secret
   - QPAY_CLIENT_SECRET: encrypted secret
   - QPAY_INVOICE_CODE: merchant invoice code, secret
   - ADSENSE_APPROVED: true (Google approval авсны дараа)
   - ADSENSE_CLIENT_ID: ca-pub-... (public)
   - ADSENSE_SLOT_ID: ad slot ID (public)
6. Redeploy хийгээд live URL → /api/session шалгана.
7. QPay sandbox invoice → sandbox payment → /api/status → entitlement test, дараа нь production test.

## Анхаарах
- GitHub-д код байрласан нь Cloudflare deployment, merchant registration, AdSense approval, OpenAI billing хийгдсэн гэсэн үг биш.
- QPay-аас баталгаажуулаагүй төлбөрөөр эрх олгохгүй. API түлхүүрийг GitHub-д хийж болохгүй.
- Эхний хувилбар хөтчийн cookie-д эрх холбодог тул төхөөрөмж хооронд сэргээх боломжгүй.
- Cloudflare KV нь atomic transaction биш; ачаалал ихсэхэд D1 / Durable Objects + хэрэглэгчийн нэвтрэлт шаардлагатай.
- Үнэтэй үйлчилгээний буцаалтын бодлого, албан ёсны холбоо барих мэдээллийг production эхлэхээс өмнө нэм.
- Зөвхөн зөвшөөрөлтэй контент боловсруулах. AdSense Publisher Policies мөрдөх.

## Endpoint
- GET /api/session
- POST /api/checkout
- GET /api/status
- POST /api/translate
- GET/POST /api/qpay-callback
