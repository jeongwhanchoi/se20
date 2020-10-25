<a name=top>
<a href="http://tiny.cc/seng20"><img  width=700
  src="https://raw.githubusercontent.com/txt/se20/master/etc/img/teamBanner.png"></a>
<hr>
<p>
&nbsp;<a href="https://tiny.cc/seng20">home</a> ::
<a href="https://github.com/txt/se20/blob/master/docs/syllabus.md#top">syllabus</a> ::
<a href="https://github.com/txt/se20/blob/master/docs/syllabus.md#timetable">timetable</a> ::
<a href="https://drive.google.com/drive/folders/1ZFn6H8-4kx5uP34bpFgIFonkz9Tw3nYM?usp=sharing">groups</a> ::
<a href="https://moodle-courses2021.wolfware.ncsu.edu/course/view.php?id=3873">moodle</a> ::
<a href="http://seng20.slack.com">chat</a>  ::
<a href="https://github.com/txt/se20/blob/master/LICENSE.md#top">&copy; 2020</a>  
<br>
<hr>

# Abstraction

All these following use the same idea... abstraction

- Language features that simplify programming complex systems
  - Iterators
  - Error handling
- Hardware independent operating systems
- Languages implemented via virtual machines (LISP, Smalltalk, Java, JavaScript, Erlan,...)
- Languages implemented via transpiles
- "Jump technologies" 
  - Virtual machines
  - Containers
  - Serverless computing

Note that while the applications are different, and occur at very different scales,
the essential idea of _abstraction_ is used  throughout

- Between the programmer and the hardware there are layers of abstraction
- Programmers talk to the abstraction
- Automated tools make abstraction to the hardware



## Queuing Theory 
Before we start... this is going to seem a digression, but just stay with me here

N tellers/ checkout people. How many lines? One? N?

<img src="https://csbcorrespondent.com/sites/default/files/WSJ%20Teller%20Graphic_050418%20v.4.jpg" width=500>

