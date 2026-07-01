# Live Stock Screener Android Widget + Backend

This package contains a fuller starter implementation for a **live-results-only stock screener** with an Android home-screen widget.

## What is included

- `android/` — Kotlin Android app with a home-screen widget, manual refresh button, background polling via WorkManager, and notifications when matches are found.
- `backend/` — FastAPI backend that applies the strict scan filters and returns only confirmed matches.

## Your scanner filters

The backend only returns symbols that pass every configured filter:

1. Price between `$1.50` and `$10.00`
2. Relative volume at least `5x`
3. Daily gain at least `20%`
4. Float `30,000,000` shares or less
5. Market cap between `$1,000,000` and `$100,000,000`
6. Current price below both 9-day EMA and 20-day EMA
7. MACD line below `0`
8. Current price below intraday VWAP
9. Recent catalyst/news/social engagement verification

If a field cannot be verified from the configured live/current data source, the symbol is excluded.

## Quick start

### 1) Run the backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# edit .env and add your FMP_API_KEY
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Open:

```text
http://localhost:8000/health
http://localhost:8000/matches
```

### 2) Run Android app

Open `android/` in Android Studio, sync Gradle, run the app on your Android phone/emulator, then add the **Live Stock Scanner** widget to your home screen.

On a physical phone, your backend URL must be reachable from the phone. For local testing, use your computer's LAN IP, for example:

```text
http://192.168.1.25:8000
```

For Android Emulator, use:

```text
http://10.0.2.2:8000
```

## Important production notes

- This is a starter project, not investment advice or a production trading system.
- For strict “live only” behavior, use a paid real-time market data plan and a real social/news engagement source.
- Android may delay background work depending on battery, network, and OS optimization settings.
- If you expose the backend publicly, use HTTPS, authentication, rate limiting, and do not put provider API keys in the Android app.

## APK build note

See `BUILD_APK.md` for Android Studio, command-line, and GitHub Actions build steps.

