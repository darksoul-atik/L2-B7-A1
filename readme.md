# B7A1 — Advanced Problem Solving with TypeScript & OOP

## Overview

Solutions to the Level 2 TypeScript assignment covering data typing, interfaces, classes, generics, and OOP concepts.

## File Structure

```
├── solutions.ts   # All 7 coding problems
├── blog-1.md      # Blog: any vs unknown & type narrowing
├── blog-2.md      # Blog: Pick & Omit utility types
└── README.md
```

## Problems Solved

| # | Function/Class | Concept |
|---|---------------|---------|
| 1 | `filterEvenNumbers` | Array filtering |
| 2 | `reverseString` | String manipulation |
| 3 | `checkType` | Union types & type guards |
| 4 | `getProperty` | Generics with constraints |
| 5 | `toggleReadStatus` | Interfaces & object spreading |
| 6 | `Person` / `Student` | Classes & inheritance |
| 7 | `getIntersection` | Set-based array intersection |

## Blog Posts

- **blog-1.md** — `any` vs `unknown`: why `any` is a type safety hole and how type narrowing makes `unknown` the safer choice.
- **blog-2.md** — `Pick` & `Omit`: how utility types create interface slices without duplication, keeping code DRY.

## How to Run

```bash
npx ts-node solutions.ts
```