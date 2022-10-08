# EVMOS TRACKER NEED API
### __Home Module__
##### `GET /api/documents/tokens`
- Retrieve all Evmos tokens data (sorted by number of holders?). 
- We should somehow blacklist those airdroped tokens that are distributed in bulk to wallets because those are mostly scams. 
```
Query parameters:
- page_no=1
- page_size=50
```
```js
Response body:
{
  "token_list": [{
      "token_name": string,
      "token_ticker": string,
      "token_supply": string,
      "token_holders": number,
      "token_last_price": number,
      "token_24hours_change?": number,
      "token_7days_change?": number
  }],
  "count": number
}
```
##### `GET /api/documents/nfts`
- Retrieve Evmos NFTs Collections (sorted by number of contract interactions?). 
- We should somehow exclude those collections that are not on marketplaces like [OrbitMarkets](https://www.orbitmarket.io).
```
Query parameters:
- page_no=1
- page_size=50
```
```js
Response body:
{
  "nft_collections_list": [{
      "collection_name": string,
      "collection_volume": number,
      "collection_floor": number,
      "collection_item_numbers": number,
      "collection_volume?": number
  }],
  "count": number
}
```


-----










### __Wallet Inspector__
##### `GET /api/wallet/tokens`
- Retrieve all Evmos tokens for wallet with native blockchain token amount ($EVMOS).
```
Query parameters:
- wallet=0xf2f5c73fa04406b1995e397b55c24ab1f3ea726c
```
```js
Response body:
{
  "wallet_tokens_list": [{
      "token_name": string,
      "token_ticker": string,
      "token_amount": number,
      "token_usd_value": number,
      "token_type": string
  }],
  "count": number
}
```
##### `GET /api/wallet/relations`
- Retrieve all connected wallets sorted by time of interactions and with information on the amount of capital transfers.
```
Query parameters:
- wallet=0xf2f5c73fa04406b1995e397b55c24ab1f3ea726c
```
```js
Response body:
{
  "related_wallets_list": [{
      "wallet": string,
      "transaction_type": "inflow" | "outflow",
      "token_transferred": string,
      "token_amount": number,
      "token_usd_value": number
  }],
  "count": number
}
```
##### `GET /api/wallet/performance`
- Retrieve wallet performance over time in USD value.
- Decide how many point API will give for specific timeframe (check query parameter `time`).
```
Query parameters:
- wallet=0xf2f5c73fa04406b1995e397b55c24ab1f3ea726c
- time=24h|7d|30d|60d|180d|1y|all
```
```js
Response body:
{
  "wallet_performance": [{
      "date_point": Date,
      "usd_value_holding": number
  }]
}
```
##### `GET /api/wallet/activity`
- Retrieve wallet activity over choosen period of `time`.
- Route gives back contract interaction without DEX contracts.
```
Query parameters:
- wallet=0xf2f5c73fa04406b1995e397b55c24ab1f3ea726c
- time=24h|7d|30d|60d|180d|1y|all
```
```js
Response body:
{
  "wallet_activity": [{
      "date_point": Date,
      "interactions": number,
      "contracts?": [{
          "contract": string,
          "contract_label": string,
          "number_of_interactions": number
      }]
  }]
}
```
##### `GET /api/wallet/trading_activity`
- Retrieve wallet trading activity over choosen period of `time`.
- Route gives back contract interactions with DEX routers.
```
Query parameters:
- wallet=0xf2f5c73fa04406b1995e397b55c24ab1f3ea726c
- time=24h|7d|30d|60d|180d|1y|all
```
```js
Response body:
{
  "trading_activity": [{
      "date_point": Date,
      "trades": number,
      "dex_contract?": [{
          "contract": string,
          "contract_label": string,
          "trading_amount": number,
          "number_of_trades": number
      }]
  }]
}
```








-----
### __Token Inspector Module__
##### `GET /api/token`
- Retrieve token data.
```
Query parameters:
- contract=0xD4949664cD82660AaE99bEdc034a0deA8A0bd517
```
```js
Response body:
{
  "token_details": {
      "total_supply": number,
      "holders": number,
      "transfers": number,
      "decimals": number,
      "token_type": number,
      "token_price?": number,
      "token_fdv?": number
  }
}
```
##### `GET /api/token/transfers`
- Retrieve token transactions (swaps/burns/contract interactions etc.).
```
Query parameters:
- contract=0xD4949664cD82660AaE99bEdc034a0deA8A0bd517
```
```js
Response body:
{
  "token_details": {
      "transaction_type": "transfer" | "swap" | "burn",
      "from_address": string,
      "to_address": string,
      "token_amount": number,
      "token_usd_value?": number
  }
}
```
##### `GET /api/token/swaps`
- Retrieve token DEX swaps.
```
Query parameters:
- contract=0xD4949664cD82660AaE99bEdc034a0deA8A0bd517
- time=24h|7d|30d|60d|180d|1y|all
```
```js
Response body:
{
  "token_swaps": {
      "date_point": Date,
      "trades": number,
      "usd_volume?": number,
      "dex_contract?": [{
          "contract": string,
          "contract_label": string,
          "trading_amount": number,
          "number_of_trades": number
      }]
  }
}
```
-----
