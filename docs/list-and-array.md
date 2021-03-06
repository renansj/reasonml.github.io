---
title: List & Array
---

## List

Lists are:

- homogeneous
- immutable
- fast at prepending items

```reason
let myList = [1, 2, 3];
```

Reason lists are simple, singly linked list.

### Usage

You'd use list for its resizability, its fast prepend (adding at the head), and its fast split, all of which are immutable yet efficient!

The standard lib provides a [List module](/api/List.html) (and its counterpart with labeled arguments, `ListLabels`).

#### Immutable Prepend

Use the spread syntax, which is just `List.cons`:

```reason
let myList = [1, 2, 3];
let anotherList = [0, ...myList];
```

`myList` didn't mutate. `anotherList` is now `[0, 1, 2, 3]`. This is efficient (constant time, not linear). `anotherList`'s last 3 elements are shared with `myList`!

**Note that `[a, ...b, ...c]` is a syntax error**. We don't support multiple spread for a list. That'd be an accidental linear operation (`O(b)`), since each item of b would be one-by-one added to the head of `c`. You can use `List.concat` for this.

Updating an arbitrary item in the middle of a list is discouraged, since its performance and allocation overhead would be linear (`O(n)`).

#### Access

`switch` (described in the [pattern matching section](pattern-matching.md)) is usually used to access list items:

```reason
let message =
  switch myList {
  | [] => "This list is empty"
  | [a, ...rest] => "The head of the list is the string " ++ a
  };
```

To access an arbitrary list item, use `List.nth`.

### Tips & Tricks

Feel free to allocate as many empty lists as you'd like. As explained in the [variant section for list](variant.md#honorable-mentions), an empty list is actually a parameter-less variant constructor under the hood, which compiles to a mere integer. No extra memory allocation needed.

To understand how prepend can be immutable and `O(1)` at the same time, see also the previous link.

### Design Decisions

In the future, we might provide an out-of-the-box list data structure that's immutable, resizable and features all-around fast operations.

## Array

Arrays are like lists, except they are:

- mutable
- fast at random access & updates
- fix-sized on native (flexibly sized on JavaScript)

You'd surround them with `[|` and `|]`.

```reason
let myArray = [|"hello", "world", "how are you"|];
```

### Usage

Standard library [Array](/api/Array.html) and [ArrayLabel](/api/ArrayLabels.html) module. For JS compilation, you also have the familiar [Js.Array](https://bucklescript.github.io/bucklescript/api/Js.Array.html) bindings API.

Access & update an array item like so:

```reason
let myArray = [|"hello", "world", "how are you"|];

let firstItem = myArray[0]; /* "hello" */

myArray[0] = "hey";

/* now [|"hey", "world", "how are you"|] */
```

The above array access/update is just syntax sugar for `Array.get`/`Array.set`.

### Tips & Tricks

If you're compiling to JavaScript, know that Reason arrays map straightforwardly to JavaScript arrays, and vice-versa. Thus, even though arrays are fix-sized on native, you can still use the `Js.Array` API to resize them. This is fine.
