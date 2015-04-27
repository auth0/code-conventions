# Auth0 Front-end Guidelines

**Semantics**

Keep your markup as lean as possible and make sure you make good use of HTML tags, use the right elements to describe your content.

```html
<!-- Not so good -->
<div class="text">This is a user.</div>
<div class="avatar">
  <img src="avatar.jpg">
</div>

<!-- Better -->
<p>This is a user.</p>
<img class="avatar" src="avatar.jpg" alt="Username">
```

**Accessibility**

- Use the alt attribute properly and always include it in required elements.
- Write links and buttons as ‘button’ and ‘a’ accordingly.
- Remember as per HTML5 [anchors can be block level](http://html5doctor.com/block-level-links-in-html-5/).
- Label all your form controls
- Make sure your page is navigable using the keyboard’s tab key.
- `:focus` styles are just as important as `:hover` styles.

```html
<!-- Not so good -->
<img src="cat.jpg" onclick="window.location = '/cats'">

<div class="button">Click me I'm a button</div>

<!-- Better -->
<a href="/cats" title="Go to Cats section">
  <img src="cat.jpg" alt="A cat.">
</a>

<button>Click me I'm a button</button>
```
**Boolean attributes**

Boolean attributes don’t need a value specified. The presence (true) or absence (false) of them in an HTML tag will suffice as per HTML5 spec.

**Declaration order & grouping**

When possible, try to enforce the following order and grouping on your property declarations. This makes css dramatically more readable.

- Positioning
- Box model
- Typographic
- Visual

**Naming**

- Use dashes and lowercase letters for your class names. 
- Keep classes as short as possible, but prefix them accordingly when they belong to a specific component.
- Namespace js behaviors and bindings using `.js-*`, but keep css styling out of them. This way they won’t get deleted by error.
- Namespace component states by using a `.is-*` prefix. (e.g. `.is-open`, `.is-collapsed`, `.is-loading`)

**Animations**

Use transitions when possible. Transition only affected properties to save on performance. (e.g. `border 0.5s ease` is better than `all 0.5s ease`)

Avoid animating properties other than `opacity` or `transform` as these perform much better.

```css
/* Not so good */
.cabinet {
  transition: all 0.5s ease;
  margin-top: 0;
}

.cabinet.open {
  margin-top: 20px;
}
  
/* Much better */
.cabinet {
  transition: transform 0.5s ease;
  transform: translateY(0px);
}

.cabinet.open {
  margin-top: 20px;
  transform: translateY(20px);
}
```

**Units**

Use the right unit for the job. 

- Favor `rems` or `ems` for relative values, preferably typography.
- Don’t be afraid of using `px` when you want to exactly define a measure.
- Use `vh`, `vw`, `vmin` and `vmax` when you need measures [relative to the viewport](https://dev.opera.com/articles/css-viewport-units/). 
- Use `rgba` if you need transparent colors, else use hex values (eg. `#000`).
- Don't abuse [`calc()`](https://developer.mozilla.org/es/docs/Web/CSS/calc).
- Don't abuse `!important`.

**Variables**

Avoid arbitrary colors and [magic numbers](https://css-tricks.com/magic-numbers-in-css/). Favor defined units and have your colors be or descend from defined colors.

```css
/* Not so good */
.cabinet {
  background: #BADA55;
  padding-bottom: 27px;
}
  
/* Much better */
.cabinet {
  background-color: color_black;
  padding-bottom: gutter;
}
```

**Selectors & Architecture**

- Avoid selectors tightly coupled to the dom. Use only classes and no element tags. This keeps your components element flexible.
- Avoid nesting. Try to keep the cascade to a maximum of 3 elements. Use prefixes to avoid unnecessary nesting.
- When keeping CSS in different files, organize them by components rather than pages. 

**Breakpoints**
- Mobile `(min-width: 320px)`
- Mobile landscape `(min-width: 568px)`
- Tablet `(min-width: 768px)`
- Desktop `(min-width: 992px)`
- Desktop HD `(min-width: 1400px)` 

Use our [breakpoint mixin](https://github.com/auth0/styleguide/blob/master/lib/mixins/index.styl#L11) to declare your media queries. It helps keep code clear and media queries consistent.

**Breakpoint usage and media queries**

- Keep media-query declarations as close as possible to their relevant selector. This will prevent other developers from missing them or making mistakes.
- Ideally, use one media query declaration per each rule, as opposed to having them at the end of a document or in separate files.
- Use mobile-first media queries when possible. (e.g. `min-width: 480px`).

```javascript
/* Very maintainable */
.hero-cta
  color: blue;
  border-radius: 3px;
  padding: 10px 20px;
  max-width: 100%;
  
  +breakpoint("tablet")
    max-width: 620px;
    font-size: 18px;
  
  +breakpoint("desktop")
    max-width: 740px;
    font-size: 20px;
    
.main-area .hero-cta
  color: red;
  
  +breakpoint("desktop")
    max-width: 100%;
    
.other-cta
  color: yellow;
  
.another-cta
  color: teal;
    
/* Not very maintainable */
.hero-cta
  color: blue;
  border-radius: 3px;
  padding: 10px 20px;
  max-width: 100%;

.main-area .hero-cta
  color: red;
  
.other-cta
  color: yellow;
  
.another-cta
  color: teal;
  
@media (min-width: 768px)
  .hero-cta
    max-width: 620px;
    font-size: 18px;
  
@media (min-width: 992px)
  .hero-cta
    max-width: 740px;
    font-size: 20px;
    
  .main-area .hero-cta
    max-width: 100%;
```

**Complex Grids**

Use the [grid mixin](https://github.com/auth0/styleguide/blob/master/lib/mixins/index.styl#L36) when a declarative grid doesn’t meet your needs. This can help you reorder components across different resolutions.

```javascript
/* Stylus */
.item-list
  grid(2, "20px", "li")
  
  +breakpoint("tablet")
    grid(3, "20px", "li")
    
  +breakpoint("tablet-landscape")
    grid(4, "20px", "li")
    
  +breakpoint("desktop")
    grid(5, "20px", "li")
```

```html
<!-- Markup -->
<ul class="item-list">
  <li><img class="avatar" src="mark.jpg" alt="Mark"></li>
  <li><img class="avatar" src="steve.jpg" alt="Steve"></li>
  <li><img class="avatar" src="bill.jpg" alt="Bill"></li>
  <li><img class="avatar" src="linus.jpg" alt="Linus"></li>
  <li><img class="avatar" src="larry.jpg" alt="Larry"></li>
  <li><img class="avatar" src="sergey.jpg" alt="Sergey"></li>
</ul>
```

**Retina assets**

Always [prefer SVG](https://css-tricks.com/using-svg/) over raster assets. When using raster assets like `.png` or `.jpg` remember to [compress them](https://tinypng.com/) and have them be retina-ready.

**Recommended Plugins**

Use [Boostrap](http://getbootstrap.com/) plugins, available through [Styleguide](https://github.com/auth0/styleguide), when possible.

We also like:
- [Owl Carousel](http://www.owlcarousel.owlgraphic.com/) for responsive sliders and carousels.
- [MomentJS](http://momentjs.com/) to handle dates.
- [TweenMax](http://greensock.com/tweenmax) to handle js animations.

