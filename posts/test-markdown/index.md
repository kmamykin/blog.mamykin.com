---
permalink: /posts/test-markdown/
title: "Test blog post"
author: "Kliment Mamykin"
date: "2018-07-21"
image: test-markdown/sharegrid-474930-unsplash.jpg
tags: 
  - markdown
draft: true
---

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Heading 2

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Heading 3

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Heading 4

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Heading 2

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

* List 1
* List 1
    * List 2
    * List 2
        * List 3
        * List 3
* List 1
    1. List 2
    2. List 2

1. One
2. Two 
3. Three

## Math test

Some formula inline $E(a*X + b) = a*E(X) + b$

$$
\begin{equation}
    X = \sum_{i=1}^{N} x_i
\end{equation}
$$

Alpha: $\alpha A$, Beta: $\beta B$, Gamma: $\gamma \Gamma$  and some other symbols $\int \oint \sum \prod$ and in some sentences $n$ instances may be distributed into 
$m$ bins according to a total nonsense $x_i \sim N(\mu, \sigma^2)$.

In physics, the mass-energy equivalence is stated by the equation $E=mc^2$, discovered in 1905 by Albert Einstein.

This display math is not using `\begin{equation}` and just wraps display math in `$$...$$`
$$ 
E=mc^2
\tag{1}
\label{eq:einstein}
$$

This math uses `\begin{aligned}` (environment defined in KeTex)
$$ 
\begin{aligned}
    E=mc^2
    \tag{1a}
\end{aligned}
$$

This math uses `\begin{align}` (environment defined in LaTex but not in KeTex)
$$ 
\begin{align}
    E=mc^2
    \tag{1b}
\end{align}
$$

This math uses `\begin{equation}` (not defined in KeTex)
$$ 
\begin{equation}
    E=mc^2
    \tag{1c}
\end{equation}
$$

References to equation by label look like this `$\eqref{eq:einstein}$`: $\eqref{eq:einstein}$ and `$\ref{eq:einstein}$` $\ref{eq:einstein}$. 
Automatic numbering of equations is not available, the only option is to manually tag with `\tag{1}` command. 

Regular ([link to Einstein equation](#eq:einstein)) has to use `#eq:einstein` href. Regular link to tagged equations
[link to equation by tag](#1a)

This equation somehow give trouble parsing...

$$
\begin{equation}
    \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15} 
    \tag{2}
\end{equation}
$$

## Image test

![large image 1](test-markdown/ramiro-martinez-1317998-unsplash.jpg "Logo Title Text 1")

![large image 2](test-markdown/tom-grimbert-tomgrimbert-1349885-unsplash.jpg "Logo Title Text 2")

![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)


## Inline code snippets

Lorem ipsum `bash ls --all` dolor sit amet, consectetur adipiscing elit, 
sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. 
Ut enim ad minim veniam, quis nostrud `<div>Some html</div>` ullamco laboris nisi ut aliquip ex ea commodo consequat. 
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. 
Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Coding test

```python
# Import the modules
import sys
import random

ans = True

while ans:
    question = raw_input("Ask the magic 8 ball a question: (press enter to quit) ")
    
    answers = random.randint(1,8)
    
    if question == "":
        sys.exit()
    
    elif answers == 1:
        print "It is certain"
    
    elif answers == 2:
        print "Outlook good"
    
    elif answers == 3:
        print "You may rely on it"
    
    elif answers == 4:
        print "Ask again later"
    
    elif answers == 5:
        print "Concentrate and ask again"
    
    elif answers == 6:
        print "Reply hazy, try again"
    
    elif answers == 7:
        print "My reply is no"
    
    elif answers == 8:
        print "My sources say no"
```

## Javascript code test

```javascript
var now = new Date();

var days = new Array('Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday');

var months = new Array('January','February','March','April','May','June','July','August','September','October','November','December');

var date = ((now.getDate()<10) ? "0" : "")+ now.getDate();

function fourdigits(number)	{
	return (number < 1000) ? number + 1900 : number;
								}
today =  days[now.getDay()] + ", " +
         months[now.getMonth()] + " " +
         date + ", " +
         (fourdigits(now.getYear())) ;

document.write(today);
```

## JSX code snippets

```jsx
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```


# Live demo

Changes are automatically rendered as you type.

* Implements [GitHub Flavored Markdown](https://github.github.com/gfm/)
* Renders actual, "native" React DOM elements
* Allows you to escape or skip HTML (try toggling the checkboxes above)
* If you escape or skip the HTML, no `dangerouslySetInnerHTML` is used! Yay!

## HTML block below

<blockquote>
  This blockquote will change based on the HTML settings above.
</blockquote>

## How about some code?
```js
var React = require('react');
var Markdown = require('react-markdown');

React.render(
  <Markdown source="# Your markdown here" />,
  document.getElementById('content')
);
```

Pretty neat, eh?

---------------

## Tables?

| Feature   | Support |
| --------- | ------- |
| tables    | ✔ |
| alignment | ✔ |
| wewt      | ✔ |

## More info?

Read usage information and more on [GitHub](//github.com/rexxars/react-markdown)

