# FluxLang Guide  
A lightweight JavaScript‑style DSL for building Fluxer bots. FluxLang compiles into FluxAsm bytecode and executes through the FluxAsmRT runtime.

---

## Language Overview  
FluxLang is a **JS‑inspired DSL** designed specifically for bot logic.  
It focuses on:

- simple event handlers  
- minimal syntax  
- predictable structure  
- easy Assembly‑level parsing  
- fast compilation  

FluxLang is *not* a general‑purpose language — it is intentionally small, expressive, and bot‑focused.

---

## Core Syntax  
FluxLang uses **function‑call expressions** combined with a custom `->` operator to define bot behavior.

### Event → Action  
```
message("ping") -> reply("pong");
message("hello") -> send("Hi there!");
```

### Direct Actions  
```
send("FluxAsm online!");
reply("pong");
```

You can absolutely add **explicit types** (int, string, bool) to FluxLang *without breaking the JS‑DSL vibe*. The trick is to make types **optional**, **lightweight**, and **compiler‑friendly**, so beginners can ignore them while advanced users can be explicit.

Here’s the expanded **Variables** section for your `guide.md`, including typed and untyped forms.

---

## Variables  
FluxLang supports both **implicit** and **explicit** typing.  
This keeps the language flexible while still allowing strictness when needed.

---

### Implicit Variables (JS‑style)
```
let greet = "Hello!";
let count = 5;
let active = true;

message("hi") -> send(greet);
```

Implicit typing is the default.  
The compiler infers the type based on the literal.

---

### Explicit Variables (C++/TS‑style)
FluxLang also supports explicit type declarations:

```
string greet = "Hello!";
int count = 5;
bool active = true;
```

This is optional but useful for:

- clarity  
- debugging  
- static analysis  
- future optimizations  
- Assembly‑level type enforcement  

---

### Full Typed Example
```
string greet = "Hello from FluxAsm!";
int times = 3;
bool enabled = true;

message("hello") -> {
    if (enabled == true) {
        send(greet);
        send("Times: " + times);
    }
};
```

---

### Supported Types  
Here are the built‑in types:

- **string** — text  
- **int** — whole numbers  
- **bool** — true/false  

---

### Mixed Example (typed + untyped)
```
string greet = "Hello!";
let name = "Fluxer";

message("hi") -> send(greet + " " + name);
```

Typed and untyped variables can coexist without restrictions.

---

### Variable Rules  
- Variables must be declared with `let` or a type keyword  
- Semicolons are required  
- Identifiers must be simple (no destructuring, no patterns)  
- Types are optional  
- Type mismatch errors occur at compile time  
- Strings use double quotes only  

---

## Example: Using variables inside functions
```
function welcome(user) {
    string msg = "Welcome " + user;
    send(msg);
}

join(user) -> welcome(user);
```

---

### Blocks  
```
message("start") -> {
    send("Booting...");
    send("Ready!");
};
```

---

## Events  
FluxLang supports event handlers using the pattern:

```
event(args) -> action;
event(args) -> { action1; action2; };
```

### Built‑in Events  
- `message(content)` — triggered when a message is received  
- `join(user)` — triggered when a user joins  
- `leave(user)` — triggered when a user leaves  

### Examples  
```
join(user) -> send("Welcome " + user);
leave(user) -> send("Goodbye " + user);
```

---

## Actions  
Actions are simple function calls.

### Built‑in Actions  
- `send(text)` — sends a message  
- `reply(text)` — replies to the triggering message  

### Examples  
```
message("ping") -> reply("pong");
send("FluxAsm online!");
```

---

## Operators  
FluxLang uses a small operator set to keep parsing simple.

| Operator | Meaning |
|---------|---------|
| `->` | event → action mapping |
| `=` | variable assignment |
| `+` | string concatenation |
| `==` | equality comparison |

Guided links:  
- **operators**  
- **variables**  
- **events**  

---

## Conditions  
FluxLang supports simple JS‑style conditions.

```
message(content) -> {
    if (content == "ping") reply("pong");
    if (content == "hello") send("Hi!");
};
```

### Condition Rules  
- Only `==` is supported  
- No `===`  
- No logical operators (`&&`, `||`)  
- No nested `if` blocks  

This keeps the parser extremely small.

---

## Functions  
FluxLang supports lightweight JS‑style functions.

### Definition
```
function name(param1, param2) {
    action1;
    action2;
}
```

### Example
```
function greet(name) {
    send("Hello " + name);
}

message("hi") -> greet("Fluxer");
```

### Function Rules  

- Functions begin with function
- No return values
- No nested functions
- No closures
- No async
- Parameters must be simple identifiers
- Statements inside must end with semicolons

Guided link: **functions**

---

## Comments  
FluxLang uses JS‑style comments.

```
// single-line comment
# also allowed
```

---

## Full Example Program  
```
let greet = "Hello from FluxAsm!";

message("ping") -> reply("pong");

message("hello") -> {
    send(greet);
    send("How can I help?");
};

join(user) -> send("Welcome " + user);
```