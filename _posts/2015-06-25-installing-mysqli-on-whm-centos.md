---
layout: post
title: Mysqli extension for linux vps via whm
date: "2015-06-25 10:25:06"
comments: true
published: true
---

For MySQLi extension in your WHM panel (access root required), follow these steps:

1. Log into the WHM with your root credentials.
2. Go to the "EasyApache (Apache Update)" menu, located in the "Software" section or use the search box to find it.
3. On the EasyApache page, make sure your Previously Saved (Default) configuration is selected and click on "Customize Profile".
4. Keep clicking "Next Step", until you reach the "Short Options List" page and scroll to the bottom of the page.
5. Click on the "Exhaustive Options List" button.
6. On this page, scroll down to the PHP section and find MySQL “Improved” extension.
You can use the page search option of your browser to locate the extension faster (Ctrl+F).
7. Ensure the check box is filled in and scroll to the bottom.
8. Click the "Save Only" button.
9. On the next page, click the "Build profile I just saved" button.
10. A pop box will appear and ask you to recompile Apache and PHP, select "Yes" and "I understand", if prompted.
11. Wait until the Build ouput is complete and the MySQLi extension should be installed/enabled. Please do not log out of the WHM or interrupt the rebuild process and wait for it to be completed.
