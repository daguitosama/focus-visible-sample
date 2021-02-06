---
title: Making focus-visible happen where do you live
published: false
description: A quick guide to seting up focus-visible on your nuxt/tailwind projects
tags: Nuxt,Tailwindcss
cover_image: https://images.unsplash.com/photo-1578406033714-319ffaf023eb
---

In case you don't know  the `focus-visible` css selector allow us to target
DOM elements that have been focused by a keyboard interaction, instead the tradiccional `focus` that target every focused element.

Why we  need that distintion in the firs place ?
It all start with  accessibility (a11y) in  mind. If a user is browsing with a mouse
or a phone, they know exactly what they are clicking. They don't need any clue about it, it might become even in a distraction. 
But is a very diferent story for the keyboard user. Think for a moment, a father carring their baby, people with injuries in a hand, people with a temporary or cronical healt illness that inables them to use a mouse... there is a lot of [them](https://www.interactiveaccessibility.com/accessibility-statistics) out there  and you should be including them in your target audience. It's the very right thing to do, **no excuses**.

If you haven't tried, go out there with your TAB key and browse around. You will need a visual feedback that shows you where you are, step by step. Some folks are very good at this, like twiter, but reallity is that offen, we don't even plan a keyboard only navigation in the first place.

So quick recap: we need a way to tell our keyboard people what elements they have focused in our app. Here is where our best buddy `focus-visible` came to play. Even well supported in modern browsers it's kinda new in the CSS playground. So we are going to set things up in the more compatible way.

OK OK let's do some pair programing.