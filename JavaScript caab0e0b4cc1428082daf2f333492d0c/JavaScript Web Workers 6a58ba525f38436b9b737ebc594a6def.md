# JavaScript: Web Workers

Date: Dec 18, 2020 12:11 AM
Property: https://blog.sessionstack.com/how-javascript-works-the-building-blocks-of-web-workers-5-cases-when-you-should-use-them-a547c0757f6a
Tags: Log, Study

Javascript was designed to operate in single thread.
- The problem with this is that when the code requires CPU intensive operations (i.e, heavy network transactions), the browser will be blocked and freeze the UI.
- this is where asynchronous function comes into rescue.
- Requests that take a long time can be made asynchronously and while the client is waiting for response, other codes can be executed.

- But requests are handled by the WEB API of the browsers.
    - How can other codes be made async? (i.e, code inside the callback is very CPU intensive(?))
    - There’s no way to unfreeze the blocked UI of the browser. -> unresponsive to the user.
- ::Therefore Async functions solve only a small part of the single-thread limitations of Javascript::

## WEBWORKERS

- HTML5 brought lots of great things
    - SSE (Server-Sent Event)
    - Geolocation
    - Application cache
    - Locaal Storage
    - Drag and Drop
    - ::WEB WORKERS::
- Web Workers are in-browser threads that can be used to execute JS without blocking.
- Allow to put long-running and computationally intensive tasks on the background without blocking the UI. -> RESPONSIVE
- DEMO: [http://afshinm.github.io/50k/](http://afshinm.github.io/50k/) (Sorting 50K w, w/o Webworkers)

> Web workers are not part of Javascript, they’re browser feature which can be accessed through Javascript.

- Web Workers are not implemented in Node.JS

![JavaScript%20Web%20Workers%206a58ba525f38436b9b737ebc594a6def/Untitled.png](JavaScript%20Web%20Workers%206a58ba525f38436b9b737ebc594a6def/Untitled.png)

## Types of Workers

- Dedicated Workers
    - Instantiated by main process and can only communicate with it.
- Shared Workers
    - Shared Workers can be reached by all processes running on the same origin.
    - (Different browser tabs, frames or other shared workers)
- Service Workers
    - Event-driven worker registered against an origin and a path.
    - Can control the web page/site it is associated with, intercepting and modifying the navigation and resource requests and caching resources.

*This article focuses on Dedicated Workers (referred to as Workers)*

## How Web Workers work

- Web Workers are implemented as .js files which are included via async HTTP requests in the page.
    - Requests are hidden by the Web Worker API

`var worker = new Worker('task.js');`

- If task.js exists, browser spawns new thread which downloads the file asynchronously
    - Then it will execute and worker will begin.
    - If returns 404, the worker will fail silently.
- In order to start created worker, invoke postMessage method:

`worker.postMessege();`

## Web Worker Limitation

- Have NO ACCESS to very crucial Javascript features:
    - The DOM
    - The window object
    - The document object
    - The parent object
- Web Worker cannot manipulate UI but can be used as ‘computing machines’.

## Good use cases for Web Workers

```
- Ray Tracing
	- GPU intensive computations
- Encryption
- Prefetching data
- Progressive Web Apps
```