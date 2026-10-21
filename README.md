# XAUUSD Quasimodo (QM) Signal Dashboard

Dashboard analisis teknikal Quasimodo **Over & Under** untuk XAU/USD, dengan level dikira terus dari data OHLC sebenar (Bybit `XAUUSDT`) — bukan angka hardcoded.

![status](https://img.shields.io/badge/data-Bybit%20XAUUSDT-f59e0b) ![status](https://img.shields.io/badge/deploy-Vercel-000)

## Apa yang dibuat

| Bahagian | Keterangan |
|---|---|
| **Chart candlestick** | Dilukis pada canvas sendiri, skala harga dari data sebenar |
| **Deteksi QM** | Pivot → zigzag → struktur Over & Under (LS → Neck → Head → Under) |
| **QML** | Garis level bahu kiri = level entry utama |
| **SL / TP1 / TP2 / TP3** | Head / Neck / Under / unjuran |
| **Penapis kualiti** | Min risk 0.8×ATR, min RR 1.5, confirmation close |
| **Sejarah** | Setiap setup diuji bar demi bar → TP2 atau SL, dengan pip sebenar |
| **Multi-timeframe** | M15 / H1 / H4 / D1 |
| **Kalkulator lot** | Auto-isi jarak SL dari setup aktif |

## Struktur pola (Over & Under)

**SELL (bearish)**
```
2·LS   = pivot HIGH (bahu kiri → level QML)
3·Neck = pivot LOW
4·Over = pivot HIGH lebih tinggi dari LS   (head = SL)
5·Under= pivot LOW lebih rendah dari Neck  (TP2)
entry  = harga naik balik ke QML → SELL
```

**BUY (bullish)** — mirror penuh.

## Struktur projek

```
.
├── index.html      # dashboard (semua logik inline, tiada build step)
├── vercel.json     # config deploy statik + security headers
├── README.md
├── qm_quasimodo_indicator.pine   # indikator TradingView (Pine v5)
└── *.pdf                          # bahan rujukan
```

## Deploy

### Vercel (auto)
```bash
vercel --prod
```

### Vercel (Git integration)
Push ke GitHub → import repo di [vercel.com/new](https://vercel.com/new) → framework preset **Other** → deploy.

### Tempatan
```bash
python -m http.server 8000
# buka http://127.0.0.1:8000
```

## Nota teknikal

- **Tiada build step** — HTML/CSS/JS tunggal, jadi Vercel deploy sebagai statik.
- **Data API**: `https://api.bybit.com/v5/market/kline` (CORS dibenarkan).
- **Fallback**: jika API disekat, dashboard masuk *Mod Simulasi* (badge kuning) — tidak menyamar data sintetik sebagai live.
- **Pip emas** = 0.1; 1 lot XAUUSD = 100 oz.

## Penafian

Alat pendidikan/analisis. Dagangan emas berisiko tinggi. Bukan nasihat pelaburan.
