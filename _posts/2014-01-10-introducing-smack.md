---
layout: post
title: "Introducing Smack - really simple templating for JQuery"
description: "A tiny templating plugin for JQuery with syntax inspired by Knockout"
category: 
tags: []
---

I finally got around to posting my tiny roughly 700 byte minified templating engine to github.

It supports, automatic html encoding, attribute binding, iteration, conditionals and string interpolation, with a surprisingly powerful Knockout inspired syntax such as this.

```html
<a data-bind="href: blogUrl">Home</a> 
<h1 data-bind="title"></h1>
<p>
  Published <abbr data-bind="friendlyDate,title:date"></abbr>
  <span data-bind="in {category}, "></span>
  <span data-bind="!'by <strong>{author}</strong>.'"></span>.
</p>
<ul>
  <li data-bind="#tags"><a data-bind=".,href:'?tag={.}'"></a></li>
</ul>
<p data-bind="?admin"><a href="#">Edit</a></p>
<div data-bind="!content"></div>
<h3 data-bind="'Comments ({comments.length})'"></h3>
<ul>
  <li data-bind="#comments">
    <strong data-bind="author"></strong>
    <p data-bind="text"></p>
    <p data-bind="?likes.length">Liked by <em data-bind="#likes,'{.} '"></em></p>
  </li>
</ul>
```

Using it is as simple as this

```javascript
var template = $("#template").html()
var context = {
  admin: false,
  blogUrl: "http://example.com",
  author: "Bob Bobson",
  category: "javascript",
  title: "Hello world", 
  date: "2014-01-01",
  friendlyDate: "Yesterday",
  tags: ["tiny","template","jquery"], 
  content: "<p>This is a simple <strong>smack</strong> template.</p>",
  comments: [
    {author: "bob", text: "Great stuff!", likes: ["Cecil", "Mallory"]},
    {author: "alice", text: "Indeed!", likes: []}
  ]
}

// Apply template
var element = $(template)
  .smack(context)
  .appendTo(document.body);
```

Check it out at [github.com/kaa/smack](https://github.com/kaa/smack) if you're interested.