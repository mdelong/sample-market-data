# Market Data Generator

The `marketdata.py` data generator pulls live pricing data from the [Yahoo Finance API](https://github.com/ranaroussi/yfinance) **every 60 seconds**, and uses the [Kafka Python client](https://kafka-python.readthedocs.io/en/master/) (`kafka-python`) to push events into Redpanda.

**Sample Event:**

```javascript
{
	"ticker": "AAPL",
	"bid": 164.75,
	"ask": 165.21,
	"currentPrice": 165.12
}
```

The price events can be enriched with the sample security reference data (also pre-retrieved from the Yahoo Finance API) provided in [`/data`](../data) based on the `symbol` field.

**Example:**

```javascript
{
	"icao24":"c00734",
	"manufacturericao":"AIRBUS",
	"manufacturername":"Airbus",
	"model":"ACJ319 115X",
	"typecode":"A319",
	"icaoaircrafttype":"L2J",
	"operator":"Qatar Executive",
	"operatorcallsign":"",
	"operatoricao":"QQE",
	"built":"2009-01-01",
	"categorydescription":"Large (75000 to 300000 lbs)"
}
```

## Tweaking the code

Since the Python code is volume mounted in the container, you can simply stop and restart the application inside the container to pick up any changes.

To change the subscribed list of securities or the real-time fields to subscribe to, you can update the respective `SUBSCRIBED_TICKERS` and `SUBSCRIBED_FIELDS` environment variables in the [`.env`](../.env) file and restart the docker-compose network.
