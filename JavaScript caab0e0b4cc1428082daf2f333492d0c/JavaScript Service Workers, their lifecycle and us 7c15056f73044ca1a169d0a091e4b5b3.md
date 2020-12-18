# JavaScript: Service Workers, their lifecycle and use cases

Date: Dec 18, 2020 12:18 AM
Property: https://blog.sessionstack.com/how-javascript-works-service-workers-their-life-cycle-and-use-cases-52b19ad98b58
Tags: Javascript, Log, Notes, Study

![JavaScript%20Service%20Workers,%20their%20lifecycle%20and%20us%207c15056f73044ca1a169d0a091e4b5b3/Untitled.png](JavaScript%20Service%20Workers,%20their%20lifecycle%20and%20us%207c15056f73044ca1a169d0a091e4b5b3/Untitled.png)

## Service Worker

- Type of Web Worker (similar to Shared Worker)
    - SW runs in its own global script context.
    - Not tied to specific web page
    - Cannot access the DOM
- **Shared Worker**: worker that can be accessed from several browsing contexts (windows, frames or workers)
    - Implement interface different than dedicated workers and have different global scope.
- It allows your web apps to support offline experiences.

## Lifecycle of a Service Worker

*Lifecycle of a service worker is completely separated from your web page one*

- **Download**
    - Browser downloads the .js file which contains the SW
- **Installation**
    - Register then it prompts the browser to start SW install step in the background.
    - By registering the SW, you tell the browser where your SW js file lives.
    - Best to load and cache some static assets. (Not must, but recommended)
- **Activation**
    - Once activated, the SW will control all pages that fall under its scope.

![JavaScript%20Service%20Workers,%20their%20lifecycle%20and%20us%207c15056f73044ca1a169d0a091e4b5b3/Untitled%201.png](JavaScript%20Service%20Workers,%20their%20lifecycle%20and%20us%207c15056f73044ca1a169d0a091e4b5b3/Untitled%201.png)

## Features using Service Workers:

```
- Push notifications
- Background sync
- Periodic sync
- Geofencing
```