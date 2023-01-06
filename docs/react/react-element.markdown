---
title:  "React Element"
parent: React
---

# What is React Element

When we do:

```jsx
const element = <Child/>
```

it is what we called an **Element**. It a syntax sugar for function `React.createElement()` that returns a plain object
that describes the component that we want to render.

React's `createElement()` method is called internally for JSX. We could use it as replacement for the
returned JSX (for the sake of learning).

**Only** when we include it in the return result, which is a synonym for _render those stuff_ in function components,
and
**only** after Parent component renders itself, will the actual render of `Child` component be triggered.

```jsx
const Parent = () => {
    const child = <Child/>
    return (
        <div>
            {child}
        </div>
    )
}
```

## Updating Element

Elements are **immutable** objects. The only way to update an Element, and trigger its rerender, is
to re-create an object itself.

If Parent re-renders, child will be re-created. child is a new Element from React's point of view, but in exactly the
same place and exactly the same type, so React will just update the existing component with the new data (i.e.,
rerender)

This is how `React.memo` works: because of memorization, the object will not be re-created, thus React will think that
it does not need to update, and child's re-render won't happen.

## Elements describe the tree

An element is a plain object describing a **component instance** or **DOM node** and its _desired properties_.
An element is not an actual instance. Rather, it is a way to tell React what you want to see on the screen.

It has only two field:

- type: string \| function \| class
- props: Object

When an element's type is a string, it represents a DOM node with the tag name. When the type is a function or a class,
it represents a component. They can be nested and mixed with each other.

When React sees an element with a function or class `type`, it will pass the `props` to the function/class and render;
it will repeat the process until it knows the underlying DOM tag elements for every component on the page.

## Top-Down Reconciliation

React will compare the element and its children to the previous one, and only apply the DOM updates necessary to bring
the DOM to the desired state.

React reconciliation starts when you call `ReactDOM.render()` or `setState()`. By the end of reconciliation,
React knows the resulting DOM tree, and a renderer like react-dom or react-native applies the minimal
set of change necessary to update the DOM to the desired state.

 

