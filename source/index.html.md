---
title: API Reference

toc_footers:
  - <a href='https://triunits.com'>TRIUNITS EXCHANGE</a>

includes:
  - markets
  - errors

search: true

code_clipboard: true
---

# General Info

## General API Information

- The base endpoint is: **https://fapi.triunits.com**
- All endpoints return either a JSON object or array.
- Data is returned in ascending order. Oldest first, newest last.
- All time and timestamp related fields are in milliseconds.

## Intervals

m -> minutes; h -> hours; d -> days; w -> weeks; M -> months

- 1m
- 3m
- 5m
- 15m
- 30m
- 1h
- 2h
- 4h
- 6h
- 8h
- 12h
- 1d
- 3d
- 1w
- 1M

## Filters

### PRICE_FILTER

> ExchangeInfo Format:

```json
{
  "filterType": "PRICE_FILTER",
  "minPrice": "0.00000100",
  "maxPrice": "100000.00000000",
  "tickSize": "0.00000100"
}
```

The PRICE_FILTER defines the price rules for a symbol. There are 3 parts:

- minPrice defines the minimum price/stopPrice allowed; disabled on minPrice == 0.
- maxPrice defines the maximum price/stopPrice allowed; disabled on maxPrice == 0.
- tickSize defines the intervals that a price/stopPrice can be increased/decreased by; disabled on tickSize == 0.

Any of the above variables can be set to 0, which disables that rule in the price filter. In order to pass the price filter, the following must be true for price/stopPrice of the enabled rules:

- price >= minPrice
- price <= maxPrice
- (price-minPrice) % tickSize == 0

### PERCENT_PRICE

> ExchangeInfo Format:

```json
{
  "filterType": "PERCENT_PRICE",
  "multiplierUp": "1.3000",
  "multiplierDown": "0.7000",
  "avgPriceMins": 5
}
```

The `PERCENT_PRICE` filter defines valid range for a price based on the average of the previous trades. `avgPriceMins` is the number of minutes the average price is calculated over. 0 means the last price is used.

In order to pass the `percent price`, the following must be true for price:

price <= `weightedAveragePrice` * `multiplierUp`
price >= `weightedAveragePrice` * `multiplierDown`

### LOT_SIZE

> ExchangeInfo format:

```json
{
  "filterType": "LOT_SIZE",
  "minQty": "0.00100000",
  "maxQty": "100000.00000000",
  "stepSize": "0.00100000"
}
```

The LOT_SIZE filter defines the quantity (aka "lots" in auction terms) rules for a symbol. There are 3 parts:

- minQty defines the minimum quantity/icebergQty allowed.
- maxQty defines the maximum quantity/icebergQty allowed.
- stepSize defines the intervals that a quantity/icebergQty can be increased/decreased by.
In order to pass the lot size, the following must be true for quantity/icebergQty:

- quantity >= minQty
- quantity <= maxQty
- (quantity-minQty) % stepSize == 0

### MIN_NOTIONAL

> ExchangeInfo Format:

```json
{
  "filterType": "MIN_NOTIONAL",
  "minNotional": "0.00100000",
  "applyToMarket": true,
  "avgPriceMins": 5
}
```

The MIN_NOTIONAL filter defines the minimum notional value allowed for an order on a symbol. An order's notional value is the price * quantity. If the order is an Algo order (e.g. STOP_LOSS_LIMIT), then the notional value of the stopPrice * quantity will also be evaluated. If the order is an Iceberg Order, then the notional value of the price * icebergQty will also be evaluated. applyToMarket determines whether or not the MIN_NOTIONAL filter will also be applied to MARKET orders. Since MARKET orders have no price, the average price is used over the last avgPriceMins minutes. avgPriceMins is the number of minutes the average price is calculated over. 0 means the last price is used.


# Wallet Endpoints

## System Status (System)

> Response:

```json
{
  "status": 0,      // 0: normal，1：system maintenance
  "msg": "normal"   // normal or system maintenance
}
```

This endpoint fetch the system status.

### HTTP Request

`GET /spot/systemStatus`

