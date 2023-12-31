
In this session, we will create a new React app and connect it to a Winglang backend.

## Prerequisites

First, let's verify that you can create a React application using `create-react-app`. Make sure you are at the project root directory (not inside `backend`).
```sh
npx create-react-app client
cd client
npm i --save @babel/plugin-proposal-private-property-in-object
npm start
```

The result should be a running React website. Once you verify it is working, you can close the server with `Ctrl-C`.

## React in Wing

1. Replace `backend/main.w` with the following content:
```ts
bring ex;
bring cloud;
bring http;

let react = new ex.ReactApp(
  projectPath: "../client",
);
```
2. Terminate (`Ctrl-C`) your existing `wing run` command and rerun it with the `BROWSER=none` environment variable:
   ```sh 
   BROWSER=none wing run backend/main.w
   ```
> `BROWSER=none` prevents React from starting a new browser window on every run.
    
As you can see, you now have a running React web app from within the Wing toolchain.

## Pass Configuration from Backend to Frontend

Wing creates `client/public/wing.js` to pass configuration from the Wing backend to the frontend code. 

If you examine its content, it is currently:
```js
// This file is generated by Wing
window.wingEnv = {};
```

Once we add the following code to `backend/main.w`:
```ts
react.addEnvironment("key1", "value1");
```

The running `wing run` command will generate a new version of `client/public/wing.js`:
```js
// This file is generated by wing
window.wingEnv = {
  "key1": "value1"
};
```

Let's see how we can use this mechanism:

1. Include `wing.js` in your frontend code - Add the following line inside `client/public/index.html` (just before the `<title>` section):
     ```html 
     <script src="./wing.js"></script>
     ```
2. Let's see this in action. In `client/src/App.js`, look for "Learn React" (probably around line 18) and replace it with:
   ```js
     {window.wingEnv.title || "Learn React"}
   ```
3. Go to `backend/main.w` and add:
   ```ts
   react.addEnvironment("title", "Learn React with Wing");
   ```
  
## GET Title from API 

Once we know how to pass parameters from the backend to the client, let's use this capability to set `window.wingEnv.apiUrl` on the client and fetch the title from our API.

1. Enable cross-origin resource sharing (CORS) by adding `cors: true` and `corsOptions: ...` in `backend/main.w`: 
```ts
let api = new cloud.Api(
  cors: true
);
```
2. To set `apiUrl`, go to `backend/main.w` and add:
```ts
  react.addEnvironment("apiUrl", api.url);
```
3. Create a new `/title` route in `backend/main.w`: 
```ts
api.get("/title", inflight () => {
  return {
    status: 200,
    body: "Hello from the API"
  };
});
```
4. Use React hooks to read the title from our API. Replace the content of `client/src/App.js` with the following code:
```js
import logo from './logo.svg';
import { useEffect, useState } from "react";
import './App.css';

function App() {
  const [title, setTitle] = useState("Default Value");
  const getTitle = async () => {
    const response = await fetch(`${window.wingEnv.apiUrl}/title`);
    setTitle(await response.text());  
  }
  
  useEffect(() => {
    getTitle();
  }, []);
  
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        {title}
      </header>
    </div>
  );
}

export default App;
```

🚀 Check that every change, either on the client side or the server side will hot-reload your app. 🚀
---
