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
HTML components:
```
foo[data] =: .foo#bar > ul > li`cut data` + p{hi}
/ =: foo['hello world']
```
Links:
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
## JS dsl

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

## Array oriented
All operations are defined to work on all array values, without having to specify for loops. 
Here `1 2 3` is an array.
```
1 + 1 2 3
>>> 2 3 4
```
Types are rank polymorphic.

## Reactivity
Defining an id allows it to be accessible through the programmable parts of the code, in the form of an event stream.
This code counts the number of times a button has been clicked.
```
button#b{Click me!}+p{You clicked the button `#b` times!}
```

```
ta#i + p{`'!' ,~ iV`}
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
