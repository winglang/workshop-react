Let's learn Wing in 5 minutes, shall we?

## Primitive Types

```js
let myString: str = "hello";
let myNumber: num = 1234;
let myBoolean: bool = true; // or "false"
```

And those you've always wished your language will have:

```js
let myDuration: duration = 12h; // 12m, 12s
```

## Collection Types

```js
let myArray: Array<num> = [ 1, 2, 3, 4 ];
let mySet: Set<str> = { "dog", "cat" };
let myMap: Map<bool> = {
  "key1" => true,
  "key2" => false
};
```

## Interpolated Strings

```js
let name = "jane";
assert("hello, {name}" == "hello, jane");
```

To avoid interpolation:

```js
assert("hello \{joe}" == "hello, {joe}");
```

## Json

```js
let myJson = {
  foo: 123,
  bar: {
    zig: ["zag"]
  }
};

assert(myJson.get("foo").asNum() == 123);
assert(myJson.get("bar").get("zig").getAt(0) == "zag");
```

You can use structs to access JSON objects in a strongly-typed way (see next).

## Structs

Structs represent data shapes:

```js
struct Person {
  name: str;
  last: str;
  phone: str?;
  age: num?;
}

// structs can be extended
struct Employee extends Person {
  id: str;
}

let emp = Employee {
  name: "super",
  last: "man",
  id: "100",
};
```

Structs interoperate very nicely with `Json`:

```js
let json = {
  name: "hello",
  last: "world"
};

let p = Person.fromJson(json);
assert(p.name == "hello");
```

`Struct.fromJson()` performs validation:

```js
let json = {
  name: "hello",
  last: "world"
};

Employee.fromJson(json);
// runtime error: unable to parse Employee:
//  - instance requires property "id"
```

It's common to parse JSON from a string:

```js
let response = http.get("/employees/100");
let emp = Employee.parseJson(response.body);
assert(emp.name == "super");
```

## Function Closures

```js
let fn = (x: str) => {
  return "hello, i am {x}";
};
```

Return type is optional for closures:

```js
let getLength = (s: str): num => { 
  return s.length;
};
```

Is the same as:

```js
let getLength = (s: str) => { 
  return s.length;
};
```

The `inflight` modifier for closures indicates that a closure can run on the cloud. This is called
an **inflight closure**:

```js
let handler = inflight () => {
  log("i am flying!");
};

new cloud.Function(handler);
```

Use `void` to indicate no return value:

```js
let printName = (name: str): void => {
  log(name);
};
```

## Assignment

Use `let` to assign a value to a variable:

```js
let x = 12;
x = 77; // ERROR: x is non reassignable
```

Use `let var` to make the variable reassignable:

```js
let var y = "hello";
y = "world"; // OK (y is reassignable)
```

## Loops

The usual suspects:

```js
for i in iterable {
  break;
  continue;
}
```

This prints `0` through `4`:

```js
for i in 0..5 {
  log(i);
}
```

This prints `0` through `5`:

```js
for i in 0..=5 {
  log(i);
}
```

## Conditions

```js
if condition {
  // code
} elif condition {
  // code
} else {
  // code
}
```

```js
while condition {
  break;
  continue;
}
```

## Classes

```js
bring expect;

struct NumbersProps {
  max: num?;
}

class Numbers {
  numbers: MutArray<num>;
  max: num;

  new(opts: NumbersProps?) {
    this.max = opts?.max ?? 10;
    this.numbers = MutArray<num>[];
  }

  pub add(n: num): num {
    if this.numbers.length == this.max {
      throw "cannot add more numbers";
    }

    this.numbers.push(n);
  }

  pub sum(): num {
    let var s = 0;
    for n in this.numbers {
      s += n;
    }
    return s;
  }
}

let n = new Numbers(max: 3);

n.add(12);
n.add(3);
n.add(3);

expect.equal(n.sum(), 18);
```


## Keyword Arguments

If the last argument of a function is a struct, it's fields become keyword arguments:

```js
struct Options {
  prefix: str;
  delim: str?;
}

let join_str = (a: Array<str>, opts: Options) => {
  let delim = opts.delim ?? ".";
  return prefix + a.join(delim);
};

assert(join_str(["hello", "world"], "!") == "!hello.world");
assert(join_str(["hello", "world"], "!!", delim: "/") == "!!hello/world");
```

## Optionality

```js
let var x: str? = "hello";

// check if a variable has a value
if x? {
  print("x has a value")
}
```

Nullish coalescing:

```js
let y: str = x ?? "default-value";
```

Optional fields in structs:

```js
struct Foo {
  optValue: num?;
}
```

Optional arguments in functions:

```js
fn my_func(x: str, y: num?) {
  // code
}
```

## Enums

```js
enum Animal {
  DOG,
  CAT,
  MOUSE
}

// then
let x = Animal.DOG;
assert(x == Animal.DOG);
```

## Modules

Standard library modules:

```js
bring cloud;
bring fs;
bring util;
bring http;
bring math;
bring expect;
bring ui;

new cloud.Bucket();
```

Bring types from a file in the same project:

`producer.w`:

```js
// "pub" is needed here
pub class MyType {}
```

`consumer.w`:

```js
bring "./producer.w" as p;

new p.MyType();
```

Bring directories (all the `pub` types within the directory and subdirectories can be accessed):

`subdir/file1.w`

```js
pub class MyClass {}
```

`subdir/foo/file2.`

```js
pub class YourClass {}
```

`consumer.w`:

```js
bring "./subdir" as s;

new s.MyClass();
new s.foo.YourClass();
```

[Winglibs](https://github.com/winglang/winglibs) are 3rd party libraries that are published by the
Wing project to the `@winglibs` npm scope and used like this:

```sh
npm i @winglibs/bedrock
```

Then:

```js
bring bedrock;

new bedrock.Model("anthropic.claude-v2");
```

## Exceptions

```js
let fn = () => {
  throw "message";
};

try {
  fn();
} catch msg {
  log("error: {msg}");
}
```

## Preflight and Inflight

See [Inflights](https://www.winglang.io/docs/concepts/inflights) in the Wing docs.

Use the `inflight` modifier on methods to indicate that this method is part of the inflight API of
the object:

```js
class MyStorage {
  b: cloud.Bucket;

  new() {
    this.b = new cloud.Bucket();
  }

  pub inflight write(data: str) {
    this.b.put("data.txt", data);
  }

  pub inflight read(): str {
    return this.b.get("data.txt");
  }
}

let storage = new MyStorage();

inflight () => {
  storage.write("hello");
  assert(storage.read() == "hello");
};
```

By default, classes can only be instantiated from *preflight* context:

```js
inflight () => {
  new Numbers();
};

// ERROR: Cannot create preflight class "Numbers" in inflight phase
```

To define inflight classes, use the `inflight` modifier:

```js
inflight class Bang {
  // all members are inflight
}

inflight () => {
  new Bang();
};
```
