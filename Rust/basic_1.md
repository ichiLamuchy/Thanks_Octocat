## Rust

-----

### Quick reference


a`.`len() - accessing array function

`...` including range


`..` Excluding range


`::` accessing inside enum, or crate to mod or mod to function.  

`use <crate> ::<mod>` (i.e. std::mem)      

`<mod> :: <function>` (i.e. mem::size_of_val()). 


`<variable> : <type>` (i.e. i:u8)


`{:?}` when you do not know the type such as when println!


`"{}"` placeholder 




-----

Play ground:
https://play.rust-lang.org/


How to start --- setting up the project

get in the VS code


This makes hello_world folder and src folder with main,rs as well as cargo.toml

```
cargo new hello_world --bin
```


You could also cargo init which use the actial folder you are init, and also create git ignoire file on it as wll


You can run it by

`cargo run`. 

This meands compile and run


### Core data types


`fn` for function

fn main (){} 

`let <variable>:<type> = <value>;`

example :

`let a:u8 = 123;` // 8bits  unsigned

if i8 then it would be signed 

`println!` for printing

Example:

`println!("a = {}", a); ` // it is like literal string on vcl

RUST is everything is IMMUTABLE

you must specify if you wanna change the value.

`let mut b:i8 = 0`

you can het rust to figure out what data typw it would be

so if you do below it will guess that wouldbe a bout u32

`let mut c = 123456;`

Let's print out size:


`println!("c = {}, size = {} bytes",c, mem::size_of_val(&c));` // mem is memory

to use the function do this above the main

`use std::mem`


```
let z:isize = 123; //isize/usize
let size_of_z = mem::size_of_val(&z);
println!(" z = {}, takes up to {} bytes, {}-bit os", z, size_of_z, size_of_z * 8);
```


let d = 'x'; //. let d:char = 'x';

println!(" d = {}, size = {} bytes", d, mem::size_of_val (&z));




## Operators

Pretty standard - +-*/ then %

they do not support ++ --
but they can   
`a += 1;  ` 


there is no pow you need to do

`let a_cubed =i32::pow(a, 3);`. / a * a * a

you need specify the data type when you use pow fn 


let b = 2.5;

`let b_cubed = f64::powi(b, 3); `        //powi is power with int data type only whole number
`let b_to_pi = f64::powf(b, std::f64::consts::PI);`  // consts is capital power of pi

`println!("{} cubed = {}, {}^pi = {}", b, b_cubed, b, b_to_pi);`


### bitwise - just like C

let c = 1|2; // | OR & AND ^ XOR !NOR. // XOR is like 
// this is 01 OR 10 = 11= 3_10

### Logical operaterts - usual

`let pi_less_4 = std::f64::consts::PI < 4.0` // true

>= 
<= 
==

```
let x = 5
let x_is_5 = x == 5; //true
```

### Scope and shadowing

 variable only lives in {}

It kind of remind me C

fn scope_and_shadowing (){
  let a = 123;
  {
   let b = 345;  
   println!("inside, b = {}", b);
   
   // let a = 777; // then it become the different variable a
   println!("inside, a = {}", a);  // if the one above is not present itr will get the valure of 123
  
  }
  println!("outside, a = {}", a);n // this become a 123 in b oth way if it ahs been declared within the scope or not

}

### CONST


you can have global like out side any fnction
how to do!
`const MEANING_OF_LIFE:u8 = 42;`. // this has no fixed address

it will be just replaced to 42 so wherever you write MEANING_OF_LIFE  beccome `42`

`static z:i32 = 123;`

you can make mutable. ----- however it is not very safe

you can create unsafe block like

```

fn whatever(){
  unsafe
  {
  }
}
```


### Memory - construct



from bottom - stack
a bit like c stuck computation. it stucks from the bottom, 
so since it is immutable you can just stuck any variable and function from the bottom, 
then it will pop when you use it so it will no need to delete the allocated memory.

#### heap

so when you using heap, it is like

`let x = Box::new(5);`

what is essentially doing this is that there is varible decalration of 
`let x = box` on the bottom of stuck, the box contstruct is point at the memory of top of the memory available,
then it store 5. So what it would happen when `prilntln!`

It will be the `println!("{}", *x);`
here `*` is dereferencing the x, so you are not printing out box, you are printing out 5


Function duct::sh

`pub`for public function
                            

```                            
#![allow(dead_code)]
use std::mem;

struct {
  x: f64,
  y: f64
}

fn origin ()-> point
{
  point {x: 0.0, y:0.0}
}

pub fn stack_and_heP()
{
  let P1 = origin();   // this contents the point itself
  let P2 = Box:: NEW((origin()));  // this contents just Box that contents pointer to origin

  println!("p1 takes up {} bytes, mem::size_of_val(&p1);
  println!("p2 takes up {} bytes, mem::size_of_val(&p2);
  
  let p3 = *p2;  // this is following - unboxing ( may be called dereferencing)
  println!("{}, p3.x);
 
}

```

