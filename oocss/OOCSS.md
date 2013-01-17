# Object Oriented CSS at Sunday Afternoon
## Why Object oriented CSS?

Object Oriented CSS is an approach to writing CSS that’s fast, maintainable, and standards-based. OOCSS applies exisiting concepts of Object Oriented programming to CSS. This adds much needed predictability to CSS so that a team of developers can cooperatively design a website or webapp.

### What defines a CSS Object?
"Basically, a CSS “object” is a repeating visual pattern, which can be abstracted into an independent snippet of HTML, CSS, and possibly JavaScript. Once created, an object can then be reused throughout a site."

By abstracting these components, a developer is able to reduce a lot of repetition from the CSS code, and make the CSS much more predictable and maintainable. 

For instance, the [media](http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code/) object describes a content block containing a fixed-size media element (e.g. image or video) along with other variable-size content (e.g. text). Another example is the module object, which describes a generic content block with a required body area and optional header and footer areas.

The concept of OOCSS was first presented by Nicole Sullivan at Web Directions North in Denver and is widely used, for example in popular frameworks like [Twitter Bootstrap](http://bootstrap.github.com), and [Zurb foundation](http://foundation.zurb.com).


## Main Principles of OOCSS
### Separate structure and skin
Separate structure and skin means to define repeating visual features (like background and border styles) as separate “skins” that you can mix-and-match with your various objects to achieve a large amount of visual variety without much code.

Separating structure and skin can also mean using classes to name your objects and their components, rather than relying solely on the semantics of HTML. For example, the media object, as proposed by Nicole Sulivan, is named with class="media", and its components are named with class="img" (for the image/video component) and class="bd" (for the body/text component).

By referencing these classes in your stylesheets (say, rather than directly styling the <img> element), your HTML can be flexible. For instance, if a new media element were to take off in the next few years (e.g. ``<svg>``), it could be integrated into the HTML without having to touch the CSS.

### Separate container and content
Essentially, this means “rarely use location-dependent styles”. An object should look the same no matter where you put it. So instead of styling a specific ``<h2>`` with ``.myObject h2 {…}``, create and apply a class that describes the ``<h2>`` in question, like ``<h2 class="category">``.

This gives you the assurance that: (1) all unclassed ``<h2>``s will look the same; (2) all elements with the category class (called a mixin) will look the same; and 3) you won’t need to create an override style for the case when you actually do want ``.myObject h2`` to look like the normal ``<h2>``. 
> An object should be open to extention, but closed to modification. 

In many large-scale websites there are occations where a block can be reused, but needs to be slightly adapted. Often, developers choose to make a block context-specific by applying a different styling based on the parent item as shown in the example below.

    .block {
      background: #fff;
      color: black;
      padding:10px;
    }
    
    #right-sidebar .block {
      padding: 20px;
      width: 200px;
      background: orange;
      color:#000;
    }
    
    #right-sidebar .sub-content .block {
      padding:10px;
      background: transparent;
      color:#fff;
    }
    
In small projects this often doesn't lead to any problems, however when a project becomes bigger this could lead to code-repetition and inconsistencies. 

This method of 'context-specific' styling has two problems. 
1. In large teams, each developer should be able to depend on a consistent behavior when applying styling. When styling is context-specific this might lead to unexpected behavior.
2. However the application of the ``.block`` class is not predictable. Because the styling is not predictable, developers might need to add additional styling to achieve the desired result.

A better solution would be to define a ``.block`` module, and extend it for specific purposes.

    .block {
      padding:20px;
      color: black;
      width: 100%;
    }
    
    .block .block--constrained {
      width: 200px;
      padding: 10px;
      margin:10px;
    }
    
    .block block--emphasis {
      border: 1px solid black;
      background: orange;
    }

In this example the block module can be reused for different content-containers. Next, the block can be constrained  in size by applying the extention ``block--constrained`` in other areas of the site. The emphasis extention can be used in situations where you would like to attract the attention of the user.

    <div id="main">
        <div class="block"> … </div>
        <div class="block block--emphasis"> … </div>
    </div>

    <div class="sidebar-right">
        <div class="block block--constrained"> … </div>
        <div class="sub-content">
            <div class="block block--constrained block--emphasis"> … </div>
        </div>
    </div>
    
