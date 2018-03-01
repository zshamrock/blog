{:title "Go Lang Data Types Cheatsheet"
:layout :post
:tags ["go" "development"]
}

I see the value of learning and practicing new programming languages. Although the value is when the new language introduces the different paradigm than the languages you already know, and change/challenge they way you think or program. Also the important aspect is the market share for the language, i.e. whether there is the demand on the market for such skills.

In that regards the two languages I focus on apart from the main language (which is Java) which I use for day to day development these are: Go and Clojure.

I think you will agree if you are familiar with these languages that they bring new paradigms to the area of the programming languages. Well, yes both of them have inspiration of the languages existed before, but if you are young enough (as I am) to not catch them, the ideas in these language then feel new. Also these languages have a few interesting ideas by itself, which other languages didn't have.

I use Clojure these days mostly for solving puzzles on the <a href="https://www.codingame.com/profile/ae433b2b7200a27dc94b3abe13ee3b2c803079" target="_blank">CodinGame</a>.
And Go for writing various CLI utilities for the projects I am working on. Many of those utilities are very much project specific, so not possible to open source them or share. But some of the ideas are, and currently I implement some of them in the <a href="https://github.com/zshamrock/vmx" target="_blank">vmx</a> tool (which I will write its own post about).

On the radar languages which also look interesting and have some demand on the market are Rust and Racket. I am planning to have some look into Racket this year. No plans so far regards Rust.

And yes, because my main work is still with the Java language, I keep my skills up to date in this area, while I think it is equally important to look around, and extend your professional competencies, and become familiar with new ideas and techniques which even if taken from the different languages, could improve writing the code in Java.

So here below is the summary of the Go data types I came with. I hope it is useful.

### Go Data Types Cheatsheet

Also available as <a href="https://docs.google.com/spreadsheets/d/1AqSlVvLtyLc62BIe4hc0X-NblMFfFrg9bvlGzSxCy5s/preview" target="_blank"><span class="glyphicon glyphicon-zoom-in" aria-hidden="true"></span>preview</a>, as well as download-able as <a href="https://docs.google.com/spreadsheets/d/1AqSlVvLtyLc62BIe4hc0X-NblMFfFrg9bvlGzSxCy5s/export?format=pdf" target="_blank"><span class="glyphicon glyphicon-cloud-download" aria-hidden="true"></span>PDF</a>.

