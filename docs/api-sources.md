# API Sources (FinViz Project)

## 📈 Price & Volume Data
**Provider:** [Twelve Data](https://twelvedata.com/)  
- **Free Limit:** 800 requests/minute (sufficient for top 20 Nasdaq stocks)  
- **API Key:** Stored securely in AWS Secrets Manager  
- **Example Endpoint:**
```

[https://api.twelvedata.com/time_series?symbol=AAPL,MSFT,NVDA,TSLA,AMZN,META,GOOGL,NFLX,AMD,INTC&interval=1day&outputsize=100&apikey=YOUR_KEY](https://api.twelvedata.com/time_series?symbol=AAPL,MSFT,NVDA,TSLA,AMZN,META,GOOGL,NFLX,AMD,INTC&interval=1day&outputsize=100&apikey=YOUR_KEY)

```
- **Usage:** Daily close, volume, open/high/low values.  
- **Rate Limit Strategy:** Combine multiple symbols per request; handle 429 errors with exponential backoff (200ms → 400ms → 800ms).  

---

## 🧾 Fundamental Data
**Provider:** [Financial Modeling Prep (FMP)](https://financialmodelingprep.com/developer/docs/)  
- **Free Limit:** 250 requests/day  
- **Key Metrics:** P/E, P/B, P/S, PEG, EPS, EBITDA Margin, Debt/Equity  
- **Example Endpoints:**
```

# Key metrics (ratios)

[https://financialmodelingprep.com/api/v3/key-metrics/AAPL?apikey=YOUR_KEY](https://financialmodelingprep.com/api/v3/key-metrics/AAPL?apikey=YOUR_KEY)

# Company profile

[https://financialmodelingprep.com/api/v3/profile/AAPL?apikey=YOUR_KEY](https://financialmodelingprep.com/api/v3/profile/AAPL?apikey=YOUR_KEY)

# Income statement

[https://financialmodelingprep.com/api/v3/income-statement/AAPL?period=annual&limit=5&apikey=YOUR_KEY](https://financialmodelingprep.com/api/v3/income-statement/AAPL?period=annual&limit=5&apikey=YOUR_KEY)

```
- **Usage:** Daily retrieval of core fundamentals and ratios.

---

## 🔄 Backup Provider
**Provider:** [AlphaVantage](https://www.alphavantage.co/)  
- **Free Limit:** 5 requests/minute  
- **Usage:** Redundant data source for prices or indicators.  
- **Example Endpoint:**
```

[https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=AAPL&outputsize=compact&apikey=YOUR_KEY](https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=AAPL&outputsize=compact&apikey=YOUR_KEY)

````

---

## 🔐 API Key Storage
API keys are stored in **AWS Secrets Manager** under secret name `finviz/api_keys`.

Example secret value:
```json
{
  "twelvedata_key": "xxxxx",
  "fmp_key": "xxxxx",
  "alphavantage_key": "xxxxx"
}
````

Lambda functions retrieve keys at runtime:

```python
import boto3, json
sm = boto3.client('secretsmanager')
raw = sm.get_secret_value(SecretId='finviz/api_keys')['SecretString']
keys = json.loads(raw)
TD_KEY  = keys['twelvedata_key']
FMP_KEY = keys['fmp_key']
AV_KEY  = keys['alphavantage_key']
```
