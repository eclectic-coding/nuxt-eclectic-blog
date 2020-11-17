---
title: Express Bodyparser
date: 2020-07-19
published: true
tags: ['node', 'webdev', 'javascript']
series: false
cover_image: images/technology.jpg
canonical_rul: false
description: I am learning how much I love back-end architectures. This week I decided to throw away the tutorials and code my first Node Express server. After reviewing a lot of code samples I started to notice something around one middleware package. The way that Body-parser is including in a project can be subjective.
---
I am learning how much I love back-end architectures. This week I decided to throw away the tutorials and code my first Node Express server. After reviewing a lot of code samples I started to notice something around one middleware package. The way that Body-parser is including in a project can be subjective.

## Body Parser
What is Body-parser? It is a middleware which parses the `request` stream. In most cases, not always, we are parsing the stream as `json` in the `req.body` request object.

**How do we use it?** It is so easy, *but* at the same time, subjective to the programmer. We are going to look at five ways to use `body-parser`, all four methods are equally valid as acceptable methods to parse data, and all our methods are subjective. in that it becomes a preference to the programmers methodology.

### Most widely accepted method
The first method is to import the `body-parser` package into to your server file and set the middleware like so:

```javascript
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json())
```
**But do we need the Body-parser package?**

I ask this question because of a little Express history. The Body-parser package is a middleware that was part of Express but in early 2014 it was removed from the Express package. This is why most project source code you review, include `body-parser` as a separate package in the project `package.json`.

However, as early as September 28, 2017, `Body-parser` was added back into Express as a [dependency](https://github.com/expressjs/express/blob/4486fa6324eaeb8e53482df961c755cc9013a36e/lib/express.js#L15). Therefore, we do not technically need to add it as a project dependency.

So, the second method I propose is a way to clean up our code a little:
```javascript
const express = require('express');

const app = express();
app.use(express.json())
```
We have removed the package and are using Express, which uses Body-parser. It is a little cleaner. If I am coding an Express server in native code this is my preference.

### Babel and ES6
What about using ES6 and Babel in your project. I am all for it. It is sometimes hard to retool the brain away from ES6. If your project is setup with Babel (beyond the scope of this article), then how does Body-parser imports look?

Let's look at our first example. It is basically the same except for the import statements:
```javascript
import express from 'express';
import bodyParser from 'body-parser';

const app = express();
app.use(bodyParser.json())
```
But we can abstract this example a little more, using named exports. This really cleans up the call to `json()`:
```javascript
import express from 'express';
import { json } from 'body-parser';

const app = express();
app.use(json())
```
Remember our second example? We do not technically need the Body-parser import.
```javascript
import express from 'express';

const app = express();
app.use(express.json())
```
Or we can really clean it up with named exports. If I am using ES6, this is my preference:
```javascript
import express, { json } from 'express';

const app = express();
app.use(json())
```
Remember each method works the same way. It is just a different way to present your code. Which method do you use?

## Footnote
This has been fun. Leave a comment or send me a DM on [Twitter](http://twitter.com/EclecticCoding).

Shameless Plug: If you work at a great company and you are in the market for a Software Developer with a varied skill set and life experiences, send me a message on [Twitter](http://twitter.com/EclecticCoding) and check out my [LinkedIn](http://www.linkedin.com/in/dev-chuck-smith).





