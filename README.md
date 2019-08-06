# Session Timeout

Warn users when their session is about to expire. Dependency-free.

When this function is called (usually each time a page is loaded), a timer starts in the background.
When the timer goes off, a warning is displayed to the user that their session is about to expire.
The user has two options: Log out now or stay connected. If they choose to log out, they are brought
to your site's log out page. If they choose to stay connected, a keep-alive URL is requested in the
background, the warning is hidden, and the timer resets.

This project is an adaptation of the project at https://github.com/travishorn/session-timeout.
This adds features such as user interrupts, function callback for keepalive, logout, renew functionalities instead of URLs.
This project also adds a count down timer.
Keepalive frequency can be controlled with a keepAliveFrequency property.

## Installation

### Method 1: CDN

Include the script on your page via UNPKG.

```html
<script src="https://unpkg.com/santonica/session-timeout"></script>
```

### Method 2: Download

Download [dist/session-timeout.js](dist/session-timeout.js).

Include it on your page.

```html
<script src="session-timeout.js"></script>
```

### Method 3: ES6 Module using npm and webpack (or similar)

Install via npm.

```
> npm install santonica/session-timeout
```

Include it in your scripts.

```javascript
import sessionTimeout from 'santonica/session-timeout';
```

## Usage

Call it in JavaScript.

```javascript
sessionTimeout();
```

Provide options as an object.

```javascript
 sessionTimeout({
                keepAliveMethod: "GET",
                keepAliveFrequency: 300000, //Every 5 minutes, keep server alive
                keepAliveFn: getUserInfo,
                stayConnectedBtnText: "Renew",//TODO I18N
                logOutBtnText: "Sign out", //TODO I18N
                logOutFn: self.logout,
                message: "Your session will expire in ## seconds",//TODO I18N
                sessionExpiredMessage: "Your session expired. You will be automatically redirected to sign in page.",//TODO I18N
                showExpiredMessageFor: 2000,
                timeOutAfter: 90000, //15 minutes
                timeOutFn: self.logout,
                warnAfter: 760000 //13 minutes
            });
```

## Options


### keepAliveMethod

The HTTP method to use when making the keep-alive request.

Default: `POST`

### keepAliveFrequency

This is the frequency at which a server ping is made using the keepAliveFn provided.

Default: 300000 or 5 minutes

### keepAliveFn

This callback should have logic to call a server resource so server session is maintained.

Default: Does nothing if a callback was not provided.

### logOutBtnText

The text on the log out button.

Default: `Logout`

### logOutFn

When the user clicks the "Log out" button, this callback is invoked.

Default: Clears all timers by default. Supply a callback to perform a logout action.

### message

The message to display when warning the user of inactivity.

Default: `Your session will expire in ## seconds.`
Note: The '##' is required if you want to see the count down.

### stayConnectedBtnText

The text on the "stay connected" button.

Default: `Stay connected`

### timeOutAfter

The amount of time, in milliseconds, to wait until automatically timing out the user and invoking the timeoutFn. 

Default: `900000` (15 minutes). This will make the application PCI DSS 3.2 compliant.

### timeOutFn

Once the time out period has elapsed, this callback will be invoked.

Default: Clears all timers by default. Supply a callback to perform a time out action.

### warnAfter

The amount of time, in milliseconds, to wait until displaying a warning to the user. If the
user is active, this warning timer will be reset.  the warning disappears, and the timer is reset. The warning
will re-appear after the same amount of time after reset.

Default: `900000` (15 minutes)
