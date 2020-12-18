# JavaScript: The mechanics of Web Push Notifications

Date: Dec 18, 2020 12:21 AM
Property: https://blog.sessionstack.com/how-javascript-works-the-mechanics-of-web-push-notifications-290176c5c55d
Tags: Javascript, Log, Notes, Study

Push is based on Service Workers.
- Operates in the background
- their code is executed only when user interacts with the notification

### Push & Notification

- **Push**: invoked when server supplies information to SW.
- **Notification**: Action of SW/script in a web app that shows information to the user.

## Push

Three general steps to implementing a push:

1. The UI — adding client-side logic to subscribe user to a push
    - JavaScript logic that web app UI needs for the user to register to push messages
2. Sending the push message — API call on server that triggers a push message to user’s device
3. Receiving the push message — handling the push message once it arrives in the browser.

### Detailed Process.

### 1. Browser support detection.

- Check if browser supports push messaging
    1. Check for Sw on navigator object
    2. Check for PushManager on window object

### 2. Register Service Worker

### 3. Request Permission

![JavaScript%20The%20mechanics%20of%20Web%20Push%20Notifications%2040d0d4e7617749c9a2d2a2250f49086d/Untitled.png](JavaScript%20The%20mechanics%20of%20Web%20Push%20Notifications%2040d0d4e7617749c9a2d2a2250f49086d/Untitled.png)

### 4. Subscribing a user with PushManager

![JavaScript%20The%20mechanics%20of%20Web%20Push%20Notifications%2040d0d4e7617749c9a2d2a2250f49086d/Untitled%201.png](JavaScript%20The%20mechanics%20of%20Web%20Push%20Notifications%2040d0d4e7617749c9a2d2a2250f49086d/Untitled%201.png)

### 5. Sending the push message

- Tell push service (via API call) what data to send, recipient, and how to send the message

### 6. Push Services

- Push Service receives requests, validates them and delivers the push message to proper user.

![JavaScript%20The%20mechanics%20of%20Web%20Push%20Notifications%2040d0d4e7617749c9a2d2a2250f49086d/Untitled%202.png](JavaScript%20The%20mechanics%20of%20Web%20Push%20Notifications%2040d0d4e7617749c9a2d2a2250f49086d/Untitled%202.png)