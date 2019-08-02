# Nightwatch Setup
A simple step-by-step instructions in getting Nightwatch set up.

Nightwatch 1.0 and above no longer requires, nor is it recommended to use Selenium Standalone Server unless you are testing agaisnt legacy browsers, such as Internet Explorer.

Create a new directory called whatever you like(Do not call it the same as "Nightwatch" as you'll run into errors later on).

## Step 1

Run `npm init -y` as this will create a basic package.json file.
Under ```"scripts": {
    "test":```    
    Add `"Nightwatch"`


## Step 2

Install Nightwatch as a global npm module. `npm i nightwatch -g`(This will make it so you can run Nightwatch in any directory).

Within your newly created directory install your WebDriver dependencies. [Nightwatch.js](https://nightwatchjs.org/gettingstarted#installation) has a great page showcasing all webdrivers and how to install them. For now we'll just use **npm** to install Chrome's WebDriver:

## ChromeDriver
ChromeDriver can be downloaded from the [ChromeDriver Downloads](https://chromedriver.chromium.org/downloads) page. Or you can use the chromedriver NPM package as a dependency in your project:
`npm i chromedriver`

## Step 3
Create a `nightwatch.conf.js` file and insert this block of code: 
```
{
  "src_folders" : ["tests"],

  "webdriver" : {
    "start_process": true,
    "server_path": "node_modules/chromedriver/lib/chromedriver/chromedriver.exe",
    "port": 9515
  },

  "test_settings" : {
    "default" : {
      "desiredCapabilities": {
        "browserName": "chrome"
      }
    }
  }
}
```

- `src_folders` indicates where your tests are stored within your directory.
- `server_path` Is where you're storing ChromeDriver. **Note** If you run into an error ```An error occurred while trying to start ChromeDriver: cannot resolve path: ""```  Check in your `node_modules` folder to see where `chromedriver.exe` is located and update the server path. 

## Step 4

Using the preferred CSS selector model to locate elements on a page, Nightwach makes it very easy to write automated End-to-End tests.

Create a `tests` folder. Within `tests` create a .js file(This is to test to make sure everything is set up correctly and working. For now, name it `googleTest.js`).
Insert block of code to `googleTest.js`:

```
module.exports = {
  'Demo test Google' : function (browser) {
    browser
      .url('https://www.google.com')
      .waitForElementVisible('body')
      .setValue('input[type=text]', 'nightwatch')
      .waitForElementVisible('input[name=btnK]')
      .click('input[name=btnK]')
      .pause(1000)
      .assert.containsText('#main', 'Night Watch')
      .end();
  }
};
```

Remember always to call the .end() method when you want to close your test, in order for the browser session to be properly closed.

Thats it!