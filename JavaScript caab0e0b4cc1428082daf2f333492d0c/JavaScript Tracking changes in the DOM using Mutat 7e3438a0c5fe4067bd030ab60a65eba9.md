# JavaScript: Tracking changes in the DOM using MutationObserver

Date: Dec 18, 2020 12:53 PM
Property: https://blog.sessionstack.com/how-javascript-works-tracking-changes-in-the-dom-using-mutationobserver-86adc7446401
Tags: Javascript, Notes, Study

![JavaScript%20Tracking%20changes%20in%20the%20DOM%20using%20Mutat%207e3438a0c5fe4067bd030ab60a65eba9/Untitled.png](JavaScript%20Tracking%20changes%20in%20the%20DOM%20using%20Mutat%207e3438a0c5fe4067bd030ab60a65eba9/Untitled.png)

**Web apps** are getting heavy on the client-side: richer UI to accomodate, real-time calcuations.

## `MutationObserver`

Mutation Observer is a Web API provided by modern browsers for detecting changes in the DOM.

- This allows to listen to newly added or removed nodes, attribute changes or changes in the text content of text nodes.

**Use Cases**

- Notify users that some change has occured on the page he's on.
- Working on a new framework that loads dynamically JavaScript modules based on how the DOM changes.
- Detect changes so you can easily undo/redo on a WYSIWYG editor

![JavaScript%20Tracking%20changes%20in%20the%20DOM%20using%20Mutat%207e3438a0c5fe4067bd030ab60a65eba9/Untitled%201.png](JavaScript%20Tracking%20changes%20in%20the%20DOM%20using%20Mutat%207e3438a0c5fe4067bd030ab60a65eba9/Untitled%201.png)

- Covers every single change that can possibly occur in the DOM.
- Way more optimized as it fires the changes in batches.
- Supported by all major modern browsers

### Alternatives

- **Polling**

    Simplest and most unsophisticated way.

    Using the browser's setInterval WebAPI, you can periodically check if any changes have occurred.

    This degrades web app/website performance.

- **MutationEvents**

    Mutation events are fired on every single change in the DOM → Performoance Issues 🔥

- **CSS animations**

    Create an animation which would be triggered once an element has been added to the DOM.

    When animatinon starts: `animationstart` event will be fired.

    if you attached an event handler, you know when the element has been added to the DOM.