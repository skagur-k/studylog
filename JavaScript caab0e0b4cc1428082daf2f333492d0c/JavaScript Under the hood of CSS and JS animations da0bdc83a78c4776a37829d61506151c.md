# JavaScript: Under the hood of CSS and JS animations + how to optimize their performance

Date: Dec 18, 2020 1:21 PM
Property: https://blog.sessionstack.com/how-javascript-works-the-rendering-engine-and-tips-to-optimize-its-performance-7b95553baeda
Tags: Javascript, Notes, Study, important

When building web apps, you don't just write isolated JavaScript code that runs on its own. → JS interacts with the environment.

Understanding this environment, how it works and what it's composed of will allow you to build better apps and be well prepared ffor potential issues.

## Main components of browsers

![JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled.png](JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled.png)

- **User Interface:** includes all the UI elements the browser displays except for the window for the web page.
- **UI Backend**: used for drawing the core widgets such as checkboxes and windows.
    - Exposes a generic interface that is not platform-specific. Uses OS UI methods underneath.
- **Browser Engine:** handles interaction between UI and rendering engine.
- **Rendering Engine**: responsible for displaying the web page.
    - Parses the HTML and the CSS and displays the parsed contents.
- **Networking**: network calls such as XHR requests made by using different implementations for the different platforms.
    - Behind a platform-independent interface. [Details of networking layer](JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126.md)
- **JavaScript Engine**: [V8 JavaScript Engine](https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e)
- **Data Persistence**: your app might need to store all data locally.
    - `localStorage, indexDB, WebSQL and FileSystem`

### Rendering Engine

Main responsibility of the rendering engine is to display requested page on the browser screen.

- Can display HTML, XML documents and images.
- Can also display documents like PDF if using additional plugins.

Different browsers use different rendering engines like JavaScript Engines.

- **Gecko** — Firefox
- **WebKit** — Safari
- **Blink** — Chrome, Opera

---

## Rendering process

![JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%201.png](JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%201.png)

Rendering engine receives the contents of the requested document from the networking layer.

1. **Constructing the DOM tree**

    First step of the rendering engine is paarsing the HTML document and converting the paarsed elements to actual DOM nodes in a DOM tree.

    Each element is represented as parent node to all of the elements which are contaained inside of it, and is applied recursively.

    Example code:

    ```html
    <html>
      <head>
        <meta charset="UTF-8">
        <link rel="stylesheet" type="text/css" href="**theme.css**">
      </head>
      <body>
        <p> Hello, <span> friend! </span> </p>
        <div> 
          <img src="smiley.gif" alt="Smiley face" height="42" width="42">
        </div>
      </body>
    </html>
    ```

    The DOM tree:

    ![JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%202.png](JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%202.png)

    **Constructing the CSSOM tree**

    CSSOM refers to the CSS Object Model.

    While the browser wasa constructing the DOM of the page, it encountered a `link` tag in the `head` section that references `theme.css`

    Browser dispatches a request for it.

    `theme.css`

    ```css
    body { 
      font-size: 16px;
    }

    p { 
      font-weight: bold; 
    }

    span { 
      color: red; 
    }

    p span { 
      display: none; 
    }

    img { 
      float: right; 
    }
    ```

    The engine converts the CSS into **CSSOM**.

    ![JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%203.png](JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%203.png)

    **Not a comlete CSSOM tree**
    Only shows the styles that are overrided in the style sheet.
    Every browser provides default set of styles known as "user agent styles"

    **Why does CSSOM have a tree structure?
    →** Browser starts with the most general rule applicable to that node. and then recursively refines the computed styles by applying more specific rules

2.  **Constructing the trees**

    **Constructing the render tree**

    The visual instructions in the HTML & CCSOM tree, are used to create a render tree

    **Render tree:** tree of visual elements constructed in the order they will be displayed on screen.

    Purpose of this tree is to enable painting the contents in their correc torder.

    Each node in the tree is known as renderer or render object in **WebKit**

    ![JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%204.png](JavaScript%20Under%20the%20hood%20of%20CSS%20and%20JS%20animations%20da0bdc83a78c4776a37829d61506151c/Untitled%204.png)

    **Constructing the render tree**

    - Starting at the root of the DOM tree, it traverses each visible node. (Some are not visible i.e. script tags, meta tags etc. → not reflected in the rendered output)
        - Some nodes are hidden via CSS aand are omitted from the rendder tree (span node — `display none`)
    - For each visible node, CSSOM rules are applied
    - Emits visible nodes with content and computed styles.
3. **Layout of the render tree**

    When the renderer is created added to the tree, it does not have position and size → Calculating these values = layout.

    HTML uses flow-based layout model. (can compute geometry in a single pass)

    Coordinate system is relative to the root renderer (top left coordinates)

    Layout is a recursive process — begins at the root renderer `<html>`. 

    - Computes geometric info for each renderer.

    Position of the root renderer is `(0,0)` and have the size of the visible browser window (or the viewport)

4. **Painting the render tree**

    Renderer tree is traversed and the renderer's `paint()` method is called to display the content on the screen.

    Painting can be *global or incremental*

    - **Global —** entire tree gets repainted
    - **Incremental —** only some renderers change that does not affect the entire tree.
        - OS sees the renderer as a region that needs repaint and generates a `paint` event.

    Renderer engine renders the contents as soon as possible. 

    Parts of the contents will be parsed and displayed.

### Order of processing scripts and style sheets

Scripts are parsed and executed when parser reaches `<script>` tag **(Synchronous process)**

> If the script is external then it first has to be fetched from the network (also **synchronously**). All the parsing stops until the fetch completes.

HTML5 adds asynchronous options so it is executed on a different thread.

## Optimizing rendering performance

1. **Javascript** — Write optimized code that doesn't block the UI and is memory efficient. Need to consider how JS code will interact with the DOM element.
2. **Style calculations** — When rules are defined they are applied and the final styles for each element are calculated
3. **Layout** — Browser begins to calculate how much space the element takes up and where it is located on the browser screen.
4. **Paint** — Pixels are filled. (Drawing out text, colors, images, borders and shadows)
5. **Compositing** — Page parts are in multiple layers so need to be painted in correct order.  

**Optimizing your JavaScript**

- Avoid `setTimeout` or `setInterval` for visual updates.
    - They will invoke callback at some point in the frame.
    - Trigger the visual change right at the start of the frame.
- Move long running JS computations to [Web Workers](JavaScript%20Web%20Workers%206a58ba525f38436b9b737ebc594a6def.md)
- Use micro-tasks to introduce DOM changes over several frames.

**Optimize your CSS**

- Reduce complexity of selectors.
- Reduce number of elements on which style calculation must happen.
    - Make changes to elements directly rather than invalidating the page as a whole.

**Optimize the layout**

- Reduce number of layouts
- Use `flexbox` over older layout models. It works faster and creates huge performance advantage.
- Avoid forced synchronous layouts.

**Optimize the paint**

This is often the longest running of all the tasks.

- Changing any property other than transforms or opacity triggers a paint.
- Triggering layout triggers paint.
- Reduce paint areas through layout promotion and orchestration of animations.