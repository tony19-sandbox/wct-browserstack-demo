### About

This project demonstrates [Web Component Tester 4.2.2](https://github.com/Polymer/web-component-tester) with [BrowserStack](http://browserstack.com).

### Usage

 1. Edit `default-browserstack-browsers.json` to select the browsers to be tested. You could use the [BrowserStack Config Generator](https://www.browserstack.com/automate/node#setting-os-and-browser).

 2. From a terminal, run:

        $ BROWSERSTACK_KEY=... BROWSERSTACK_USER=... gulp wct:browserstack

    where:
     * `BROWSERSTACK_KEY` is your BrowserStack access key from your profile settings
     * `BROWSERSTACK_USER` is your BrowserStack username

      The output looks something like this:

          $ BROWSERSTACK_USER=... BROWSERSTACK_KEY=... gulp wct:browserstack
          [21:27:24] Using gulpfile ~/src/polymer/wct-browserstack-demo/gulpfile.js
          [21:27:24] Starting 'wct:browserstack'...
          [21:27:24] Starting 'starttunnel'...
          [21:27:25] Finished 'starttunnel' after 1.25 s
          [21:27:25] Starting 'wct:sauce'...
          Web server running on port 2000 and serving from /Users/tony/src/polymer/wct-browserstack-demo
          chrome                   Beginning tests via http://localhost:2000/components/wct-browserstack-demo/generated-index.html?cli_browser_id=0
          chrome                   Tests passed
          Test run ended with great success
          
          chrome (4/0/0)
          [21:27:48] Finished 'wct:sauce' after 23 s
          [21:27:48] Starting 'stoptunnel'...
          [21:27:49] Finished 'stoptunnel' after 489 ms
          [21:27:49] Finished 'wct:browserstack' after 25 s

  **NOTE:** WCT uses `"wct:sauce"` as the task name to start the tests. It's not actually running tests on SauceLabs in this case.

### Notes

 * Based on [Polymer Starter Kit 1.3.0](https://github.com/PolymerElements/polymer-starter-kit/releases/tag/v1.3.0) with these changes:
   - Added `default-browserstack-browsers.json`
   - Added Gulp task (`wct:browserstack`)
   - Added dependency on [`gulp-browserstack`](https://www.npmjs.com/package/gulp-browserstack)
   - Modified `wct.conf.js` to setup BrowserStack
 * Tests are run serially (same as SauceLabs tests) even though BrowserStack supports parallel tests

### Manual Testing

Prefer the hard way?

 1. Download the [BrowserStack local binary](https://www.browserstack.com/local-testing#command-line).
 2. From a terminal, run:

        $ ./BrowserStackLocal ${access_key}

    The tunnel will stay open for the lifetime of this command (<keybd>CTRL</keybd>-<keybd>C</keybd> to stop).

 3. In `wct.conf.js`, modify the `ret` object so that it contains an array named `activeBrowsers`. Each array entry must at least contain the following properties:

         {
           browserName: '...',
           platform: '...',
           url: 'http://${BROWSERSTACK_USER}:${BROWSERSTACK_KEY}@hub.browserstack.com/wd/hub',
           'browserstack.local': 'true'
         }

    Example:

        var ret = {
          'suites': ['app/test'],
          'webserver': {
            'pathMappings': []
          },
          activeBrowsers: [
            {
              browserName: "chrome",
              platform: "windows",
              'browserstack.local' : 'true',
              url: 'http://johndoe:abcde0123456789abcde@hub.browserstack.com/wd/hub'
            },
            {
              'browserName' : 'android',
              'platform' : 'ANDROID',
              'device' : 'Samsung Galaxy S5',
              'browserstack.local' : 'true',
              url: 'http://johndoe:abcde0123456789abcde@hub.browserstack.com/wd/hub'
            }
          ]
        };

4. From a terminal, run:

        $ BROWSERSTACK_KEY=... BROWSERSTACK_USER=... gulp test:remote
