# Stylesheet API Experiment

This is a small [WebExtension](https://developer.mozilla.org/en-US/Add-ons/WebExtensions) API experiment providing basic access to Mozilla's [StyleSheetService](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XPCOM/Reference/Interface/nsIStyleSheetService). It can be used by WebExtensions to apply CSS freely to any document, including the browser UI itself. The API was originally created to allow the [paxmod](https://github.com/numirias/paxmod) Firefox theme (it's cute, check it out!) to be built as a pure WebExtension.

## Background

Mozilla is gradually dropping support for all legacy and SDK extensions in favor of the new WebExtension standard. On the downside, the features of many powerful extensions can't be rewritten with the existing WebExtension APIs and many are too intrusive to ever be allowed in the future. However, one possible workaround is to write most parts of an add-on as a pure WebExtension and create individual APIs for unsupported features. So I made this API to enable [paxmod](https://github.com/numirias/paxmod) to modify the browser skin without restrictions. Read more about WebExtensions experiments [here](https://webextensions-experiments.readthedocs.io/en/latest/).

## Installation

- Browse the [latest release](https://github.com/numirias/stylesheet-api-experiment/releases/latest) and download the `.xpi` extension file.

- Install the extension into Firefox. Make sure you use either Firefox Developer or Nightly, **and** have extension signature checks disabled. (Go to `about:config` and set `xpinstall.signatures.required` to `false`.) That's necessary because this API needs to be implemented as a legacy extension and hence can't get signed by Mozilla.

 You can also clone the repo and zip the extension yourself:

      $ git clone https://github.com/numirias/stylesheet-api-experiment
      $ cd stylesheet-api-experiment
      $ zip stylesheet-api.xpi api.js install.rdf schema.json
      $ firefox-developer stylesheet-api.xpi


## Usage

(If you want to use this API in your own WebExtension, note that you can't sign and distribute your extension through Mozilla anymore, since you're using an unverified experimental API. Also, any user who uses your extension will need to have this API installed.)

In your `manifest.json`, add a permission for `experiments.stylesheet`:

    ...
    "permissions": [
      ...
      "experiments.stylesheet",
    ],

The API will be available in the `browser.stylesheet` namespace. Examples:

    let url = browser.extension.getURL("style.css");
    browser.stylesheet.load(url, "AUTHOR_SHEET");
    browser.stylesheet.unload(url, "AUTHOR_SHEET");
    browser.stylesheet.isLoaded(url, "AUTHOR_SHEET").then(loaded => {
      console.log("Sheet loaded:", loaded);
    });

Have a look at [`schema.json`](https://github.com/numirias/stylesheet-api-experiment/blob/master/schema.json) for details on the API schema. Also be aware that all functions are asynchronous.

---

Finally, note that I'm neither a seasoned extension developer nor a JS veteran, so use at your own risk and please let me know about any flaws.
