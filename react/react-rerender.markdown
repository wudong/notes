---
title:  "Understanding React ReRender"
date:   2023-01-04 00:00:00 +0000
categories: react web
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