##Seperation of responsibility between classes.
In object-oriented programming, the single responsibility principle states that: 
> every class should have a single responsibility, and that responsibility should be entirely encapsulated by the class. All its services should be narrowly aligned with that responsibility. 

This [SPR](http://csswizardry.com/2012/04/the-single-responsibility-principle-applied-to-css/) can also be applied in OOCSS. Take the following example:

    <a href=/product class=promo>Buy now!</a>
    
    .promo{
        display:block;
        padding:20px;
        margin-bottom:20px;
        background-color:#09f;
        color:#fff;
        text-shadow:0 0 1px rgba(0,0,0,0.25);
        border-radius:4px;
    }
    
In this case, the class doesn;t follow the SPR. The class, which styles a promotional box 1). positions a box and sets margins and spacing, and 2). Defines specific colors to attract attention.

By seperating the concern of this class in two seperate classes we get the following code:

    <a href=product class="box promo">Buy now!</a>
    
    .box{
        display:block;
        padding:20px;
        margin-bottom:20px;
    }
    
    .promo{
        background-color:#09f;
        color:#fff;
        text-shadow:0 0 1px rgba(0,0,0,0.25);
        border-radius:4px;
    }

The ``.box`` class can be reused in another context whithout direcly attacting attention like the promotional box intends to do. Also, by applying the ``.promo`` class we can attract attention to other inline elements.

Keep in mind to apply this method responsibily. It's easy to take SRP to far, which means you end up with one class for about every css-property.

We would be defeating our own purpose by defining classes for different font-sizes like so: ``.font-size-14`` and ``.font-size-18``. As mentioned earlier in this article, a seperation between structure and skin is often a sensible solution.

Applying this methodology has several benefits:

* Your CSS is a lot DRYer.
* You can make far-reaching changes to your designs by simply modifying a base abstraction only once.
* You can make safer changes because you know that when editing a class you are only ever altering one responsibility.
* You can combine responsibilities to make a variety of components from a lot of abstracted classes.

**Zurb Foundation** makes uses this method of seperation of responsibilites a lot in it's framework. For example the ``.radius`` and ``.rounded`` classes are globally applicable on block-elements to add the globally defined border-radius or fully round style.



## CSS Styleguide
A styleguide is used to ensure all developers in a team use the same method for naming ID's and Classes. These guidelines enourage the use of exisiting, common and sensible patterns. 

These days most people write CSS with hyphens as word separators. But hyphens alone are usually not enough to distinguish between different types of classes.

