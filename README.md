### 1. Is it possible to measure the quality of an arbitrary section of code? If so, how should we measure?  If not, why not?

Some my own thoughts after class of week 6. On the class, we learn some metrics to measure the code. <br />
* *Halstead's Complexity Measures*:
1. Operators: traditional(+, -), and keywords(return, if, break), N1(unique n1) <br />
2. Operands: identifiers, constants, N2(unique n2) <br />
Length: N = N1 + N2 <br />
Vocabulary: n = n1 + n2 <br />
Volume: V = Nlog<sub>2</sub>n <br />
Difficulty: D = (n1/2)*(N2/n2) <br />
Effort: E = V * D <br />
Bugs delivered: E<sup>2/3</sup>/3000 <br />

* *Cyclocmatic Complexity Measures*: <br />
v(G) = #edges - #vertices + 2 <br />
v(G) = #binaryDescisions + 1 <br />

* *Maintainability Index*: <br />
MI = 171 - 5.2ln(V) - 0.23v(g) + 16.2ln(LOC) <br />

First, it looks like we can measure the quality of a piece of code. But after think twice, I have a key concern that there is no clear explanation for the specific derived formula like bugs delivered and maintainability index. The set of programs used to derive the metric and evaluate it was small, and contained small programs only. And the programs were written in C, which may have rather different characteristics than current object-oriented languages such as C++ or Java.

There is a study questioning software maintainance metrics. <br />
https://www.mn.uio.no/ifi/personer/vit/dagsj/sjoberg.anda.mockus.esem.2012.pdf

The conclusion is as follows: <br />
1. The considered common maintainability metrics were not mutually consistent in the considered projects. <br />
2. Among the considered maintainability metrics, only size and the inverse of cohesion were strongly correlated with the actual maintenance effort observed in the study. <br />

So we may measure the quality of a small program like MI, but for real application I think it is really hard to measure. Moreover, the quality has many attributes.

Here is a summary of quality attributes. <br />
https://www.spinellis.gr/codequality/intro.html <br />

The functionality of software is the quality characteristic associated primarily with what the software does, rather than how it does it. The elements of the software's functionality are: the suitability of the functions for the specified tasks and the user's objectives, the accuracy of its results or operation, the interoperability of the software with other systems, and the security the software affords to its data. The suitability and interoperability characteristics are difficult to discern from code.

The reliability of software refers to its capability to maintain its specified level of performance under the specified conditions. The three elements of reliability mirror the prevention, mitigation, and recovery concepts we use for dealing with crises and natural disasters. Maturity refers to the absence of faults in the software, while fault tolerance is associated with the capacity of the software to continue functioning despite some faults, and recoverability deals with functions in the software that allow it to get back the data and continue operation after a failure.

The usability of the software is primarily an external quality characteristic. Three of its elements roughly correspond to a typical timeline of software use: understandability, how easily we can understand whether the software is suitable for our needs, and how to use it to accomplish a particular task; learnability, the effort required to learn it, and operability, the effort required to use it. In addition, attractiveness deals with the feeling the software leaves on us. Although usability is a very important quality element, it is quite difficult to determine it by examining the software's code. One could for example look for the use of appropriate APIs to select a file, a color, or a font, but in the end, usability is judged by the interaction of the user with the software.

Software efficiency deals with the ying and yang elements of computation: space and time. These two primal opposing but complementary concepts are what make practical computation possible. Disable all your computer's caches (gaining space), and your machine will grind to a halt (loosing time). Distribute your SETI calculation over the internet (occupying more space), and your processing will fly (gaining time). In true ying/yang style, in some rare cases it is even possible to for a program to gain in both directions: squeeze a tight loop's instructions to fit a cache, and the smaller code will also run faster. Trying to separate the two concepts, we talk about the software's time behavior, which deals with response, processing times, and throughput rates, and resource utilization, which refers to the material resources (memory, CPUs, network connections) used by the software.

The maintainability of software is probably the element that can best be approached at the level of the software's design and actual code. When talking about the software's maintainability we are interested about analyzability, how easy it is for us to locate the elements we want to improve or fix; changeability, how much work we need to do to implement a modification; stability, how few things break after our changes; and testability, our ability to validate our modifications.

Finally, portability refers to how easy it is to take the software from one environment (for example Windows) and transfer it to another (for example Mac OS X). Our main goal here is adaptability, the capability of the software's code to function in different environments. Other sub-characteristics of portability are mainly operational in nature: installability deals with the software's installation in different environments, co-existence examines how well the software plays in a crowded playground, and replaceability denotes the extend to which a software can be used as a drop-in replacement for another.

So from a more general point of view of quality, I think it is even harder to measure.

### 2. On a scale of 1 to 10, what is the strength of the connection between quality and refactoring?  (Assume 1 means almost no connection, and 10 means a very strong connection)

Refactoring: a change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its obserable behavior.

*Beck's Four Simple Design Rules*:
1. Runs all the tests <br />
2. Contains no duplications <br />
3. Expresses intent <br />
4. Minimum classes and methods <br />

There is quantitative analysis showing that refactoring indeed does not decrease Cyclomatic Complexity. On the other hand, the qualitative analysis showed that a refactoring tends to improve code in terms of readability and maintainability. <br />
http://www.mauricioaniche.com/wp-content/uploads/2013/04/wbma2013-refactoring1.pdf

From Wikipedia, there are two general categories of benefits to the activity of refactoring. <br />
https://en.wikipedia.org/wiki/Code_refactoring <br />

Maintainability. It is easier to fix bugs because the source code is easy to read and the intent of its author is easy to grasp. This might be achieved by reducing large monolithic routines into a set of individually concise, well-named, single-purpose methods. It might be achieved by moving a method to a more appropriate class, or by removing misleading comments.

Extensibility. It is easier to extend the capabilities of the application if it uses recognizable design patterns, and it provides some flexibility where none before may have existed.

From my own experience, especially homework one and two, I think it makes a lot of sense to me. For example, reducing large monolithic routines into a set of individully concise, well-named, single-purpose methods. <br />
Before: <br />
```python
def gs(self, n, m, d, s): # generate screens
    self.sl.append(screen(n, m, d, s))
    self.sl[-1].gt(4)
```

After refactoring: <br />
```python
def generate_screens(self, n, movie, date, seats): # generate screens
    self.screens_list.append(Screen(n, movie, date, seats))
    self.screens_list[-1].generate_tickets(4)
```

The refactoring makes the source code easier to read and the intent of its author easier to catch.

I also took the OOP class last quarter and learned a lot of design patterns such as factory. I think design is very very important for a software in real business application. Using design patterns can provide us better organization and structure when requirements become very complicated. This makes unit test easier and also makes the capabilities of the application easier to extend. <br />
https://en.wikipedia.org/wiki/Software_design_pattern

I do not think refactoring code can run faster nor use lower resource utilization since those are not the purpose of refactoring. I agree with that refactoring can only improve maintainability and extensibility not other quality attributes, so I will give 4 out of 10 for the strength of the connection between quality and refactoring. <br />
https://arxiv.org/pdf/1502.03526v1.pdf