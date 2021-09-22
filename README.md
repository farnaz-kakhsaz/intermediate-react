# Intermediate React

> This is a note from Intermediate React, v2 - Frontend Masters course

## Table of Contents

1. Hooks
2. [TailwindCSS](#tailwindcss)
   - [CSS & React](#css--react)
   - [Tailwind Setup](#tailwind-setup)

## TailwindCSS

### CSS & React:

The thing I'll say about TailwindCSS is that, one, this isn't really tied to React all that much. Everything I'm about to tell you, you could probably lift and shift this to Angular or Ember or just plain HTML and CSS, so that's one benefit of it. Two metally to kinda get up to speed of how Tailwind workd, bucause it's very different from writing normal CSS. But once you've done it, I think you can go really fast with it.

So th thing to say about Styled Components and Emothion is they have **run times**. So they have something that **executes in JavaScript** with your React App. Which incurs some overhead. Even they're very fast, they're very small, **there're still two or three kilobytes**, **they're still have JavaScript execution cost**, which can get hung up by the main thread, **you still have to worry about performance with your CSS**. There's just a little bit of overhead which is not zero overhead.

> The benefit of **TailwindCSS** is that, it has **no execution layer**, it is just a style sheet.

**TailwindCSS** has no execution layer, it is just a style sheet. You've offloaded everything to the CSS rendering engine, which is very very fast and very performant.

> **TailwindCSS** => runs with CSS rendering engine => Which is very very fast and very performant
>
> **Styled Components** and **Emothion** => run with JavaScript engine (run times) => not very fast and performant

So with that in mind, all we have to do with TailwindCSS is **we don't have to import any JavaScript or anything like that, we're just gonna add CSS classes**, that's it.

### Tailwind Setup:

Let's get it set up. Run this:

```properties
npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat@2.0.3 postcss@7.0.35 autoprefixer@9.8.6 @tailwindcss/postcss7-compat@2.0.3
```

- Under the hood, Parcel processes all your CSS with PostCSS with the autoprefixer plugin. This works like Babel: it means you can write modern code and it'll make it backwards compatible with older browsers. Since we're modifying the PostCSS config (like we did with Babel earlier in this project in the Intro part) we have to give it the whole config now.

- We're using Parcel 1 in this project. They're heads-down on making Parcel 2 a reality which supports PostCSS 8. Parcel 1 is stuck on PostCSS 7. Tailwind 2 requires PostCSS 8 but luckily they provide a compatibility library with PostCSS 7. That's what `npm:@tailwindcss/postcss7-compat@2.0.3` is doing: it's called an alias. We're installing `@tailwindcss/postcss7-compat` and then aliasing it to `tailwindcss`. If you're brave you could try upgrading to Parcel 2 and then this wouldn't be necessary.

Okay, now let's get our Tailwind project going. Run `npx tailwindcss init` (which is going to generate a config for us). Like `tsc init` for TypeScript, this will spit out a basic starting config in tailwind.config.js. Should look like:

```JavaScript
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
};
```

Lastly, let's go modify our style.css file.

```CSS
@tailwind base;
@tailwind components;
@tailwind utilities;
```

This is how we include all the things we need from Tailwind. This is what allows Tailwind to bootstrap and only include the CSS you need to make your app run.

Then create `.postcssrc` in the root directory of the project. And in th `.postcssrc`:

```properties
{
    "plugins": {
        "autoprefixer": {},
        "tailwindcss": {}
    }
}
```

Then delete your `.cache`, delete your `dist`, `note_modules` folders, and then reinstall everything again (`npm install`). And then run `npm run dev` again after that.

In App.js, put this:

```JavaScript
// the div right inside <ThemeContext.Provider>
<div
  className="p-0 m-0"
  style={{
    background: "url(http://pets-images.dev-apis.com/pets/wallpaperA.jpg)",
  }}
>
  […]
</div>

```

What's cool about the TailwindCSS is that's a lot of class names. But what Tailwind actually does for you is it just goes in detects, you only used `p-0` and `p-8` and I'm only going to include those classes, then it leaves out the rest of the classes automatically based on the build process. Kind of like tree shakes your styles. Which is neat, then you get the minimum viable CSS, which is to say that your Tailwind CSS files are going to be microscopic. They're gonna be absolutly tiny, it's gonna be the bare minimum styles need to accomplish what you need.

Your trade-off here is that your classNames here are going to get very long. Cuz if you wanna apply a lot of styles like sometimes you do in CSS
they can tend to get pretty long. So this is the trade-off that you're making here.

**`em` measurement:** My parent element's font-size. `em` actually acale with your font size, cuz it's the size of a literal "m" (like an "m" no the page)
**`rem` measurement:** The root element's font-size. It's root em

> While `em` is relative to the font-size of its direct or nearest parent, `rem` is only relative to the html (root) font-size.

> All of the measurements in Tailwind are all done with `rem`s.

> PostCSS, you can kind of think of to fill a similar role ot SASS.
> PostCSS, in and of itself, is like Babel that all PostCss does is process CSS. Babel by itself does nothing, Babel needs plugins and presets and configuration stuff for it to do anything. PostCss is the exact same way, whereas SASS out of the box does a bunch of stuff. SASS is a language unto itself.
>
> So in this particular case, PostCSS is the processor, but the actual transformations are coming from Tailwind. And that's the difference there. Tailwind needs PostCSS. PostCSS doesn't necessarily need Tailwind.