Expanding on the work of Nicolas Galagher, [Philip Walton](http://engineering.appfolio.com/2012/11/16/css-architecture/) illustrates the need for a naming convention. Consider the following example:

    /* A component */
    .btn-group { }
    
    /* A component modifier (modifying .button) */
    .btn-primary { }
    
    /* A component sub-object (lives within .button) */
    .btn-icon { }
    
    /* Is this a component class or a layout class? */
    .header { }
    
A more logical, human understandable and readable structure would be:

    /* A component */
    .btn-group { }
    
    /* A component modifier (modifying .button) */
    .btn--primary { }
    
    /* A component sub-object (lives within .button) */
    .btn__icon { }
    
    /* A layout class */
    .l-header { }
    
    /* State Rules */
    .is-state-type
    
    /* Non-styled JavaScript Hooks */
    .js-action-name
    
        
Sub-objects are prefixed with the module name. Some might argue that an .icon sub-element can be safely styled by using .btn.icon without having to worry about the styles spilling over to any other elements styled with the same class, however, this doesn’t prevent the styling of classes with those same names from spilling into this component. Read more about the [Open/Closed principle of CSS](]http://csswizardry.com/2012/06/the-open-closed-principle-applied-to-css/) on Harry Roberts Blog. Or take a look at this [presentation](http://lanyrd.com/2012/sxsw-interactive/spmqc/) by Andy Hume.

The `.js-action-name` tag is solely used to target elements trough javascript and shouldn't contain any styling. There is discussion about either using `.js-` classes or ``data-attributes``. Both have their merits, but Philip Walton makes a strong case for using `.js-` classes [based on performance](http://jsperf.com/class-vs-data-attribute-selector-performance).

#### Example of a Button module
The following piece of code shows a good example of defining a module. The code is a simplification of the excellent [Beautons](www.csswizardry.com/beautons) module by Harry Roberts.

    /* BASE BUTTON */

    .btn{
        display:inline-block;    
        vertical-align:middle;   
        cursor:pointer;          
        color:#fff;              
        background-color:#999;
        border-radius: 3px;
        line-height: 3em;
        padding-right: 1em;
        padding-left: 1em;   
    }
    
    .btn,
    .btn:hover{
        text-decoration:none;
    }
    
    .btn__icon{
        font-family: 'icon-font';
        display:inline-block;
        width:16px;
        height:16px;
    }

    /* BUTTON SIZES */ 
   
    .btn--small{
        padding-right:0.5em;
        padding-left: 0.5em;
        line-height:2;
    }
    
    .btn--large{
        padding-right:1.5em;
        padding-left: 1.5em;
        line-height:4;
    }
    

    .btn--full{
        width:100%;
        padding-right:0;            
        padding-left: 0;            
        text-align:center;
    }
    
    /* BUTTON SHAPES */ 
    
    .btn--round{
        border-radius:999px;        
    }
    
    .btn--square{
        border-radius:0;
    }
   

## Code Organization

Jonathan Snook offers a stucture for CSS organization in his book SMACSS, in which he presents his view on DRY and maintainable CSS. The structure he proposes is organized in 5 levels of specificity:

1. **Base** - Global Styles en resets
2. **Layout** - Positionering van de pagina-elementen (``.l-header``, ``.l-sidebar``)
3. **Modules** - De verschillende modules welke worden gebruikt
4. **State** - Defines the appearance of the current state a module/component is in. These states should indicate a temporary styling based on a specific temporary ruleset. (``.is-active``, ``.is-clicked``, ``.is-invalid``). These are different from extentions in the sense that extentions differ based on context, not states.
5. **Themes** - Especially used in very large projects which need to be able to switch between different 'themes' depending on for example the season or user-settings.

This structure can also be applied to the organization of the files in a project.

One specific css file (which is referenced in the index.html) imports  base.css, layout.css and modules.css. Modules.css in itself imports the seperate modules, which are defined in their own file for easy lookup and maintenance.

    --css/
     | --app.css
     | --base.css
     | --layout.css
     | --modules.css
     | --modules/
       | --module-1.css
       | --module-2.css
       | --module-3.css
       | --module-4.css
      

These files should be further optimized trough concatination and compression in order to reduce HTTP-requests and filesize. This can be done trough [GruntJS](www.grunt.com) or the [r.js optimizer](www.requirejs.com) of RequireJS. Otherwise a CSS-preprocessor like [Sass](www.sass.com) can help with the optimization process, as well as add several other benefits.

##Final Notes

The concepts described in this article build upon the excellent work of several other developers and authors:

* Nicole Sullivan (OOCSS, Stubbornella)
* Jonathan Snook (SMACSS, YAHOO, Shopify)
* Nicolas Gallagher (Normalize.css, )
* Harry Roberts (Inuitcss, CSSWizardry)
* Andy Hume (Microsoft, Clearleft)

###SOURCES:
I highly recommend reading the following articles about OOCSS, CSS architecture, BEM-methodology (analogous to OOCSS)

#### OOCSS
- [Exhastive article on the merits of OOCSS and SASS](http://engineering.appfolio.com/2012/11/16/css-architecture/)
- [HTML Semantics and Front-End architecture](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)
- [Single Responsibility Principle applied to CSS](http://csswizardry.com/2012/04/the-single-responsibility-principle-applied-to-css/)
- [A Nav Abstraction example](http://csswizardry.com/2011/09/the-nav-abstraction/)
- [Nicole Sullivan on OOCSS](https://github.com/stubbornella/oocss/wiki)
- [CSS for grownups](http://lanyrd.com/2012/sxsw-interactive/spmqc/)

#### CSS Styleguide
- [Nicolas Gallagher on Idiomatic CSS](https://github.com/necolas/idiomatic-css)

#### Block Element Module method
- [BEM Method](http://bem.info/method/)

