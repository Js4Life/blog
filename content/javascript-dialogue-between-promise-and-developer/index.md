---
title: "JavaScript — dialogue between promise and developer"
description: "Developer:\nI am going to call to asynchronous API, and I need a way to let my consumers know when the data is available. Promise:\nOk, I will give you two functions. One is the resolve function and…"
date: "2016-12-20T12:03:47.701Z"
categories: 
  - JavaScript
  - Promises
  - Web Development

published: true
canonical_link: https://netbasal.com/javascript-dialogue-between-promise-and-developer-40dfa99ecdf1
redirect_from:
  - /javascript-dialogue-between-promise-and-developer-40dfa99ecdf1
---

![](./asset-1.jpeg)

**Developer:**  
I am going to call to asynchronous API, and I need a way to let my consumers know when the data is available.

**Promise:  
**Ok, I will give you two functions. One is the resolve function and the second is the reject function.

When your data is available, and it’s valid, you need to call the resolve function with the data.  
When your data is available, and it’s not valid, you need to call the reject function with the data (or error).

**Developer:**  
Sounds cool, you can show me how to do this in JavaScript?

**Promise:**

<Embed src="https://gist.github.com/NetanelBasal/b1f121c31903beb13473fce37d829ff1.js" aspectRatio={0.357} caption="" />

**Developer:**  
Got it! But my consumers also need a way to do something based on the results, can you help me with that?

**Promise:**  
Of course, I also have two methods to help you with that.  
The _then_ method — they can register a function when they want to react to valid data.  
The _catch_ method — they can register a function when they want to react to invalid data.

**Developer:**   
Sounds cool, you can show me how to do this in JavaScript?

**Promise:**

<Embed src="https://gist.github.com/NetanelBasal/6035e00caa2039047ff43b6c1662cf25.js" aspectRatio={0.357} caption="" />

**Developer:**  
Now I understand promises!

---

### The promise metaphor:

> Imagine that you are going to a job interview. After the interview, you don’t get an answer immediately.

> In the meantime, you are thinking to yourself what you will do if the CEO resolve your request or reject it.  
> At some point later, the hot secretary of the CEO is calling you and tell you the answer. The answer could be that the CEO resolved your request or reject it.

This is how you write this with a promise:

<Embed src="https://gist.github.com/NetanelBasal/b9e423e49840245ac34cad8bcbe42a27.js" aspectRatio={0.357} caption="" />

That’s all!

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
