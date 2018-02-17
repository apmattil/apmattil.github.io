---
layout: default
commentIssueId: 1
---
{% include head.md %}


**intro**

So I needed to learn golang.. I tought I'll share my experiences.

Of course I made some assosations in my mind with C/C++/python. I know golang tutorials let you know that you should not make such assumptions.. but I could not help my self but to make them anyway.

So here is what I learned this far :

# [](#header-1) standalone binary/executable

The thing why I really like golang (so far) is that it makes standalone executable.

Of course this means that everything is staticly linked to the executable/binary and the size grows compared e.g C.. but compared to benefit of having executable standalone, I could not care less.

No different path settings/exports/registry entries, install programs etc.. just run or compile+run the binary ;)

have you ever seen python or ruby script to fail due imported library incombability ;)

And so far what I have tried, the same code compiles to windows and runs the same way.. standalone. (You can even copile it at linux for windows).


# [](#header-1) first look
e.g edited https://tour.golang.org/basics/4
```golang
package main
import "fmt"
func add(x int, y int) int {
	return x + y
}
func main() {
	a := "hello"
	b := 123456

	fmt.Println(add(42, 13))
}
```
nothing too complicated.. they(=golang developers) just switched variable first and type after that.

variable type fixed to the type of initialized... fine.
Return value is at the end of function defination.. makes sence.

You need brackes if you have more than one return value (btw really nice feature).

I think it is stated somewhere but I missed first that if function name starts with CAPITAL e.f Foo() it is public when in package.

# [](#header-1) funtions

Basic function is simple as above.
But then there is sometimes stuff just after the 'func' keyword, tutorials somehow seem to skip this .. I leared this by googling the very similar question.

But then function can be also 'class/struct' function.

That is the stuff in brackets after 'func' is like automaticly made class/struct instance.. and used like this-pointer in C++.

```golang
package main
import "fmt"

type zoo struct {
     i string
}

func (y zoo) Foo(x int) {
      fmt.Printf("something %d %s\n", x, y.i)
}

func main() {
     y := zoo{}
     y.i = "hello" 
     y.Foo(5)
}
```
output of this is : 
something 5 hello


# [](#header-1) go routines

I think them as threads, fork etc.

Here main() waits for childs to exit and first foo() finnishes last.

```golang
package main

import "fmt"
import "time"

func foo(id int) {
	sleepTime := 100*time.Millisecond
	sleepTime = sleepTime * time.Duration(id)

	fmt.Printf("%d: hello from go func, sleep %d\n", id, sleepTime)
	time.Sleep(sleepTime)
	fmt.Printf("%d: exit go func\n",id)
}

func main() {
	threadId := 3
	fmt.Printf("hello from main\n")
	go foo(threadId)
	threadId = 1
	go foo(threadId)
	time.Sleep(500*time.Millisecond)
}
```
For some really good reason there ++really is no++ official api/function to get the go routine id (thread id).
So I just use some id for now. I did try to use uuid but those make log line too long.

Perhaps in future I'll use the 'context' stuff they included quite resently.
I also heard that golang has inbuilt thread/go routine/function engine.. so you should be able to run more than real threads.. never needed that much yet.

# [](#header-1) struct

struct is like class in C++.

# [](#header-1) packages

package imports are like the one's in python, and acts like class in C++.
Each package file can even have init() function.. like class constructor in C++.

As I wrote earlier funtion names with starting capitals are public with small letter private to the file (or in C++ to the class).

This does not only apply to the functions but also to the e.g constans, structs etc.

Again nothing really compilated.

in file src/my_utils.go
```golang
import "fmt"

func Foo(x int) {
	fmt.Printf("foo %d", x)
}
```
so in my_prog.go I can call it.

```golang
package main
import my_utils

func main() {
	Foo(5)
}
```

# [](#header-2) imported packages

Public funtions you can overwrite, like virtual function in C++.
Now the real mess starts when you include/import some package that uses private structs and/or constants.

in file my_utils.go, I added private my_server

```golang
package my_utils
import "fmt"

type my_server struct {
   bar string
}

func Foo(x int, y my_server) {
	fmt.Printf("foo %d, %s\n", x, y)
}
```

At main I try to use it.

```golang
package main
import "my_utils"

func main() {
     var y my_utils.my_server
     y.bar = "not bar"
     my_utils.Foo(5, y)
}
```

Because my_server is private I can not use public funtion Foo().
I can not even access/set the bar variable if I change the my_server to public My_server !!
Same apply if the private is the this-pointer instance I mentioned at function paragraph.

In C++ you could inherit the whole class (my_utils), and then overwrite the Foo().

As far I know ,you can not do this in golang with out tricks ;(

## [](#header-3) some whining about this issue

"They are privat for a reason".. bollocks, my personal quess is that I would do it.. because as said in C++ you can just inherit and do what ever you want with members.

Yeah, I know this is what the golang interface stuff is for but you see this all the time.. and end up ripping others code.

A lot of code/project what I have seen are not writen to be extended or used anywhere else than specific case, even the projects intention was to create interface !

In general I have started to be little sceptical of projects started 2/3 years ago.. A lot of them seem to be playing with golang and just dumped to github.
I hope standard library has sceptical members what to put there or were are in the mess as some many other languages.

So I learned **my lesson**, I have to think what members really need be private or use interface in an github project. 
If I think it is any use to anybody other than just cut and paste code.




```
The end so far.
```

[![HitCount](http://hits.dwyl.io/apmattil/apmattil.github.io.svg)](http://hits.dwyl.io/apmattil/apmattil.github.io)

{% include post_comments.html %}
