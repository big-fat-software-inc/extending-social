# extending-social
Chrome Extension for enhancing Social Media experience while improving productivity

# Objective
To provide a step-by-step tutorial in creating a chrome-extension that changes
(i) how facebook appears to users
(ii) Later on, we may extend other social media websites
(iii) Monitor usage and warn users if they're getting distracted too much

# Table of Contents
* [Getting started with creating a chrome-extension]()
  * [Hello World]()
  * [Injecting Scripts and CSS]()
  * [Using jQuery to manipulate some DOM elements]()
  * [Reloading extension]()
* [Getting started with React]()
  * [Why and how to use React]()

## Getting started with creating a chrome-extension
> **Note:** Creating a chrome-extension is very easy. Don't get intimidated by the humongous documentation.

* Create a folder, `git init` if you prefer to do so
* Create a new file, save it as `manifest.json`
* Add a 19x19px `.png` image, preferably with transparent background to the folder. If you can't make one, download it from the [Getting Started: Building a Chrome Extension](https://developer.chrome.com/extensions/getstarted) guide. Here is the [link to the image](https://developer.chrome.com/extensions/examples/tutorials/getstarted/icon.png).
* We're almost ready for our `helloworld` extension

> Read More about [manifest file format](https://developer.chrome.com/extensions/manifest)

### Hello World
#### Creating the manifest file
Let's go to the `manifest.json` file and add few lines of code.
```javascript
{
  "manifest_version": 2,
  "name": "Hello World",
  "description": "Our first hello world chrome extension",
  "version": "1.0"
  ...
}
```
**What we did**: We provided a `manifest_version`, which is `2` and that is going to be default as of now for all chrome-extensions. We gave our extension a `name`, a `description` and a `version`. This `version` number will be of importance while publishing upgrades to our chrome plugin in the play store.

> **Bonus Tip:** The `default_icon` property can be changed dynamically during run-time. We'll read more about this in [another section](link unavailable)

> **Note:** Now onwards, keep in mind that the three dots mean we're going to add more code in place of them. So, let's go ahead and add some more declarations in place of the `...`

#### Setting the browser action
```javascript
  "browser_action": {
    "default_icon": "icon.png",
    "default_title": "Click here!"
  },
  ...
```
**What we did**: We created another property `browser_action` in the `manifest.json`. `browser_action` property is responsible for making the icon visible all the time on the main Google Chrome toolbar i.e. to the right of the address bar. And, additionally, it allows us to set the tooltip by setting the `default_title`, and the icon`default_icon` properties. Whenever our users hover on our icon, they'll see the value against `default_title`. [Read more about `browser_action`](https://developer.chrome.com/extensions/browserAction).

#### Setting permissions for the extension
The `permissions` property lets the extension have temporary access to the currently active tab if the user invokes the extension. Thus, it prevents the extension from requesting full, persistent access to every web site, which could pose a potential threat.

Additionally, we can allow the extension to operate on all `http` and `https` sites.
```javascript
  "permissions": [
    "activeTab",
    "http://*/",
    "https://*/"
  ],
  ...
```

If you wish to restrict the operation only to `facebook`, then you can do the following:
```javascript
  "permissions": [
    "activeTab",
    "https://???.facebook.com/"
  ],
  ...
```
### Injecting Scripts and CSS
Let's go and create two files, a `.js` and a `.css` file in the same directory where we've kept our `manifest.json` file.
**CSS**: Let's color the search bar `red`. Go and inspect the search bar in `facebook`. You'll notice the search bar's properties. Now, in order to override the existing CSS specificity, we'll have to use `!important` in our css.

```css
div[role="search"] {
  background: #F00 !important;
  border-radius: 0px;
}
```
Create a file, code that up & save the file as `mystyles.css`.

**JS**:
Let's say, we want an `alert("Hello World")` to check if our extension is working.
So, create a file, code that up & save the file as `myscript.js`.

Now, we've to specify when the code shall run. Let's do it after the DOM loading is complete. We do that by specifying the `run_at` property, which can take 

```javascript
 "content_scripts": [
   {
     "matches": ["https://www.facebook.com/*"],
     "css": ["mystyles.css"],
     "js": ["myscript.js"],
     "run_at": "document_end"
   }
 ]
  ...
```
> **Notice:** The `js` and `css` properties are arrays. You can specify multiple `css` and `js` files in them. Please be mindful of the sequence, because the files get loaded in the same order as they're ordered in the array. Example - If you've plans to use `jQuery`, include the file in the first entry, before entering any other scripts into the array.

#### Loading the extension
So, if you've followed all steps correctly, your final `manifest.json` should look like

```javascript
{
  "manifest_version": 2,
  "name": "Hello World",
  "description": "Our first hello world chrome extension",
  "version": "1.0",

  "browser_action": {
    "default_icon": "icon.png",
    "default_title": "Click here!"
  },

  "permissions": [
    "activeTab",
    "http://*/",
    "https://*/"
  ],

  "content_scripts": [
    {
      "matches": ["https://www.facebook.com/*"],
      "css": ["mystyles.css"],
      "js": ["myscript.js"],
      "run_at": "document_end"
    }
  ]
}
```
* Go to `chrome://extensions/`
* Select (check) the `Developer Mode` checkbox located on the top-right-corner.
* Click on the `Load Unpacked Extension` button
* Browse and locate the directory you're creating your extension in
* Click select
* Now, open `https://www.facebook.com/` and you'll see the search bar is red and the alert saying `Hello World!`

Congratulations! You've created your first `Hello World` extension.

### Using jQuery to manipulate some DOM elements]()
Now that we've created our first extension, let's add some more action to it.
In this section we'll do the following:
* Create and append burger-menu button to the header
* Hide the left-nav and right-nav
* Set the main container to 100% of the screen width
* Show the left-nav when the user clicks on burger-menu

---------------------

Now, since we're developing an extension for facebook which will act behind the scenes, active presence of the icon is not required on the Chrome toolbar. So, let's make some changes.

Change `browser_action` to `page_action`. This is how it should look like.
```javascript
  "browser_action": {
    "default_icon": "icon.png",
    "default_title": "Click here!"
  },
  ...
```
Save the `manifest.json` file and go to `chrome://extensions/` and hit `reload`. As soon as you reload, and go back to the tab where `facebook` was open, you will notice the icon disappeared from the Chrome toolbar.
