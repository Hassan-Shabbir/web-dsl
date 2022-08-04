# A dsl for concisely writing web applications
On average, my language is able to reduce the size of the code by around 78.5%! See the [conciseness examples section](https://github.com/Hassan-Shabbir/web-dsl#conciseness-examples) for more information.

## HTML dsl
Uses a form of Emmet for generating html templates. (Note that most spaces are not required, only function application and array literals need spaces between values.)
```
.foo#bar > ul > li`cut 'hello world'` + p{hi}

<div id="bar" class="foo">
	<ul>
		<li>hello</li>
		<li>world</li>
	</ul>
	<p>hi</p>
</div>
```

### HTML components:
Use `[]` for html component values, based on the syntax for html attributes.
```
foo[data] =: .foo#bar > ul > li`cut data` + p{hi}
/ =: foo['hello world']
```

### Links:
```
/foo = a[href=/bar]{Click to go to bar}
/bar = p{This is bar} 
```

## CSS dsl
CSS is specified using a Tailwindcss-like syntax after the `@`, although no css-reset is used on the pages.
Yellow text on blue background.
```
p@cyellow@bblue{Hello world}
```

### Grids
Create a 2 by 2 grid. 
```
div@g2x2 > p{1} + p{2} + p{3} + p{4}
```

## JS dsl
Programs are written in J (a decendant of the APL programming language). The defining features of J are that it is an array oriented functional programming language with a high
level of abstractness and conciseness.

For example, here is how you would write the sum function:
```
sum  =:  +/
```

### Array oriented
All operations are defined to work on all array values, without having to specify for loops. 
Here `1 2 3` is an array, to which a `1+` function is being mapped.
```
1 + 1 2 3
>>> 2 3 4
```
And values can be combined using the reduce (`/`) suffix (higher order function) on functions such as plus (giving sum) and times (giving product):
```
+/ 1 2 3
>>> 6
1 + 2 + 3
>>> 6

*/ 0 1 0
>>> 0
0 * 1 * 0
>>> 0
```

Types are rank polymorphic so `[a] -> a ==> [[a]] -> [a]`.

### Reactivity
Defining an id allows it to be accessible through the programmable parts of the code, in the form of an event stream.
This code counts the number of times a button has been clicked.
```
button#b{Click me!}+p{You clicked the button `#b` times!}
```

```
ta#i + p{`'!' ,~ iV`}
```

### Trains
J allows a user to quickly write programs using trains which are two or three functions in a row (or more) separated by spaces (and usually surrounded by parenthesis).

For example, this is how you would write the average function using a fork (three functions in a row):
```
avg  =:  +/ % #

sum  =:  +/
divide  =:  %
length  =:  #

averageTacit  =:  sum divide length
averageExplicit  =:  {{ (sum y) divide (length y) }}
```

And this is how you would check if a (string or numeric) array is a palindrome using a hook (two functions in a row):
```
pal  =:  -: |.

match  =:  -: 
reverse  =:  |.

palindromeTacit  =:  match reverse
palindromeExplicit  =:  {{ y match (reverse y) }}
```

## SQL dsl
Create a table (header name & header type (`:x`)):
```
person = name:s color:s email:e website:u, 'hassan' 'blue' 'hassan@gmail.com' 'www.c3g.nfshost.com'
```

Convert data from relational values to JSON data automatically.
```
/person = person
```

Updatable views and simple relational joins. The following creates a student relation (sid sname), a teacher relation (tid tname) and a studentTeacher view.
```
studentTeacher = sid sname * tid tname
```

## REST
Assign values to variables, so that GETs to that url resolve to the value.
```
/ =: p{hello world}
```

Route variables:
```
/foo/$i = `41 + i`
```

## Conciseness (Examples)
Here is a table that shows how concise my language is against other popular languages used for web development.

| Example # | Percent | Characters (My lang) | Characters (Other lang) | Other lang |
| --------- | ------- | -------------------- | ----------------------- | ---------- |
| 1 | 19% | 50  | 263 | HTML    |
| 2 | 29% | 88  | 303 | HTML    |
| 3 | 15% | 22  | 143 | Svelte  |
| 4 | 22% | 59  | 258 | Svelte  |
| 5 | 30% | 91  | 295 | Reflex  |
| 6 | 11% | 50  | 429 | Reflex  |
| 7 | 31% | 63  | 200 | Flask   |
| 8 | 15% | 81  | 508 | Mithril |

AVERAGE: my language examples are 21.5% the size (in characters) of other language examples, meaning a reduction by 78.5%!

### Example 1
```
.c@g2x3>p{hey}+p{this}+p{is}+p{a}+p{test}+p{grid}
```
```
<div class="c">
	<p>hey</p>
	<p>this</p>
	<p>is</p>
	<p>a</p>
	<p>test</p>
	<p>grid</p>
</div>
<style>
	.c {
		display: grid;
		grid-template-rows:    repeat(2, minmax(min-content, 1fr));
		grid-template-columns: repeat(3, minmax(min-content, 1fr));
	}
</style>
```

### Example 2
```
(nav>.logo+.search+.button)+(.body>.page1+.page2+.page3)+(footer>.first+.second+.third)
```
```
<nav>
	<div class="logo"></div>
	<div class="search"></div>
	<div class="button"></div>
</nav>
<div class="body">
	<div class="page1"></div>
	<div class="page2"></div>
	<div class="page3"></div>
</div>
<footer>
	<div class="first"></div>
	<div class="second"></div>
	<div class="third"></div>
</footer>
```

### Example 3
```
b{Clicked `#b` times}
```
```
<script>
	let count = 0;
	function handleClick() {
		count += 1;
	}
</script>
<button on:click={handleClick}>
	Clicked {count} times
</button>
```

### Example 4
```
b{Count: `#b`}+p{`#b` * 2 = `2*#b`}+p{`2*#b` * 2 = `4*#b`}
```
```
<script>
	let count = 1;
	$: doubled = count * 2;
	$: quadrupled = doubled * 2;
	function handleClick() {
		count += 1;
	}
</script>
<button on:click={handleClick}>
	Count: {count}
</button>
<p>{count} * 2 = {doubled}</p>
<p>{doubled} * 2 = {quadrupled}</p>
```

### Example 5
```
h1{Welcome to Reflex-Dom}+div>p{Reflex-Dom is:}+ul>li{Fun}+li{Not difficult}+li{Efficient}
```
```
{-# LANGUAGE OverloadedStrings #-}
import Reflex.Dom
main :: IO()
main = mainWidget $ do
  el "h1" $ text "Welcome to Reflex-Dom"
  el "div" $ do
    el "p" $ text "Reflex-Dom is:"
    el "ul" $ do
      el "li" $ text "Fun"
      el "li" $ text "Not dificult"
      el "li" $ text "Efficient"
```

### Example 6
```
b#a{Increment}+b#b{Decrement}+p{Total: `(#a)-#b`}
```
```
{-# LANGUAGE RecursiveDo #-}
{-# LANGUAGE OverloadedStrings #-}
import Reflex.Dom
main :: IO ()
main = mainWidget bodyElement 
bodyElement :: MonadWidget t m => m ()
bodyElement = do
  rec el "h2" $ text "Combining Events with leftmost"
      counts <- foldDyn (+) (0 :: Int) $ leftmost [1 <$ evIncr, -1 <$ evDecr]
      el "div" $ display counts
      evIncr <- button "Increment"
      evDecr <- button "Decrement"
  return ()
```

### Example 7
```
/foo = a[href=/bar]{Click to go to bar}
/bar = p{This is bar} 
```
```
from flask import Flask
app = Flask(__name__)
@app.route("/foo")
def foo():
    return f"<a href={url_for('bar')}>Click to go to bar</a>"
@app.route("/bar")
def bar():
    return "<p>This is bar</p>"
```

### Example 8
```
/hello = main>h1.title{My first app}+b#a{`#a` clicks}
/ = a[href=/hello]{Enter!}
```
```
var root = document.body
var count = 0
var Hello = {
    view: function() {
        return m("main", [
            m("h1", {
                class: "title"
            }, "My first app"),
            m("button", {
                onclick: function() {count++}
            }, count + " clicks"),
        ])
    }
}
var Splash = {
    view: function() {
        return m("a", {
            href: "#!/hello"
        }, "Enter!")
    }
}
m.route(root, "/splash", {
    "/splash": Splash,
    "/hello": Hello,
})
```

## Want to learn more?
Currently this project is a work in progress, though you can find my notes in the `notes` text file above.
