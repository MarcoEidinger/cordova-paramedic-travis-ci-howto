# Guide to leverage cordova-paramedic for Travis CI

Dear Cordova lovers,

are you developing your own Cordova plugin(s)? Are you interested in continous integration options for your Cordova plugins?Then please continue to read :)

![Target setup](https://github.com/MarcoEidinger/cordova-paramedic-travis-ci-howto/raw/master/cordova-paramedic-travis-ci.jpeg "Target setup")

The idea is to write tests for a Cordova plugin and execute these tests in Travis CI for every commit in the public Github repository in which the Cordvoa plugin and tests are stored.

But lets start slow and anwer some questions first

## How to write tests?
[Cordova-plugin-test-framework](https://github.com/apache/cordova-plugin-test-framework) allows you to write unit/integration tests for your Cordova plugin in form, what to you think, a Cordova plugin :)

Tests are written in Jasmine and will cover the JS as well as the native code of your 'plugin under test'.

Three simple steps to setup such Cordova plugin hosting tests 'plugin under test':

* create a `tests` folder in your 'plugin under test' repository
* add a plugin.xml file inside the `tests` folder to define it as a Cordova plugin containing your tests
* add a JS file including your tests written in Jasmine

As an example have a look at one of my plugins, i.e. [cordova-disable-copy-paste](https://github.com/MarcoEidinger/cordova-disable-copy-paste)

## How to run tests?
You can use the  [cordova-template-test-framework](https://www.npmjs.com/package/cordova-template-test-framework) to easily create a test application containing your 'plugin under test' as well as your 'test plugin' locally. Starting the test application will show you the test results.

Hold on, there is even an easier way. You can use [cordova-paramedic](https://github.com/apache/cordova-paramedic). Recommendation is to install cordova-paramedic globally with NPM:   `npm install -g cordova-paramedic`

Example:
```bash
git clone https://github.com/MarcoEidinger/cordova-disable-copy-paste.git
cd cordova-disable-copy-paste
cordova-paramedic --platform ios --plugin /Users/travis/build/MarcoEidinger/cordova-disable-copy-paste
```

cordova-paramedic will create a Cordova test application for the specified platform, adds the specified plugin as well as the the related test plugin (in the tests folder), deploys the Cordova app in the simulator/emulator and prints out the results of your test cases.

### How to combine this nwo for Continous Integration?

The final step is to leverage cordova-plugin-test-framework and cordova-paramedic for continuous integration. For your public GitHub repositories a good choice is Travis CI. [Travis CI](https://travis-ci.org/) is a hosted, continuous integration service used to build and test software projects hosted on GitHub.

Login to Travis CI, and the follow their steps:

![Travis CI setup](https://github.com/MarcoEidinger/cordova-paramedic-travis-ci-howto/raw/master/travisinstructions.png "Travis CI setup")

Example [.travis.yml](https://github.com/MarcoEidinger/cordova-disable-copy-paste/blob/master/.travis.yml) for iOS build

```bash
language: objective-c
node_js:
  - "4.2"
install:
  - npm install -g cordova
  - npm install -g ios-sim
  - npm install -g ios-deploy
  - npm install -g cordova-paramedic
  - npm install
script:
  - cordova-paramedic --platform ios --plugin /Users/travis/build/MarcoEidinger/cordova-disable-copy-paste
```

I hope this guide is helpful for you.

Best regards,
[Marco Eidinger](https://eidinger.us)
