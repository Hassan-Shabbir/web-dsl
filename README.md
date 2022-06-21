# A dsl for concisely writing web applications

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
```

And this is how you would check if a (string or numeric) array is a palindrome using a hook (two functions in a row):
```
pal  =:  -: |.
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

## Concise
Most programming languages (except for React with JSX) force you to write something like this (example from Mithril docs):
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

When really all of that functionality can be written in only 2 lines of simple code.
```
/hello = main > h1.title{My first app} + b#a{`#a` clicks}
/ = a[href=/hello]{Enter!}
```

## Want to learn more?
Currently this project is a work in progress, though you can find my notes in the `notes` text file above.
