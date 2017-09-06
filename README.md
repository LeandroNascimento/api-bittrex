# Api Bittrex

A Node.js API client for the [https://github.com/ionic-team/ionic][Ionic Framework].

The client supports public and private calls.

For private calls, the user secret is never exposed to other parts of the program or over the Web. The user key is sent as a header to the API, along with a signed request.

Repo home: [github.com/leandronascimento/api-bittrex][repo]


## License

This Library was created by using parts of [Adrian Soluch (@n0mad01)](https://github.com/n0mad01/) [soluch.us](http://soluch.us) [node.bittrex.api] (https://github.com/n0mad01/node.bittrex.api/blob/master/node.bittrex.api.js) MIT, open source. See LICENSE file.

## Make API calls

Before you start
----
This is just a quick reminder that you are handling coins with this library (and thus real money), so, understand the situation as much as possible and make everything to prevent losing them.

Here is a small checklist you should go through before you start:

1. Make sure you don't give your api key more rights as absolutely necessary - for first testing READ INFO alone should be enough! (bittrex.com under: Settings/API Keys)
![bittrex_ap_keys_control](https://user-images.githubusercontent.com/260321/29748739-a6c2c00e-8b1c-11e7-95ec-1b0221348235.png)
2. make sure to understand the API Key permissions
    1. READ INFO - Allows you to read private details such as open orders, order history, balances, etc
    2. TRADE LIMIT - Allows you to create/cancel trade limit buy/sell orders
    3. TRADE MARKET - allows you to create/cancel market buy/sell orders
    4. WITHDRAW - Allows you to withdraw to another address
3. Make use of the Bittrex IP Whitelist as also the Withdrawal Whitelist features
4. Do not ever commit your API Keys to GitHub or expose them under any circumstances!

Quick start
----
```sh
$ npm install api-bittrex
```

## Importe as a module in Ionic

In your app, import the module:
 ```sh
    import * as bittrex from 'node.bittrex.api';
```
## Create an instance of the client

If only public API calls are needed, then no API key or secret is required:
    
 ```sh   
    const APISECRET = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';
    const APIKEY = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';

    bittrex.options({ 
      'apikey' : APIKEY, 
      'apisecret' : APISECRET, 
      'stream' : false, 
      'verbose' : false, 
      'cleartext' : false 
    });

    bittrex.getbalances( function( data, err ) {
      console.log( data );
    });
```
Examples
--
After configuration you can use the object right away:
example #1
```javascript
bittrex.getmarketsummaries( function( data, err ) {
  if (err) {
    return console.error(err);
  }
  for( var i in data.result ) {
    bittrex.getticker( { market : data.result[i].MarketName }, function( ticker ) {
      console.log( ticker );
    });
  }
});
```

example #2
```javascript
bittrex.getbalance({ currency : 'BTC' }, function( data, err ) {
  if (err) {
    return console.error(err);
  }
  console.log( data );
});
```


Methods
----

Optional parameters may have to be looked up at https://bittrex.com/Home/Api.

> It may happen that some Bittrex API methods are missing, also they could have been forgotten in the documentation. In this case, if this strikes you, feel free to open a issue or send me a pull request.

> Also: the method **sendCustomRequest** enables completely custom requests, regardless the specific API methods.

##### sendCustomRequest 
- url           String
- callback      Function
- credentials   Boolean     optional    whether the credentials should be applied to the request/stream or not, default is set to false.

example #1
```javascript
var url = 'https://bittrex.com/api/v1.1/public/getticker?market=BTC-LTC';
bittrex.sendCustomRequest( url, function( data, err ) {
  console.log( data );
});
```

example #2 (credentials applied to request/stream)
```javascript
bittrex.sendCustomRequest( 'https://bittrex.com/api/v1.1/account/getbalances?currency=BTC', function( data, err ) {
  console.log( data );
}, true );

will result in (the Header is being set too):
https://bittrex.com/api/v1.1/account/getbalances?currency=BTC&apikey=API_KEY&nonce=4456490600
```

##### getticker
```javascript
bittrex.getticker( { market : 'BTC-LTC' }, function( data, err ) {
  console.log( data );
});
```

##### getbalances
```javascript
bittrex.getbalances( function( data, err ) {
  console.log( data );
});
```

##### getmarkethistory
```javascript
bittrex.getmarkethistory({ market : 'BTC-LTC' }, function( data, err ) {
  console.log( data );
});
```

##### getcandles (v2 method)
```javascript
bittrex.getcandles({
  marketName: 'USDT-BTC',
  tickInterval: 'fiveMin', // intervals are keywords
}, function( data, err ) {
  console.log( data );
});
```

##### getmarketsummaries
```javascript
bittrex.getmarketsummaries( function( data, err ) {
  console.log( data );
});
```

##### getmarketsummary
```javascript
bittrex.getmarketsummary( { market : 'BTC-LTC'}, function( data, err ) {
  console.log( data );
});
```

##### getorderbook
```javascript
bittrex.getorderbook({ market : 'BTC-LTC', depth : 10, type : 'both' }, function( data, err ) {
  console.log( data );
});
```

##### getwithdrawalhistory
```javascript
bittrex.getwithdrawalhistory({ currency : 'BTC' }, function( data, err ) {
  console.log( data );
});
```

##### getdepositaddress
```javascript
bittrex.getdepositaddress({ currency : 'BTC' }, function( data, err ) {
  console.log( data );
});
```

##### getdeposithistory
```javascript
bittrex.getdeposithistory({ currency : 'BTC' }, function( data, err ) {
  console.log( data );
});
```

##### getbalance
```javascript
bittrex.getbalance({ currency : 'BTC' }, function( data, err ) {
  console.log( data );
});
```

##### withdraw
```javascript
bittrex.withdraw({ currency : 'BTC', quantity : '1.5112', address : 'THE_ADDRESS' }, function( data, err ) {
  console.log( data );
});
```