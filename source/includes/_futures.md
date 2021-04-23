# Futures Market Endpoints

## Futures Test Connectivity

> Response:

```json
{}
```

Test connectivity to the Rest API.

### HTTP Request

`GET /fapi/ping`

## Futures Check Server Time

> Response:

```json
{
  "serverTime": 1499827319559
}
```

Test connectivity to the Rest API.

### HTTP Request

`GET /fapi/time`

## Futures Exchange Information

> Response:

```json
{
    "exchangeFilters": [],
    "rateLimits": [
        {
            "interval": "MINUTE",
            "intervalNum": 1,
            "limit": 2400,
            "rateLimitType": "REQUEST_WEIGHT" 
        },
        {
            "interval": "MINUTE",
            "intervalNum": 1,
            "limit": 1200,
            "rateLimitType": "ORDERS"
        }
    ],
    "serverTime": 1565613908500, 
    "symbols": [
        {
            "symbol": "BLZUSDT",
            "pair": "BLZUSDT",
            "contractType": "PERPETUAL",
            "deliveryDate": 4133404800000,
            "onboardDate": 1598252400000,
            "status": "TRADING",
            "maintMarginPercent": "2.5000",   // ignore
            "requiredMarginPercent": "5.0000",  // ignore
            "baseAsset": "BLZ", 
            "quoteAsset": "USDT",
            "marginAsset": "USDT",
            "pricePrecision": 5,    // please do not use it as tickSize
            "quantityPrecision": 0, // please do not use it as stepSize
            "baseAssetPrecision": 8,
            "quotePrecision": 8, 
            "underlyingType": "COIN",
            "underlyingSubType": ["STORAGE"],
            "settlePlan": 0,
            "triggerProtect": "0.15", // threshold for algo order with "priceProtect"
            "filters": [
                {
                    "filterType": "PRICE_FILTER",
                    "maxPrice": "300",
                    "minPrice": "0.0001", 
                    "tickSize": "0.0001"
                },
                {
                    "filterType": "LOT_SIZE", 
                    "maxQty": "10000000",
                    "minQty": "1",
                    "stepSize": "1"
                },
                {
                    "filterType": "MARKET_LOT_SIZE",
                    "maxQty": "590119",
                    "minQty": "1",
                    "stepSize": "1"
                },
                {
                    "filterType": "MAX_NUM_ORDERS",
                    "limit": 200
                },
                {
                    "filterType": "MAX_NUM_ALGO_ORDERS",
                    "limit": 100
                },
                {
                    "filterType": "MIN_NOTIONAL",
                    "notional": "1", 
                },
                {
                    "filterType": "PERCENT_PRICE",
                    "multiplierUp": "1.1500",
                    "multiplierDown": "0.8500",
                    "multiplierDecimal": 4
                }
            ],
            "OrderType": [
                "LIMIT",
                "MARKET",
                "STOP",
                "STOP_MARKET",
                "TAKE_PROFIT",
                "TAKE_PROFIT_MARKET",
                "TRAILING_STOP_MARKET" 
            ],
            "timeInForce": [
                "GTC", 
                "IOC", 
                "FOK", 
                "GTX" 
            ]
        }
    ],
    "timezone": "UTC" 
}
```

Current exchange trading rules and symbol information

### HTTP Request

`GET /fapi/exchangeInfo`

## Futures Order Book

> Response:

```json
{
  "lastUpdateId": 1027024,
  "E": 1589436922972,   // Message output time
  "T": 1589436922959,   // Transaction time
  "bids": [
    [
      "4.00000000",     // PRICE
      "431.00000000"    // QTY
    ]
  ],
  "asks": [
    [
      "4.00000200",
      "12.00000000"
    ]
  ]
}
```

Test connectivity to the Rest API.

### HTTP Request

`GET /fapi/depth`

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |
limit     | INT    |    NO      | Default 500; Valid limits:[5, 10, 20, 50, 100, 500, 1000]

## Futures Recent Trade List

> Response:

```json
{
  "lastUpdateId": 1027024,
  "E": 1589436922972,   // Message output time
  "T": 1589436922959,   // Transaction time
  "bids": [
    [
      "4.00000000",     // PRICE
      "431.00000000"    // QTY
    ]
  ],
  "asks": [
    [
      "4.00000200",
      "12.00000000"
    ]
  ]
}
```

Test connectivity to the Rest API.

### HTTP Request

`GET /fapi/trades`

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |
limit     | INT    |    NO      | Default 500; Max: 1000.

## Futures Kline/Candlestick Data

> Response:

```json
[
  [
    1499040000000,      // Open time
    "0.01634790",       // Open
    "0.80000000",       // High
    "0.01575800",       // Low
    "0.01577100",       // Close
    "148976.11427815",  // Volume
    1499644799999,      // Close time
    "2434.19055334",    // Quote asset volume
    308,                // Number of trades
    "1756.87402397",    // Taker buy base asset volume
    "28.46694368",      // Taker buy quote asset volume
    "17928899.62484339" // Ignore.
  ]
]
```

Kline/candlestick bars for a symbol.
Klines are uniquely identified by their open time.

### HTTP Request

`GET /fapi/klines`

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |
interval  | ENUM   |    NO      |
startTime | LONG   |    NO      |
endTime   | LONG   |    NO      |
limit     | INT    |    NO      | Default 500; max 5000

<aside class="notice">
If startTime and endTime are not sent, the most recent klines are returned.
</aside>