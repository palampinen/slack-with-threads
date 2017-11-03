# Slack With Threads
Guide how to achieve Flowdock styled threads in Slack. *Guide is only for desktop Slack Mac & Win Applications.*

![Threads](https://raw.githubusercontent.com/palampinen/slack-with-threads/master/thread.png)


# How to

Mac: open `/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/index.js`
Win: open `%HOME%\AppData\Local\slack\app-2.8.2\resources\app.asar.unpacked\src\static`

After startup function add:

```js
var loadCustomStyle = function() {
  var cssURL = 'https://raw.githubusercontent.com/palampinen/slack-with-threads/master/threads.css';
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

# What is needed for this kind of UI to work
Thread messages should be posted to main chat. Unfortunately this is disabled by default (checkbox is unchecked).

# Issues
* File/Image threads do not work like text threads.
* File/Image messages do not have general parent id (text threads have). This makes it more difficult to have same color for file threads.

# Development

## Developer menu
In your terminal run the command `launchctl setenv SLACK_DEVELOPER_MENU true` and restart Slack. Now devloper menu is available.



Originally from https://gist.github.com/DrewML/0acd2e389492e7d9d6be63386d75dd99
