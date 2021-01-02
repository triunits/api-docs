# Market Data Endpoints

## Check Server Time 

> Response:

```json
{
  "serverTime": 1609577382447
}
```

Test connectivity to the Rest API and get the current server time.

### HTTP Request

`GET /spot/time`

## Exchange Information

> Response:

```json
{
  "timezone": "UTC",
  "serverTime": 1565246363776,
  "rateLimits": [
    {
      //These are defined in the `ENUM definitions` section under `Rate Limiters (rateLimitType)`.
      //All limits are optional
    }
  ],
  "exchangeFilters": [
    //These are the defined filters in the `Filters` section.
    //All filters are optional.
  ],
  "symbols": [
    {
      "symbol": "ETHBTC",
      "status": "TRADING",
      "baseAsset": "ETH",
      "baseAssetPrecision": 8,
      "quoteAsset": "BTC",
      "quotePrecision": 8,
      "quoteAssetPrecision": 8,
      "orderTypes": [
        "LIMIT",
        "LIMIT_MAKER",
        "MARKET",
        "STOP_LOSS",
        "STOP_LOSS_LIMIT",
        "TAKE_PROFIT",
        "TAKE_PROFIT_LIMIT"
      ],
      "icebergAllowed": true,
      "ocoAllowed": true,
      "isSpotTradingAllowed": true,
      "isMarginTradingAllowed": true,
      "filters": [
        //These are defined in the Filters section.
        //All filters are optional
      ],
      "permissions": [
         "SPOT",
         "MARGIN"
      ]
    }
  ]
}
```

Current exchange trading rules and symbol information

### HTTP Request

`GET /spot/exchangeInfo`

## Order Book

> Response:

```json
{
  "lastUpdateId": 1027024,
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

Exchange Order Book

### HTTP Request

`GET /spot/depth`

### Limits

5, 10, 20, 50, 100, 500, 1000, 5000

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |
limit     | INT    |    NO      | Default 100; max 5000

## Recent Trades List

```json
[
  {
    "id": 28457,
    "price": "4.00000100",
    "qty": "12.00000000",
    "quoteQty": "48.000012",
    "time": 1499865549590,
    "isBuyerMaker": true,
    "isBestMatch": true
  }
]
```

Get recent trades.

### HTTP Request

`GET /spot/trades`

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |
limit     | INT    |    NO      | Default 500; max 5000

## Kline/Candlestick Data

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

`GET /spot/klines`

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

## 24hr Ticker Price Change Statistics

> Response:

```json
{
  "symbol": "DOTSBTC",
  "priceChange": "-94.99999800",
  "priceChangePercent": "-95.960",
  "weightedAvgPrice": "0.29628482",
  "prevClosePrice": "0.10002000",
  "lastPrice": "4.00000200",
  "lastQty": "200.00000000",
  "bidPrice": "4.00000000",
  "askPrice": "4.00000200",
  "openPrice": "99.00000000",
  "highPrice": "100.00000000",
  "lowPrice": "0.10000000",
  "volume": "8913.30000000",
  "quoteVolume": "15.30000000",
  "openTime": 1499783499040,
  "closeTime": 1499869899040,
  "firstId": 28385,   // First tradeId
  "lastId": 28460,    // Last tradeId
  "count": 76         // Trade count
}
```
> OR

```json
[
  {
    "symbol": "DOTSBTC",
    "priceChange": "-94.99999800",
    "priceChangePercent": "-95.960",
    "weightedAvgPrice": "0.29628482",
    "prevClosePrice": "0.10002000",
    "lastPrice": "4.00000200",
    "lastQty": "200.00000000",
    "bidPrice": "4.00000000",
    "askPrice": "4.00000200",
    "openPrice": "99.00000000",
    "highPrice": "100.00000000",
    "lowPrice": "0.10000000",
    "volume": "8913.30000000",
    "quoteVolume": "15.30000000",
    "openTime": 1499783499040,
    "closeTime": 1499869899040,
    "firstId": 28385,   // First tradeId
    "lastId": 28460,    // Last tradeId
    "count": 76         // Trade count
  }
]
```
24 hour rolling window price change statistics. Careful when accessing this with no symbol.

### HTTP Request

`GET /spot/ticker/24hr`

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |

## Symbol Price Ticker

> Response:

```json
{
  "symbol": "LTCBTC",
  "price": "4.00000200"
}
```
> OR

```json
[
  {
    "symbol": "LTCBTC",
    "price": "4.00000200"
  },
  {
    "symbol": "ETHBTC",
    "price": "0.07946600"
  }
]
```

Latest price for a symbol or symbols.

### HTTP Endpoint

`GET /spot/ticker/price`

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |

<aside class="notice">
If the symbol is not sent, prices for all symbols will be returned in an array.
</aside>

## Symbol Order Book Ticker

> Response:

```json
{
  "symbol": "LTCBTC",
  "bidPrice": "4.00000000",
  "bidQty": "431.00000000",
  "askPrice": "4.00000200",
  "askQty": "9.00000000"
}
```
> OR 

```json
[
  {
    "symbol": "LTCBTC",
    "bidPrice": "4.00000000",
    "bidQty": "431.00000000",
    "askPrice": "4.00000200",
    "askQty": "9.00000000"
  },
  {
    "symbol": "ETHBTC",
    "bidPrice": "0.07946700",
    "bidQty": "9.00000000",
    "askPrice": "100000.00000000",
    "askQty": "1000.00000000"
  }
]
```

Best price/qty on the order book for a symbol or symbols.

### HTTP Endpoint

`GET /spot/ticker/bookTicker`

### URL Parameters

Parameter | Type   |  Mandatory | Description
--------- | ------ | ---------- | -----------
symbol    | STRING |    YES     |

<aside class="notice">
If the symbol is not sent, bookTickers for all symbols will be returned in an array.
</aside>
