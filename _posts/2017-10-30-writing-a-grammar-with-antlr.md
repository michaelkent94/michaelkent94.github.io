---
layout: post
title: Writing a Grammar with ANTLR
tags: [willow, antlr]
comments: true
---

Originally, my plan for [Willow](http://github.com/michaelkent94/willow) was to write my own lexer and parser.
I wanted the experience, custom control over everything, speed, etc. I still want those things.
But I realized that there's something *far* more important at this point in the development of Willow. That's the
ability for rapid prototyping of the grammar.

I started out writing a lexer, which was very straight forward. No issues. Then, when I began the parser,
I realized that it was going to be slow and tedious to make small (what I wanted to be trivial) changes in the grammar.
I still haven't completely settled on how to write various constructs in Willow and I need to have the ability to easily
implement those changes in the parser. So I scrapped what I wrote and...
<div id="excerpt-end"></div>
### Enter ANTLR

[ANTLR](http://www.antlr.org) (ANother Tool for Language Recognition) is a lexer and parser generator. Unlike the
well known flex/lex and bison/yacc, it's actually enjoyable to use. ANTLR can target several different languages, include Swift,
which was the important part for me since I'm implementing Willow's compiler in Swift.

I can write a grammar in something similar to EBNF notation and ANTLR kindly generates a lexer, parser, and a few nice
interfaces to help me walk the autogenerated parse tree. In addition to all of that, it includes a tool, called `grun`,
which lets you visualize the resulting parse tree from an input file. Now, there are a few options you can provide to `grun`
but my favorite is `-gui`. This option turns a program like this:

{% highlight willow %}
// FizzBuzz
{:fizzBuzz num::Int -> String
    {? num % 15 == 0 ?
        -> "FizzBuzz"
     ? num % 3 == 0 ?
        -> "Fizz"
     ? num % 5 == 0 ?
        -> "Buzz"
    }

    -> "\(num)"
}

// Print from 1 to 100
{@ :i <- 1...100
    {print {fizzBuzz num: i}}
}
{% endhighlight %}

into a parse tree that looks like this:
![Willow parse tree]({{ site.url }}/public/img/willow_parse_tree.png)
(you may want to open that in a new tab or something to get a better look).

It also provides a helpful UI to navigate the tree, focus on subtrees, zoom in and out, etc.

### Next Steps

From here, I'll integrate my ANTLR parser into the rest of the willow compiler. ANTLR provides a couple interfaces (Listeners, and Visitors)
to walk the resulting parse tree. I'll do some analysis work to find declarations of classes, structs, functions, and operators. Then I'll go
crazy with type checking/inference and structuring what are currently lists of binary operators to apply their precedence. I'll give context
to function calls. After all of that, I'll need to implement some of the builtin types and operators and then I should be good to go for some
code generation.

As always, make sure to keep checking back here for updates regarding Willow and my other projects!
