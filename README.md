# TIL
TIL (Today I Learned) is a place to document what I learned through development journey that doesn't deserve a blog post.

This document is updated daily at 10PM (GMT+7)

WebAPI:

1. Notification:
This API allows you to enable desktop notification that is visible at the corner of your desktop.
It allows your web app to remind or notify user of necessary information even when they're not on your app.
Using it is quite simple:

 * You need to check whether the browser supports desktop notification:
```javascript
if (!("Notification" in window)) {
    alert(message);
  } 
```
 * If it is, check if permission has been granted then post a notification
```javascript
if (Notification.permission === "granted") {
    var notification = new Notification(message);
}
```
 * If permission hasn't been requested, request for a permission then post a notification
```javascript
if (Notification.permission !== 'denied') {
    Notification.requestPermission(function (permission) {
      if (permission === "granted") {
        var notification = new Notification(message);
      }
    })
```
 * If permission is denied, you might not want to bother the user, or just use normal alert message
```javascript
if (Notification.permission === 'denied') {
  alert("permission is denied");
}
```
