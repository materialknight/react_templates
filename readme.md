# React Templates Documentation

## React, JavaScript XML (JSX) and Babel

**React** is the most popular JavaScript library to make **User Interfaces** (**UI**s), but it can be confusing to set up because there are many ways to do so. This documentation will guide you through the different ways you can set up React.

React is special because it allows you to define the UI as a set of nested **components**; a **component** is a function that returns a piece of UI as HTML or JavaScript XML (JSX) (the meaning of all that becomes clear once you start learning and using React).

**JavaScript XML** (**JSX**) is a special syntax used with React to write the pieces of UI returned by components; it's not necessary to use JSX to use React, but most people do beause it makes complex components easier to read and enables React to use a virtual DOM, which is a lightweight representation of the DOM that React can use to increase your page's performance. For example, the following `Button` _component_ returns a _JSX_ button _element_:

```jsx
function Button({handleClick, text}) {
   return <button onClick={handleClick}>{text}</button>
}
```

The browser can't understand JSX (JSX is merely a development tool to make your life a bit easier; no JSX should reach production), so you need to translate it into vanilla JS; the tool for that is Babel.

**Babel** is a transpiler that translates JSX ("JavaScript XML", React's syntax to render HTML) and modern JS (JavaScript) into vanilla, non-modern JS (non-modern, to make it as compatible as possible with older browsers). If you have a file with modern JS but no JSX, you can choose not to translate it, but your page will work correctly for less users.

Since it's a sensible assumption you'll use JSX, I also cover how to set up Babel.

## Reading this will help you understand better the instructions:

### Difference between **inline** and **external** scripts:

A script can be **inline** or **external**. An **inline script** is code between the opening and closing script tags, it's the content of a script element:

```html
<script>
   'use strict'
   console.log('I was logged by an inline script')
   // More code...
</script>
```

An **external script** is a separate **.js** file linked to the **.html** via the `scr` attribute of a script tag:

```html
<script defer src='index.js'></script>
```

### Node.js, **npm** and **npx**

**Node.js** is a JavaScript runtime environment that allows you to run JS on a server, outside of a browser; it includes its own package manager, **npm** (Node package manager), and its own package runner, **npx** (It's not an acronym, but you can think of the command as **N**ode **P**ackage e**X**ecute).

<span id='verify-intall_node'>**If you're instructed to run a command starting with `npm` or `npx`, you must have installed Node.js for the command to work**. To verify if you have Node.js: Open a terminal and run `node --version` (if you already have it, you'll get its version number, if not, you'll get an error); if you don't have Node.js, install it from https://nodejs.org (if the commands starting with `npm` or `npx` still don't work after that, close and re-open the terminal).</span>

When you enter a command like `npx create-react-app` in a terminal:

1. If there's a **node_modules** folder in the current working directory (the folder where you are located in the terminal, it should be your project's folder), the package runner (**npx**) will try to find there the **create-react-app** package and run it.
2. If there's no **node_modules** folder in the current working directory, or if there's no **create-react-app** package there, **npx** will try to run the **create-react-app** package directly from the npm registry without installing it in your computer first.

To [use Babel via npm](#babel-via-npm), you'll be instructed to install Babel first via **npm** before running it with **npx**, but couldn't you run it via **npx** right away? In Babel's case, **no** because, for some reason, **npx** will run a deprecated version of Babel if you don't install it first via **npm**.

To [run Create React App](#run-create-react-app), you'll be instructed to do it via **npx** right away without installing it first via **npm**; in this case, unlike Babel's, it makes sense to do so, because:

1. **npx** will get you the most recent version of **create-react-app**, as expected.
2. **create-react-app** installs a full toolchain of dependencies to develop React apps, but **create-react-app** itself isn't part of that toolchain, and no part of your project will depend on it, it's function is to be an installer, thus it's meant to be run only once, so it doesn't make sense to install it first via **npm** before running it with **npx**.


### package.json

**package.json** is the file to organize your project's dependencies and metadata. You can open a terminal **in your project's folder** and run `npm init` to create your project's package.json, but if you don't, it will be created automatically when you install your first package, for example by running `npm install react react-dom`. A package.json looks like this:

```json
{
   "name": "your-project",
   "version": "1.0.0",
   "description": "",
   "main": "index.js",
   "dependencies": {
      "react": "^18.2.0",
      "react-dom": "^18.2.0"
   },
   "devDependencies": {
      "@babel/cli": "^7.21.0",
      "babel-preset-react-app": "^10.0.1"
   },
   "author": "your-name",
   "license": "ISC",
}
```

Notice that **package.json** can have 2 sections that save the name and version of dependecies as key-value pairs:

1. A `dependencies` section, intended for dependencies needed in production (the dependencies actually needed for the app to work).
2. A `devDependencies` section, intended for dependencies needed only during development (just to ease the developer's work).

In the instructions to [use Babel via npm](#babel-via-npm) you'll learn how to save a dependency in one section or the other.

In the above **package.json** example, `"react": "^18.2.0"` means that the project was made with the **react** package, version **18.2.0**, but it's also compatible with any higher version **below** **19.0.0**; that's because in **semantic versioning**, increasing the first part of the version number means you've done **breaking changes**.

### Getting a resource from a Content Delivery Network (CDN):

A Content Delivery Network (CDN) is a server net. There are several ways to use React that involve [requesting it from a CDN](#using-react). This is how that request works:

Let's say you have a website (**yoursite.com**) that links React from a CDN; that means its html document (**index.html**), which is stored in your server, links React from a CDN through a `<script>` tag like `<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>`. That goes like this:

1. When a user visits your website (**yoursite.com**), his browser sends a requests to your server, asking for **index.html**.
2. Your server responds by sending **index.html** to the user's browser.
3. **index.html** tells the user's browser to request other resources, like images (through `<img>` tags), a CSS style sheet (through a `<link>` tag), and your **.js** files and React (through `<script>` tags). That request for React is a **cross-origin request**. What does that mean?:

   A **resource** is any file used by an app (a website is a web app); it can be an image, an HTML file, a CSS stylesheet, a JS module, etc.

   The **origin** of a resource is the **domain** of the server that hosts (serves) the resource, for example: **index.html** and any other resource hosted by your server share the same origin: **yoursite.com**.

   After getting **index.html**, the browser will request any resource linked by a tag (`<link>`, `<script>`, `<img>`, etc.). Any such request sent to the same domain (**yoursite.com**) is a **same-origin request**. Any such request sent to a different domain (other than **yoursite.com**), is a **cross-origin request**.

   In this case, the origin of **index.html** is **yoursite.com**, but the origin of React is its CDN, thus the user's browser sends a **crossorigin request** to get React.

   A **crossorigin request** resolves differently depending on its script tag's `crossorigin` attribute: If the attribute is not present, you'll get deficient error logging for the requested script (React in this case) when `window.onerror` triggers (`window.onerror` triggers when a resource failed to load or couldn't be used). If the `crossorigin` attribute is present, it the request is done using the **Cross-Origin-Resouce-Sharing** (CORS) Protocol, which enables the server to share more information or honor previously impossible requests, if the request comes from a trusted origin.

   If `crossorigin` is set to "anonymous" and the server responds with the header `Access-Control-Allow-Origin: yoursite.com` or with `Access-Control-Allow-Origin: *`, it means **yoursite.com** is a trusted origin to make that GET request for the linked script (React), so you'll get improved error logging for that fetched script (React) when `window.onerror` triggers. You can set `crossorigin` to "anonymous" by writing `crossorigin='anonymous'`, `crossorigin=''`, `crossorigin=*nonExistentValue*` or simply `crossorigin` (I use the last option in this documentation).

   In our case, the user's browser sends a request to the React CDN, asking for the React script. This request includes an "Origin" header, which specifies the origin (**yoursite.com**) of the request.
4. When the React CDN server receives the request, it checks the "Origin" header to ensure that the request is coming from an allowed origin. If the origin (**yoursite.com**) is allowed, the server responds by sending the React script to the user's browser, along with `Access-Control-Allow-Origin: yoursite.com` or `Access-Control-Allow-Origin: *` as a response header, thereby enabling you to see better React error messages whenever `window.onerror` triggers.

   You can check the **Access-Control-Allow-Origin HTTP response header** to see the allowed origins (in the case of React CDNs, it usually is "*", meaning all origins are allowed).

5. The user's browser runs the React script, thereby rendering your website using React.


## Using Babel

You can use Babel via **Content Delivery Network** (**CDN**), or by installing it as a development dependency (a dependency used only during development) via **Node.js' Package Manager** (**npm**, in lowercase, for some reason).

Babel via CDN is slower because it translates at runtime; use it when you want to experiment with React without wasting space in a local Babel installation, but don't inconvenience your users by using it in production.

For serious development, install Babel and use it only during development to watch for changes in the **.js** files you want to translate, if you do, Babel will create (automatically upon change) translated versions (in vanilla, non-modern JS) of those files into the location you choose; those Babel-translated versions (in vanilla, non-modern JS) are the ones you'll release into production, NOT the original ones with JSX!

**Possibly incorrect clarification (testing & research required)**: If you want Babel to translate an **inline script** (code between the opening and closing script tags, the content of a script element), you must use Babel via CDN because using Babel via npm _apparently_ requires you to point Babel to **.js** files so it can create translated versions of them, however, inline scripts are not in a separate **.js** file but in the **.html** itself, so _apparently_, you can't point Babel to inline scripts. If this is correct, that'd also mean you shouldn't use inline scripts for serious development, because you'd have to use Babel via CDN, which, as said above, is slower.

### Babel via CDN

First, link Babel's CDN with this script tag:

```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```

**If you want Babel to translate a script**, add the attribute `type='text/babel'` to that script's tag, and if the script is **inline**, you also **need** to add the attribute `data-type='module'`.

Tell Babel to translate an **inline** script:

```html
<script type='text/babel' data-type='module'>
   // Script with modern JS features or with JSX.
</script>
```
Tell Babel to translate an **external** script:

```html
<script type='text/babel' src='/src/index.js'></script>
```

You don't need the attribute `defer` in either case because scripts with `type='text/babel'` are deferred by default, even inline scripts (which can't be deferred by the `defer` attribute), so no need to put those script tags at the end of the **.html**, you can put them in the `<head>` element.

That's it, now let's set up React!

### Babel via npm

[Make sure you have Node.js installed](#verify-intall_node), open a terminal **in your project's folder's** and run these commands:

>`npm init -y`

That will create a **package.json**. The `-y` flag is to say 'yes' to all default options, if you omit it, you'll be repeatedly prompted to provide those options one by one.

>`npm install --save-dev @babel/cli babel-preset-react-app`

Breakdown of the above command:

- The `npm install` command installs packages locally from the npm registry, it accepts 0 or more package names as arguments; it works like this:
  - It creates a **node_modules** folder in your current folder (in your project's folder) if it doesn't exist. **node_modules will have all installed packages**.
  - If you didn't pass any argument to the command, it installs the most recent compatible version of all `dependencies` and `devDependencies` saved in **package.json**.
  - If you passed one or more package names as arguments, it installs those packages.

  We passed 2 arguments: **@babel/cli** and **babel-preset-react-app**. You can also specify a version, like this: `npm install --save-dev @babel/cli@7 babel-preset-react-app@10`. That would install **@babel/cli** version 7 and **babel-preset-react-app** version 10; if you don't specify a version, you'll get the most recent version.

- `@babel/cli` is Babel's command-line interface, which includes the Babel core.
- `babel-preset-react-app` is Babel's ruleset to translate JSX.
- The `--save-dev` flag saves the name and version of the packages in `devDependencies`; if you omit it, they will be saved in `dependencies` by default.

>`npx babel --watch src --out-dir . --presets babel-preset-react-app/prod`

Breakdown of the above command:

- `npx babel` executes the Babel transpiler.
- `--watch src` tells Babel to watch the files in the **src** directory for changes to re-transpile them automatically.
- `--out-dir .` tells Babel to save the transpiled files into the current folder (your project's folder in this case).
- `--presets babel-preset-react-app/prod` tells Babel to use the **babel-preset-react-app/prod** preset, which is a set of plugins and configuration options to optimize React apps for production.
That's it! Now, whenever you modify a file in the **src** folder, Babel will automatically re-transpile it into a separate file that will be created in the current folder (your project's folder). Let's set up React!

## Using React

In all examples, you'll enable 2 libraries: **React**, which is a library to build UIs, and **ReactDOM**, which is a library to render React components into the DOM. All options that involve linking React from a CDN have 2 examples: One for development with better error logging, and one for production, with better performance.

To use React, you have these options:

### Import React from a CDN:

In this file's folder, there's a playground for each way to use React in this section.

The 3rd playground (**3-non-es-modules.html**) is the only one that has no **import** statements for React, StrictMode, createRoot nor for the external React component called **Component** that is in **component.js**, in this file's folder; also, **component.js** has no **export** statement. All of that is possible because, in that playground, React and ReactDOM come from non-ECMAScript modules, so those objects are loaded globally (into the **window** object; verify that by adding `console.log(window)` to the inline script), just like Babel, thus everything is in the global scope.

That means the JSX of **Component** won't throw a **Syntax Error** because Babel can see it and translate it; when that happens, the result is a bunch of React functions that won't throw **Uncaught ReferenceError: React is not defined** because React is also available globally.

In the playgrounds 1 and 2, React and ReactDOM come from ECMAScript modules, so they can't be loaded globally but only for importing scripts (the inline scripts in playgrounds 1 and 2); the proof is that you need import statements for React, StrictMode and createRoot in those cases.

So, in playgrounds 1 and 2, if I import **Component** normally (by **export** and **import** statements), it won't be available globally but only for the importing inline script, that means the JSX of **component.js** will throw a **Syntax Error** because it's not available globally for Babel to translate. And if I load **component.js** globally with a script tag (as in playground 3), it will throw **Uncaught ReferenceError: React is not defined** because React is not available globally but only for the importing inline script.

The only way to make an external component (like **Component**) work in those cases is by loading globally with a script tag (as in playground 3) so it's available to Babel, and importing React again in the **.js** file of every external component (**component.js**) to avoid **Uncaught ReferenceError: React is not defined**. I thought that solution would be redundant, so I didn't apply it for playgrounds 1 and 2; instead, I copied **Component** into the inline script of each playground.

Anyway, you can **import React from a CDN**:

1. As ECMAScript module via import map:

   **Development build**:

   ```html
   <script type="importmap">
   {
      "imports": {
         "react": "https://esm.sh/react?dev",
         "react-dom/client": "https://esm.sh/react-dom/client?dev"
      }
   }
   </script>
   ```

   **Production build**:

   ```html
   <script type="importmap">
   {
      "imports": {
         "react": "https://esm.sh/react",
         "react-dom/client": "https://esm.sh/react-dom/client"
      }
   }
   </script>
   ```

   In either case, the import map defines what an import `from 'react'` means and what an import `from 'react-dom/client'` means. For example, the **Development Build** import map defines that importing `from 'react'` means importing from **https://esm.sh/react?dev** and importing from `from 'react-dom/client'` means importing from **https://esm.sh/react-dom/client?dev**. Thus to use any of the above import maps, you need these import statements:

   ```js
   import React, { StrictMode } from 'react'
   import { createRoot } from 'react-dom/client'
   ```

2. As ECMAScript module via import statement.

   **Development build**:

   ```js
   import React, { StrictMode } from 'https://esm.sh/react?dev'
   import { createRoot } from 'https://esm.sh/react-dom/client?dev'
   ```

   **Production build**:

   ```js
   import React, { StrictMode } from 'https://esm.sh/react'
   import { createRoot } from 'https://esm.sh/react-dom/client'
   ```

3. As **non**-ECMAScript module from a CDN.

   **Development build**:

   ```html
   <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
   <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
   ```

   **Production build**:

   ```html
   <script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin></script>
   <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js" crossorigin></script>
   ```

### Install React Locally:

4. Via **npm**, with nothing else.

   [Make sure you have Node.js installed](#verify-intall_node), open a terminal **in your project's folder's**, and run this command:

   >`npm install react react-dom`

   This is a good option for adding a bit of React to an already deployed project that was originally made **without** React.

5. Via **npx**, with a complete framework.

   A framework is a tool that installs a pre-configured environment for developing an app (so all React frameworks install more than just React) that restrict to some degree your app's structure.

   All the following frameworks install Babel, so **if you use this option, don't install Babel separately**, and if you've already installed Babel as described in this documentation, uninstall it with `npm uninstall @babel/cli babel-preset-react-app`.

   Of the following frameworks, only **Create React App** is a purely front-end framework (which means you might need to complement it with a backend framework, like **Express**), the rest are full-stack frameworks.

   To use a framework, [make sure you have Node.js installed](#verify-intall_node), open a terminal **in your project's folder's**, and run the framework's command:

   [Create React App](https://create-react-app.dev/)

   >`npx create-react-app your-app`

   [Next.js](https://nextjs.org/)

   >`npx create-next-app`

   [Remix](https://remix.run/)

   >`npx create-remix`

   [Gatsby](https://www.gatsbyjs.com/)

   >`npx create-gatsby`

   [Expo](https://expo.dev/) (for native apps)

   >`npx create-expo-app`
