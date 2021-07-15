# Use On Click Outside

This package can be used for listening to clicks outside of an element.

In React, event handlers determine what action is to be taken whenever an event is fired. This could be a button click or a change in a text input.

Essentially, event handlers are what make it possible for users to interact with your React app.

If you have tried to develop your own dropdown, modal or popover. I’m guessing you’ve been in this situation. “How do I capture clicking outside component so I can close it??”

Luckily it’s not that complex.

## Usage

```js
import * as React from 'react'
import useOnClickOutside from 'useonclickoutside'

export default function Modal({ close }) {
  const ref = React.useRef(null)
  useOnClickOutside(ref, close)

  return <div ref={ref}>{'Modal content'}</div>
}
```

## Update

If installing packages is not your thing, or you have too many packages, I've also decided to include a step by step process.

1. Create a reference to your outer div (We’ll be using `useRef`)

```react
const node = useRef();
... some other code ...
return (
  <div ref={node}>
    ...
  </div>
);
```

2. Add event listener mousedown (or click) to the document whenever this component is appear on screen (eg. mount) and also don’t forget to remove the event on unmount too. (This time, We will be using `useEffect` to handle it.)

```react
useEffect(() => {
  // add when mounted
  document.addEventListener("mousedown", handleClick);
  // return function to be called when unmounted
  return () => {
    document.removeEventListener("mousedown", handleClick);
  };
}, []);
```

3. Inside the event (handleClick) **node.current.contains(e.target)** will return true if whatever you are clicking is inside the “node” ref.

```react
const handleClick = e => {
  if (node.current.contains(e.target)) {
    // inside click
    return;
  }
  // outside click 
  ... do whatever on click outside here ...
};
```

There you have it.