-----

## About Mod

the module system as providing name-spacing and importing.
Just like library

```
Rust has a module system that enables the reuse of code in an organized fashion. A module is a namespace that contains definitions of functions or types, and you can choose whether those definitions are visible outside their module (public) or not (private).

A crate is a project that other people can pull into their projects as a dependency.
```

https://users.rust-lang.org/t/confused-by-rust-module-system/1991/2

```
From main you would have:

mod ai;
mod ai_simple;
mod ai_complex;
mod driver;
mod driver_mio;

```

At the top of the file. This declares that these name-spaces exist at all 
- it’s how the compiler knows to compile them. 
The code for each module lives either in a file named after that module at the same place in the filesystem it is declared 
(as you have done) or in a directory with the same name, and a file named mod.rs. 
In this way, src/ai.rs and src/ai/mod.rs are equivalent.

To use code from a given module, you need to use it. This is the importing side of the equation 
- it makes the functions/structs/traits available in the current namespace. So, to use the struct SimpleAI from main, you would include:

use ai::SimpleAI;
SimpleAI::new(); // SimpleAI is available without specifying its namespace

You could also do this:

use ai;
ai::SimpleAI::new(); // SimpleAI is available because its module is imported
So, a few simple rules:

For code to be compiled it must be declared as a **mod** first, **or** explicitly **added to Cargo.toml as a creat**.
For code to be used it must be imported into the current namespace via use - either explicitly or made available through its namespace.





-----

#### Control flow

if
```
if temp > 30
{
   println!("really hot");
}
else if temp < 10
{
  println!("really cold");  
}
else{
   println!("pretty ok");
}
```

#### if statement is expression !


let day = **if** temp > 20 **{**"sunny" **else** "cloudy"**}**;

you cannot omit `{}`. It is a bit like ?:

you can even have that all on println!

#### while for loop

`cotinue;`
skip rest of it back to the top of while loop

#### loop 

`loop` is the same as while true

```
 y *=n2;
 loop {
if y == 1<<10 {break;}. // this is 10000000000 in binary == 1024 in dec
}
```

#### for loop
`

```
fn for_loop(){
  for x in 1..11  // 1 to 10 inclusive
  {
  if x = {continue;}
  if x = {break;}    // beak out
    println!("x = {}", x);
  }
  
  for (pos, y) in (30..41).enumerate().  // pos goes to 30-40, y goes to 0-9
  {
    println! ("pos is {} which is {}", pos, y);
  }
}

```

##### match

this is  bit like switch
can be used for pattern match use `=>`

```
fn match_statement();
{
  let country_code = 44;
  let country = match country_code
  {
    44 => "UK",      // otherwise
    46 => "Sweden,
    7 => "russia,
    _ => "unknown".  / catch all statement
  };
  println! ("the country with code {} id {}". country_code, country);
}
```


you can match against range

```
fn match_dtatement();
{
  let country_code = 44;
  let country = match country_code
  {
    44 => "UK",      // otherwise
    46 => "Sweden,
    7 => "russia,
    1...999 => "unknown"  // this is like else if statement - this is inclusive !!!!!!!!!!!!!!!!!!!!!!!! what?
    _ => "invalid"  // this would be like -1 or like 777777 out side range of the one above
  };
  println! ("the country with code {} id {}". country_code, country);
}
```

triple `.` is inclusive, double `.` is exclusive
you cannot use `..` in match


it will be used for patturn match 

### Data structure !!!!!


use keyword `struct`

here using `.` to access elements in struct
think data sructure as class maybe ...


```
struct point
{
  x ; f64
  y : f64
}

fn structures (){
  let p = point {x: 3.0, y:4.0}
  println! ("Point p is at {}, {}", p.x, p.y);

}
```



### enum
The `enum` keyword allows the creation of a type which may be one of a few different variants. Any variant which is valid as a struct is also valid as an enum.

accessing inside is `::`

```
use std::mem;

enum color {
  Red,
  Green,
  Blue
}
```

```
fn enum()
{
  let c:color = color::Red;  // so this is like module aka library

  match c 
  {
    color::Red => println!("r"),
    color::Green => println!("g"),
    color::Blue => println! ("b")  
  }
}
```
so essentially what it is that enum is like data type,
In the data type, you pick Red,here `::` is referencing

Then it check what is matching

so it is like if (c == color::Red) then print "r"
`=>` is like `{}`

if one of element above is missing rust with throw error 

You can use below to use as else

```
_=>println!("something else!");

