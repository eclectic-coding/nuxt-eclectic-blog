---
title: Learn a New Language
date: 2020-07-05
published: true
tags: ['webdev', 'tutorial']
series: false
cover_image: images/learn-library.jpg
canonical_rul: false
description: When was the last time you had the desire to learn a new programming language? It can at first seem like a daunting task. However, it can be easier than you think. In this article we will look at a a commonality found in programming languages and allow us to think differently about undertaking a new learning path.
---
When was the last time you had the desire to learn a new programming language? It can at first seem like a daunting task. However, it can be easier than you think. In this article we will look at a a commonality found in programming languages and allow us to think differently about undertaking a new learning path.

## My Path
I have always loved learning. I first learning about programming logic using PASCAL in a college course over twenty years ago. After years as a hobbyist (read me about my [Journey](https://www.eclecticsaddlebag.com/passionate-journey/)), I start at [Flatiron School]() in August 2019. When I started I enrolled in the Part-time Online curriculum, because I wasn'y sure I could manage a full-time job and the curriculum. After the first project I switched to teh Full-time curriculum because I was so far ahead of my co-hort. Why? Well, as I explained to my Tech Lead at Flatiron: "*a while loop is a while loop*."

## Common Ground
So, even though languages are different, logic is not. The logic behind a while loop is thus:
```
while (condition is true) {
  do stuff
}
```

This is the logic. So, let's look at a few examples:

**PHP**
```
$x = 1; 
while($x <= 5) {
  echo "The number is: $x <br>";
  $x++;
}
```
In PHP, variable are prepended by `$`, so as long `x` is greater than or equal to 5, then the while loop will print the enclosed statement. For that matter, **Perl** is almost identical:
```
my $counter = 10;
while($counter > 0){

   print("$counter\n");
   $counter++;
}
```
Perl uses the same `$` pretending to variables, uses `print` instead of `echo` to print out the statement.

For examples:
**JavaScript**
```
var i = 1
while (i <= 5) {
  text += "The number is " + i;
  i++;
}
```

**Ruby**
```
i = 0
while i <= 5
  puts "The number is #{i}"
  i += 1
end
```


**Java**
```
int i=10;
while(i<=5){
    System.out.println("The number is["+i"]");
    i++;
}
```

The basic logic is the same, loop until a condition is met. The only differences are the language specific syntax. The next time you are interested in another language remember that the logical building blocks remain the same, it is the language specific rules and syntax which change. So, in my case, a "*while loop was a while loop*."

## Footnote
This has been fun. Leave a comment or send me a DM on [Twitter](http://twitter.com/EclecticCoding).

Shameless Plug: If you work at a great company and you are in the market for a Junior Developer with a varied skill set and life experiences, send me a message on [Twitter](http://twitter.com/EclecticCoding) and check out my [LinkedIn](http://www.linkedin.com/in/dev-chuck-smith).
