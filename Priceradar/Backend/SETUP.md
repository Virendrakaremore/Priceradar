# PriceRadar Backend — Setup Guide

## Step 1: Install Python (if not installed)
Download Python 3.10+ from https://python.org

## Step 2: Install dependencies
Open terminal/command prompt in this folder and run:

    pip install -r requirements.txt

## Step 3: Install Playwright browser
    
    playwright install chromium

## Step 4: Run the server

    python app.py

Server starts at: http://localhost:5000

---

## Testing the API

### Check server is alive:
    GET http://localhost:5000/api/health

### Get full product info:
    GET http://localhost:5000/api/product?url=https://www.amazon.in/dp/B09G3HRMVB

### Get only current price (call this every 5-10 seconds in frontend):
    GET http://localhost:5000/api/price?url=https://www.amazon.in/dp/B09G3HRMVB

---

## Connecting to your React Frontend

Replace the mock price function in price-tracker.jsx with:

```javascript
// Call this once on start
const res = await fetch(`http://localhost:5000/api/product?url=${encodeURIComponent(url)}`);
const data = await res.json();

// Call this every 10 seconds for live price
const priceRes = await fetch(`http://localhost:5000/api/price?url=${encodeURIComponent(url)}`);
const priceData = await priceRes.json();
```

---

## Important Notes

- Do NOT call /api/price every 1 second — Amazon/Flipkart will block your IP
- Safe polling interval: every 10-30 seconds
- For college demo: run backend on your laptop, open frontend in browser
- For deployment: host backend on a server (e.g. Railway, Render, VPS)

---

## Common Issues

| Problem | Fix |
|---|---|
| `playwright install` fails | Run as administrator |
| Amazon shows CAPTCHA | Try again — rotate user agent if repeated |
| Price not found | The page selector may have changed — update selectors in app.py |
| CORS error in browser | Make sure flask-cors is installed and server is running |
