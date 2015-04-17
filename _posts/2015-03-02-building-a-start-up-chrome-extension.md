---
layout: post
title:  "Building a start-up chrome extension."
date:   2015-03-02 18:30:46
categories: work
shortcut: This is only my own understanding of the Chrome Extension.....
---
My own understanding of the Chrome Extension:
In a Chrome Extension, in this case we take `“gmail”` as an instance, there are some files. The structure of the file is like this: There is a manifest file, and normally, it is “manifest.json”. It defines a lot of the metadata of the extension, like name, description, icon, permissions, etc. 

There are some js files, as well as css/html files.

The manifest file is a must-have. Other files are optional.

In the manifest file we can define when the extension will be called. It can wake up in the background all the time, or only in some specific websites, or only when you click the icon button.

In the manifest file, there is:
 {% highlight Java %}
 "browser_action": {
    "default_icon": "icon.png",
    "default_popup": "popup.html"
  },
 {% endhighlight %}
this means when you click the icon button, you want to show some UI to the user. 
`"default_icon" `means the icon file you are using, and `"default_popup"` means the html file that you are building for the UI.

When you click the icon, some content would pop-up, and the content is defined in the HTML file.

In HTML file, you can import .js file in this way:
    <script src="popup.js"></script>
So popup.js has been imported.
