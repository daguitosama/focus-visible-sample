---
title: 
published: true
description: A quick guide to setting up focus-visible on your nuxt/tailwind projects.
tags: Nuxt,Tailwindcss, a11y
cover_image: https://images.unsplash.com/photo-1578406033714-319ffaf023eb
---
# [Making focus-visible happen where do you live](https://dev.to/d4g0/making-focus-visible-happen-where-do-you-live-pho)
### **A quick guide to setting up focus-visible on your nuxt/tailwind projects.**

![Color Lights](https://images.unsplash.com/photo-1578406033714-319ffaf023eb)

In case you don't know  the `focus-visible` css selector allows us to target DOM elements that have been focused by a keyboard interaction, instead the traditional `focus` that targets every focused element.
 
## Why do we  need that distinction in the first place ?
It all starts with  accessibility (a11y) in  mind. If a user is browsing with a mouse or a phone, they know exactly what they are clicking. They don't need any clue about it, it might even become a distraction.
 
But it is a very different story for the keyboard user. Think for a moment, a father carrying their baby, people with injuries in a hand, people with a temporary or chronic health illness that disable them to use a mouse... there are a lot of [them](https://www.interactiveaccessibility.com/accessibility-statistics) out there  and you should be including them in your target audience. As Steve Krug says, it's the very right thing to do! **no excuses**.
 
If you haven't tried, go out there with your TAB key and browse around. You will need a visual feedback that shows you where you are, step by step. Some folks are very good at this, like twitter, but reality is that sometimes we don't even plan a keyboard only navigation in the first place.
 
So quick recap: we need a way to tell our keyboard people what elements they have focused in our app. Here is where our best buddy `focus-visible` selector came to play. Even well supported in modern browsers it's kinda new in the CSS playground. So we are going to set things up in the more compatible way.
 
## OK  let's make this happen !
 
You can find this [repo](https://github.com/d4g0/focus-visible-sample) at GitHub, feel free to clone it.
Also you can check the live version [here](https://ecstatic-euler-92587b.netlify.app/) .
 
I'm going to use [Nuxt.js](https://nuxtjs.org/) and [Tailwind](https://tailwindcss.com/) because those are my favorite tools, but you can apply what you will learn here to every project. The real hero behind the scenes here is the `focus-visible` [Polyfill](https://github.com/WICG/focus-visible).
 
So create a new `nuxt-app` ( do not select tailwind yet):
```bash
npx create-nuxt-app focus-visible-sample
```
 
Install Tailwind  on your own:
```bash
cd focus-visible-sample
yarn add -D @nuxtjs/tailwindcss tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
```
 
And install the polyfill and a postcss plugin to make it even easier to work with it:
```bash
yarn add focus-visible postcss-focus-visible
```
 
Make sure to bundle the actual polyfill in your page.
Nuxt  serves the static assets from the `static` folder off your project, so i will clone the polyfill file there in a `scripts` folder. You can find him at `node_modules/focus-visible/dist/focus-visible.min.js`. I also clone the `.map` version to avoid unuseful warnings in the browser developer console.
Do what you have to,  i will use nuxt config file since he allows you declare metadata there super easy ( i 💚️ that framework) .
 
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


 
I will add the postcss plugin because I want to use it in conjunction with my tailwind utilities, but you will be just fine using the advice in the polyfill [README](https://github.com/WICG/focus-visible).
 
I will add that plugin to my [nuxt postcss config](https://nuxtjs.org/docs/2.x/features/configuration#postcss-plugins):
 
```js
/*  nuxt.config.js  */
export default {
   // ...
   postcss:{
       plugins:{
           'postcss-focus-visible': {}
       }
   }
}
```
 We need to set up Tailwind too of course:

```js
/*  nuxt.config.js  */
buildModules: [
    // https://go.nuxtjs.dev/tailwindcss
    '@nuxtjs/tailwindcss',
  ],
```
 
 
Time to set up our Tailwind config:
```bash
npx tailwind init
```
 
 
Since the 'focus-visible' variant is by default [off](https://tailwindcss.com/docs/hover-focus-and-other-states) we should manually get it on:
( I will be targeting the `ring` utility since that's the exact effect we want in our Ui)
 
```js
/* tailwind.config.js  v2.x */
 
module.exports = {
   //...
   variants: {
       extend: {
           ringColor: ['focus-visible'],
       },
 },
}
 
```
 
## All setted up, let's do some pair programming !
 
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

 
![The App Screen Shot](https://dev-to-uploads.s3.amazonaws.com/i/q4mmh167jkot2ztcw86v.png)

Look closely.
In the input i have used the `focus:outline-none` utility to remove the default outline of the browser, if you aren't going to replace it, leave it there, it is better than nothing. Then I have used the `focus:ring` utility to show an enfasis for everyone, since it's a text input can be useful to show where we are going to write.
But for the submit button i have used the `focus-visible` utility instead, allowing us to render the rings only four our keyboard users, the ones that actually needed in this case. Check for yourself. There is no ring for the mouse submission. But if you hit the TAB you will see it. It's matching the default selector string that our `focus-visible` polyfill is expecting.
 
 
If you want to take a deep dive on accessibility i recommend chek this [awesome](https://webaim.org/) resource. It’s a little extra work i will not lie to you. But I will make a tremendous impact on those users that need it; and often will improve the experience for everyone. Like those caption ready videos deliver the media for people can’t hear, but also for those that can’t understand that language. And if you want to go for a quality workshop about it, check [“Accessibility in JavaScript Applications”](https://frontendmasters.com/courses/javascript-accessibility/) by Marcy Sutton. That was the place where I started to learn this stuff in the first place. Thanks Marcy for introducing me to the a11y world. 

That’s it my friend, go out there and craft good stuff. Your users will thank  your hard work (even if they never sayit). If you have any doubts or want to say something, send me a tweet at [@dagocarralero](https://twitter.com/dagocarralero)
 
 
## Useful links
[Nuxt docs](https://nuxtjs.org/)
[Tailwind docs](https://tailwindcss.com/)
[Focus Visible Repo](https://github.com/csstools/postcss-focus-visible)
[WebAim](https://webaim.org/)
 

