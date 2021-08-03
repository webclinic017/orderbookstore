# orderbookstore
A utility to store daily end-of-the-day complete order book to clickhouse DB. This can be used to fetch historical order book data in future.

## Installation
```
go get -u github.com/ranjanrak/orderbookstore
```

## Usage
```
package main

import (
    "github.com/ranjanrak/orderbookstore"
)

func main() {
    // Store current orderbook data to clickhouse DB
    orderbookstore.DataLoad()

    // Fetch all historical order's for the requested symbol
    orderbookstore.QueryDB("SBIN")
    
    // Fetch average buy and sell price for the mentioned symbol and period
    time_start := time.Date(2021, 6, 29, 9, 41, 0, 0, time.UTC)
    time_end := time.Date(2021, 8, 3, 15, 05, 0, 0, time.UTC)
    avgBook := orderbookstore.QueryAvgPrice("IOC", time_start, time_end)
    fmt.Printf("%+v\n", avgBook)

}
```

## Response
1> Response for `orderbookstore.QueryDB("SBIN")`:
```
order_timestamp: 2021-06-23 09:15:14 +0000 UTC, order_id: XXXXXXX, tradingsymbol: SBIN, average_price: 421.800000
order_timestamp: 2021-06-23 09:16:48 +0000 UTC, order_id: XXXXXXX, tradingsymbol: SBIN, average_price: 421.500000
order_timestamp: 2021-06-25 09:07:46 +0000 UTC, order_id: XXXXXXX, tradingsymbol: SBIN, average_price: 421.000000
order_timestamp: 2021-06-25 09:15:08 +0000 UTC, order_id: XXXXXXX, tradingsymbol: SBIN, average_price: 420.250000
order_timestamp: 2021-06-25 09:17:14 +0000 UTC, order_id: XXXXXXX, tradingsymbol: SBIN, average_price: 420.500000
order_timestamp: 2021-06-28 12:50:24 +0000 UTC, order_id: XXXXXXX, tradingsymbol: SBIN, average_price: 427.600000
order_timestamp: 2021-06-28 12:50:53 +0000 UTC, order_id: XXXXXXX, tradingsymbol: SBIN, average_price: 427.750000
```
2> Response for `orderbookstore.QueryAvgPrice("IOC", time_start, time_end)`:
```
{symbol:IOC buy_avg:107.41 buy_qty:39 sell_avg:107.61 sell_qty:17 realizedpnl:-22.9}
```