Answer: see [7 insights into Queue Theory](http://www.treewhimsy.com/TECPB/Articles/SevenInsights.pdf)

[According to the RIOT paper](https://arxiv.org/pdf/1708.08127.pdf)
Computers
execute in highly dynamic environment.

- For example, Schad et al. [11] found that the
runtime of a widely used benchmarks suite can vary by up
to 33% even when run on supposedly identical instances
within the same cloud environment. 
- Not only CPU, but
also bandwidth can be highly variable within the cloud.
  - Schad et al. report that network bandwidth between the
same type of EC2 instances can vary from 410KB/s to
890KB/s. 
- Hence, even after a workflow is planned and
deployed, it is important to monitor instances and repeat
the scheduling process during deployment when necessary.
- If repeated rescheduling are too slow, then there is much waste of computer rescues.

## Abstraction

Abstraction in computer science had to be invented.
- Barbara Liskov won at $250K prize for her work on  abstraction in programming languages.
  - [Her award lecture](https://amturing.acm.org/vp/liskov_1108679.cfm)
    is amazing
    - at minute  1:09 to 1:11 the whole world disappears and her and Guy Steele debate
      iterator design and the rest of us can just listen

Why abstraction? well?

- If you underlying data structures are complex, you want some way to talk to them
  without having to know everything about them
- If you hardware is different, 
  -  you want to write code in  hardware independent manner
     - so code can run all over the place
  - Add an abstraction layer between
    - operating systems below and
    - the application above
    - Once you get "it" going for one OS on one machine
      - It runs wherever that OS runs
    - e g. UNIX
- If computing environments are dynamic, we want to bounce our
computation around the hardware to take full advantage of unused
resources.
 - And when we bounce a "program", we want to bounce with it:
     - all the current
   variable settings, 
     - contents of RAM, 
     - activation records of current
   functions, 
     - return addresses for when those functions return, 
     - network
   connections, 
     - hooks to GUIs and databases, etc etc. 
 - Think of the _layers_ pattern, on steroids
     - Where you grab whole layers and move them to another piece hardware
 - Under the hood, again, this is abstraction.

This tactic has been applied, successfully, dozens of times in the history of computing

- Language features that simplify programming complex systems
  - Iterators
  - Error handling
- Hardware independent operating systems
- Languages implemented via virtual machines (LISP, Smalltalk, Java, JavaScript, Erlan,...)
- Languages implemented via transpiles
- "Jump technologies" 
  - Virtual machines
  - Containers
  - Serverless computing

## Error handling

Throw a ball into a stadium, wait for what returns while holding a catchers mitt.
Only catch things you know how to handle

```python
def atom(token: str) -> Atom:
    "Numbers become numbers; every other token is a symbol."
    try: return int(token)
    except ValueError:
        try: return float(token)
        except ValueError:
            return Symbol(token)
```

## Iterators
When you code recursive data structures, give programmers
so way to way around that code.

e.g. What does this return?

```python
for x in [10,[[11,12,13],15], 20,30,[40,50,"tim"]]:
   print(x)
```

```python
def items(x):
  if isinstance(x,(str,list,tuple)):
   for y in x:
     for z in items(y):
       yield z
  else:
    yield x
```
Another example (in awk):

```awk
while(csv(lst,file))
  doSOmething(lst)
```

```awk
function csv(a,file,     i,j,b4, ok,line,x,y) {
  file  = file ? file : "-"     # [1] read from standard input or file       
  ok = getline < file
  if (ok <0) {                  # [2] complain if missing file
     print "missing "file > "/dev/stderr"; exit 1 }
  if (ok==0) { close(file);return 0 }                                    
  line = b4 $0                         
  gsub(/([ \t]*|#.*$)/, "", line) # [3] kill white space and comments      
  if (!line)       return csv(a,file, line) # [4] skip blanks lines
  if (line ~ /,$/) return csv(a,file, line) # [5] contact incomplete rows with next
  split(line, a, ",")                       # [6] split line into cells on comma
  for(i in a) {
    x=a[i]
    y=a[i]+0
    a[i] = x==y ? y : x                     # [7] coerce number strings, if any
  }
  return 1
}
```

## UNIX

If iterators and error handlers are "little abstractions", then operating systems are
"big abstractions".

<img align=right width=500 src="https://upload.wikimedia.org/wikipedia/commons/4/49/Baby_Bells.svg">

- Pre-unix: operating systems were hardware-specific
- The breakup of the Bell System was mandated on January 8, 1982
  - So after laying down all that copper
    - Bell Corporation was not allowed to use it all. 
    - And they actually set up commercial competitors!
- They needed someway for different organizations to work together
  - The ultimate "why can't we all get along"
- Step1: a hardware independent systems language "C"
- Step2: an operating system written in "C"
- Step3: design patterns to allow for interactions
  - Uniform file representation (everything is "ASCII test", no special binary formats for each fie).
  - Enter the _pipe_ pattern

Examples
- `nroff` is a text formatter (old version of latex)
- `lp` is the line printer
- `col` handles escale sequences that layes up text in 2 columns
- `tbl` is an nroff pre-processor that expresses tables in terms of lower-level nroff commands
- `eqn` is an nroff pre-processor that expresses equations in terms of lower-level nroff commands

```bash
nroff files | col | lp

tbl file-1 file-2 . . . | eqn | nroff -ms
````

Think of pipes as `produce`, `translate`, `filter`, `consume`:

```bash
find /usr/bin/ |                #produce 
sed 's:.*/::'  |                #translate: strip directory part
grep -i '^z'   |                #filter   : select items starting with z
xargs -d '\n' aFinalConsumer    #consume 
```

## Containers

If iterators is small abstraction
- and OS is large abstraction
- then containers are in the middle

<img src="https://imesh.github.io/images/contvsvm.png">


## Serverless

Servless is small than containers

- Serverless
  - e.g. AWS Lambda (btw, why is it called "lambda"? See below).
  - Why even have an operating system, just start with your current application and only ship the little bits
    needed for the application
  - Good news:
    - Cheaper
    - Smaller memory (so more scalable)
    - Productivity: less to configure, less messing with threading.
      Just explore  the unit of code to the outside world are simple event driven functions.
  - Bad news:
    - Debugging. If you ship a bare bones unit of functionality, you may not get
      the debugging tools needed to understand a drash.
    - Security: more tiny components, less protection from other OS facilites (which
      not execist in your tiny comptuational slice)
    - Vendor lock-in : 
          - But in the serveless world, no OS abstraction between comptuation and support
        environment
        - So not "write once, run everywhre" 
    - Configuration: can't make _any_ assumptions about background utilities from
      "the operating system" cause their ain't one
      - You have to do it all.



