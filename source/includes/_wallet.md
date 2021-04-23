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