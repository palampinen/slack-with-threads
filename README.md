# Slack With Threads

Guide how to achieve Flowdock styled threads in Slack. _Guide is only for
desktop Slack Mac & Win Applications._

![Threads](https://raw.githubusercontent.com/palampinen/slack-with-threads/master/thread.png)

Tested with Slack client version 3.0.

# How to

Mac: open
`/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/ssb-interop.js`

Windows: open
`%HOME%\AppData\Local\slack\app-3.0.0\resources\app.asar.unpacked\src\static\ssb-interop.js`
_HOX check the app version in the path_

Add in bottom of file:

```
  document.addEventListener("DOMContentLoaded", function() {
    var cssURL = "https://raw.githubusercontent.com/palampinen/slack-with-threads/master/threads.css";
    var cssPromise = fetch(cssURL).then(response => response.text());

    cssPromise.then(function(css) {
      $("<style></style>")
        .appendTo("head")
        .html(css);
    });
  });
```

_Restart Slack and you should have Threads_

# What this does

Loads CSS file from github server and inserts CSS inside a style tag which is
appended to head. This might be considered dangerous since changes in file could
mess your Slack client styles. CSS file could also be local or you can paste css
code straight to index.js file and insert it without fetching.

# What is needed for this kind of UI to work

Thread messages should be posted to main chat. Unfortunately this is disabled by
default (checkbox is unchecked).

# Issues

* File/Image threads do not work like text threads.
* File/Image messages do not have general thread id (like text threads have).
  This makes it more difficult to color code file threads consistently.

# Development

## Developer menu

In your terminal run the command `launchctl setenv SLACK_DEVELOPER_MENU true`
and restart Slack. Now devloper menu is available.

Originally from https://gist.github.com/DrewML/0acd2e389492e7d9d6be63386d75dd99
