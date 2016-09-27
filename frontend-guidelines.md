#About

These Guidelines have been created to help alleviate some of the problems with multiple developers of different skills and levels working on large projects. It also helps with creating consistency between projects so that if a new developer takes on a project they will have some understanding of it.

##Our main goals are:

- Maintainability
- Transparency and Readability
- Scalability

##Class Naming - Custom BEM

BEM (Block, Element, Modifier) is a class naming method. When multiple developers are working on a project there is the chance for things to be overwritten. BEM is a way of solving this. The key principle of BEM is to keep Blocks (or Modules) independent, this is handled by having unique class names. The version of BEM we are using is a modified one based on a version by Nicholas Gallagher. 

*Example*

```
.block{}
.block__element{}
.block--modifier{}
```

.block represents the higher level of an abstraction or component / module.
.block__element represents a descendent of .block that helps form .block as a whole.
.block--modifier represents a different state or version of .block.

It’s easier to read the article below than for me to write it all out again. One key thing to remember is that not everything needs to use BEM. Elements such as .heading or .site-logo can be kept out of it. We also want to try and create reusable modules as much as possible. For modules where there are some differences I would recommend applying a modifier on the top level of the module.

*Example*

```
<section class=”listing”>
    <article class=”listing__item”>...</article>
    <article class=”listing__item”>...</article>
</section>

<section class=”listing listing--small”>
    <article class=”listing__item”>...</article>
    <article class=”listing__item”>...</article>
</section>
```

In the example above all of the markup for these modules would be the same. It’s just the styles that differ. By using the .listing--small modifier we can minimise markup changes and reuse the module. We can then reuse the .listing styles and overwrite what we need with the .listing-small modifier in CSS. 
We can then use nesting in CSS if the parent has a modifier to overwrite the styles.

*Example*

```
.listing{
    background-color:#EBEBEB;

    &__item{
        width:100%;
        border:1px solid #CCC;
    }
}
.listing--small{
    background-color:#FFF;

    .listing__item{
        width:50%;
        border-top:0;
    }
}
```

Must read:
http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/

More info: 
http://www.smashingmagazine.com/2012/04/16/a-new-front-end-methodology-bem/
http://www.mathayward.com/modular-css-with-sass-and-bem/

##Decoupling

We should embrace the concept of decoupling HTML, CSS and Javascript where possible. Decoupling is the practice of not using HTML classes for more than one thing meaning should have specific classes for Javascript and different ones for CSS. This makes the code more readable and there is less chance of someone changing something and it having unintended consequences. At present changes to one class or the HTML order can have negative effects on Javascript and CSS. This helps to prevent it.

Class naming conventions become key when Decoupling. The method does involve the increased use in classes but that is negligible when it comes down to file size and the benefits of maintainability are much greater.

More info:
http://philipwalton.com/articles/decoupling-html-css-and-javascript/

##Decoupling - CSS

Decoupling CSS can seem a bit like you are being inefficient and lazy. However due to the complex structure of some sites and the amount of developers it’s beneficial as it creates less conflicts and more modular pieces of work. Most have seen it where a change to one style can cause something in a different part of the site to act unexpected. The key thing with CSS Decoupling is to not rely on the HTML being in the order you expect it to be. If something needs a style put a class on it, do not nest your selectors and do not tag qualify them either. I would go so far as to have classes on most things. 

*Don’t Tag Qualify*

```
a.button{}
li.item{}
```

// Tag Qualifying is where you put the HTML element before the class

There are exceptions to these rules. If a CMS or plugin you are using makes it difficult to add specific classes then by all means nest your selectors. Or if you want a specific style to take effect if an element follows or is nested within another - however if you can use a class do so.

Note: Sass makes it very easy to nest selectors. Just because you can doesn’t mean you should. There are cleverer ways to nest with the Ampersand.

*Don’t*

```
a.button{}
.menu li a{}
.menu .link{}
```

*Do*

```
.button{}
.menu__item{}
```

##Decoupling - Javascript part 1

This is where I have seen Decoupling having the most benefit. You can easily create pieces of functionality that can be reused across the site. The key thing with Javascript Decoupling is that if the stylesheet was removed the functionality would still be there. 

The example below has classes for styling. The Javascript specific classes are prepended with js-- , this is clear indication that this class is used for Javascript purposes. When completing the JS class name try and use something that could be reused across the site, but is descriptive enough so that people won’t be struggling to decipher it.

*Do*

```
<section class=”tab-section js--tab”>
    <div class=”tab-section__panel js--tab__panel”>
    …
    </div>
</section>
```

##Decoupling - Javascript part 2

The second part of Javascript decoupling is when you are adding classes to the markup via Javascript. These should have their own naming convention again. The easiest one to understand seems to be is-- . This allows you to be descriptive with the second part of the class name. You shouldn’t prepend js-- to the beginning of these classes as then it creates a difference between what classes Javascript is using and what it is creating.

*Do*

```
$(‘.element’).addClass(‘is--hidden’);
```

##ID’s and Classes

Don’t use ID’s for styling purposes as specificity can become an issue
Don’t nest unnecessarily. 3 should be the maximum but not the usual.
Don’t target elements based on their element type. If an element needs styling use a class instead. This prevents issues if the element needs to be changed.

##Font Stacks

When declaring a new font make sure you are using a font stack with web safe fonts as a fallback. Any font properties should be written as a string except the final serif / san-serif / monotype property as this is a default value.

*Don’t*

