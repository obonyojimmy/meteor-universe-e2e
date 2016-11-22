<p align="center"><a href="http://unicms.io"><img src="http://unicms.io/banners/standalone.png" height="300"></a></p>

# Universe:E2E

Complete end-to-end/acceptance testing solution for Meteor: Mocha/Chai & Selenium/WebdriverIO

This package is currently in early beta.

<!-- toc -->

- [Why?](#why)
- [Installation](#installation)
- [Usage](#usage)
  * [Running tests in watch mode](#running-tests-in-watch-mode)
  * [Running tests in Continuous Integration mode](#running-tests-in-continuous-integration-mode)
  * [Meteor "full application test mode" caveats](#meteor-full-application-test-mode-caveats)
- [Writing tests](#writing-tests)
- [Exported variables](#exported-variables)
- [Batteries included](#batteries-included)
  * [Mocha](#mocha)
  * [Chai](#chai)
  * [Selenium](#selenium)
  * [WebdriverIO](#webdriverio)
- [Roadmap](#roadmap)

<!-- tocstop -->

### Why?

From [Wikipedia](https://en.wikipedia.org/wiki/End-to-end_testing):

> End-to-end Testing (E2E) is type of software testing used to validate different integrated components of an application by testing the flow from start to end. It also tests the behavior according to the user requirements.

From [Meteor guide](https://guide.meteor.com/testing.html#acceptance-testing):

> Acceptance testing is the process of taking an unmodified version of our application and testing it from the “outside” to make sure it behaves in a way we expect. Typically if an app passes acceptance tests, we have done our job properly from a product perspective.

There are other software that would allow you to perform E2E/acceptance tests of your Meteor app (e.g. Chimp, Nightwatch, Starrynight) but we found them really cumbersome.

This package is using test drivers introduced with Meteor 1.3 and integrates more seamlessly with whole Meteor stack.

Tests are started and everything is managed from within the Meteor app, so when writing test specs you can use everything you would normally use in your app.

### Installation

`universe:e2e` is a [test driver package](https://guide.meteor.com/testing.html#driver-packages) for Meteor.

You need to `meteor add` it to your app, but it does nothing unless you specify it while starting Meteor in test mode.

```
meteor add universe:e2e
```

This package won't be bundled with your production build, nor loaded during normal development (it has `testOnly` flag).

### Usage

#### Running tests in watch mode

To run tests you need to start Meteor in [full app test mode](https://guide.meteor.com/testing.html#test-modes).

Example command could look like this (you probably want to add this in [`npm scripts`](https://docs.npmjs.com/cli/run-script)):

```
meteor test --full-app --driver-package universe:e2e --raw-logs
```

Raw logs flag is optional, but it helps with displaying test results.

In watch mode app will start and reload on any file change (either in app code or in tests).
You need to stop it as you would normally stop Meteor in development mode.

If you want your tests running at the same time you work on your app, you can start them on different port (e.g. using `--port 4000` flag).

#### Running tests in Continuous Integration mode

This package is developed to use with CI servers in mind.

In this scenario you probably want to run the tests once and exit with code depending on tests results.
This could be achieved with:

```
CI=1 meteor test --full-app --driver-package universe:e2e --once --raw-logs
```

`raw-logs` are still optional. Note `--once` flag and the `CI` environment variable - it must be set to truthy value (but it usually already is by CI providers).

Otherwise app won't stop with correct exit code after the tests end.

#### Meteor "full application test mode" caveats

Meteor in this mode will start your application as it normally would (but with empty DB after each start, it keeps DB data during restarts in watch mode).

It will also load files matching `*.app-test[s].*` and `*.app-spec[s].*`, e.g. `my-test.app-tests.js`, so we will put our testing code over there.

`Meteor.isE2E` flag is set to `true` (useful if you want to distinguish between tests made for this package and for some other test driver)

### Writing tests

Examples coming soon...

### Exported variables

Complete list of available exported variables **on the server side**:

- Mocha API
    - `after`
    - `afterEach`
    - `before`
    - `beforeEach`
    - `context`
    - `describe`
    - `it`
    - `specify`
    - `xcontext`
    - `xdescribe`
    - `xit`
    - `xspecify`
- Chai API
    - `assert`
    - `chai`
    - `expect`
- WebdriverIO API
    - `browser`

### Batteries included

This package intention is to give everything required to write acceptance tests from within the Meteor app.

Below you find quick info about software we bundle to make this package work for you out of the box.

If you need any extra libs/helpers (`faker` etc.) you can import them from npm as you would normally do in a Meteor app.

#### Mocha

> Mocha is a feature-rich JavaScript test framework, making asynchronous testing simple and fun. Mocha tests run serially, allowing for flexible and accurate reporting, while mapping uncaught exceptions to the correct test cases.

Mocha is used to run all test suites in `universe:e2e`.

If you want some test to be executed you better have it inside `describe`/`it` block.

Docs can be found at [mochajs.org](https://mochajs.org/)

#### Chai

> Chai is a BDD / TDD assertion library for node and the browser that can be delightfully paired with any javascript testing framework.

Mocha doesn't come with any assertion library, so we need to bring our own.

We decided to use Chai, but if have other preferences feel free to ignore this dependency.

Chai docs about assertion styles can be found at [chaijs.com](http://chaijs.com/guide/styles/) 

#### Selenium

> Selenium is a suite of tools to automate web browsers across many platforms.

When using `universe:e2e` you don't have to interact with Selenium directly.

Everything is taken care of, and selenium is installed and started under the hood right before your tests starts.

#### WebdriverIO

> WebdriverIO lets you control a browser or a mobile application with just a few lines of code. Your test code will look simple, concise and easy to read. 

WebdriverIO is core of the whole package.

Using it's `browser` object you can manipulate real browsers inside your tests, making sure that everything is your app is working correctly.

Full API can be found at [webdriver.io](http://webdriver.io/api.html)

### Roadmap

- [] Write test examples
- [] Write tests for test driver :)
- [] Make sure/write guide for most popular CI services
- [] Allow to customize settings for mocha, selenium, webdriverio etc.
- [] Better Meteor/React aware location strategy - probably as an external package
