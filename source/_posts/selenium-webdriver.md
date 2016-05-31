---
title: selenium-webdriver
date: 2016-04-26 08:51:57
tags: 
	- tester
	- selenium-webdriver
---
* 延迟至某条件达成后执行
````
driver.wait(function () {
    return driver.isElementPresent(webdriver.By.name("username"));
}, timeout);
/*==========*/
driver.wait(until.elementLocated(By.name('username')), 5 * 1000).then(function(elm) {
    elm.sendKeys(username);
});
/*==========*/
driver.findElement(webdriver.By.id(element)).then(function(webElement) {
        console.log(element + ' exists');
    }, function(err) {
        if (err.state && err.state === 'no such element') {
            console.log(element + ' not found');
        } else {
            webdriver.promise.rejected(err);
        }
    });
}
````

````
// Import Selenium Dependency
var Webdriver = require('selenium-webdriver');
 
// Few Settings
var PAGE_LOAD_TIMEOUT_MS = 10000;
var SCRIPT_LOAD_TIMEOUT_MS = 1000;
var WINDOW_WIDTH_PX = 1024;
var WINDOW_HEIGHT_PX = 768;
 
// Initialize a webdriver with desired capabilites
var driver = new Webdriver.Builder().withCapabilities(Webdriver.Capabilities.firefox()).build();
// Manage timeouts of the webdriver instance
driver.manage().timeouts().pageLoadTimeout(PAGE_LOAD_TIMEOUT_MS);
driver.manage().timeouts().setScriptTimeout(SCRIPT_LOAD_TIMEOUT_MS);
// Manage window settings of the webdriver instance
driver.manage().window().setSize(WINDOW_WIDTH_PX, WINDOW_HEIGHT_PX);
 
// Request to render google.com
driver.get("http://google.com").then(function () {
        // On Success get the title of the rendered page
    driver.getTitle().then(function (title) {
            // On Success log it out
        console.log(title);
    });
});
- See more at: http://itsallabtamil.blogspot.com/2014/10/selenium-webdriver-in-nodejs-javascript.html#sthash.QYKh3mjf.dpuf
````

````

Wednesday, October 8, 2014
Selenium Webdriver in Nodejs + Javascript
Phewww getting back to blog after almost an year.
So, am back to discuss an interesting node module selenium-webdriver [Ref: Project Doc and npm registry]
Am assuming the reader has prior knowledge on what nodejs, npm and node modules are. And a little bit of selenium fun ;)
What is selenium?
As wiki says, Selenium is a portable software testing framework for web applications.
It is a prominently used tool for browser automation. For further details checkout SeleniumHQ.

What are we doing here?
Basically Selenium provides bindings in many languages like java, python, php and now even nodejs. 
Here am planning to run through few code samples of using selenium webdriver in nodejs.

Where to start?

im@mylaptop$ npm install --save selenium-webdriver

A Simple Webdriver Example

// Import Selenium Dependency
var Webdriver = require('selenium-webdriver');
 
// Few Settings
var PAGE_LOAD_TIMEOUT_MS = 10000;
var SCRIPT_LOAD_TIMEOUT_MS = 1000;
var WINDOW_WIDTH_PX = 1024;
var WINDOW_HEIGHT_PX = 768;
 
// Initialize a webdriver with desired capabilites
var driver = new Webdriver.Builder().withCapabilities(Webdriver.Capabilities.firefox()).build();
// Manage timeouts of the webdriver instance
driver.manage().timeouts().pageLoadTimeout(PAGE_LOAD_TIMEOUT_MS);
driver.manage().timeouts().setScriptTimeout(SCRIPT_LOAD_TIMEOUT_MS);
// Manage window settings of the webdriver instance
driver.manage().window().setSize(WINDOW_WIDTH_PX, WINDOW_HEIGHT_PX);
 
// Request to render google.com
driver.get("http://google.com").then(function () {
        // On Success get the title of the rendered page
    driver.getTitle().then(function (title) {
            // On Success log it out
        console.log(title);
    });
});

// Initialize a webdriver with desired capabilites
What are the default capabilities?
Checkout capabilities.js
It includes
1. BROWSER_NAME
2. SUPPORTS_JAVASCRIPT
3. SUPPORTS_CSS_SELECTORS
4. TAKES_SCREENSHOT


Webdriver.Capabilities.firefox()
Creates a new capabilities object by overriding the default BROWSER_NAME from 'browserName' to 'firefox'.

So, What are all other browser options?
Checkout Browsers Section 

Do all the browsers work by default?
Not really. To initialize a webdriver, we need to make sure that executable of desired browser is present in the system PATH.
For Ex: To use firefox, the following should be checked. If this fails, then appropriate binaries should be installed.


iam@mylaptop$ which firefox
/usr/bin/firefox

Apart from this, few browsers use separate driver executables to abstract the communication via webdriver to actual browser. For Ex: Chrome needs chromedriver. Download and make sure it is available in the system PATH.
Internally chromedriver expects google-chrome to be available in the system PATH.

What if I wish to start chrome or firefox from webdriver with specific options?

var driver = new Webdriver.Builder().withCapabilities(Webdriver.Capabilities.firefox()).setFirefoxOptions({
    firefox_profile: "Name"
}).build();

How do I manage exceptions while loading [like pageload timesout]?
Selenium webdriver is full of promises. Almost every interaction with the webdriver [irrespective of whether it is sync or async] returns a promise.
As we can see in our code,

driver.get("http://google.com").then(function success() {
    console.log('Successful');
    i++;
}, function error(err) {
    console.log(err);
}).thenCatch(function (e) { 
    console.log("Any exceptions in success or err callbacks gets thrown here", e);
    // You need to decide the resolution value here
}).thenFinally(function () {
    console.log("We can tear down stuffs here");
    // You need to decide the resolution value here. If nothing specified undefined will be returned.
});

How do I handle actions that are dependent on some conditions [like may be after some ajax calls you need click some button]?
We can ask driver to wait implicitly on a timeout basis or explicitly on a condition basis.

driver.sleep(MY_SLEEPTIMER_MS).then(function () {
    console.log("I'll do my action");
});
driver.wait(function () {
    if (document.readyState === "complete") {
        return true;
    }
}, TIMEOUT_TO_WAIT).then(function () {
    console.log("I'll do my action here");
}, function () {
    console.log("Either the condn satisfied or the TIMEOUT_TO_WAIT happened; luck is yours");
});
- See more at: http://itsallabtamil.blogspot.com/2014/10/selenium-webdriver-in-nodejs-javascript.html#sthash.QYKh3mjf.dpuf
````