# nuxt-storyblok-query-example

This is the simplest example of creating and reading `posts` from storyblok cms
which I could come up with. 

## Preview
![demo](sbimage.gif)

## Quickstart

Copy `.env.example` to `.env` in your workspace and fill in your Storyblok API credentials.

Get your [Client API access Token](https://www.storyblok.com/docs/guide/getting-started#nuxtjs-example) (Read stories)
Get your [Management API OAuth Token](http://app.storyblok.com/#!/me/account) (Create stories)


#### Install
```
yarn
````

## Caveats

If you want to create posts in a folder named `posts` then you have
to add your `PARENT_STORY_ID` inside `index.vue`. Unfortunately I haven't
found a way to add a pretty name for the parent story/folder yet.

* pages/index.vue
```
const PARENT_STORY_ID = '<insert your parent id here>'
```

#### Run
````
yarn dev
````