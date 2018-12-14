# The Kite Connect API Python client
The official Python client for communicating with the [Kite Connect API](https://kite.trade).

Kite Connect is a set of REST-like APIs that expose many capabilities required to build a complete investment and trading platform. Execute orders in real time, manage user portfolio, stream live market data (WebSockets), and more, with the simple HTTP API collection.

[Rainmatter](http://rainmatter.com) (c) 2016. Licensed under the MIT License.

## Documentation
- [Python client documentation](https://kite.trade/docs/pykiteconnect)
- [Kite Connect HTTP API documentation](https://kite.trade/docs/connect/v1)

## Installing the client
`pip install kiteconnect`

## API usage
```python
from kiteconnect import KiteConnect

kite = KiteConnect(api_key="your_api_key")

# Redirect the user to the login url obtained
# from kite.login_url(), and receive the request_token
# from the registered redirect url after the login flow.
# Once you have the request_token, obtain the access_token
# as follows.

data = kite.request_access_token("request_token_here", secret="your_secret")
kite.set_access_token(data["access_token"])

# Place an order
try:
	order_id = kite.order_place(tradingsymbol="INFY",
					exchange="NSE",
					transaction_type="BUY",
					quantity=1,
					order_type="MARKET",
					product="NRML")

	print("Order placed. ID is", order_id)
except Exception as e:
	print("Order placement failed", e.message)

# Fetch all orders
print(kite.orders())
```

Refer to the [Python client documentation](https://kite.trade/docs/pykiteconnect) for the complete list of supported methods.

## WebSocket usage
```python
from kiteconnect import WebSocket

# Initialise.
kws = WebSocket("your_api_key", "your_public_token", "logged_in_user_id")

# Callback for tick reception.
def on_tick(tick, ws):
	print tick

# Callback for successful connection.
def on_connect(ws):
	# Subscribe to a list of instrument_tokens (RELIANCE and ACC here).
	ws.subscribe([738561, 5633])

	# Set RELIANCE to tick in `full` mode.
	ws.set_mode(ws.MODE_FULL, [738561])

# Assign the callbacks.
kws.on_tick = on_tick
kws.on_connect = on_connect

# Infinite loop on the main thread. Nothing after this will run.
# You have to use the pre-defined callbacks to manage subscriptions.
kws.connect()
```

## Changelog
- 2015-07-15	Fixed: Different response type recevied for large number of subscriptions
- 2016-05-31	Added `WebSocket` class for streaming data.
- 2016-04-29	`instruments()` call now returns parsed CSV records.
- 2016-05-04	Added `historical()` call.
- 2016-05-09	Added `parent_order_id` param for multi-legged orders.
- 2016-07-25    Option to disable SSL cert verification (Ubuntu 12.04 openssl bug)
- 2016-08-26    Full compatability for Python 3
- 2016-10-21	Released **version 3.3** with following fixes and features
				* Added `tag` support to order APIs
				* Added proxy support for api and websocket streaming
				* Fixed market depth `orders` integer overflow issue.

### Support:

If you want the good work to continue please support us on

* [PAYPAL](https://www.paypal.me/ishandutta2007)
* [BITCOIN ADDRESS: 3LZazKXG18Hxa3LLNAeKYZNtLzCxpv1LyD](https://www.coinbase.com/join/5a8e4a045b02c403bc3a9c0c)
