---
title:  "Understanding React ReRender"
parent: React
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

## Ways of preventing re-renders:
## Children as props
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

## components as props
It is same as the above. **props are not affected by the state change**.

## React.memo
Wrapping a component in `React.memo` will stop downstream chian of re-renders that is triggered somewhere up in the
render tree, unless this component's props really have changed.

**All props** that are not primitive values have to be memorized for `React.memo` to work.

There is only one scenario, when memoizing props on a component make sense: **when every single prop and the component 
itself are memorized**. In this case, the component will only rerender when the props change.

