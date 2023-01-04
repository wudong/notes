---
title:  "Understanding React ReRender"
date:   2023-01-04 00:00:00 +0000
tags: react web
---

# Why React ReRender?
The causes of all rerender are:
* state changes;
* parent re-renders: it will rerender all of its children;
* context changes: when the value in Context Provider changes, **all** components that use this Context will rerender,
  even if they don't use the changed portion of the value. 
* hooks changes: state/context changes within the hook, or chain of the hooks.
* props changes: when the props change, the component will rerender. However, in order for props to change, the parent
  component must rerender, which will cause all of its children to rerender anyway.
  
  It only matters when memoization is used, then props change becomes important.

## Preventing re-renders:
### Children as props
When the state is used on an element that wraps a slow portion of the render tree, it cannot be extracted
easily. A tyipical example would be `onScroll` or `onMouseMove` callbacks attached to the root element of a component.

In this case, state management and components that use it can be extracted into a small component, and the slow 
component can be passed tot it as `children` prop.

Because `children` are just prop, they will not be effected by the state change and therefore will not rerender.

```jsx

const ComponentWithScroll = ({children})=> {
    const [scroll, setScroll] = useState(0);
    const onScroll = (e) => {
        setScroll(e.target.scrollTop);
    }
    return (
        <div onScroll={onScroll}>
          {children}
        </div>
    )
}

const Component = () => {
    // some slow code
    return (
        <ComponentWithScroll>
            <VerySlowComonent />
        </ComponentWithScroll>
    )
}

```

When `ComponentWithScroll` re-renders because of state change, its props stay the same; thus any `Element` passed 
as `children` will not be re-created, therefore will not be re-rendered.

However, if the children is passed as a **render function** (`{children({data: 'some'})}`), it will be re-rendered even 
if it does not depend on the changing state.

### components as props
It is same as the above. **props are not affected by the state change**.

### React.memo
Wrapping a component in `React.memo` will stop downstream chian of re-renders that is triggered somewhere up in the
render tree, unless this component's props really have changed.

**All props** that are not primitive values have to be memorized for `React.memo` to work.

There is only one scenario, when memoizing props on a component make sense: **when every single prop and the component 
itself are memorized**. In this case, the component will only rerender when the props change.

# React Element
When we do:
```jsx
const element = <Child/>
```
it is what we called an **Element**. It a syntax sugar for function `React.createElement()` that returns a plain object 
that describes the component that we want to render.

**Only** when we include it in the return result, which is a synonym for _render those stuff_ in function components, and 
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
same place and exactly the same type, so React will just update the existing component with the new data (i.e., rerender)

This is how `React.memo` works: because of memorization, the object will not be re-created, thus React will think that 
it does not need to update, and child's re-render won't happen.

## Elements describe the tree
An element is a plain object describing a **component instance** or **DOM node** and its _desired properties_.
An element is not an actual instance. Rather, it is a way to tell React what you want to see on the screen.

It has only two field:
- type: string | function | class
- props: Object

When an element's type is a string, it represents a DOM node with the tag name. When the type is a function or a class,
it represents a component. They can be nested and mixed with each other.

When React sees an element with a function or class `type`, it  will pass the `props` to the function/class and render; 
it will repeat the process until it knows the underlying DOM tag elements for every component on the page.

## Top-Down Reconciliation

React will compare the element and its children to the previous one, and only apply the DOM updates necessary to bring
the DOM to the desired state.

React reconciliation starts when you call `ReactDOM.render()` or `setState()`. By the end of reconciliation, 
React knows the resulting DOM tree, and a renderer like react-dom or react-native applies the minimal
set of change necessary to update the DOM to the desired state.

