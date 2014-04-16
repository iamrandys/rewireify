# Rewireify

Rewireify is a port of [Rewire](https://github.com/jhnns/rewire) for [Browserify](http://browserify.org/) that adds setter and getter methods to each module so that their behaviour can be modified for better unit testing. With Rewireify you can:

- Inject mocks for other modules
- Leak private variables
- Override variables within the module

## Usage

First install and save Rewireify into your project dependencies:

```sh
$ npm install rewireify --save-dev
```

Include the Rewireify transform as part of your Browserify test build:

```sh
$ browserify -e app.js -o test-bundle.js -t rewireify -s test-bundle
```

Now you can inspect, modify and override your modules internals in your tests:

```js
var bundle = require("./path/to/test-bundle");

// Private variables can be leaked...
subject.__get__("secretKey");

// ...or modified
subject.__set__("secretKey", 1234);

// Multiple, nested properties can be changed
subject.__set({
  "user.firstname": "Joe",
  "user.lastname": "Bloggs"
});

// Dependencies can be mocked...
subject.__set__("config", {
  cache: false,
  https: false
});

// ...or methods stubbed
subject.__set__("http.get", function(url, cb) {
  cb("This method has been stubbed");
});
```

## API

#### rewiredModule.__get__(name)

- `name`

    Name of the variable to get. The variable should be defined with var in the top-level scope of the module.

#### rewiredModule.__set__(name, value)

- `name`

    Name of the variable to set. The variable should be defined with var in the top-level scope of the module.
- `value`

    The value to set.

#### rewiredModule.__set__(map)

- `map`

    Takes all keys as variable names and sets their values respectively.
