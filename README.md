# SASS Coding Guidelines

Bigcommerce uses [SASS](http://sass-lang.com/) for style generation.

Bigcommerce's naming conventions are heavily influenced by the SUIT CSS framework
and align closely to [Medium](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06)'s
thoughts on CSS. Which is to say, it relies on _structured class names_ and
_meaningful hyphens_ (i.e., not using hyphens merely to separate words). This
helps to work around the current limits of applying CSS to the DOM (i.e., the
lack of style encapsulation), and to better communicate the relationships between
classes.


**Table of contents**

* [General Principles](#principles)
* [Specificity](#specificity)
  * [Performance](#performance)
* [Formatting](#formatting)
  * [Indentation](#indentation)
  * [Commenting](#commenting)
  * [Spacing](#spacing)
  * [Quotes](#quotes)
  * [Value Declaration](#value-declaration)
  * [Declaration Order](#declaration-order)
* [Pseudo Elements and Classes](#pseudo)
* [Units](#units)
* [Nesting](#nesting)
* [@extend or @inlcude](#extendorinclude)
* [Components](#components)
  * [componentName](#componentName)
  * [componentName--modifierName](#componentName--modifierName)
  * [componentName-descendantName](#componentName-descendantName)
  * [componentName.is-stateOfComponent](#is-stateOfComponent)
* [Utilities](#utilities)
  * [u-utilityName](#u-utilityName)
* [Variables and Mixins](#variables-and-mixins)
  * [Variables](#variables)
  * [Component / Micro app variables](#component-variables)
  * [Maps](#variable-maps)
  * [colors](#colors)
  * [z-index](#zindex)
  * [font-weight](#fontweight)
  * [line-height](#lineheight)
  * [Animations](#animations)
  * [Mixins](#mixins)
* [Polyfills](#polyfills)
* [JavaScript](#javascript)
* [Folder Structure](#folders)



<a name="principles"></a>
## General Principles
Strictly adhere to the agreed-upon style guide listed below. The general
principle is to develop DRY (Don't Repeat Yourself) SCSS, built around reusable
components and patterns.

* All code should look like a single person has typed it.
* Don't try to prematurely optimize your code; keep it readable and understandable.
* When building a component, always start by looking at existing patterns.
* Break down complex components until they are made up of simple components.
* Save your complex components as patterns so they can be easily reused.
* Build your component as a mixin which outputs _optional_ css.


<a name="specificity"></a>
## Specificity

On large code bases, it's preferable and a tonne more maintainable if the
specificity of selectors are all as equal and as low as humanly possible.


> **Do:**
> Use classes in your SCSS for styling.

```css
.component {
    ...
}
```


> **Don't:**
> Use ID's for styling. There is literally no point in using them.

```css
#component {
    ...
}
```


> **Do:**
> Style the base elements (such as typography elements).

```css
h1 {
    ...
}
```

> **Don't:**
> Reference or style descendent elements in your class selectors.

```css
.component h1 {
    ...
}
```


> **Don't:**
> Use overqualified selectors in your CSS. Do not prepend a class or ID with an element.

```css
div.container {
    ...
}
```

<a name="performance"></a>
#### Performance
Overly specific selectors can also cause performance issues. Consider:

```css
ul.user-list li span a:hover {
    color: red;
}
```

Selectors are resolved right to left, exiting when it has been detected the
selector does not match. This requires a lot of DOM walking and for large
documents can cause a significant increase in the layout time. For further
reading checkout: https://developers.google.com/speed/docs/best-practices/rendering#UseEfficientCSSSelectors

If we know we want to give all `a` elements inside the `.user-list` red on
hover we can simplify this style to:

```css
.user-list-link:hover {
    color: red;
}
```


<a name="formatting"></a>
## Formatting

The following are some high level page formatting style rules.

* Remove all trailing white-space from your file, Sublime Text can even do this upon saving.
  * Tip: set your editor to show white-space.
* Leave one clear line at the bottom of your file.

<a name="indentation"></a>
####Indentation

* Don't mix spaces with tabs for indentation.
* Use a soft-tab of 4 spaces.
* Use white-space to improve readability.
* Feel free to use indentation to show hierarchy.

> **Do:**

```css
.component {
    ...
}

.component-child {
    ...
}

.component-childSecond {
    ...
}
```

<a name="commenting"></a>
####Commenting
* Separate your code into logical sections using standard comment blocks.
* Leave one clear line under your section comments.
* Leave two clear lines above comment blocks.
* Annotate your code inside a comment block, leaving a reference # next ot the line.

> **Do:**
> Comment your code
> No really, comment your code

```css
// =============================================================================
// FILE TITLE / SECTION TITLE
// =============================================================================


// Comment Block / Sub-section
// -----------------------------------------------------------------------------
//
// Purpose: This will describe when this component should be used. This comment
// block is 80 chars long
//
// 1. Mark lines of code with numbers which are explained here.
// This keeps your code clean, while also allowing detailed comments.
//
// -----------------------------------------------------------------------------

.component {
    ... // 1
}

```

<a name="spacing"></a>
#### Spacing

* CSS rules should be comma separated but live on new lines.
* Include a single space before the opening brace of a rule-set.
* Include a single space after the colon of a declaration.
* Include a semi-colon at the end of every declaration in a declaration block.
* Include a space after each comma in comma-separated property or function values.
* Place the closing brace of a rule-set on its own line.
* CSS blocks should be separated by a single clear line.
* Add two blank lines between sections and one between sub-sections.

> **Do:**

```css
.content,
.content-edit {
    padding: 0;
    margin: 0;
    font-family: "Helvetica", sans-serif;
}


.newSection {
    ...
}

.newSection-edit {
    ...
}
```

> **Don't:**

```css
.content, .content-edit{
    padding:0; margin:0;
    font-family: "Helvetica",sans-serif}
.newSection {
    ...
}
.newSection-edit {
    ...
}
```

<a name="quotes"></a>
#### Quotes

> **Do:**
> Always use double quotes when available.
> Quote attribute values in selectors

```css
input[type="checkbox"] {
    background-image: url("/img/you.jpg");
    font-family: "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial;
}
```

> **Don't:**

```css
input[type=checkbox] {
    background-image: url(/img/you.jpg);
    font-family: Helvetica Neue Light, Helvetica Neue, Helvetica, Arial;
}
```

<a name="value-declaration"></a>
####When declaring values
* Use lower-case and shorthand hex values
* Use unit-less line-height values
* Where allowed, avoid specifying units for zero values
* Never specify the height property unless it's specifically needed (`min-height` is cool)
* Never use `!important` (Utility classes are an exception but still should be
avoided)
* Try to only style the property you are explicitly concerned with to reduce
over zealously resetting something you might want to inherit
  * `background-color: #333` over `background: #333`
  * `margin-top: 10px` over `margin: 10px 0 0`
  * Use shorthand if you can, be sensible

> **Do:**

```css
.component {
    background-color: #ccc;
    color: #aaa;
    left: 0;
    line-height: 1.25;
    min-height: 400px;
    padding: 0 20px;
    top: 0;
}
```

> **Don't:**

```css
.component {
    background: #ccc;
    color: #AAAAAA;
    left: 0px;
    line-height: 24px;
    height: 400px !important; //jerk #yolo FUUUUUU
    padding: 0px 20px 0px 20px;
    top: 0px;
}
```


<a name="declaration-order"></a>
####Declaration order
There are a millions opinions and thoughts on logical ordering and grouping.
Don't force someone to learn your opinion, ordering doesn't matter, consistency
does. Just use the alphabet, _everyone_ knows it.
* @extend
* @include
* Alphabetical, always.

> **Do**

```css
.component {
    @extend %a-placeholder;
    @include silly-links;
    color: #aaa;
    left: 0;
    line-height: 1.25;
    min-height: 400px;
    padding: 0 20px;
    top: 0;
    width: 150px;
}
```

> **Don't:**

```css
.component {
    min-height: 400px;
    left: 0;
    @include silly-links;
    top: 0;
    width: 150px;
    color: #aaa;
    @extend %a-placeholder;
    line-height: 1.25;
    width: 200px;
    padding: 0 20px;
}
```

<a name="pseudo"></a>
##Pseudo Elements and Classes
Pseudo elements and classes are very different things, as is the syntax used to
declare them. Declare pseudo _**classes**_ with a single colon. Declare pseudo
_**elements**_ with a double colon.

> **Do**

```css
.component:focus {
    ...
}

.component:hover {
    ...
}

.component::before {
    ...
}

.component::after {
    ...
}
```

> **Don't**

```css
.component:after {
    ...
}
```


<a name="units"></a>
##Units

> **Do:**

* Use `rem` units as primary unit type. This includes:
    * Positioning (`top`, `right`, `bottom`, `left`)
    * Dimensions (Such as `width`, `height`, `margin`, `padding`)
    * Font size
* Use `px` units as primary unit type for the following properties:
    * Border widths (`border: 1px solid #bada55;`)
* Use `%` units only if necessary, where `rem` will not suffice:
    * Positioning (`top`, `right`, `bottom`, `left`)
    * Dimensions (`width`, `height`)
* Line-height should be kept unit-less. If you find you're using a line-height
with a set unit type, try to think of alternative ways to achieve the same outcome.
If it's a unique case which requires a specific `px` or `rem` unit, outline the
reasoning with comments so that others are aware of its purpose.

> **Don't:**

* Avoid all use of magic numbers. Re-think the problem. (`margin-top: 37px;`)



<a name="nesting"></a>
##Nesting

Nesting is handy, _sometimes_, but will quickly conflict with our
[Specificty](#specificity) and [Performance](#performance) guidelines.

As we follow conventions and thoughts from popular and widely accepted
methodologies such as BEM, SMACSS and SUIT, the use of the Suffix can help immensely though.

* Just because you can, doesn't mean you should.
* [Parent Selector Suffixes](http://thesassway.com/news/sass-3-3-released#parent-selector-suffixes)
are neat, but not very searchable
* Watch your output, be mindful of [Specificty](#specificity) and
[Performance](#performance)
* Aim for a maximum depth of just 1 nested rule

> **Do:**

```css
.panel-body {
    position: relative;
}

.panel-sideBar {
    z-index: 10;
}

.panel-sideBar-item {
    cursor: pointer;
}

.panel-sideBar-item-label {
    color: #AEAEAE;

    &.has-smallFont {
        font-size: 13px;
    }
}
```

At its worst, this produces:
```css
.panel-sideBar-item-label.has-smallFont {
    font-size: 13px;
}
```

> **Don't:**

```css
.bc-tab-panel {

    .panel-body {
        position: relative;
        ...

        .panel-side-bar {
            z-index: 10;
            ...

            .panel-side-item {
                cursor: pointer;
                ...

                .panel-side-item-label {
                    color: #AEAEAE;

                    &.small-font {
                        font-size: 13px;
                    }
                }
            }
        }
    }
}
```

At it's worst, this produces:
```css
.bc-tab-panel .panel-body .panel-side-bar .panel-side-item .panel-side-item-label.small-font {
    font-size: 13px;
}
```

<a name="extendorinclude"></a>
## @extend or @include

* Excessive use of `@include` can cause unnecessary bloat to your stylesheet, but
gzip should help with that.
* Excessive use of `@extend` can create large selector blocks (not helpful in web inspector)
and hoisting of your selector can cause override and inheritance issues.
* We advise to `@include` over `@extend` generally, but use common sense. In situations where it's better to  `@extend` it's safer to do so on a placeholder selector.

> **Do:**
> Make use of placeholder selectors to separate repeated local styles

```css
%placeholderSelector {
    background-color: #eee;
}

.component1 {
    @extend %placeholderSelector;
    color: red;
}

.component2 {
    @extend %placeholderSelector;
    color: blue;
}
```

<a name="components"></a>
## Components

Syntax: `<componentName>[--modifierName|-descendantName]`

This component syntax is mainly taken from [Suit CSS](http://suitcss.github.io/)
with minor modifications.

Component driven development offers several benefits when reading and writing
HTML and CSS:

* It helps to distinguish between the classes for the root of the component,
descendant elements, and modifications.
* It keeps the specificity of selectors low.
* It helps to decouple presentation semantics from document semantics.

You can think of components as custom elements that enclose specific semantics,
styling, and behaviour.

**Do not choose a class name based on its visual presentation or its content.**

The primary architectural division is between components and utilities:

* componentName (eg. `.dropdown` or `.buttonGroup`)
* componentName--modifierName (eg. `.dropdown--dropUp` or `.button--primary`)
* componentName-descendantName (eg. `.dropdown-item`)
* componentName.is-stateOfComponent (eg. `.dropdown.is-active`)
* u-utilityName (eg. `.u-textTruncate`)
* `[<namespace>-]<componentName>[--modifierName|-descendentName]`



<a name="componentName"></a>
#### ComponentName

The component's name must be written in camel case. Use class names that are as
short as possible but as long as necessary.

* Example: `.nav` not `.navigation`
* Example: `.button` not `.btn`

```css
.myComponent { /* ... */ }
```

```html
<article class="myComponent">
  ...
</article>
```

<a name="componentName--modifierName"></a>
#### componentName--modifierName

A component modifier is a class that modifies the presentation of the base
component in some form. Modifier names must be written in camel case and be
separated from the component name by two hyphens. The class should be included
in the HTML _in addition_ to the base component class.

```css
/* Core button */
.button {
    ...
}

.button--primary {
    ...
}
```

```html
<button class="button button--primary">...</button>
```
<a name="componentName-descendantName"></a>
#### componentName-descendantName

A component descendant is a class that is attached to a descendant node of a
component. It's responsible for applying presentation directly to the descendant
on behalf of a particular component. Descendant names must be written in camel case.

```html
<article class="tweet">
  <header class="tweet-header">
    <img class="tweet-avatar" src="{$src}" alt="{$alt}">
    ...
  </header>
  <div class="tweet-body">
    ...
  </div>
</article>
```

You might notice that `tweet-avatar`, despite being a descendant of `tweet-header`
does not have the class of `tweet-header-avatar`. Why? Because it doesn't necessarily
**have** to live there. It could be adjacent to `tweet-header` and function the same
way. Therefore, you should **only** prepend a descendant with its parent if must
live there. Strive to keep class names as short as possible, but as long as necessary.

When building a component, you'll often run into the situation where you have a
list, group or simply require a container for some descendants. In this case, it's
much better to follow a pattern of pluralising the container and having each
descendant be singular. This keeps the relationship clear between descendant levels.

> **Do:**

```html
<nav class="pagination">
  <ul class="pagination-list">
    <li class="pagination-listItem">
      ...
    </li>
  </ul>
</nav>
```

```html
<ul class="breadcrumbs">
  <li class="breadcrumb">
    <a class="breadcrumb-label" href="#"></a>
  </li>
</ul>
```

> **Don't:**
> Avoid verbose descendant names

```html
<nav class="pagination">
  <ul class="pagination-pages">
    <li class="pagination-pages-page">
      ...
    </li>
  </ul>
</nav>
```

```html
  <ul class="breadcrumbs">
    <li class="breadcrumbs-breadcrumb">
      <a class="breadcrumbs-breadcrumb-label" href="#"></a>
    </li>
  </ul>
```

<a name="is-stateOfComponent"></a>
#### componentName.is-stateOfComponent

Use `is-stateName` for state-based modifications of components. The state name
must be Camel case. **Never style these classes directly; they should always be
used as an adjoining class.**

JS can add/remove these classes. This means that the same state names can be used
in multiple contexts, but every component must define its own styles for the state
(as they are scoped to the component).

```html
<article class="tweet is-expanded">
  ...
</article>
```

```css
.tweet {
    ...
}

.tweet.is-expanded {
    ...
}
```

<a name="utilities"></a>
## Utilities

Utility classes are low-level structural and positional traits. Utilities can
be applied directly to any element; multiple utilities can be used together;
and utilities can be used alongside component classes.

Utility classes should be used sparingly, lean towards component level styling
to make for as reusable HTML patterns as possible.


<a name="u-utilityName"></a>
#### u-utilityName

Syntax: `u-<utilityName>`

Utilities must use a camel case name, prefixed with a `u` namespace.


<a name="variables-and-mixins"></a>
## Variables and Mixins

Variables and Mixins should follow similar naming conventions.

<a name="variables"></a>
####Variables

Syntax: `[<componentName>[--modifierName][-descendentName]-]<propertyName>-<variablename>[--<modifierName>]`

Variables should be named as such, things that can change over time.

Variables should also follow our component naming convention to show context
and be in camelCase. If the variable is a global, generic variable, the property
name should be prefixed first, followed by the variant and or modifier name for
clearer understanding of use.

> **Do:**
> Abstract your variable names

```CSS
$color-brandPrimary:  #aaa;
$fontSize:            1rem;
$fontSize--large:     2rem;
$lineHeight--small:   1.2;
```

> **Don't:**
> Name your variables after the color value

```css
$bigcommerceBlue:     #00abc9;
$color-blue:          #00ffee;
$color-lightBlue:     #eeff00;
```

<a name="component-variables"></a>
####Component / Micro App level variables

Micro apps must base their local variables on the global variables primarily.
You may add your own specific variables as required if no global variable is available.

For portability, your component should declare it's own set of variables inside
it's own settings partial, inside the settings folder. Even if at the time, your
component only uses globally available variables from Bigcommerce's Library,
you should reassign the global variable to a local one.
If your component styles change from those global variables at all in the future,
less of your SCSS will have to change, as you only change the local variable value.

If your variable is scoped to your component, it should be namespaced as such following
our component naming conventions.

> **Do:**

```css
$componentName-fontSize:                                fontSize("small");
$componentName-decendantName-backgroundColor:           #ccc;
$componentName-decendantName-marginBottom:              fontSize("large");
$componentName-decendantName--active-backgroundColor:   #000;
```

```css
.componentName {
    font-size: $componentName-fontSize;
}

.componentName-decendantName {
    background-color: $componentName-decendantName-backgroundColor;
    margin-bottom: $componentName-decendantName-marginBottom;
}

.componentName-decendantName--active {
    background-color: $componentName-decendantName--active-backgroundColor;
}
```

<a name="variable-maps"></a>
####Maps, maps are cool

Variable maps with a simple getter mixin, can help simplify your variable names
when calling them, and help better group variables together using their
relationship. [More info](http://erskinedesign.com/blog/friendlier-colour-names-sass-maps/)

> **Do:**

```scss
// Setting variables and mixin
// -----------------------------------------------------------------------------

$colors: (
    primary: (
        base: #00abc9,
        light: #daf1f6,
        dark: #12799a
    ),
    secondary: (
        base: #424d55,
        light: #ccc,
        lightest: #efefef,
        dark: #404247
    ),
    success: (
        base: #bbd33e,
        light: #eaf0c6
    )
);

@function color($color, $tone: "base") {
    @return map-get(map-get($colors, $color), $tone);
}
```


```scss
// Usage
// -----------------------------------------------------------------------------

body {
    color: color("secondary");
}

h1,
h2,
h3 {
    color: color("secondary", "dark");
}

.alert {
    background-color: color("primary", "light");
}

.alert-close {
      color: color("primary");
    }

.alert--success {
    background-color: color("success", "light");

    > .alert-close {
        color: color("success");
    }
}
```

**Every variable used in the core architecture must be based off the global
variables.**

<a name="colors"></a>
#### Colors

Please only use the globally available colors from the Bigcommerce Library.
Your Micro app or component shouldn't really have a need for a *new* color.
This creates consistency and sanity.

Avoid using the `darken(color, %)` and `lighten(color, %)` mixins for similar reasons.

Use the color maps available to you:
```css
.component {
    background-color: color("brand", "primary");
}
```

<a name="zindex"></a>
#### z-index scale

Please use the z-index scale defined in the Bigcommerce Library under global settings.

`zIndex("lowest")` or `zIndex("high")` for example.


<a name="fontweight"></a>
#### Font Weight

Bigcommerce apps share a strict set of font weights. Never declare a new font weight,
only use the available font settings from the Bigcommerce Library. e.g.

```css
fontWeight("light");
fontWeight("semibold");
```


<a name="lineheight"></a>
#### Line Height

The Bigcommerce Library also provides a line height scale. This should be used for blocks
of text. e.g.

```css
lineHeight("smallest");
lineHeight("large");
```

Alternatively, when using line height to vertically centre a single line of text,
be sure to set the line height to the height of the container - 1.

```CSS
.button {
  height: remCalc(50px);
  line-height: remCalc(49px);
}
```

<a name="animations"></a>
#### Animations

Animation delays, durations and easing should be taken from the global framework

<a name="mixins"></a>
#### Mixins

Mixins follow regular camel case naming conventions and do not require namespacing. If you are creating a mixin for a utility, it will need to match the utility name (including `u` namespacing).

* `@mixin buttonVariant;`
* `@mixin u-textTruncate;`

<a name="polyfills"></a>
## Polyfills

At Bigcommerce, we try not to replicate CSS polyfills that auto-prefixer can
supply in a Grunt or Gulp task. This keeps our SCSS code base lean and future proof.

> **Do:**

```css
.button {
    border-radius: 3px;
}
```

> **Don't:**
> Add vendor prefixes at all.

```css
.button {
    @include border-radius(3px);
}
```
```css
.button {
    -ms-border-radius: 3px;
    -o-border-radius: 3px;
    -webkit-border-radius: 3px;
    border-radius: 3px;
}
```

<a name="javascript"></a>
## JavaScript

syntax: `js-<targetName>`

JavaScript-specific classes reduce the risk that changing the structure or theme
of components will inadvertently affect any required JavaScript behaviour and
complex functionality. It is not necessary to use them in every case, just
think of them as a tool in your utility belt. If you are creating a class, which
you don't intend to use for styling, but instead only as a selector in JavaScript,
you should probably be adding the `js-` prefix. In practice this looks like this:

```html
<a href="/login" class="btn btn-primary js-login"></a>
```

**Again, JavaScript-specific classes should not, under any circumstances, be styled.**

<a name="folders"></a>
##Folder Structure

####General principle
The SASS folder structure we're proposing, will have two slight differences
between the core framework and micro apps, however the bulk of the structure is
identical between the two.

The idea is to have the least amount of folders as possible, but as many as we
need to define clear, structured patterns in your SASS.

####Core Folder Structure

```
.
├── sass
|   ├── settings/
|   └── tools/
|   └── vendor/
|   └── components/
|   └── utilities/
```


**/settings:** Contains all of your SCSS variables for your framework. Within
this folder is 1 primary file `_settings.scss`, which imports all other variable
files that have been broken into logical files such as `_colors.scss`,
`_typography.scss`, `_z-index.scss` and your chosen frameworks variables, for
example `_foundation.scss`.

**/tools:** Contains all of your SASS mixins. Within this folder is 1 primary
file `_tools.scss`, which imports all other mixin files that have been broken into
logical files. No framework mixins should appear in this folder as they can be
consumed from their own respective `/vendor` or `/components` folder.

**/vendor:** Contains all of your vendor files, such as normalize, bootstrap,
foundation, animate.css, etc. All readily consumable third party files belong
here, and can be imported in the framework base file as required. No file in
the /vendor folder should ever be modified.

**/components:** Contains all of your components. This folder will make up the
vast majority of your compiled CSS. All custom components simply live inside
this folder, for example `/components/component/_component.scss`. It also contains
the consumed version of your chosen /vendor framework's components, which will be
reworked to adhere to the Naming Conventions and Style Guide. They will live
inside a subfolder of the framework's name, for example `/components/foundation/`.

**/utilities:** Contains all CSS snippets which can be applied to your HTML for
quick prototyping, or a case by case basis where a unique, yet repeatable style
is required. Every utility found within this folder will have both a class and a
mixin. An example being, truncatedText. You can utilise it by applying the class
`.u-truncatedText` or by applying a mixin, `@include truncatedText;`.


####Micro App Folder Structure

```
.
├── sass
|   ├── settings/
|   └── tools/
|   └── vendor/
|   └── layouts/
|   └── components/
|   └── utilities/
|   └── shame/
```

There are only two minor differences in a micro app, when compared to the core
framework. Firstly you'll notice that the /framework folder has been replaced by
a /layouts folder, as well as the addition of the /shame folder.

**/layouts:** Contains your micro app "layouts" and page specific styling.
Essentially creating the wrapping sections and grids for your app, where the
core framework's components will live inside. For example, a layout file could
potentially be your micro app's navigation. It is important to note that the
styling for individual navigation items and all other inner components styling
do not live in the layout file. There purpose is purely for the containing
elements that set up your app.

**/shame:** This interestingly named folder has one goal: to remain empty. It's
purpose is the place for all of those hot fixes or quick hacks in throwing
something together. Any code which you don't feel is "complete" can also live
here. It creates clear visibility on less than perfect code, especially when it
comes to code reviews, and creates a trail for your dodgy code that if left
somewhere in your component/layout code could be forgotten about and left.

**A note on: /components & /utilities:** Within your micro app, these folders should
only house your app's unique code. Any repeatable component or utility that could
be re-used across other micro apps should be flagged and a PR opened for adding
it into the core framework.
