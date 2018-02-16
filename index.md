---
layout: default
commentIssueId: 1
---
{% include head.md %}


**intro**

So I needed to learn golang.. I tought I'll share my experiences.

The thing why I really like golang (so far) is that it makes standalone executable.
Of course this means that everything is staticly linked to the executable/binary and the size grows compared e.g C.. but compared to benefit of having executable standalone, I could not care less.

No different path settings/exports/registry entries, install programs etc.. just run or compile+run the binary ;)

have you ever seen python or ruby script to fail due imported library incombability ;)

And so far what I have tried, the same code compiles to windows and runs the same way.. standalone. (You can even copile it at linux for windows).

Of course I made some assosations in my mind with C/C++/python. I know golang tutorials let you know that you should not make such assumptions.. but you will anyway.

So here is what I learned this far :


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

Fixed type if initialized... fine.
Return value is at the end of function defination.. makes sence.

You need brackes if you have more than one return value (btw really nice feature).

# [](#header-1) go routines

I think them as threads, fork etc.
(I don't know how to add neat golang play area here yet, but if you run this first foo funtion exits last.)

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
For some really good reason there ++really is no++ official api/function to get the go routine id (thread id), so I just use some id for now. I did try to use uuid but those make log line too long.
Perhaps in future I'll use the 'context' stuff they included quite resently.
I also heard that golang has inbuilt thread/go routine/function engine.. so you should be able to run more than real threads.. never needed yet.

# [](#header-1) packages
coming soo..

```
The end so far.
```

[![HitCount](http://hits.dwyl.io/apmattil/apmattil.github.io.svg)](http://hits.dwyl.io/apmattil/apmattil.github.io)

{% include post_comments.html %}
