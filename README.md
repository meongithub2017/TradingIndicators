## HF Indicators

[![Build Status](https://travis-ci.org/bitfinexcom/bfx-hf-indicators.svg?branch=master)](https://travis-ci.org/bitfinexcom/bfx-hf-indicators)

This repo contains a collection of trading indicators implemented to support incremental updates, both from trade and candle data (depending on the indicator). All indicators provide the same set of base methods:

* `reset()` - clears indicator values
* `update(value or candle)` - updates the current indicator value with a different data point
* `add(value or candle)` - adds a new data point/value to the indicator
* `l()` - returns the number of available indicator values
* `v()` - returns the current indicator value
* `prev(n = 1)` - returns the nth previous indicator value

#### Indicator Seeding

All indicators have a seed period which should be respected before valid data can be obtained, which can be read via `i.getSeedPeriod()`

#### Indicator Data Types

To query which type of data an indicator requires, `getDataType()` and `getDataKey()` are available. The data type can be either `'trade'`, `'candle'`, or `'*'` to signal that both are acceptable. For candle data, the data key will be either `'open`', `'high'`, `'low'`, or `'close'`.

### Indicator Metadata

All indicators provide a set of meta values describing how they should be rendered on a chart. Specifically:

* `Indicator.args` - an array describing the indicator arguments, with objects of the form { label, default }
* `Indicator.ui` - an object describing how to render the indicator, details below
* `Indicator.id` - the internal unique identifier for the indicator
* `Indicator.label` - the short, uppercase label for the indicator
* `Indicator.humanLabel` - the complete descriptive indicator label

As an example, here is the metadata for bollinger bands:
```js
BollingerBands.id = 'bbands'
BollingerBands.label = 'BB'
BollingerBands.humanLabel = 'Bollinger Bands'
BollingerBands.ui = {
  position: 'overlay',
  type: 'bbands'
}

BollingerBands.args = [{
  label: 'Period',
  default: 20,
}, {
  label: 'Multiplier',
  default: 2,
}]
```

### Indicator Rendering

For each indicator, the `Indicator.ui` field contains an object describing how it should render on a chart, with `position` and `type` keys. Two positions are possible, `'external'` to be shown below a chart and `'overlay'` to be shown on top of candle data. Supported types are `'line'`, `'lines'`, `'bbands'`, `'rsi'`, and `'macd'`, with indicators of type `'lines'` having an extra `lines: [...]` field naming the indicator values that should be plotted.

For indicators with multiple values on each data point, such as Envelope, the `lines` keys should correspond to the keys on the indicator data points:

```js
Envelope.ui = {
  position: 'overlay',
  type: 'lines',
  lines: ['upper', 'basis', 'lower']
}
```

## Example Usage

```js
const { RSI } = require('bfx-hf-indicators')

const rsi = new RSI([14])

rsi.add(14000)
rsi.add(14010)
rsi.add(14025)
rsi.add(14035)
// ...
// 8 more data points
// ...
rsi.add(13998)
rsi.add(13952)

const v = rsi.v() // query current RSI(14) value
```

## Available Indicators
* Absolute True Range
* Acceleration
* Accumulation/Distribution
* Accumulative Swing Index
* Arnoud Legoux Moving Average
* Aroon
* Average Directional Index
* Awesome Oscillator
* Balance of Power
* Bollinger Bands
* Chaikin Money Flow
* Chaikin Oscillator
* Chande Momentum Oscillator
* Coppock Curve
* Detrended Price Oscillator
* Donchian Channels
* Ease of Movement
* Envelope
* Exponential Moving Average
* EMA Volume
* Know Sure Thing
* MACD
* Mass Index
* Momentum
* Net Volume
* On Balance Volume
* Price Channel
* Price Oscillator
* Price Volume Trend
* RSI
* Rate of Change
* Relative Vigor Index
* Relative Volatility Index
* Simple Moving Average
* Standard Deviation
* Stochastic
* Stochastic RSI
* TRIX
* True Strength Index
* VWAP
* Volume Oscillator
* Volume Weighted Moving Average
* Williams %R
