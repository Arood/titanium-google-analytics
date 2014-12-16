# Google Analytics Module

Connects to Google Analytics for app event tracking in a Titanium Appcelerator Application. The 2.x release of this module is targeted for the [Google Analytics SDK for iOS v3](https://developers.google.com/analytics/devguides/collection/ios/v3/) and [Google Analytics SDK v4 for Android](https://developers.google.com/analytics/devguides/collection/android/v4/).

## Set up Google Analytics

First step: you have to create a new app property in your Google Analytics-account. Refer to [this page](http://support.google.com/analytics/bin/answer.py?hl=en&answer=2587086&topic=2587085&ctx=topic) for more information.


## Usage

To access this module from JavaScript, you need to do the following:

```javascript
var GA = require("analytics.google");
```

The GA variable is a reference to the Module object. To interact with the tracker, create a tracker instance using the `getTracker` method.

```javascript
var tracker = GA.getTracker("UA-1234567-8");
tracker.trackEvent({
  category: "Loading",
  action: "cancelled"
});
```

## Reference

### Module.trackUncaughtExceptions

Set this property on the module to true when you want to enable exception tracking. iOS only.

```javascript
var GA = require("analytics.google");
GA.trackUncaughtExceptions = true; // ios only
```

### Module.optOut

Set this property to true if you want to disable analytics across the entire app.

```javascript
var GA = require("analytics.google");
GA.optOut = false;
```

### Module.dryRun

Set this property to true if you want to enable debug mode. This will prevent sending data to Google Analytics.

```javascript
var GA = require("analytics.google");
GA.dryRun = true;
```

### Module.dispatchInterval

Data collected using the Google Analytics SDK for Android is stored locally before being dispatched on a separate thread to Google Analytics. By default, data is dispatched from the Google Analytics SDK for Android every 30 minutes. By default, data is dispatched from the Google Analytics SDK for iOS every 2 minutes. Set this property if you would like to change the interval in seconds.

```javascript
var GA = require("analytics.google");
GA.dispatchInterval = 900; // seconds
```


### Module.getTracker(id)

Retrieve an analytics tracker object. Must pass the "UA-XXXXXXX-X" id string that you get from Google Analytics.

### Tracker.setUser(params)

Tracks a user sign in with an 'anonymous' identifier. You will be breaking Google's terms of service if you use any of the user's personal information (including email address).

| Property | Type   | Description | Required |
| -------- |:------:| ----------- |:--------:|
| userId   | String | Unique identifier | Yes |
| category | String | The event category | Yes |
| action   | String | The event action | Yes |

```javascript
tracker.setUser({
  userId: "123456",
  category: "UX",
  action: "User Sign In"
});
```

### Tracker.trackEvent(params)

Tracks an event taking the following parameters:

| Property | Type   | Description | Required |
| -------- |:------:| ----------- |:--------:|
| category | String | The event category | Yes |
| action   | String | The event action | Yes |
| label    | String | The event label | No |
| value    | Integer | The event value | No |

```javascript
tracker.trackEvent({
  category: "category",
  action: "test",
  label: "label",
  value: 1
});
```

### Tracker.trackSocial(params)

Tracks social interactions taking the following parameters:

| Property | Type   | Description | Required |
| -------- |:------:| ----------- |:--------:|
| network  | String | e.g. facebook, pinterest, twitter | Yes |
| action   | String | The event action | Yes |
| target   | String | The event target | No |

```javascript
tracker.trackSocial({
  network: "facebook",
  action: "action",
  target: "target"
});
```

### Tracker.trackTiming(params)

Tracks a timing taking the following parameters:

| Property | Type   | Description | Required |
| -------- |:------:| ----------- |:--------:|
| category | String | The event category | Yes |
| time     | Number | In milliseconds | Yes |
| name     | String | The event name | No |
| label    | String | The event label | No |

```javascript
tracker.trackTiming({
  category: "Loading",
  time: 150,
  name: "",
  label: ""
});
```

### Tracker.trackScreen(params)

Tracks a screen change using the screen's name.

| Property | Type   | Description | Required |
| -------- |:------:| ----------- |:--------:|
| screenName | String | The name of the screen you want to record | Yes |

```javascript
tracker.trackScreen({
  screenName: "Home Screen"
});
```

### Tracker.trackTransaction(params)

Tracks an ecommerce transaction.

| Property | Type   | Description | Required |
| -------- |:------:| ----------- |:--------:|
| transactionId | String | A unique identifier | Yes |
| affiliation   | String | A category for the transaction. e.g. StoreFront | Yes |
| revenue       | Number | The total revenue including tax and shipping | Yes |
| tax           | Number | The amount of tax in the transaction | Yes |
| shipping      | Number | The amount of shipping in the transaction | Yes |
| currency      | String | The currency code. e.g. USD, CAD, etc. | No |

```javascript
tracker.trackTransaction({
  transactionId: "123456",
  affiliation: "Store",
  revenue: 24.99 * 0.7,
  tax: 0.6,
  shipping: 0,
  currency: "CAD"
});
```

### Tracker.trackTransactionItem(params)

Tracks an ecommerce transaction's item.

| Property | Type   | Description | Required |
| -------- |:------:| ----------- |:--------:|
| transactionId | String | A unique identifier that matches the identifier used in `trackTransaction` | Yes |
| name          | String | The name of the item | Yes |
| sku           | String | A reference number for the item | Yes |
| category      | String | A category for the item | No |
| price         | Number | The price of the item | Yes |
| quantity      | Number | The number of items purchased | Yes |
| currency      | String | The currency code. e.g. USD, CAD, etc. | No |

```javascript
tracker.trackTransactionItem({
  transactionId: "123456",
  name: "My Alphabet Book",
  sku: "ABC123",
  category: "product category",
  price: 24.99,
  quantity: 1,
  currency: "CAD"
});
```

### Custom Dimensions and Metrics

The above method parameters can be extended using customMetric and customDimention properties. For example:

```javascript
tracker.trackSocial({
  network: "facebook",
  action: "action",
  target: "target",
  customDimension: {
    "1": "Ottawa",
    "2": "Toronto"
  },
  customMetric: {
    "1": 45.4,
    "2": 68.3
  }
});
```

As per the Google Analytics API, custom metrics and dimensions are 1-based. Each property is expected to be defined by a string representation of the numeric index with its corresponding value. Metric values are expected to be numeric and Dimension values are expected to be strings.

## Google Play Services

Version 2.1.0 of the android module uses Google Play Services Version 6.1 (rev 21 - 6171000).

## Authors

* [Matt Tuttle](https://github.com/MattTuttle "Matt Tuttle")
* [Adam St. John](https://github.com/astjohn "Adam St. John")

## License

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
