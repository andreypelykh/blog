---
title: Pug mixins for retina images and BEM methodology
description: The most frequently used PUG mixins
date: "2019-06-09T15:24:45+03:00"
---

## Useful mixin

I found this mixin the most broadly useful in my projects. It introduces cleaner syntax and preserves duplication of an image file name.

```pug
mixin retina-image(fullName)
  - const [name, extension] = fullName.split('.');
  img(src=`${name}.${extension}` srcset=`${name}@2x.${extension} 2x, ${name}@3x.${extension} 3x` alt="")&attributes(attributes)
```

It assumes that you will have only one dot in `fullName`. So if you use domains in the `src` you can change it a little.

```pug
mixin retina-image(fullName)
  -
    const [...rest, extension] = fullName.split('.');
    const name = rest.join('.');
  img(src=`${name}.${extension}` srcset=`${name}@2x.${extension} 2x, ${name}@3x.${extension} 3x` alt="")&attributes(attributes)
```

## One block expansion syntax

Imaging you have next code:

```pug
.section
  .container
    .row
      p Text1
    .row
      .column
        p Text2
      .column
        p Text3
    .image
      img(src='' alt='')
```

You could notice that it is not very complex markup, but it takes 11 lines of code. Also, it's very narrow. If you have much more lines that you need to scroll a lot in your document.

So there is a thing that you can improve a little. I'can say that this dramatically changes your markup, but it is helpful to know about this feature. Fairly saying I just recently discovered it. The feature is [block expansion](https://pugjs.org/language/tags.html#block-expansion). In this case, your code will look like this:

```pug
.section: .container
    .row: p Text1
    .row
      .column: p Text2
      .column: p Text3
    .row: .image: img(src='' alt='')
```

As you can see we saved 5 lines of code and still maintain readability. Personally, I think that it's even easier to read it because you don't need to move your eyes vertically a lot.

## BEM helpers

If you haven't used BEM methodology you should check their [website](http://getbem.com/). The talk is about BEM naming convention.

```pug
.block
  .block__element1 Text1
  .block__element2.block__element2--modificator Text2
```

You see there is a lot o repeating. And it's hard to read this. I found 2 options how you can make using BEM with Pug much easier.

### Option 1
It's a popular [bemto.pug](https://www.npmjs.com/package/bemto.pug) library. So basically it's just a set of Pug mixins. Using it your code will look this way:

```pug
+b.block
  +e.element1 Text1
  +e.element1.--modificator Text2
```

The disadvantage of this plugin is if you want to use a tag different from `div` you need to specify it as your first uppercased CSS class.

```pug
+b.block
  +e.H2.element Text1
```

### Option 2

I tried searching for a better solution and found [gulp-pugbem](https://www.npmjs.com/package/gulp-pugbem) plugin. Using it you can write code like this:

```pug
.block
  ._element1 Text1
  ._element1.-modificator Text2
```

It's very clean and you can easily use other tags and compose it with other mixins.
