# Front-End-Development-Guidelines
An ongoing, continually updated, repository of my personal front end development guidelines

* [Atomic Design](http://patternlab.io/)
* Object-oriented SCSS for modular development 
* [CSS guidelines](http://cssguidelin.es/) / [ITCSS](https://twitter.com/itcss_io) scss structure 
* Loosely follows [BEM](https://css-tricks.com/bem-101/) to create clear CSS components but with less parent element name repetition and allowing low-level nesting. 

### Updates coming soon
* Basic accessibility guidelines
* Media query usage instructions
* Further elements / components to be added to style guide
* New grid system - susy / neat

Brad Frost's Atomic Design and Harry Robert's CSSGuidlines / ITCSS share the same fundamental approach - begin with simple elements / atoms and building up to complex groups of elements / molecules. This works in favor of the cascade and reduces CSS specificity issues.

###Further Reading
Harry Roberts and Brad Frost have strongly influenced the guidelines set out below and I highly recommend giving them a read.
* [Harry Roberts - MindBEMding – getting your head ’round BEM syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
* [Harry Roberts - CSS guidelines](http://cssguidelin.es/) 
* [Brad Frost - Atomic Design](https://vimeo.com/67476280)

#Front End Guidelines

###CSS
The CSS should aim to be object orientated to create reusable, flexible classes which makes future updates easy and logical. Write component based css inline with an atomic design methodology. 

###Main objectives of CSS
* Component driven
* Object Orientated
* Maintainable

###Components
Component based CSS keeps the code clean and modular. A BEM approach would be like this, note repeated parent class name and lack of nesting:
```
.car {}
	.car__wheel {}
	.car__door {}
```
When looking at both the HTML markup and the CSS it's often clear where and what the parent is. So it is acceptable to not reference the parent name in the child element class names. Instead we can use nesting for child elements when working with clear CSS components:
```
.car {
	.wheel {}
	.door {}
}
```

##Nesting and specificity
Specificity can cause issues when working with the cascade, but if component nesting is kept low (ideally no more than 3) and clear, it will cause minimal problems. 

BEM approach encourages having a unique class for every element, keeping all elements at the root of the cascade, eliminating any cascade entirely. However the cascade can be useful, speed up development and cause no issues if components are built responsibly with specificity in mind.

###Class names
Classe names are all lower case and use underscores for spacing:   
```
<header class="site_header"></header>
```

**Modifier classes** use the element's class name with an added double hyphen followed by the modifier name. Use 3 spaces before modifiers for readability and modifier class names come last within the class list.:   
```
<div class="modal   modal--small"></div>
```


### Context vs component / element styles
Implementation styles are based on context, visual styles are based on component / element.
Style elements based on what they are - not where they are.
Style code should be based on the component.
Implementation code should be based on thier context.

For example, if a button is in a component in which the button needs to be floated right, it is due to being inside the parent component. The button styling doesnt need modifying. Simply reference the button inside of the compnent class and add the implementation classes. 

A button modifier class in this situation would be another option, although it may create unneeded additional markup, as well as making future style modifications within layout changes more difficult.

###Content agnostic class names
Component class names should not be tied to the content inside the component. For example using the the class name `welcome_message` would not be ideal. As that layout / component could be used elsewhere for other purposes a more appropriate class name would be `message`.

###Avoid use of IDs 
IDs have the highest specificity of any CSS selector and as a result they cause issue with overwriting styles in the cascade. Avoid using any IDs.
Sometimes developers will add an ID to an element for the sole purpose of referencing the ID in javascript. However this isnt neccessary, instead use a class and reference the element using getElementsByClassName("class_name")[0]; (note 'ClassName' is beneficial over 'ClassList' due to it's wider browser support).

###Quasi-Qualified Selectors
>`ul.nav {}`
Here we can see that the .nav class is meant to be used on a ul element, and not on a nav. By using quasi-qualified selectors we can still provide that information without actually qualifying the selector    
`/*ul*/.nav {}`

###Javascipt hooks 

All javascript hooks should be prefixed with 'js_'. for example:   
```
<button class="btn js_open_modal"></button>
```

Toggle classes for state changes. Do not toggle data attributes for state changes:
>A common practice is to use data-* attributes as JS hooks, but this is incorrect. data-* attributes, as per the spec, are used to store custom data private to the page or application (emphasis mine). data-* attributes are designed to store data, not be bound to.


###CSS structure
```
/**
 * CONTENTS
 *
 * GLOBAL
 * Global...............Globally-available variables, mixins and config.
 * Gloablly availible settings. Config switches. Brand colors, mixins etc
 *
 * GENERIC
 * Normalize.css........A level playing field.
 * Box-sizing...........Better default `box-sizing`.
 * Ground zero styles. Low specificty. Far-reaching. Resets, noramlize etc
 *
 * BASE
 * Headings.............H1–H6 styles.
 * unclassed HTML elements - ul, p, a, buttons, input, textarea, select, blockquote,
 *
 * OBJECTS
 * Wrappers.............Wrapping and constraining elements.
 * OOCSS. Design patterns. No cosmetics. Begin using classes exclusively. Agnostically named (eg ui_list)
 *
 * COMPONENTS
 * Page-head............The main page header.
 * Page-foot............The main page footer.
 * Buttons..............Button elements.
 * Designed pieces of UI. Still only using classes. Explicitly named.
 *
 * Optional - Theme Layer 
 * Optional - A/B Test layer
 *
 * TRUMPS
 * Text.................Text helpers.
 * overides, utiliies and helpers, 
 * affect one piece of the DOM at a time
 * usually carry important
 */
 ```

###CSS titles
All section titles are listed as:   
```
/*------------------------------------*\ 
Section Title
\*------------------------------------*/
```

Section subtitles listed as:   
```
/*-----------buttons-------------*/ 
```

##CSS Components
CSS is to be structured in modular, component based blocks. This means that all styles for individual components are kept in one place for easier edits and maintainability.

```
/*----------- Hero -------------*/
.hero { width: 100%; height: 400px; position: relative;
	.callout { position: absolute; right: 10%; bottom: 10%; width: 380px; height: 300px; background: #0E1E3F; color: white; overflow: hidden; padding: 2rem;
		h2 { margin-top: 0; font-size: 4rem; color: white; }
		p,a{font-size:2rem;}
		p { color: yellow; font-weight: bold; margin: 1rem 0; }
		a { color: yellow; font-weight: bold;}
	}

    /*modifiers*/ 
    &.hero--callout_left {
    	.callout { right: auto; left: 10%; }
    }   

    /*responsive*/ 
    @media (max-width:830px){
    	.callout {
    		width:100%; right:0; left:0; bottom:0; top:auto; height:auto;
    		h2 {font-size: 3rem; }
    		p,a{font-size:1.8rem;}
    		p {margin: 1rem 0; }
    		a {}
    	}
    }
}
```

###HTML Markup

### Sematics
Use appropriate semantic elements where possible. If unfamiliar with semantic HTML please see here - [HTML5 Semantics – Smashing Magazine](https://www.smashingmagazine.com/2011/11/html5-semantics/)

3 spaces before modifiers for readability, and odifiers come last within the class name list:   
```
<div class="modal   modal--small"></div>
```   

####Meaningful use of white space
>As with our rulesets, it is possible to use meaningful whitespace in your HTML. You can denote thematic breaks in content with five (5) empty lines, for example:

```
<header class="page_head">
    ...
</header>





<main class="page_content">
    ...
</main>
```


