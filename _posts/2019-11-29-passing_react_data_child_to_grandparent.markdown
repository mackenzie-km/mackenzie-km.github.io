---
layout: post
title:      "Passing React Data: Child to Grandparent"
date:       2019-11-29 00:54:47 -0500
permalink:  passing_react_data_child_to_grandparent
---


Yesterday, while working on my final project for React, I got super confused about onClick events & passing data.

Also.... next week I have a React interview!! Wish me luck!

Here's what I learned:

## Handling events
My first question: when do I write `onClick={handleClick}`,  `onClick={handleClick(value)}`,  `onClick={() => handleClick(value)}` ,  `onClick={(value)=>handleClick(value)}`.....???


### When to write function or function()
Imagine you have  
const handleClick = () => {
  alert('You clicked!')
 }
 
* **PASS IT ON**: `onClick={handleClick}` => handleClick is just the function you already defined. So it will find the const you defined as handleClick, and just spit out a handleClick *reference*.
* **INVOKE IT**: `onClick={handleClick()}` every time you render the page, handleClick() runs. This is actually invoking your function each time. You probably don't want an alert each time, so avoid this. 

### When to write () => function() or just function
> How do I pass a parameter to an event handler or callback? You can use an arrow function to wrap around an event handler and pass parameters.

Imagine you have  
```
const handleClick = (message) => {
  alert(message)
 }
```
 

*  **ANNOYING**: we didn't want to do `onClick={handleClick("hello")}` because a hello alert will pop up each itme you refresh.  
* **FAIL**: `onClick={handleClick()}` what am I doing... telling a function to just go ahead and be invoked, yet not giving it any of the data it needs to run properly?! You will just get a popup that says undefined. **If you fail to pass in a parameter where applicable, JS doesn't give you an error... but it does say undefined.**
*  **REMEMBER**: If you just say `onClick={handleClick}` like before, no values will be passed on. The popup will say [Object Object]. And `console.log(handleClick)` will show the function again... 
```
value => {
    props.sendToParent(value);
  }
```
	
* **WAY TO GO:** `onClick={() => handleClick("hello")}` - A) the () will ensure that you tell onClick to run a function when it receives a click event. B) This function has the right context (defined by the arrow function). C) It also allows you to pre-populate handleClick with the message (preventing the undefined issue, or preventing it from just spitting out the function again). 


### Passing parameters & dealing with event methods 

**What if I want to access the event itself** (such as to do event.preventDefault() for submit, or to manipulate an HTML element?)

```
const handleClick = (event, value) => {
    console.log(event.target)
		alert(value)
}
```

`onClick={(event) => handleClick(event, "hello")}`

This would console.log the html element that triggered the event.

**Final note:** using an arrow function like` () => function()` creates a new function each time. This may slow down your app. Look into optimization techniques if your app is getting slowed down by this. 

![http://giphy.com/gifs/JMV7IKoqzxlrW/html5](http://giphy.com/gifs/JMV7IKoqzxlrW/html5)

On that note, let's get back to the second question: how do I pass data from a child to a grandparent?

## Passing from child to grandparent
### You start at the bottom
* Create an element that can be clicked/toggled (such as the button below).
* But this isn't any old button... it has **onClick**! When you click on it, it invokes a pre-populated function.* Remember all we talked about above. *

```
// child.js
import React from 'react';

const Child = (props) => {
  const handleClick = (value) => {
  props.sendToParent(value)
}

  return (
    <div>
      <button onClick={()=>handleClick(true)}>Click!</button>
    </div>
  )
}

export default Child;
```

### Now we go to the parent

**#### LONG SIDEPOINT:**
Note how receiveFromChild isn't written as () => receiveFromChild(value). 

Why? props.sendToParent(value) was in the child. This was equal to our pre-populated function.
**Sooo... receiveFromChild is pre-populated. **

If you try to do `receiveFromChild(value),` it complaints that value is not defined.
If you try to do `receiveFromChild("hello!")`, it complains that **props.sendToParent is not a real function (in child.js)**. A simple `console.log(receiveFromChild("hello")` prints undefined. 

Wut?!
receiveFromChild already returns a pre-populated function. To do `receiveFromChild(value)` is to try to stick a value into a callback function that doesn't exist. There's no place to put the value! Everything is pre-populated. This isn't a callback function, so adding another set of parenthesis (value) doesn't work.

To visualize this issue better, imagine this example:
```
let popup = () => {
  alert("WOW")
}
```

If you do `alert()` - you'll see a popup. If you do `alert()()`, you will see a popup... and then an error in console log. It runs `alert() `just fine, but after it then tries to run the second set of parenthesis as a function with this value (in this case, no value; empty parenthesis) plugged in. But the return value of alert() isn't a callback function. It's just undefined. So you're trying to say `undefined()`... no bueno.

**Anyways, a simple reference to the parent's callback function (receiveFromChild) will sufficiently reference our pre-filled function.**

```
// parent.js
import React from 'react';
import Child from './child.js';

const Parent = (props) => {
  const receiveFromChild = (value) => {
  props.sendToGrandparent(value)
  }

return (
  <div>
    <Child sendToParent={receiveFromChild} />
  </div>
  )
}

export default Parent;

```
### Finally - time for gramps to receive it

Nothing new here! Other than that the `alert(value) `is finally invoked.

```
// grandparent.js 
import React from 'react';
import Parent from './parent.js';

const Grandparent = (props) => {
const finallyReceive = (value) => {
alert(value)
}

return (
<div>
<Parent sendToGrandparent={finallyReceive} />
</div>
)
}

export default Grandparent;

```



## Hope this clears things up!
I feel more ready for my interview.... kinda!

