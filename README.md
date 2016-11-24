# css-animations.js

## Create, Modify, and Remove CSS3 Keyframe Animations with javascript!

This library uses the [CSS DOM
API](http://www.w3.org/TR/DOM-Level-2-Style/css.html) to access CSS3
keyframe animations, enabling you to do anything you want with them
from javascript.

You can add, modify, and remove individual keyframes from existing
animations, in addition to creating and deleting animations
themselves.

## Demos

* Adding a keyframe to an existing animation: http://jsbin.com/esocaw/19/
* Modifying a keyframe of an existing animation: http://jsbin.com/esocaw/20/
* Creating a new animation: http://jsbin.com/esocaw/22/

## Usage

Download `css-animations.js` to your project and load it. It works as
an AMD module as well as a global object.

If not using it as an AMD module, it exports a global objects named
`CSSAnimations` that allows you to access the API.

**NOTE:** This library searches all your stylesheets immediately when
  it is loaded. This will cause problems if you are somehow
  dynamically loading stylesheets after js is loaded (problems being
  missing animations). If this is common, I will change this library.

## Browser Support

This is a new library and hasn't been extensively tested. It has only
been tested in Firefox 17+ and Chrome 23+. It should work in browsers
that implement unprefixed CSS3 animations and webkit (special
prefixing has only been applied to webkit as it is not unprefixed yet).

## API

### Animations

* `CSSAnimations.get(name)`: return a `KeyframeAnimation` object
  representing the animation named `name`

* `CSSAnimations.create(name, frames)`: create a new animation named
  `name` and return the newly constructed `KeyframeAnimation` object.
  If `frames` is suppled, add them to the new animation with
  `setKeyframes` (see below). `name` is optional; if not specified a
  random name is generated. `frames` is also optional.
  
  ```js
  // Create with name and keyframes
  var anim = CSSAnimations.create('foo', {
      '0%': { 'background-color': 'red' },
      '100%': { 'background-color': 'blue' }
  });

  // Create with keyframes, names is randomly generated
  var anim = CSSAnimations.create({
      '0%': { 'background-color': 'red' },
      '100%': { 'background-color': 'blue' }
  });
  
  // Create with just name and no keyframes
  var anim = CSSAnimations.create('foo');
  
  // Create with random name and no keyframes
  var anim = CSSAnimations.create();
  ```

* `CSSAnimations.remove(name)`: remove the animation named `name`.
  `name` can also be an instance of `KeyframeAnimation`. *Right now,
  you can only remove animations created with `CSSAnimations.create`.*

### KeyframeAnimation

The `KeyframeAnimation` object represents a CSS3 animation.

* `KeyframeAnimation.getKeyframe(text)`: return a `KeyframeRule`
  object representing the animation at the specified keyframe. `text`
  is a string that represents the keyframe, such as `"10%"`.
  
  ```js
  var rule = anim.getKeyframe('10%');
  ```

* `KeyframeAnimation.setKeyframe(text, css)`: set the CSS for a
  specified keyframe. `text` is a string the represents the keyframes,
  like `"10%"`, and `css` is a javascript object with key/values
  representing the CSS to set. It returns the same `KeyframeAnimation`
  object so you can chain it.

  ```js
  anim.setKeyframe('10%': { 'background-color': '#333333' });
  ```

* `KeyframeAnimation.setKeyframes(frames)`: Same as `setKeyframe`, but
  sets multiple keyframes at once. `frames` is an object with the
  percentage values, like `10%`, as keys and css as values.
  
  ```js
  anim.setKeyframes({
      '10%': { 'background-color': '#333333' },
      '20%': { 'background-color': '#666666' },
  });
  ```

* `KeyframeAnimation.clear()`: Remove all keyframes from this animation.

* `KeyframeAnimation.getKeyframeTexts()`: Get all the texts
  representing the keyframe positions, like `"10%"` and `"100%"`.

### KeyframeRule

The `KeyframeRule` object represents a specific animation keyframe.

* `KeyframeRule.keyText`: the text representing the keyframe position,
  like `"10%"`

* `KeyframeRule.css`: a javascript object representing the CSS for
  this keyframe

**NOTE:** In several places we represent CSS as javascript objects,
  but it does not transform property names to camelCase formatting.
  The keys in the object are the raw CSS properties and you'll most
  likely have to quote them because they contain dashed. For example,
  `css = { 'background-color': 'red' }` and `css['background-color']`.

## Examples

```js

// Changing an animation

var anim = CSSAnimations.get('pulse');
anim.setKeyframe('100%', { 'background-color': 'red' });

// Dynamically creating and applying an animation

var anim = CSSAnimations.create({
    '0%': { transform: 'translateY(0)' },
    '70%': { transform: 'translateY(50px)' },
    '100%': { transform: 'translateY(150px)' }
});

$(el).css({ 'animation-name': anim.name,
            'animation-duration': '1s' });
            
$(el).on('animationend', function() {
    CSSAnimations.remove(anim.name);
});
```

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/bf126290eab2936d8b163b0e01688290 "githalytics.com")](http://githalytics.com/jlongster/css-animations.js)
