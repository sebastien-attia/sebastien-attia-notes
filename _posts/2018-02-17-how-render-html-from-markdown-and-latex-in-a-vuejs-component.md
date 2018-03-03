---
layout: post
cover: 'assets/images/cover1.jpg'
navigation: True
title: How render HTML from Markdown and LateX in a VueJS component ?
date: 2017-06-28 22:14:00
tags: programming web
subclass: 'post tag-programming tag-web'
logo: 'assets/images/innovea-tech-white.svg'
author: sebastien
categories: sebastien
---

### How to create a VueJS component to render HTML from a source mixing Markdown and LateX ?

The MarkdownMathJax's VueJS component transforms a Markdown and LateX string into HTML.  

It uses markdown-it and MathJax.  

The following piece of code supposes that:  

* it runs in a nuxt project with the following nuxt.config.js config:  

``` js
modules: [
  '@nuxtjs/markdownit'
 ],
// [optional] markdownit options
markdownit: {
  injected: true,
  preset: 'default',
  linkify: true,
  html: true,
  breaks: false,
  use: [
    // 'markdown-it-mathjax',
    'markdown-it-container',
    'markdown-it-attrs'
  ]
},
```

* it uses MathJax to render LateX in HTML.

--- MarkdownMathJax.vue ---
``` javascript
<template>
  <span ref="mathJaxEl" v-html="readMD()" class="e-mathjax"></span>
</template>

<script>
export default {
  props: [ 'mdData' ],

  methods: {
    readMD () {
      return this.$md.render(this.mdData) 
    },

    renderMathJax () {
      if(window.MathJax) {
        window.MathJax.Hub.Config({
          tex2jax: {
            inlineMath: [ ['$','$'], ["\(","\)"] ],
            displayMath: [ ['$$','$$'], ["\[","\]"] ],
            processEscapes: true,
            processEnvironments: true
          },
          // Center justify equations in code and markdown cells. Elsewhere
          // we use CSS to left justify single line equations in code cells.
          displayAlign: 'center',
          "HTML-CSS": {
            styles: {'.MathJax_Display': {"margin": 0}},
            linebreaks: { automatic: true }
          }
        });
        window.MathJax.Hub.Queue(["Typeset", window.MathJax.Hub,this.$refs.mathJaxEl]);
      }
    }
  },

  head: {
    script: [
      { src: 'https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS_HTML' }
    ]
  },

  mounted () {
    this.renderMathJax()
  }
}
</script>
```
--- End of MarkdownMathJax.vue ---
