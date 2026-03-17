---
title: "Pointers in Golang"
date: 2026-03-16
draft: false
tags: ["go", "concurrency", "backend"]
categories: ["golang"]
---

![Golang](/images/go.webp)

Hey Gopher, how’s it going?

Lately, I’ve been dedicating my free time to studying Golang, and I must admit it’s been an interesting and challenging journey.

During my learning process, I initially had some “resistance” in understanding what pointers actually are. Coincidentally, while following some blogs and pages about Go, I noticed that many people face the same obstacle — and that’s completely normal.

That’s why I decided to create this space to write a bit about them.

## So, what is a pointer?

A pointer is simply a variable whose content is a memory address. If you share this pointer elsewhere, we can directly access the address where the data is stored, and from there, manipulate that memory space in some way.

### Here’s a brief summary of how memory works:

![Addresses](/images/pointers-addresses.webp)

In short, memory is made up of addresses that store data, referred to as “values” in the image above. Each variable created in the code receives a memory address, and its corresponding value is stored at that address.

```go
package main

func main() {
  a := 15
  b := "foo"
}
```
> In the code snippet above, two variables were declared: one named a with the value 15, and another named b with the value "foo".

![Addresses](/images/pointers-addresses-2.webp)
> The image above shows the memory address assigned to each one and their respective values.

## How to declare a pointer?
A pointer is declared using `*` followed by the data type you want to declare, such as **int**, **string**, etc.

```go
package main

func main() {
  var p *int

  /* Here, the pointer is declared, 
  but no memory address is assigned to it. */
}
```

## How to assign a memory address to a pointer?
You assign the address of a given variable to the pointer using & before the variable (e.g., **&a**).

```go
package main

import "fmt"

func main() {
  var p *int

  a := 15 // declaring a variable of type int
  p = &a // getting the address of the variable (a) 
        //and assigning it to the pointer (*)

  fmt.Println(p) // OUTPUT: memory address assigned, example:
                // 0x140000140a5
}
```

## How to know the value stored at the memory address?
To get the value stored at the memory address assigned to the pointer, simply use * before the pointer (e.g., **p**).

```go
package main

import "fmt"

func main() {
  var p *int

  a := 15
  p = &a

  fmt.Println(*p) // OUTPUT: 15 -> Value stored at the address.
}
```

## When to use pointers?
It’s well known that pointers help when it comes to performance. But when should you use them?

### Mutability
The only way to modify a variable passed to a function is by passing a `pointer`. By default, passing by value means that any changes are made to a **copy** you’re working with. As a result, they are not reflected in the calling function. **Here’s an example**:

```go
package main

import "fmt"

type dog struct {
  name string
}

func main() {
  d := dog{"Fifo"}
  changeDogName(d)
  fmt.Println(d)
}

func changeDogName(d dog) {
  d.name = "Mike"
}
```
> The output of **fmt.Println** will be “**Fifo**” and not “**Mike**” because the dog’s name is being changed in a “copy” of the variable. To fix this, just use a pointer.


```go
package main

import "fmt"

type dog struct {
  name string
}

func main() {
  d := dog{"Fifo"}
  changeDogName(&d)
  fmt.Println(d)
}

func changeDogName(d *dog) {
  d.name = "Mike"
}
```
> Now the output will be “**Mike**" 🙂

There are other cases where using pointers is recommended, but in my opinion, mutability is the main one. You can find others in a very good article that I used as a reference for my studies.

## Anyway, that’s it…
Just a reminder: I came up with the idea of writing this brief summary about pointers mainly for learning purposes and maybe to help someone who has questions about the topic.

I used this excellent [article](https://medium.com/@meeusdylan/when-to-use-pointers-in-go-44c15fe04eac) by Dylan Meeus as a reference — I highly recommend checking it out. 😁
