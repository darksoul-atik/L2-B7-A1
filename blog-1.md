# Why `unknown` is Safer Than `any` in TypeScript (and What is Type Narrowing?)

## Introduction

Okay so when I first started learning TypeScript, I thought `any` was kind of a lifesaver. Like whenever I didn't know what type something was, I just slapped `any` on it and moved on. No errors, no complaints. Felt great honestly.

But then I learned that this is actually a really bad habit which is shown in the module 1. And there's a much better option called `unknown`. In this post I want to explain why `any` is dangerous and how `unknown` with type narrowing is the right way to handle data you're not sure about.

## The Problem with `any`

When you use `any`, TypeScript basically just... gives up. It stops checking that type entirely. So you can do whatever you want with it and TypeScript won't say anything — until it crashes at runtime.

Here's an example that actually happened to me (well, something similar):

```typescript
function processInput(data: any) {
  console.log(data.toUpperCase());
}

processInput(42); // crashes! numbers don't have toUpperCase
```

TypeScript didn't warn me. It just let me write broken code. That's the problem — `any` turns off the type checker for that value. It's like the whole point of TypeScript just disappears. Just like in the module, the instructor Mezba vai told us typescript means type security but `any` does not providing the security we wanted

## So What is `unknown`?

`unknown` is basically TypeScript saying "I don't know what this is, and I'm NOT going to let you use it until you prove what it is."

Same example but with `unknown`:

```typescript
function processInput(data: unknown) {
  console.log(data.toUpperCase()); // TypeScript gives an error here!
}
```

Now TypeScript complains before you even run the code. That's exactly what we want. The error at compile time is way better than a crash in production.

## Type Narrowing — Proving What the Type Is

So if we use `unknown`, how do we actually use the value? That's where type narrowing comes in.

Type narrowing just means you check the type first, and then TypeScript is smart enough to know what it is inside that check.

### Using `typeof`

This is the most basic way:

```typescript
function processInput(data: unknown): string {
  if (typeof data === "string") {
    return data.toUpperCase(); // works fine now, TypeScript knows it's a string
  }
  return "Not a string";
}
```

Inside the `if` block, TypeScript knows `data` is a string. So it lets us call `.toUpperCase()`. Outside? Still `unknown`.

### Using `instanceof`

For errors or class instances, `instanceof` works better:

```typescript
function handleError(error: unknown): string {
  if (error instanceof Error) {
    return error.message; // safe, TypeScript knows it's an Error object
  }
  return "something went wrong";
}
```

This is actually super useful when you catch errors, because in TypeScript caught errors are `unknown` by default now.

### Custom Type Guards

Sometimes you have your own objects and you need to check if a value matches them. You can write a custom type guard function for that:

```typescript
interface User {
  id: number;
  name: string;
}

function isUser(value: unknown): value is User {
  return (
    typeof value === "object" &&
    value !== null &&
    "id" in value &&
    "name" in value
  );
}

function greetUser(data: unknown): string {
  if (isUser(data)) {
    return `Hello, ${data.name}!`; // TypeScript trusts us now
  }
  return "not a valid user";
}
```

The `value is User` part is the special syntax that tells TypeScript "if this function returns true, trust that value is a User."

## When Should You Use `any` vs `unknown`?

Honestly I try to never use `any` anymore. But here's a rough guide:

| Situation | What to use |
|-----------|-------------|
| Data from an API you don't control | `unknown` |
| You genuinely have no idea and just want it to work (last resort) | `any` |
| You know it's one of a few types | Union type like `string \| number` |

## Conclusion

I used to think `any` was fine. Now I realize it's basically just turning TypeScript into JavaScript — which defeats the whole purpose. `unknown` forces you to actually think about what your data is before you use it, and type narrowing is how you prove it to the compiler. It feels like a bit more work at first but it saves you from really annoying runtime bugs later. Definitely worth it.