```
font-family: Ubuntu;
font-family: “Ubuntu”, arial;
font-family: ‘Ubuntu’, “san-serif”;
```

*Do*

```
font-family: “Ubuntu”, “Helvetica”, san-serif;
font-family: ‘Ubuntu’, ‘Helvetica Neue’, ‘Helvetica’, san-serif;
```

##Iconfonts

When creating an iconfont don’t assign arbitrary letters to symbols. Instead use a Unicode character that could be used a replacement if the iconfont doesn’t load. If there is no character suitable use a letter that is symbolic (e.g. facebook = f).

You can get Unicode characters from http://unicode-table.com/

*HOLD ON* - It turns out that some devices do not support all unicode symbols (specifically Samsung devices). So either we’ll need to find a safe list or use letters again. Urgh.

*Don’t*

```
.icon-chevron-up:before {
  content: "e";
}
```

*Do*

```
.icon-chevron-up:before {
  content: "\2191";
}
```

##Font-face

When importing multiple weights or styles of a font using font-face you can specify which font file you want to load with the font-weight and font-style properties. This makes managing your fonts easier and allows quick changing between weights and styles.

*Don’t*

```
@font-face {
  font-family: 'Museo-300';
  src: url('../fonts/Museo-300.eot');
}

@font-face {
  font-family: 'Museo-500';
  src: url('../fonts/Museo-500.eot');
}
```

*Do*

```
@font-face {
  font-family: 'Museo';
  src: url('../fonts/Museo-300.eot');
  font-weight:normal;
}

@font-face {
  font-family: 'Museo';
  src: url('../fonts/Museo-500.eot');
  font-weight:bold;
}
```

##Fonts - Sass

You should store the font stacks as a variable. This will allow a single place to manage all fonts.
Iconfonts should be in their own partial scss file.
Font-face declarations should be in their own partial scss file.

##Commenting - CSS

Comment anything that isn’t obvious to someone else coming into the project. For example, if you have used an overflow:hidden to clear a float let people know with a comment. If using a preprocessor (which all new projects should) make sure you are using // to denote single and multi line comments.

*Do*

```
.element {
    overflow:hidden; // Clear float
}

// This expands to full width of page container
.element {
    position:absolute;
    top:0;
    left:0;
    right:0;
    height:40px;
    background-color:#CCC;
}
```

##CSS Documentation

To improve transparency and maintenance times across projects we should start documenting sections of our CSS. At present we will be using the KSS format. This is a simple format that allows us to put the related markup in a comment above the relevant CSS. This does require a bit more work and making sure that you keep the main site markup and KSS markup up to date is paramount or it becomes useless and confusing. 

Not every CSS rule should be documented. You should document a rule declaration when the rule can accurately describe a visual UI element in the styleguide. Each element should have one documentation block describing that particular UI element’s various states.

For example we should be documenting base level elements such as tables, buttons, icons, colours, links and typography. Other things to consider would be sliders / hero area, content modules, breadcrumbs, accordions and tabs.

One of the advantages of using KSS is that we can use it to generate Living Style Guides which can help create consistency across projects.

More info:
http://warpspire.com/kss/

##Background Images

When calling a Background Image in CSS use a variable inside the url string (otherwise know as Interpolation). This way if the folder structure changes we can quickly fix all images with one variable. Feel free to have multiple Variables if you need different paths to images.

*Don’t*

```
.logo {
    background:transparent url(‘/assets/images/logo.png’) 0 0 no-repeat;
}
```

*Do*

```
$image : ‘/assets/images/’;

.logo {
    background:transparent url(‘#{$image}logo.png’) 0 0 no-repeat;
}
```

##SASS Variable Naming

We want variables to be clear immediately but without taking up a lot of space in the markup. For colour variables the easiest way for the majority seems to be using the Colour Name. You can get this from a Hex code and it will generate names such as ‘Tangerine Yellow’.

If you have a colour that’s going to be used a lot as it’s a brand colour you could name it with a colour agnostic variable name such as $brandColor or $brandBgColor. If you have got a colour that is used as a border or a highlight you can use those as variables e.g. $borderColor, $highlightColor.

It’s a good idea to put any values you will be reusing in Sass as a variable. This includes but is not limited to: transitions, border styles, fonts, image paths, widths, font sizes and colours.

##SASS Includes

We should use partials when including modular Sass files into the main Sass file. This is so that if we ever compile directly with Sass it knows not to compile these files into individual CSS files.

*Don’t*

```
@include ‘includes/header.scss’;
```

*Do*

```
@include ‘includes/_header’;
```

##Preprocessed Languages

When working with preprocessed languages, such as Sass, the compiled output should be ignored through Git's .gitignore file (in the case of Sass, compiled CSS should be ignored). If not using a continuous delivery system, the Head, TTL or Senior Developer in charge of the merge requests should compile and merge . If using a continuous delivery system, compiling preprocessed languages should be part of the build step and absolutely no compiled code should be put under version control.

##Loading Javascript

All javascript should be loaded last. Generally people have got used to loading Modernizr in the head but this too should be loaded at the bottom of the page. The main reason people loaded Modernizr in the head was because of the HTML5 shiv you can include with it. From now on best practice is to not include the shiv with Modernizr. Load the shiv separately with a conditional comment in the head. Everything else should be at the bottom unless there is a good reason not to. 

*Do*

```
<!--[if lt IE 9]>
    <script src="bower_components/html5shiv/dist/html5shiv.js"></script>
<![endif]-->
```

