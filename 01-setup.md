
In this section, we will ensure that Wing is installed and functioning properly on your machine.

## Prerequisites

* [Node.js](https://nodejs.org/en/) (version 18.13.0 or higher)
* [VSCode](https://code.visualstudio.com/download)
* [Docker](https://www.docker.com/) or [OrbStack](https://orbstack.dev/) installed

## Wing Toolchain

The Wing Toolchain is distributed via [npm](https://www.npmjs.com/):

```sh
npm install -g winglang
```

Verify your installation:

```sh
wing --version
```

## Wing VSCode Extension

The Wing VSCode Extension adds syntax highlighting and other conveniences for the Wing Programming Language in [VSCode](https://code.visualstudio.com/).

To install the Wing VSCode extension, [download it from the VSCode Marketplace](https://marketplace.visualstudio.com/items?itemName=Monada.vscode-wing).

## Wing it

1. Create a new directory on your filesystem (e.g., `/tmp/wing-workshop`).
2. Start VSCode from this directory.
3. Create a `backend` directory.
4. Create `backend/main.w` with the following content:
```ts
bring cloud;

let queue = new cloud.Queue(timeout: 2m);
let bucket = new cloud.Bucket();
let counter = new cloud.Counter();

queue.setConsumer(inflight (body: str): str => {
  let next = counter.inc();
  let key = "key-{next}";
  bucket.put(key, body);
});
```

Verify that the Wing toolchain is working as expected:
```sh
wing run backend/main.w
```

🚀 In the Wing Console, you can push messages to the Queue and observe the files created in the Bucket. 🚀 
