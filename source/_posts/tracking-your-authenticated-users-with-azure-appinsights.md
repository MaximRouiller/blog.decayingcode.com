---
title: "Tracking your authenticated users with Azure AppInsights"
date: "2016-06-10 08:00:00"
tags: [azure,appinsights]
---

Installing AppInsights is very easy to setup in your web application. Taking a few minutes to set few additional things however could really benefit you.

Once your initial AppInsights script has been initialized, if you can retrieve your authenticated userId you can add it to each request easily.

Simply add this on every page load where a user is authenticated:

```javascript
var userId = 'test@example.com';
appInsights.setAuthenticatedUserContext(userId);
```

This will create a cookie that will track your authenticated user on each event/page view/request.

The only bug left to iron out is when 2 users alternate session on the same browser without closing the browser. See, the cookie has a `Session` lifetime. Most of the time, it will be ok. But let's keep our data clean.

Every time a user is considered un-authenticated or that he is logging out, include the following:

```javascript
appInsights.clearAuthenticatedUserContext();
```

This will ensure that your authenticated context (the cookie) is cleared and no misattributed events are tacked on a user.