<table class="table table-hover">
<thead>
<tr>
<th scope="col">
Data type
</th>
<th scope="col">
Zero value
</th>
<th scope="col">
nil assignable? (and so
also could be compared against nil)
</th>
<th scope="col">
Comparable? == <br/>
(can
be used as the key in the map)
</th>
<th scope="col">
Reference Type?<br/>
(pass
by "reference" <a href="#l01">[1]</a>)
</th>
<th scope="col">
Declaration
</th>
<th scope="col">
Initialization
</th>
<th scope="col">
%v value in fmt
</th>
</tr>
</thead>
<tbody>
<tr class="info">
<th scope="row" colspan="8">
<p class="text-center">1. Basic data types</p>
</th>
</tr>
<tr>
<th scope="row">
numbers
<br/>
(int,int8,int16,int32,int64,<br/>
uint,uint8,uint16,uint32,uint64,<br/>
byte,<br/>
float32,
float64)
</th>
<td>
0
</td>
<td>
No
</td>
<td>
Yes
</td>
<td>
No
</td>
<td>
var x int
</td>
<td>
x := 10
</td>
<td>
10
</td>
</tr>
<tr>
<th scope="row">
string
</th>
<td>
""
</td>
<td>
No
</td>
<td>
Yes
</td>
<td>
No
</td>
<td>
var s string
</td>
<td>
s := "go"
</td>
<td>
go
</td>
</tr>
<tr>
<th scope="row">
bool
</th>
<td>
false
</td>
<td>
No
</td>
<td>
Yes
</td>
<td>
No
</td>
<td>
var b bool
</td>
<td>
b := true
</td>
<td>
true
</td>
</tr>
<tr class="info">
<th scope="row" colspan="8">
<p class="text-center">2. Composite types</p>
</th>
</tr>
<tr class="info">
<th scope="row" colspan="8">
<p class="text-center">2.1. Aggregate types</p>
</th>
</tr>
<tr>
<th scope="row">
array<br/>
(fixed size)
</th>
<td>
Zero value of all it
elements<br/>
(len() returns the size of the array declared).<br/>
So
can be used immediately after declaration without prior
initialization
</td>
<td>
No
</td>
<td>
Yes<br/>
(when array type
is comparable)
</td>
<td>
No
</td>
<td>
var a [3]int
</td>
<td>
a := [3]int{10, 20, 30}<br/>
or
<br/>
a := [...]int{10, 20, 30}<br/>
or <br/>
a := [...]{99: -1}
(which will create the array of 100 elements, with last one
initialized to -1, while the rest to their zero value, 0 in the
case of the int-s) <a href="#l02">[2]</a>
</td>
<td>
[10 20 30]<br/>
or<br/>
[0
0 0] for zero value
</td>
</tr>
<tr>
<th scope="row">
struct<br/>
(fixed size)
</th>
<td>
Zero value of all it
fields.<br/>
So can be used immediately after declaration without
prior initialization
</td>
<td>
No
</td>
<td>
Yes<br/>
(when all fields
are comparable)
</td>
<td>
No
</td>
<td>
type point struct {<br/>
x,
y int<br/>
label string<br/>
visited bool<br/>
}<br/>
var p point
</td>
<td>
p := point{10, 20, "p",
true}<br/>
or partially initialize only relevant fields<br/>
p :=
point{y: 20, visited: true}<br/>
or the special case when the
struct is part of the map (and so the type can be omitted),
i.e.:<br/>
m := map[string]point{<br/>
&quot;p&quot;: {10, 20,
&quot;p&quot;, true},<br/>
}
</td>
<td>
{10 20 p true}<br/>
or<br/>
{0
0 false} for zero value
</td>
</tr>
<tr class="info">
<th scope="row" colspan="8">
<p class="text-center">2.2. Reference types</p>
</th>
</tr>
<tr>
<th scope="row">
slice<br/>
(dynamic)
</th>
<td>
nil <br/>
(len() returns
0).<br/>
Must be initialized before the use
</td>
<td>
Yes
</td>
<td>
No<br/>
(only legal
comparison is with nil)
</td>
<td>
Yes
</td>
<td>
var a []int
</td>
<td>
a := []int{10, 20,
30}<br/>
or<br/>
a := make([]int, 0, 10)<br/>
If the slice is
initialized with make and the length is set, i.e.<br/>
a :=
make([]int, 3, 10)<br/>
then len(a) will return that length, i.e.
3
</td>
<td>
[10, 20, 30]<br/>
or<br/>
[]
for zero value (even though the zero value is nil)<br/>
or<br/>
[0
0 0] when length was provided for the make
</td>
</tr>
<tr>
<th scope="row">
map<br/>
(dynamic)<br/>
(the
key must be comparable using ==)
</th>
<td>
nil <br/>
(len() returns
0).<br/>
Must be initialized before the use
</td>
<td>
Yes
</td>
<td>
No<br/>
(only legal
comparison is with nil)
</td>
<td>
Yes
</td>
<td>
var m map[string]int
</td>
<td>
m :=
make(map[string]int)<br/>
or<br/>
m := map[string]int{}<br/>
or<br/>
m
:= map[string]int{"a": 1, "b": 2}
</td>
<td>
[]map (for empty or nil
map)<br/>
or<br/>
map[a: 1 b: 2]
</td>
</tr>
<tr>
<th scope="row">
pointer
</th>
<td>
nil
</td>
<td>
Yes
</td>
<td>
Yes<br/>
(point to the
same variable or both are nil)
</td>
<td>
Yes
</td>
<td>
var p *int
</td>
<td>
p := &amp;x<br/>
or<br/>
p
:= new(int)<br/>
(although rarely used, also in this form new()
creates the new address, so those pointers are always different,
i.e. == returns false)
</td>
<td>
&lt;nil&gt;<br/>
or<br/>
0xc420074068
(address pointer points to)
</td>
</tr>
<tr>
<th scope="row">
chan
</th>
<td>
nil
</td>
<td>
Yes
</td>
<td>
Yes<br/>
(reference to the
same channel data structure or nil)
</td>
<td>
Yes
</td>
<td>
var c chan int
</td>
<td>
c := make(chan int)<br/>
for
unbuffered channel<br/>
or<br/>
c := make(chan int, 5)<br/>
for
buffered channel
</td>
<td>
&lt;nil&gt;<br/>
or<br/>
0xc42001e180
(address of the channel)
</td>
</tr>
<tr>
<th scope="row">
func
</th>
<td>
nil
</td>
<td>
Yes
</td>
<td>
No<br/>
(only legal
comparison is with nil)
</td>
<td>
Yes<br/>
<a href="#l03">[3]</a>
</td>
<td>
var f func(int) int
</td>
<td>
f := func(x int) int
{<br/>
return x + 1<br/>
} // function value<br/>
or normally
declared function<br/>
func inc(x int) int {<br/>
return x +
1<br/>
}<br/>
f := inc
</td>
<td>
&lt;nil&gt;<br/>
or<br/>
0x48b500
(address of the defined function)
</td>
</tr>
<tr class="info">
<th scope="row" colspan="8">
<p class="text-center">3. Interface type</p>
</th>
</tr>
<tr>
<th scope="row">
interface{}<br/>
(abstract
type)
</th>
<td>
nil
</td>
<td>
Yes
</td>
<td>
No (only legal comparison
is with nil) <a href="#l04">[4]</a><br/>
</td>
<td>
No
</td>
<td>
var any interface{}
</td>
<td>
any := 5<br/>
or<br/>
any
:= false<br/>
or <br/>
any := "string"<br/>
// etc. as you
can assign value of any type to the interface{} type
</td>
<td>
&lt;nil&gt;<br/>
or <br/>
the
%v of the underlying type

