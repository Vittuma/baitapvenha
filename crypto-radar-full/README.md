# 🚀 CryptoRadar — AI Bullish/Bearish Scanner

Real-time crypto sentiment analysis dashboard với AI-powered news crawling, fake news detection, và bullish/bearish scoring 0-100.

## ✨ Features

- **Real-time news crawling** — Tự động crawl CoinDesk, CoinTelegraph, Decrypt, The Block, CryptoSlate
- **Live price data** — Giá, volume 24h, market cap từ CoinGecko API  
- **AI Sentiment Score** — Thang điểm 0-100 (Bullish/Bearish) do Claude AI phân tích
- **Fake News Detection** — Phát hiện tin giả, tin pump-dump, tin thiếu nguồn uy tín
- **7-day Price Chart** — Biểu đồ giá 7 ngày
- **Sub-scores** — Price, News, Volume, Momentum score riêng biệt

## 🛠 Tech Stack

- **Frontend**: Next.js 14, React, TypeScript, Recharts
- **AI**: Claude claude-sonnet-4-20250514 (Anthropic) với web_search tool
- **Price Data**: CoinGecko API (free tier)
- **Deploy**: Vercel

## 🚀 Deploy lên Vercel

### Bước 1: Clone & cài packages

```bash
git clone <your-repo>
cd crypto-radar
npm install
```

### Bước 2: Setup environment variables

```bash
cp .env.example .env.local
# Điền ANTHROPIC_API_KEY vào .env.local
```

Lấy API key tại: https://console.anthropic.com

### Bước 3: Deploy lên Vercel

```bash
# Cài Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
vercel

# Set environment variable trên Vercel
vercel env add ANTHROPIC_API_KEY
# → Paste API key của bạn
# → Chọn: Production, Preview, Development

# Deploy lại với env vars
vercel --prod
```

Hoặc dùng Vercel Dashboard:
1. Import GitHub repo tại vercel.com
2. Vào Settings → Environment Variables
3. Thêm `ANTHROPIC_API_KEY` = your key
4. Redeploy

### Bước 4: Test local

```bash
# Tạo .env.local
echo "ANTHROPIC_API_KEY=your_key_here" > .env.local

npm run dev
# → http://localhost:3000
```

## 📁 Project Structure

```
crypto-radar/
├── pages/
│   ├── index.tsx          # Main dashboard UI
│   ├── _app.tsx           # App wrapper
│   └── api/
│       └── analyze.ts     # Backend: news crawl + AI analysis
├── vercel.json            # Vercel config (60s timeout for AI)
├── .env.example           # Environment template
└── package.json
```

## ⚙️ API Endpoint

`POST /api/analyze`

Request:
```json
{ "symbol": "BTC" }
```

Response:
```json
{
  "symbol": "BTC",
  "priceData": { "current_price": 67000, "price_change_percentage_24h": 2.3, ... },
  "news": [{ "title": "...", "source": "CoinDesk", "sentiment": "positive", "credibilityScore": 9 }],
  "analysis": {
    "overallScore": 72,
    "sentiment": "Bullish",
    "priceScore": 75,
    "newsScore": 68,
    "volumeScore": 70,
    "momentumScore": 76,
    "fakeNewsFlags": [],
    "bullishFactors": ["Strong institutional demand", ...],
    "bearishFactors": ["Regulatory uncertainty", ...],
    "recommendation": "Buy",
    "confidence": 78
  },
  "historicalPrices": [{ "date": "10 thg 5", "price": 62000 }],
  "timestamp": "2024-05-14T..."
}
```

## 🔑 Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `ANTHROPIC_API_KEY` | ✅ | Claude API key từ console.anthropic.com |

## 📝 Notes

- CoinGecko free tier: 30 req/min — đủ cho demo
- Vercel function timeout: 60s (cần cho AI analysis + web search)
- Mỗi request tốn ~2-3 Claude API calls
