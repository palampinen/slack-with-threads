# Slack With Threads
This is guide how to achieve Flowdock styled threads in Slack.
*Guide is only for Slack Mac App.*

# How to

open `/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/index.js`

After startup function add:

```js
var loadCustomStyle = function() {
  var cssURL = 'https://gist.githubusercontent.com/palampinen/733c88d530fcc9bead0030204f0763c6/raw/38fe0f9453c51caa34d52e8428406b4e17211f64/threads.css';
  var cssPromise = fetch(cssURL).then(response => response.text());

  var webviews = document.querySelectorAll(".TeamView webview");
  webviews.forEach(webview => {
    webview.addEventListener('ipc-message', message => {
      if (message.channel == 'didFinishLoading') {
        cssPromise.then(function(css) {
          webview.insertCSS(css);
        });
      }
    });
   });
}
```

And add function call after startup()
```js
document.addEventListener("DOMContentLoaded", function() { // eslint-disable-line
  try {
    startup();
    loadCustomStyle(); // <-- Add this line
  } catch (e) {
    console.log(e.stack);

    if (window.Bugsnag) {
      window.Bugsnag.notifyException(e, "Renderer crash");
    }

    throw e;
  }
});
```


# What this does
Loads css file from github server and inserts CSS (https://electron.atom.io/docs/api/webview-tag/#webviewinsertcsscss) to webviews. This might be considered dangerous since changes in file could mess your Slack client styles. CSS file could also be local or you can paste css code straight to index.js file and insert it without fetching.

