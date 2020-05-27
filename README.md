# React Spring Mouse Parallax Example Documentation

Content

- Overview
- CodeSandbox Link
- Documentation
- Resources / Contact Info

## Overview

The spring-based animation library [React Spring](https://www.react-spring.io/) can be used to create a variety of interesting and unique user experiences.

On their site they have a list of some use cases for the library. One I really like and see a bit on creative sites is the mouse parallax. 

The mouse parallax is an effect where different elements on the page respond with different degrees of movement to the mouse moving across the window. This effect can turn a static illustration into a more lively and stimulating experience.

Take a look at the codesandbox below to see a demonstration of how a mouse parallax can affect a page.

## CodeSandbox Link

https://codesandbox.io/s/r5x34869vq?from-embed

## Documentation

This example has a background image of a beach with a cloud, bird, and sun in the foreground. When you move the mouse, each image moves separately.

The html for this example is relatively simple. There is a container `div` element that holds four `animated.div` elements. The `animated.div` elements have their images set in CSS as background images. The `animated` component is imported from React Spring and is required for the library to perform animations. You can think of it as a special `div`.

There is a variable set by the `useSpring` hook called `props`. This variable controls the animation behind the parallax. The variable is an object with the properties `xy` and `config` and is updated by the function `set`. The `xy` property has a two item array with the current `(x, y)` coordinates stored in it while the `config` property has an object that describes how the animation is configured. Read [here](https://www.react-spring.io/docs/hooks/api) to find out more about the `config` property.

The container `div` has an event listener `onMouseMove` that waits for the mouse to move in the window. When this happens, a function is fired that takes in an object with the mouse event's `clientX` and `clientY` values, assigns them to the variables `x` and `y`, and passes them through the `set` function. If you don't recognize the notation in the arrow function, that's okay. It's the same as writing the following:
```
onMouseMove={(e) => set({ xy: calc(e.clientX, e.clientY)})}
===is equal to===
onMouseMove={({ clientX: x, clientY: y}) => set({ xy: calc(x, y)})}
```
[This](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent) is some documentation on the Mouse Event and [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_function_parameter) is some documentation on the object declaration in the function.

The clientX and clientY properties are used because the animation is happening within the viewport of the browser. Read [this](https://stackoverflow.com/questions/6073505/what-is-the-difference-between-screenx-y-clientx-y-and-pagex-y) stack overflow page for a good, short comparison of the different coordinates available.

The `set` function sets the `props` object's `xy` property by passing the values `x` and `y`  through the `calc` function. The `calc` function takes in the coordinates (x, y) and returns an array of coordinates describing the distance between the mouse and the center of the visible page.

When the `xy` property of the `props` object is updated, it gets passed to each instance of `animated.div` through a unique function that returns a string describing a transform property. The different math calculations in these functions account for the difference in movement when the mouse moves, creating the parallax effect.

Here's all of the javascript for the example: 
```
import React from 'react'
import ReactDOM from 'react-dom'
import { useSpring, animated } from 'react-spring'
import './styles.css' // // Icons made by Freepik from www.flaticon.com

const calc = (x, y) => [x - window.innerWidth / 2, y - window.innerHeight / 2]
const trans1 = (x, y) => `translate3d(${x / 10}px,${y / 10}px,0)`
const trans2 = (x, y) => `translate3d(${x / 8 + 35}px,${y / 8 - 230}px,0)`
const trans3 = (x, y) => `translate3d(${x / 6 - 250}px,${y / 6 - 200}px,0)`
const trans4 = (x, y) => `translate3d(${x / 3.5}px,${y / 3.5}px,0)`

function Card() {
  const [props, set] = useSpring(() => ({ xy: [0, 0], config: { mass: 10, tension: 550, friction: 140 } }))
  return (
    <div class="container" onMouseMove={({ clientX: x, clientY: y }) => set({ xy: calc(x, y) })}>
      <animated.div class="card1" style={{ transform: props.xy.interpolate(trans1) }} />
      <animated.div class="card2" style={{ transform: props.xy.interpolate(trans2) }} />
      <animated.div class="card3" style={{ transform: props.xy.interpolate(trans3) }} />
      <animated.div class="card4" style={{ transform: props.xy.interpolate(trans4) }} />
    </div>
  )
}

ReactDOM.render(<Card />, document.getElementById('root'))
```

That's it! If you have any questions check out the resources below or reach out to me.

## Resources / Contact Info

### Email

jeffgsch@gmail.com

### Website

http://hackersupreme.com

### Resources

-[React Spring Official Docs](https://www.react-spring.io/)
-[React Spring useSpring Hook](https://www.react-spring.io/docs/hooks/use-spring)
-[Mouse Event Documentation](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent)
-[ClientX PageX ScreenX Comparison](https://stackoverflow.com/questions/6073505/what-is-the-difference-between-screenx-y-clientx-y-and-pagex-y)
-[Object Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_function_parameter)
