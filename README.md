---
title: Making focus-visible happen where do you live
published: false
description: A quick guide to seting up focus-visible on your nuxt/tailwind projects
tags: Nuxt,Tailwindcss
cover_image: https://images.unsplash.com/photo-1578406033714-319ffaf023eb
---

In case you don't know  the `focus-visible` css selector allow us to target DOM elements that have been focused by a keyboard interaction, instead the tradiccional `focus` that target every focused element.

Why we  need that distintion in the first place ?
It all start with  accessibility (a11y) in  mind. If a user is browsing with a mouse or a phone, they know exactly what they are clicking. They don't need any clue about it, it might become even in a distraction.

But is a very diferent story for the keyboard user. Think for a moment, a father carrying their baby, people with injuries in a hand, people with a temporary or cronical healt illness that inables them to use a mouse... there is a lot of [them](https://www.interactiveaccessibility.com/accessibility-statistics) out there  and you should be including them in your target audience. It's the very right thing to do, **no excuses**.

If you haven't tried, go out there with your TAB key and browse around. You will need a visual feedback that shows you where you are, step by step. Some folks are very good at this, like twitter, but reallity is that offen, we don't even plan a keyboard only navigation in the first place.

So quick recap: we need a way to tell our keyboard people what elements they have focused in our app. Here is where our best buddy `focus-visible` came to play. Even well supported in modern browsers it's kinda new in the CSS playground. So we are going to set things up in the more compatible way.

## OK  let's make this happen !

You can find this [repo](#https://github.com/d4g0/focus-visible-sample) at GitHub, feel free to clone it.

Im going to use [Nuxt.js](https://nuxtjs.org/) and [Tailwind](https://tailwindcss.com/) because those are my favorite tools, but you can apply what you will learn here to every project. The real hero behind the scenes here is the `focus-visible` [Polyfill](https://github.com/WICG/focus-visible).

So create a new `nuxt-app`:
```bash
npx create-nuxt-app
```

Install Tailwind  on your own:
```bash
 yarn add -D @nuxtjs/tailwindcss tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
```

And install the pollyfill and a postcss plugin to make it even easier to work with it:
```bash
yarn add focus-visible postcss-focus-visible
```

Make sure to bundle the actual polyfill in your page. 
Nuxt  serves the static assets from the `static` forlder off your project, 
so i will clone the polyfill file there in a `scripts` folder. 
You can find him at `node_modules/focus-visible/dist/focus-visible.min.js`. 
I also clone the `.map` version to avoid unuseful warnings in the browser 
developer console.
Do wath you have to,  i will use nuxt config file since he allows you declare 
metadata there super easy ( i 💚️ that framework) .

```js
/* nuxt.config.js */

export default {
    head: {
        script: [
                { src: '/scripts/focus-visible.min.js', async: true, defer: true }
        ],
    }
}
```

I will add the postcss plugin because i want to use it in conjuntion with my tailwind utilities, but you will be just fine using the advices in the pollyfill [README](https://github.com/WICG/focus-visible).

I will add that plugin to my nuxt postcss config:

```js
export default {
    // ...
    postcss:{
        plugins:{
            'postcss-focus-visible': {}
        }
    }
}
```



Time to set up our Tailwind config:
```bash
npx tailwind init
```


Since the 'focus-visible' variant is by default [off](https://tailwindcss.com/docs/hover-focus-and-other-states) we shuold manually get it on:
( I will be targeting the `ring` utilities since that's the exact effect we want in our Ui)

```js
/* tailwind.config.js  */

module.exports = {
    //...
    variants: {
        extend: {
            ringColor: ['focus-visible'],
        },
  },
}

```

## All setted up, let's do some pair programing !

In my home page `(pages/index.vue)` i have craft this form:

```html
    <form class="mt-10 w-full max-w-sm" @submit.prevent="">
        <fieldset>
          <label for="petName" class="block ml-4">Pet Name</label>
          <input
            id="petName"
            type="text"
            placeholder="type your pet name here"
            class="block mt-2 px-4 py-2 w-full rounded-lg bg-gray-700 shadow-md focus:outline-none  focus:ring-2 focus:ring-indigo-600"
          />
        </fieldset>

        <div class="mt-6 w-full ">
          <button
            type="submit"
            class="py-2 w-full px-4 rounded-lg bg-indigo-700 hover:bg-indigo-800 shadow-lg focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400"
          >
            Get your Free Food
          </button>
        </div>
    </form>

```

Look closely. 
In the input i have used the `focus:outline-none` utility to remove the default outline of the browser, if you aren't going to replace it leave it there, is better then nothing. Then i have used the `focus:ring` utility to show an enfasis for everyone, since it's a input can bee usefull to show where we are goin to write.
But for the submit button i have used the `focus-visible` utility instead,allowing us to render the rings only four our keyboard users, the ones that actually needed in this case. Check for your self. There is no ring for the mouse submition. But if you hit the TAB you will see it. It's matching the default selector string that our pollyfill is expecting.