# BYMA Trader PWA

Trading app para acciones argentinas + CEDEARs. Funciona 100% en el celular.

## Contenido
- `index.html` — App completa (React + Tailwind + Recharts via CDN)
- `sw.js` — Service Worker (cache, offline, background sync)
- `manifest.json` — PWA manifest (instalable en Android)

## Deploy gratis en GitHub Pages (5 minutos)

```bash
# 1. Crear repo en GitHub (puede ser privado)
# 2. Clonar y copiar archivos
git init byma-trader && cd byma-trader
cp /path/to/index.html /path/to/sw.js /path/to/manifest.json .

# 3. Push
git add -A && git commit -m "v1.0"
git branch -M main
git remote add origin https://github.com/TU_USER/byma-trader.git
git push -u origin main

# 4. Activar GitHub Pages
# Settings → Pages → Source: Deploy from branch → main → / (root) → Save
# En ~2 min tenés: https://TU_USER.github.io/byma-trader/
```

## Instalar en Android
1. Abrir `https://TU_USER.github.io/byma-trader/` en Chrome Android
2. Esperar el prompt "Instalar App" o ir a ⋮ → "Instalar aplicación"
3. Listo. Aparece como app nativa en el launcher.

## Alternativa: Servir localmente (sin hosting)
```bash
# Con Python
cd byma-trader && python3 -m http.server 8080
# Abrir http://localhost:8080 en Chrome

# Con Node
npx serve .
```
> Nota: Sin HTTPS no podés instalar el SW ni recibir notificaciones push.
> Para HTTPS local: `npx local-ssl-proxy --source 8443 --target 8080`

## Features
- 150 tickers (50 AR + 100 CEDEARs) via Yahoo Finance
- 18 indicadores técnicos: RSI, MACD, BB, ADX, Stoch, ATR, OBV, W%R, CCI, MFI, ROC, VWAP, TRIX, Ichimoku, Chaikin, EMAs, SMAs
- Scoring ensemble ponderado (18 indicadores → señal compuesta)
- Gestión de cartera: capital, posiciones, lotes, PM, P&L
- Alertas de precio configurables (compra/venta)
- Historial de operaciones con P&L realizado
- Indicadores macro: VIX, S&P500, Dow, Merval, DXY, Petróleo, Oro, US10Y
- Horario BYMA (10:30-17:00 L-V) + feriados argentinos
- Auto-scan configurable (15/30/60 min)
- Notificaciones push nativas
- Offline: cartera, historial, última data cacheada
- IndexedDB para persistencia local
- Export/import backup JSON
- Dark theme optimizado para mobile

## Arquitectura
```
Browser
├── React 18 (UI)
├── Tailwind CSS (estilos)
├── Recharts (gráficos)
├── Babel Standalone (JSX → JS)
├── IndexedDB (persistencia)
├── Service Worker
│   ├── Cache estático (CDN libs)
│   ├── Periodic Background Sync
│   └── Notification API
└── CORS Proxy → Yahoo Finance API
    ├── corsproxy.io (primario)
    └── allorigins.win (fallback)
```

## Datos iniciales precargados
- Capital: $2.122.368 ARS (COCOS Rendimientos FC)
- LAC: 300 acc (151 @ $5.945 + 149 @ $5.605)
- Alertas LAC: compra $5.500/$5.400 | venta $7.000/$8.000/$10.000
- Watchlist: LAC, EDN, ALUA

## Limitaciones v1
- ML scoring: ensemble ponderado de indicadores (no neural network aún)
- Background: Periodic Sync requiere engagement mínimo con la app
- CORS proxies: pueden tener rate limits o downtime temporal
- Yahoo Finance: ~800ms delay entre requests para no saturar