```

you can also create a tuple under the `match` using enum
you also need the tuple under enum as well.

```
color::RGBcolor(0,0,0)=>println!("black");
```

you can also use struct here

```
enum color {
  RGBcolor(u8,u8,u8),                            // tuple
  cmykcolor{cyan:u8, magento:u8,yellow:u8, black:u8} // struct
}
```

then of course need to use

```
let c:color = color::cmyk{cyan:0, magento:128, yellow:0, black:0};
```

on match if you do not care anything you can add like `_` again

```
color::cmykcolor{cyan:_, magento:_, yellow:_, black:255} => println!("black");
```

you can all catch all to do nothing

```
_=>();
```

another example as follows

```
// Create an `enum` to classify a web event. Note how both
// names and type information together specify the variant:
// `PageLoad != PageUnload` and `KeyPress(char) != Paste(String)`.
// Each is different and independent.
enum WebEvent {
    // An `enum` may either be `unit-like`,
    PageLoad,
    PageUnload,
    // like tuple structs,
    KeyPress(char),
    Paste(String),
    // or c-like structures.
    Click { x: i64, y: i64 },
}

// A function which takes a `WebEvent` enum as an argument and
// returns nothing.
fn inspect(event: WebEvent) {
    match event {
        WebEvent::PageLoad => println!("page loaded"),
        WebEvent::PageUnload => println!("page unloaded"),
        // Destructure `c` from inside the `enum`.
        WebEvent::KeyPress(c) => println!("pressed '{}'.", c),
        WebEvent::Paste(s) => println!("pasted \"{}\".", s),
        // Destructure `Click` into `x` and `y`.
        WebEvent::Click { x, y } => {
            println!("clicked at x={}, y={}.", x, y);
        },
    }
}

fn main() {
    let pressed = WebEvent::KeyPress('x');
    // `to_owned()` creates an owned `String` from a string slice.
    let pasted  = WebEvent::Paste("my text".to_owned());
    let click   = WebEvent::Click { x: 20, y: 80 };
    let load    = WebEvent::PageLoad;
    let unload  = WebEvent::PageUnload;

    inspect(pressed);
    inspect(pasted);
    inspect(click);
    inspect(load);
    inspect(unload);
}

```

You can create alias using `type`

```
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

// Creates a type alias
type Operations = VeryVerboseEnumOfThingsToDoWithNumbers;

fn main() {
    // We can refer to each variant via its alias, not its long and inconvenient
    // name.
    let x = Operations::Add;
}

```
another alias is `self` within `impl` block

```
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

impl VeryVerboseEnumOfThingsToDoWithNumbers {
    fn run(&self, x: i32, y: i32) -> i32 {
        match self {
            Self::Add => x + y,
            Self::Subtract => x - y,
        }
    }
}
```
https://doc.rust-lang.org/rust-by-example/custom_types/enum.html

### union

we do not know what is in there so use `unsafe`
access the inside of union is `.`

for allocate piece of memory

```
union IntOrFloat 
{
  i i32,
  f: f32
}

fn main (){
  let mut iof = IntOrFloat {i:123};
  iof.i = 234
  let value = unsafe {iof.i}; // because we do not know if it ire really int was assigned
  println!("iof.i {}", value);
}
```
you could also do 
```
fn process_value(iof: IntOrFloat)
{
  unsafe {
    match iof {
      IntOrFloat {i: 42}=>{
        println!("meaning of life value");
      }
      IntOrFloat {f} => {
        println ("Value = {}",f);
      }
    }
  }
}


fn main (){
  let mut iof = IntOrFloat {i:123};
  iof.i = 234
  let value = unsafe {iof.i}; // because we do not know if it ire really int was assigned
  println!("iof.i {}", value);
  process_value (IntOrFloat{i:42});
}

```
Above it if you add something like `process_value (IntOrFloat{i:5});`

Since it does not match to the first, it will fall back to the second option of match
then create a wrong float - complete nonsense.

-----
go over again below
### option

presence or absense of value

`option<t>`
let result:option<f64> = 


`some(z) none`

you can use if let some(z) to deconstructing

### Array

you need to know how many elements


### String

```
fn strings()
{
    
    let s:&'static str = "hello!";
    println!("{}", s);
    
    for c in s.chars().rev(){
    println!("{}", c);
    
    }
        if let Some(x) = s.chars().nth(0){
        println!("last letter is {}", x);
    }
    let mut letters = String::new();
    let mut a = 'a' as u8;
    while a <= ('z' as u8){
        letters.push(a as char);
        letters.push_str(",");
        a +=1;
        println!("{}", letters);
    }
    // let ou:&str = &letters;
    
    // concat 
    // String + str is finr
    //let strings = letters + &letters;   
}

fn main (){
    strings();

}

```

### Tuple

```
// tuple is correction of several values
// types can be different

fn sum_of_it(x:i32, y:i32)->(i32, i32){  // making tuple here
    (x+y, x*y)
}


fn tuples(){
    let x = 3;
    let y = 4;
    let sp = sum_of_it (x,y);
    
    println!("sp = {:?}", sp);  //sp = (7, 12)
}
 
  fn main(){
      
      tuples();
  }
```














-----