</td>
</tr>
<tr class="info">
<th scope="row" colspan="8">
<p class="text-center">4. Bonus</p>
</th>
</tr>
<tr>
<th scope="row">
bytes.Buffer<br/>
(is
actually a struct, so everything about struct applies here)
</th>
<td>
Empty buffer ready to use
</td>
<td>
<br/>
</td>
<td>
<br/>
</td>
<td>
<br/>
</td>
<td>
<br/>
</td>
<td>
var b bytes.Buffer<br/>
//
A Buffer needs no initialization, ready to use
</td>
<td>
<br/>
</td>
</tr>
<tr>
<th scope="row">
strings.Builder<br/>
(added
in Go 1.10)
</th>
<td>
Empty builder ready to
use
</td>
<td>
<br/>
</td>
<td>
<br/>
</td>
<td>
<br/>
</td>
<td>
<br/>
</td>
<td>
var b strings.Builder<br/>
//
A Builder needs no initialization, ready to use
</td>
<td>
<br/>
</td>
</tr>
</tbody>
</table>
<br/>

<p>
<span id="l01">[1]</span> Variables of these types are passed by value, as everything in the Go, although because they are “reference” types, which mean they hold the pointer (reference) to the underlying data structure, so it means that the pointer gets copied (i.e. alias is created), which allows to make the modifications to the underlying data structure pointer points (references) to.
</p>

<p>
<span id="l02">[2]</span> Another interesting example (taken from The Go Programming Language book): 
```
type Currency int 
const ( 
    USD Currency = iota 
    EUR 
    GBP 
    RMB) 
symbol := [...]string{USD: “$”, EUR: “€”, “GBP”: “£”, “RMB”: “¥”}"
```
</p>

<p>
<span id="l03">[3]</span> It said that the func is the reference type, and mentions it is possible to affect the original function, although it is not clear how. Any ideas?
</p>

<p>
<span id="l04">[4]</span> Although it is possible to compare interfaces with each other, it might fail at runtime when interface type (dynamic types are the same) represents the non comparable type, then it fails with panic. So compare interface values only if you are certain they contain dynamic values of comparable types.
</p>

****

I might have possibly missed some cases, but I believe the most common cases were described.
Will keep the table updated when new techniques are learnt or discovered in the process of practicing Go.

If you think there is something worth to add, please, share this in comments, I will appreciate!

Thanks, guys!

