# <center> A Quick Research of Active Automata Learning </center> #

----------------------------
## <center> CONTENTS </center> ##
1. Introduction
2. Target Automata Types
	- 2.1 Deterministic Finite Automata (DFAs)
	- 2.2 Nondeterministic Finite Automata (NFAs)
	- 2.3 Moore Machine
	- 2.4 Mealy Machine
	- 2.5 Register Automata (RAs)
	- 2.6 Büchi Automata (BAs)
	- 2.7 Nominal Automata
	- 2.8 Timed Automata
	- 2.9 Weighted Automata (WFAs)
	- 2.10 Hybrid Automata
	- 2.11 Symbolic Automata (Sigma3 **TBD**)
	- 2.12 Others
3. Approach
	- 3.1 Angluin's L* Algorithm
	- 3.2 RPNI Algorithm (NFAs)
	- 3.3 Rivest & Schapire's Algorithm
	- 3.4 Kearns & Vazirani's Algorithm
	- 3.5 Lω Algorithm (ω-regular sets)
	- 3.6 RPNI2 Algorithm
	- 3.7 ID and IID Algorithm
	- 3.8 DeLeTe2 Algorithm
	- 3.9 Estimation-Exploration Algorithm
	- 3.10 L* based Algorithm for Büchi automaton (Büchi automaton)
	- 3.11 TL* Algorithm (DERAs)
	- 3.12 LM* and LM+ Algorithm (Mealy Machines)
	- 3.13 NL* Algorithm (NFAs)
	- 3.14 IOA Algorithm (Mealy Machines for I/O Automata)
	- 3.15 DHC and LM* Algorithm (Mealy Machines for realistic reactive systems)
	- 3.16 RAL Algorithm (Register Automata)
	- 3.17 Ll Algorithm (finite cover automata)
	- 3.18 The TTT Algorithm
	- 3.19 Heerdt's Algorithm (Mealy Machines)
	- 3.20 SL* Algorithm (EFSM, Register Automata)
	- 3.21 FDFAs-based Algorithm (Regular ω-languages)
	- 3.22 A Mapper-Based Algorithm (Register Automata)
	- 3.23 LearnLTS Algorithm
	- 3.24 Medhat's Algorithm (Hybrid Automata)
	- 3.25 learning VPA (Visibly Pushdown Automata)
	- 3.26 learning WFAS (Weight Automata)
	- 3.27 MooreMI algorithm (Moore Machines)
	- 3.28 νL* and νNL* Algorithm (Nominal Automata)
	- 3.29 Product L* Algorithm (Moore Machines)
	- 3.30 A Tree-based Algorithm for FDFAs (Büchi automaton)
4. Tools
	- 4.1 LearnLib 
	- 4.2 Next Generation LearnLib (NGLL)
	- 4.3 Libalf
	- 4.4 RALT
	- 4.5 Tomte
	- 4.6 RALib
	- 4.7 ROLL
	- 4.8 Scikit-SpLearn
5. Application
	- **5.1 Protocol**
	- **Special topic: State machine inference for security protocols**
	- 5.2 SmartCard
	- 5.3 Legacy software
	- 5.4 LBT & Testing Finite-State Machines
	- 5.5 Conformance Testing
	- 5.6 Model Checking & Black Box Checking
	- 5.7 Improving Efficiency And Scalability of Verification
	- 5.8 Testing IoT Communication
	- 5.9 Inferring Interface Programs of Systems At Runtime
	- 5.10 Program Structures And Interface Programs
	- 5.11 Extracting Automata from Neural Networks
	- 5.12 Active Automata Learning For Real-Life Applications
	- 5.13 Learning Communicating Automata from MSCs 
	- 5.14 Automated Compositional Verification For Timed Systems
	- 5.15 Learn stateful typestates
	- 5.16 Fuzzy Learning Automata
6. Challenge And Discussion
	- 6.1 Predicates and operations on data
	- 6.2 Beyond Mealy machines
	- 6.3 Quality of models
	- 6.4 Opening the box
	- 6.5 The Challenge of Equivalent query
7. Reference

----------------------------

## <center> ARTICLE </center> ##

## 1. Introduction ##

This is a survey on active automata learning.

Automata learning, or model learning, aims to construct black-box state diagram models of software and hardware systems by providing inputs and observing outputs. In this article, we focus on one specific type of models, namely Automata, which are crucial for understanding the behavior of many software systems. Model inference techniques can be either white box or black box, depending on whether they need access to the code. In this article, we discuss black box techniques. Advantages of these techniques are that they are relatively easy to use and can also be applied in situations where we do not have access to the code or to adequate white box tools. There is a large body of research on learning automata and state machines, which can be divided into two broad categories: learning with  queries and answers(active learning), and learning only from examples (passive learning). As a final restriction, we only consider techniques for active learning, that is, techniques that accomplish their task by actively doing experiments (tests) on the software. This survey mainly foucus on active Automata learning, and the related passive learning techniques may be slightly involved.

## 2. Target Automata Types ##

The original active automata learning algorithm has originally been presented for **Deterministic Finite Automata (DFA)**, but has since been adapted to **Mealy Machines**, which are a better fit for learning actual reactive systems as they can encode system output in a natural way. A major and recent increase in expressiveness is achieved with **Register Automata (RA)** and **Buchi Automata (BA)**.

### 2.1 Deterministic Finite Automata (DFA) ###

A deterministic finite automaton M is a 5-tuple, $(Q, Σ, δ, q0, F)$, consisting of  
- a finite set of states (Q)  
- a finite set of input symbols called the alphabet (Σ)  
- a transition function (δ : Q × Σ → Q)  
- an initial or start state (q0 ∈ Q)  
- a set of accept states (F ⊆ Q)  

Let w = a1a2 ... an be a string over the alphabet Σ. The automaton M accepts the string w if a sequence of states, r0,r1, ..., rn, exists in Q with the following conditions:  
1. r0 = q0  
2. ri+1=δ(ri, ai+1), for i = 0, ..., n−1  
3. rn∈F.  

In words, the first condition says that the machine starts in the start state q0. The second condition says that given each character of string w, the machine will transition from state to state according to the transition function δ. The last condition says that the machine accepts w if the last input of w causes the machine to halt in one of the accepting states. Otherwise, it is said that the automaton rejects the string. The set of strings that M accepts is the language recognized by M and this language is denoted by L(M).

A deterministic finite automaton without accept states and without a starting state is known as a transition system or semiautomaton.

**Related Approach**  

| Related Approach   | -    | Title                                                                           |
|:------------------:|-----:| ------------------------------------------------------------------------------- |
| Angluins et al.    | 1987 | Learning regular sets from queries and counterexamples                          |
| Rivest and Schapire| 1993 | Inference of Finite Automata Using Homing Sequences                             |
| Kearns and Vazirani| 1994 | An introduction to computational learning theory                                |
| parekh et al.      | 1997 | A polynomial time incremental algorithm for regular grammar inference           |
| Denis et al.       | 2001 | Learning regular languages using RFSAs                                          |
| Bongard et al.     | 2005 | Active Coevolutionary Learning of Deterministic Finite Automata                 |
| Isberner et al.    | 2014 | The TTT Algorithm: A Redundancy-Free Approach to Active Automata Learning       |
| Volpato et al.     | 2015 | Approximate Active Learning of Nondeterministic Input Output Transition Systems |

### 2.2 Nondeterministic Finite Automata (NFA) ###

In automata theory, a finite state machine is called a deterministic finite automaton (DFA), if  
- each of its transitions is uniquely determined by its source state and input symbol, and  
- reading an input symbol is required for each state transition.  

A nondeterministic finite automaton (NFA), or nondeterministic finite state machine, does not need to obey these restrictions. In particular, every DFA is also an NFA. Sometimes the term NFA is used in a narrower sense, referring to a NDFA that is not a DFA, but not in this article.  
Using the subset construction algorithm, each NFA can be translated to an equivalent DFA, i.e. a DFA recognizing the same formal language. Like DFAs, NFAs only recognize regular languages.

**Related Approach**

| Related Approach   | -    | Title                                                                    |
|:------------------:|-----:| ------------------------------------------------------------------------ |
| Oncina et al.      | 1992 | Inferring Regular Languages in Polynomial Updated Time                   |
| Dupont et al.      | 1996 | Incremental regular inference                                            |

### 2.3 Moore Machine ###

In the theory of computation, a Moore machine is a finite-state machine whose output values are determined only by its current state. This is in contrast to a Mealy machine, whose output values are determined both by its current state and by the values of its inputs.   
A Moore machine can be defined as a 6-tuple (S, S_0, Σ, Λ, T, G) consisting of the following:  
- a finite set of states S  
- a start state (also called initial state) S_0 which is an element of S  
- a finite set called the input alphabet Σ  
- a finite set called the output alphabet Λ  
- a transition function T:S × Σ → S mapping a state and the input alphabet to the next state  
- an output function G:S → Λ mapping each state to the output alphabet  
A Moore machine can be regarded as a restricted type of finite-state transducer.

**Preliminaries**

- Moore E F. Gedanken-Experiments on Sequential Machines[M]// Automata Studies. 1956:129-153.

**Related Approach**

| Related Approach   | -    | Title                                                                    |
|:------------------:|-----:| ------------------------------------------------------------------------ |
| Georgios et al.    | 2016 | Learning Moore Machines from Input-Output Traces                         |
| Moerman et al.     | 2017 | Learning Product Automata                                                |

### 2.4 Mealy Machine ###

In the theory of computation, a Mealy machine is a finite-state machine whose output values are determined both by its current state and the current inputs. (This is in contrast to a Moore machine, whose output values are determined solely by its current state.) A Mealy machine is a deterministic finite-state transducer: for each state and input, at most one transition is possible.  
A Mealy machine is a 6-tuple (S, S_0, Σ, Λ, T, G) consisting of the following:  
- a finite set of states S  
- a start state (also called initial state) S_0 which is an element of S  
- a finite set called the input alphabet Σ  
- a finite set called the output alphabet Λ  
- a transition function T:S × Σ → S mapping pairs of a state and an input symbol to the corresponding next state.  
- an output function G:S × Σ → Λ mapping pairs of a state and an input symbol to the corresponding output symbol.  
In some formulations, the transition and output functions are coalesced into a single function T:S × Σ → S × Λ.

**Preliminaries**

- Mealy G H. A method for synthesizing sequential circuits[J]. Bell System Technical Journal, 2013, 34(5):1045-1079.

**Related Approach**

| Related Approach   | -    | Title                                                                 |
|:------------------:|-----:| --------------------------------------------------------------------- |
| Shahbaz et al.     | 2009 | Inferring Mealy Machines                                              |
| Aarts et al.       | 2010 | Learning I/O Automata                                                 |
| Steffen et al.     | 2011 | Introduction to Active Automata Learning from a Practical Perspective |
 

### 2.5 Register Automata (RA) ###

Register Automata are an extension of finite automata with data from in finite domains and are, e.g., well-suited for describing communication protocols. Register Automata are defied as follows:  

**Definition 1**. Let a symbolic input be a pair (a; p¯), of a parameterized input a of arity k and a sequence of symbolic parameters p¯ = <p1, ..., pk> Let further X = <x1, ..., xm> be a finite set of registers. A guard is a conjunction of equalities and negated equalities, e.g., pi != xj, over formal parameters and registers. An assignment is a partial mapping ρ : X → X ∪ P for a set P of formal parameters.  

**Defiition 2**. A Register Automaton (RA) is a tuple A\* = (A, L, l0, X, Γ, λ), where  
- A is a finite set of actions.  
- L is a finite set of locations.  
- l0 ∈ L is the initial location.  
- X is a finite set of registers.  
- Γ is a finite set of transitions, each of which is of form h<l, (a, p¯), g, ρ, l'>, where l is the source location, l' is the target location, (a, p¯) is a parameterized action, g is a guard, and ρ is an assignment.  
-  λ : L → {+, -} maps each location to either + (accept) or - (reject).  

Let us define the semantics of an RA A\* = (A, L, l0, X, Γ, λ). A X-valuation, denoted by v, is a (partial) mapping from X to D. A state of A\* is a pair <l, v> where l ∈ L and v is a X-valuation. The initial state is <l0, v0>, i.e., the pair of initial location and empty valuation.  

A step of A\*, denoted by <l, v> -(a,d¯)→ <l', v'>, transfers A\* from <l, v> to <l0, v0> on input (a, d¯) if there is a transition <l, (a, p¯), g, ρ, l'> ∈ Γ such that (1) g is modeled by d¯ and v, i.e., if it becomes true when replacing all pi by di and all xi by v(xi), and such that (2) v' is the updated X-valuation, where v'(xi) = v(xj) wherever ρ(xi) = xj, and v'(xi) = dj wherever ρ(xi) = pj.

**Related Approach**

| Related Approach   | -    | Title                                                                 |
|:------------------:|-----:| --------------------------------------------------------------------- |
| Howar et al.       | 2012 | Inferring Canonical Register Automata                                 |
| Cassel et al.      | 2014 | Active learning for extended finite state machines                    |
| Aarts et al.       | 2015 | Learning Register Automata with Fresh Value Generation                |


### 2.6 Büchi Automata ###

In computer science and automata theory, a Büchi automaton is a type of ω-automaton, which extends a finite automaton to infinite inputs. It accepts an infinite input sequence if there exists a run of the automaton that visits (at least) one of the final states infinitely often. Büchi automata recognize the omega-regular languages, the infinite word version of regular languages. It is named after the Swiss mathematician Julius Richard Büchi who invented this kind of automaton in 1962.  
Büchi automata are often used in model checking as an automata-theoretic version of a formula in linear temporal logic.
  
Formally, a deterministic Büchi automaton is a tuple A = (Q, Σ, δ, q0, F) that consists of the following components:  
- Q is a finite set. The elements of Q are called the states of A.  
- Σ is a finite set called the alphabet of A.  
- δ: Q × Σ → Q is a function, called the transition function of A.  
- q0 is an element of Q, called the initial state of A.  
- F⊆Q is the set of accepting states. A accepts exactly those runs in which at least one of the infinitely often occurring states is in F.  

In a non-deterministic Büchi automaton, the transition function δ is replaced with a transition relation Δ that returns a set of states, and the single initial state q0 is replaced by a set I of initial states. Generally, the term Büchi automaton without qualifier refers to non-deterministic Büchi automata.

**Preliminaries**

- Büchi J R. On a Decision Method in Restricted Second Order Arithmetic[M]// The Collected Works of J. Richard Büchi. Springer New York, 1990:511-8.  
- Calbrix H, Nivat M, Podelski A. Ultimately periodic words of rational ω -languages[J]. Comptes Rendus de l Académie des Sciences - Series I - Mathematics, 1993, 802(5):554-566.  
- Farwer B. ω-Automata[M]// Automata Logics, and Infinite Games. Springer Berlin Heidelberg, 2002:3-21.     

**Related Approach**  

| Related Approach  | -    | Title                                                                                          |
|:-----------------:|-----:| ---------------------------------------------------------------------------------------------- |
| Maler and Pnueli  | 1995 | On the learnability of infinitary regular sets                                                 |
| Farzan et al.     | 2008 | Extending Automated Compositional Verification to the Full Class of Omega-Regular Languages    |
| Angluin et al.    | 2014 | Learning Regular Omega Languages                                                               |
| Li et al.         | 2017 | A Novel Learning Algorithm for Büchi Automata Based on Family of DFAs and Classification Trees |

### 2.7 Nominal Automata ###

Nominal automata are automata for infinite alphabets uses the notion of nominal sets.
Consider now an infinite alohabet A = {a, b, c, d, ... }. The language L1 becomes {aa, bb, cc, dd, ...}. Classical theory of finite automata does not apply to this kind of languages, but one may draw an infinite deterministic automaton that recognizes L1. This automaton ostensibly have infinitely many states, but the set of states can be finitely presented in a way open to effective manipulation. More specifically, in a nominal automaton the set of states is subject to an action of permutations of a set of atoms, and it is finite up to that action.

**Preliminaries**

- Bojańczyk M, Klin B, Lasota S. Automata theory in nominal sets[J]. Logical Methods in Computer Science, 2014, 10(3).

**Related Approach**  

| Related Approach  | -    | Title                                        |
|:-----------------:|-----:| -------------------------------------------- |
| Moerman et al.    | 2017 | Learning nominal automata                    |

### 2.8 Timed Automata ###

In automata theory, a timed automaton is a finite automaton extended with a finite set of real-valued clocks. During a run of a timed automaton, clock values increase all with the same speed. Along the transitions of the automaton, clock values can be compared to integers. These comparisons form guards that may enable or disable transitions and by doing so constrain the possible behaviors of the automaton. Further, clocks can be reset. Timed automata are a sub-class of a type hybrid automata.

Formally, a timed automaton is a tuple A = (Q,Σ,C,E,q0) that consists of the following components:  
- Q is a finite set. The elements of Q are called the states of A.  
- Σ is a finite set called the alphabet or actions of A.  
- C is a finite set called the clocks of A.  
- E ⊆ Q × Σ × B(C) × P(C) × Q is a set of edges, called transitions of A, where  
	- B(C) is the set of boolean clock constraints involving clocks from C, and  
	- P(C) is the powerset of C.  
- q0 is an element of Q, called the initial state.  
An edge (q,a,g,r,q') from E is a transition from state q to q' with action a, guard g and clock resets r.

**Preliminaries**

- Alur R, Dill D L. A theory of timed automata[M]. Elsevier Science Publishers Ltd. 1994.  
- Bengtsson J, Yi W. Timed Automata: Semantics, Algorithms and Tools[J]. Lectures on Concurrency & Petri Nets, 2004, 3098:87-124.

**Related Approach**  

| Related Approach  | -    | Title                                                                             |
|:-----------------:|-----:| --------------------------------------------------------------------------------- |
| Maier et al.      | 2014 | Online passive learning of timed automata for cyber-physical production systems   |

### 2.9 Weighted Automata ###

Weighted finite automata (WFA) are finite automata whose transitions and states are augmented with some weights, elements of a semiring. A WFA induces a function over strings. The value it assigns to an input string is the semiring sum of the weights of all paths labeled with that string, where the weight of a path is obtained by taking the semiring product of the weights of its constituent transitions, as well as those of its origin and destination states.  

**Preliminaries**

- Mohri M. Weighted Finite-State Transducer Algorithms. An Overview[M]// Formal Languages and Applications. Springer Berlin Heidelberg, 2004:551-563.  
- Mohri M. Weighted Automata Algorithms[M]// Handbook of Weighted Automata. 2009:213-254.

**Related Approach**  

| Related Approach  | -    | Title                                                                                  |
|:-----------------:|-----:| -------------------------------------------------------------------------------------- |
| Balle et al.      | 2015 | Learning Weighted Automata                                                             |

### 2.10 Hybrid Automata ###

In automata theory, a hybrid automaton (plural: hybrid automata or hybrid automatons) is a mathematical model for precisely describing systems in which digital computational processes interact with analog physical processes. A hybrid automaton is a finite state machine with a finite set of continuous variables whose values are described by a set of ordinary differential equations. This combined specification of discrete and continuous behaviors enables dynamic systems that comprise both digital and analog components to be modeled and analyzed.

An *Alur-Henzinger hybrid* H comprises the following components:  
- A finite set X = {x_1, ..., x_n} of real-numbered variables. The number n is called the dimension of H. Let dot(X) be the set {dot(x_1), . . . , dot(x_n)} of dotted variables that represent first derivatives during continuous change, and let X' be the set {x'_1, ..., x'_n} of primed variables that represent values at the conclusion of discrete change.  
- A finite multidigraph (V, E). The vertices in V are called control modes. The edges in E are called control switches.  
- Three vertex labeling functions *init*, *inv*, and *flow* that assign to each control mode v ∈ V three predicates. Each initial condition *init*(v) is a predicate whose free variables are from X. Each invariant condition *inv*(v) is a predicate whose free variables are from X. Each flow condition *flow*(v) is a predicate whose free variables are from X∪dot(X).

So this is a labeled multidigraph.  
- An edge labeling function jump that assigns to each control switch e ∈ E a predicate. Each jump condition *jump*(e) is a predicate whose free variables are from X∪X'.  
- A finite set Σ of events, and an edge labeling function event: E → Σ that assigns to each control switch an event.  

**Preliminaries**

- Henzinger T A. The Theory of Hybrid Automata[M]// Verification of Digital and Hybrid Systems. Springer Berlin Heidelberg, 2000:278-292.

**Related Approach**  

| Related Approach  | -    | Title                                                                |
|:-----------------:|-----:| -------------------------------------------------------------------- |
| Medhat et al.     | 2015 | A framework for mining hybrid automata from input/output traces      |


### 2.11 Symbolic Automata (Sigma3 **TBD**) ###
***The following content comes from [http://pages.cs.wisc.edu/~loris/symbolicautomata.html](http://pages.cs.wisc.edu/~loris/symbolicautomata.html)***

Classic automata theory builds on the assumption that the alphabet is finite. Unfortunately, practical applications such as XML processing and program trace analysis use values for individual symbols that are typically drawn from an infinite domain. Even when the alphabet is finite, classic automata may sometimes be a bad choice: for example a deterministic finite automata modelling a language over the UTF16 alphabet requires 2^16 transitions out of each state!

##### What are Symbolic Automata and Transducers?#####
Symbolic Finite Automata (SFAs) are finite state automata in which the alphabet is given by a Boolean algebra that may have an infinite domain, and transitions are labeled with first-order predicates over such algebra. For example a symbolic automaton (shown on the right) can define the following property:

OddG1 = {l | l is a list of odd numbers with length greater than 1}

In order for SFAs to be closed under Boolean operations and preserve decidability of equivalence, it should be decidable to check whether predicates in the algebra are satisfiable. In the example above predicates are expressed in Presburger arithmetic which is indeed a decidable theory closed under Boolean operations. Symbolic Finite Transducers (SFTs) extend SFAs to output lists. In a SFT transitions, upon reading an input symbol, can compute an output that is expressed as a function of the input being read. Such a function has to belong to the underlying alphabet theory. Many variants of SFAs and SFTs have been proposed, and this page tries to keep up with such extensions.

##### How do they relate to classic Automata? #####
Symbolic Finite Automata are strictly more expressive than deterministic finite automata. Despite this fact, Symbolic Finite Automata are closed under Boolean operations and admit decidable equivalence. In general for large alphabets Symbolic Automata outperforms their classic counterpart. In fact even complex regular expressions over UTF16 can be analyzed using symbolic automata.

##### References #####
We recommend reading ****[this paper](http://pages.cs.wisc.edu/~loris/papers/cav17-tutorial)**** to get started. You can also watch ****[this talk](https://www.youtube.com/watch?v=ca9IF-7nSOA)****. The purpose of this page is to keep track of the latest results related to this topic. Email me (loris at cs.wisc.edu) with comments and/or suggested additions.

##### Decision Problems and Closure Properties #####
- Symbolic Finite State Transducers: Algorithms and Applications, N, Bjorner, P. Hooimeijer, B. Livshits, D. Molnar, M. Veanes, POPL12 ****[*[PDF]*](http://research.microsoft.com/pubs/151730/paper.pdf)****
- Minimization of Symbolic Automata, L. D'Antoni, M. Veanes, POPL14 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/popl14.pdf)****
- A Symbolic Decision Procedure for Symbolic Alternating Finite Automata, L. D'Antoni, Z. Kincaid, F. Wang, MFPS XXXIII ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/mfps17)****
- Forward Bisimulations for Nondeterministic Symbolic Finite Automata, L. D'Antoni, M. Veanes, TACAS 17 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/tacas17bisimulations.pdf)****
- The power of symbolic automata and transducers, L. D'Antoni, M. Veanes, TACAS 17 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/cav17-tutorial)****

##### Extensions to Trees and Nested Words #####
- Symbolic Tree Transducers, M. Veanes, N. Bjorner, PSI11, WARNING! Theorem 1 is wrong. ****[*[PDF]*](http://research.microsoft.com/apps/pubs/default.aspx?id=179252)****
- Forward and Backward Application of Symbolic Tree Transducers, Z. Fulop, H. Vogler, Acta Informatica ****[*[PDF]*](http://link.springer.com/article/10.1007%2Fs00236-014-0197-7)****
- Fast: A Transducer-Based Language for Tree Manipulation, L. D'Antoni, M. Veanes, B. Livshits, D. Molnar TOPLAS ****[*[PDF]*](http://dl.acm.org/citation.cfm?id=2791292)****
- Symbolic Visibyly Pushdown Automata, L. D'Antoni, R. Alur, CAV14 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/cav14.pdf)****
- Minimization of Symbolic Tree Automata, L. D'Antoni, M. Veanes, LICS16 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/lics16.pdf)****

##### Other Extensions #####
- Extended Symbolic Finite Automata and Transducers, L. D'Antoni, and M. Veanes, FMSD 15 ****[*[Link]*](http://link.springer.com/article/10.1007/s10703-015-0233-4?wt_mc=email.event.1.SEM.ArticleAuthorOnlineFirst)****
- A Theory of Synchronous Relational Interfaces (Section 6), S. Tripakis, B. Lickly, T. A. Henzinger, E. A. Lee, TOPLAS11 ****[*[PDF]*](http://www.eecs.berkeley.edu/~stavros/papers/acm-toplas.pdf)****
- Monadic second-order logic on finite sequences L. D'Antoni, M. Veanes POPL 17
- Abstract Symbolic Automata: Mixed syntactic/semantic similarity analysis of executables M. Dalla Preda, R. Giacobazzi, A. Lakhotia, I. Mastroieni POPL 17

##### Learning #####
Sigma\* : Symbolic Learning of Input-Output Specifications, M. Botinkan, D. Babic, POPL13 ****[*[PDF]*](http://www.domagoj-babic.com/uploads/Pubs/Popl13sigma/popl13sigma.pdf)****
Learning Regular Languages over Large Alphabets, O. Maler, I. E. Mens, TACAS14 ****[*[PDF]*](http://www-verimag.imag.fr/~maler/Papers/learn-large.pdf)****
Learning Symbolic Automata, S. Drews and L. D'Antoni, TACAS 17 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/tacas17learning.pdf)****

##### Applications #####

- Symbolic Automata Constraint Solving, M. Veanes, N. Bjorner, L. de Moura, LPAR10 ****[*[PDF]*](http://download.springer.com/static/pdf/713/chp%253A10.1007%252F978-3-642-16242-8_45.pdf?auth66=1394736291_44fb1047626c49f823aa38a171576f5f&ext=.pdf)****
- From Sequential Extended Regular Expressions to NFA with Symbolic Labels, Alessandro Cimatti, Sergio Mover, Marco Roveri, Stefano Tonetta, CIAA10 ****[*[PDF]*](https://es-static.fbk.eu/people/mover/paper/CIAA10/CimattiMoverTonettaRoveri_CIAA2010.pdf)****
- Symbolic Automata: The Toolkit, M. Veanes, N. Bjorner, TACAS11 ****[*[PDF]*](http://research.microsoft.com/pubs/157565/symAutTACAS12.pdf)****
- Fast and Precise Sanitizer Analysis with BEK, P. Hooimeijer, B. Livshits, D. Molnar, P. Saxena, M. Veanes, USENIX Security'11 ****[*[PDF]*](http://research.microsoft.com/pubs/148159/paperUSENIXSEC11.pdf)****
- Static Analysis of String Encoders and Decoders, L. D'Antoni, M. Veanes,, VMCAI13 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/vmcai13.html)****
- Applications of Symbolic Finite Automata, M. Veanes, CIAA13 ****[*[PDF]*](http://research.microsoft.com/pubs/196314/ciaa13.pdf)****
- Data-Parallel String-Manipulating Programs, M. Veanes, D. Molnar, T. Mytkowicz, B. Livshits POPL 2015 ****[*[PDF]*](http://research.microsoft.com/apps/pubs/default.aspx?id=231563)****
- DReX: A Declarative Language for Efficiently Evaluating Regular String Transformations, R. Alur, L. D'Antoni, M. Raghothaman, POPL 2015 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/popl15drex.pdf)****
- Automatic Program Inversion using Symbolic Transducers, Q. Hu and L. D'Antoni, PLDI 2017 ****[*[PDF]*](http://pages.cs.wisc.edu/~loris/papers/pldi17inversion.pdf)****

##### Tools based on Symbolic Automata and Transducers #####
- symbolicautomata: a Java symbolic automata library. L. D'Antoni ****[*[Link]*](https://github.com/lorisdanto/symbolicautomata)****
- Microsoft Automata Library: a library for symbolic automata. M. Veanes, N. Bjorner ****[*[Link]*](http://research.microsoft.com/en-us/projects/automata/)****
- Rex: a command line tool that generates matching strings for .NET regexes. M. Veanes ****[*[Link]*](http://rise4fun.com/rex/tutorial/guide)****
- Bek: a transducer-based language for string manipulation. M. Veanes, P. Hooimeijer, B. Livshits, D. Molnar, N. Bjorner ****[*[Link]*](http://rise4fun.com/bek/tutorial/guide)****
- Bex: a transducer-based language for analysis of string encoders and decoders. L. D'Antoni, M. Veanes ****[*[Link]*](http://rise4fun.com/bex/tutorial/guide)****
- Fast: a transducer-based Language for analysis of tree manipulating programs. L. D'Antoni, M. Veanes, B. Livshits, D. Molnar ****[*[Link]*](http://rise4fun.com/fast/tutorial/guide)****
- Mona: A solver for Monadic Second-Order logic. Aarhus University ****[*[Link]*](http://www.brics.dk/mona/)****

### 2.12 Others ###

**Related Approach**  

| Related Approach  | -    | Title                                                                | Automata Type            |
|:-----------------:|-----:| -------------------------------------------------------------------- |:-------------------------|
| Grinchtein et al. | 2008 | Learning of Event-Recording Automata                                 | DERA                     |
| Ipate et al.      | 2012 | Learning finite cover automata from queries                          | finite cover automata    |
| Isberner et al.   | 2015 | Foundations of Active Automata Learning: An Algorithmic Perspective  | Visibly Pushdown Automata|

## 3. Approach ##

| -                   | -     | Approach                         | Automata Type         | Title                            |
|:-------------------:|------:| -------------------------------- |:---------------------:|:--------------------------------:|
| Angluins et al.     | 1987  | Angluin's L* Algorithm           | DFA                   | Learning regular sets from queries and counterexamples |
| Oncina et al.       | 1992  | RPNI Algorithm                   | NFA                   | Inferring Regular Languages in Polynomial Updated Time |
| Rivest and Schapire | 1993  | Rivest & Schapire's Algorithm    | DFA                   | Inference of Finite Automata Using Homing Sequences |
| Kearns and Vazirani | 1994  | Kearns & Vazirani's Algorithm    | DFA                   | An introduction to computational learning theory |
| Maler and Pnueli    | 1995  | Lω Algorithm                     | ω-regular sets        | On the learnability of infinitary regular sets |
| Dupont et al.       | 1996  | RPNI2 Algorithm                  | NFA                   | Incremental regular inference |
| parekh et al.       | 1997  | ID and IID algorithm             | DFA                   | A polynomial time incremental algorithm for regular grammar inference |
| Denis et al.        | 2001  | DeLeTe2 Algorithm                | DFA                   | Learning regular languages using RFSAs |
| Bongard et al.      | 2005  | Estimation-Exploration Algorithm | DFA                   | Active Coevolutionary Learning of Deterministic Finite Automata |
| Farzan et al.       | 2008  | L* based for Büchi automaton     | ω set, Büchi automata | Extending Automated Compositional Verification to the Full Class of Omega-Regular Languages |
| Grinchtein et al.   | 2008  | TL* Algorithm                    | DERA                  | Learning of Event-Recording Automata |
| Shahbaz et al.      | 2009  | LM* and LM+ Algorithm            | Mealy Machines        | Inferring Mealy Machines |
| Benedikt et al.     | 2009  | NL* Algorithm                    | NFA                   | Angluin-Style Learning of NFA |
| Aarts et al.        | 2010  | IOA Algorithm                    | Mealy Machines        | Learning I/O Automata |
| Steffen et al.      | 2011  | DHC and LM* Algorithm            | Mealy Machines        | Introduction to Active Automata Learning from a Practical Perspective |
| Howar et al.        | 2012  | RAL Algorithm                    | Register Automata     | Inferring Canonical Register Automata |
| Ipate et al.        | 2012  | Ll Algorithm                     | finite cover automata | Learning finite cover automata from queries |
| Isberner et al.     | 2014  | The TTT Algorithm                | DFA                   | The TTT Algorithm: A Redundancy-Free Approach to Active Automata Learning |
| Heerdt et al.       | 2014  | Heerdt's Algorithm               | Mealy Machines        | Efficient Inference of Mealy Machines |
| Cassel et al.       | 2014  | SL* Algorithm                    | EFSM, RA              | Active learning for extended finite state machines |
| Angluin et al.      | 2014  | FDFAs-based Algorithm            | Regular ω-languages   | Learning Regular Omega Languages |
| Aarts et al.        | 2015  | A Mapper-Based Algorithm         | Register Automata     | Learning Register Automata with Fresh Value Generation |
| Volpato et al.      | 2015  | LearnLTS Algorithm               | DFA                   | Approximate Active Learning of Nondeterministic Input Output Transition Systems |
| Medhat et al.       | 2015  | Medhat's Algorithm               | Hybrid Automata       | A framework for mining hybrid automata from input/output traces |
| Isberner et al.     | 2015  | learning VPA                     | Visibly Pushdown A    | Foundations of Active Automata Learning: An Algorithmic Perspective |
| Balle et al.        | 2015  | learning WFAS                    | weighted Automata     | Learning Weighted Automata |
| Georgios et al.     | 2016  | MooreMI algorithm                | Moore Machines        | Learning Moore Machines from Input-Output Traces |
| Moerman et al.      | 2017  | νL* and νNL* Algorithm           | Nominal Automata      | Learning nominal automata |
| Moerman et al.      | 2017  | Product L* Algorithm             | Moore Machines        | Learning Product Automata |
| Li et al.           | 2017  | A Tree-based Algorithm for BAs   | Büchi automaton, FDFA | A Novel Learning Algorithm for Büchi Automata Based on Family of DFAs and Classification Trees |

### 3.1 Angluin's L* Algorithm [*[PDF]*](https://people.eecs.berkeley.edu/~dawnsong/teaching/s10/papers/angluin87.pdf) ###
Angluins et al. [@Angluin1987Learning] published a seminal paper in which she showed that finite automata can be learned using the so-called membership and equivalence queries. Even though faster algorithms have been proposed since then, the most efficient learning algorithms that are being used today all follow Angluin’s approach of a minimally adequate teacher (MAT).

Angluin's L* algorithm is an active learning algorithm for learning a DFA from a minimally adequate teacher with knowledge of a regular language. It defines two types of queries to gather information about the System Under Learning (SUL):

- Membership Queries (MQs) are traces of symbols from a predefined alphabet of inputs of the SUL. The learning algorithm will construct such input traces, execute these on the SUL and capture system output. From the gathered information a hypothesis model is generated.
- Equivalence Queries (EQs) compare the produced hypotheses with the target system. If the model is not accurate, a counterexample will be provided revealing a difference between the current hypothesis and the SUL. Evaluating counterexamples the learning algorithm will produce refined hypothesis models using additional MQs. Once no counterexample can be produced the learning procedure has produced an accurate model and can be stopped.

The L* algorithm of Angluin is able to learn DFAs by asking a polynomial number of membership and equivalence queries. The algorithm maintains a hypothesized DFA and refines or accepts it according to the teacher’s responses to its queries.

### 3.2 RPNI Algorithm (NFAs) [*[PDF]*](https://www.researchgate.net/publication/239643375_Inferring_regular_languages_in_polynomial_update_time) ###
Oncina et al. [@Oncina1992INFERRING] present a RPNI Algorithm. This algorithm is described such that, given a set of positive data and a set of negative data of an unknown regular language, obtains a Deterministic Finite Automaton consistent with the data. The update time of this algorithm is O(p3n) where p is the sum of the lengths of all the strings constituting the positive data and n is the sum of the lengths of those corresponding to the negative data. Moreover, this algorithm identifies any regular language in the limit.

### 3.3 Rivest & Schapire's Algorithm [*[PDF]*](http://theory.lcs.mit.edu/~rivest/RivestSchapire-InferenceOfFiniteAutomataUsingHomingSequences-STOC89.pdf) ###
Rivest and Schapire [@Rivest1993Inference]present new algorithms for inferring an unknown finite-state automaton from its input/output behavior, even in the absence of a means of resetting the machine to a start state.

Different from L\* , they found that adding a single suffix was sufficient, and that this suffix could be determined using a binary search.

### 3.4 Kearns & Vazirani's Algorithm ###
Kearns and Vazirani [@Kearns1994An] were the first to employ a discrimination tree. They replaced the observation table of the L* algorithm by the so-called discrimination trees, which are basically decision trees for determining equivalence of states.

Discrimination trees (DT) were first used in an active learning context by Kearns and Vazirani. They replaced the observation table used in previous algorithms: whereas an observation table requires to pose a membership query for every pair (u, v) ∈ Sp × D, a DT is redundancy-free in the sense that only MQs that contribute to the distinction of states have to be performed.

### 3.5 Lω Algorithm (ω-regular sets) [*[PDF]*](http://www-verimag.imag.fr/PEOPLE/Oded.Maler/Papers/learn.pdf) ###
Maler and Pnueli [@Maler1995On] extend the automaton synthesis paradigm to infinitary languages, that is, to subsets of the set Σ ω of all infinite sequences over some alphabet Σ . Their main result is a polynomial algorithm for learning a sub-class of the ω-regular sets from membership queries and counter-examples based on the framework suggested by Angluin for learning regular subsets of Σ\*.

However, this work suggested adding all suffixes of the counterexample to the table, which is inefficient.

### 3.6 RPNI2 Algorithm (NFAs) [*[PDF]*](https://www.researchgate.net/publication/225119373_Incremental_regular_inference) ###
Dupont et al. [@Dupont1996Incremental] extend the characterization of the search space of regular inference [DMV94] to sequential presentations of learning data. They propose the RPNI2 algorithm, an incremental extension of the RPNI algorithm. They study the convergence and complexities of both algorithms from a theoretical and practical point of view. These results are assessed on the Feldman task.

### 3.7 ID and IID algorithm [*[PDF]*](http://sebastian.doc.gold.ac.uk/papers/Language_Learning/PolyIncRegGram_TR97-03.pdf) ###
Parekh et al. [@parekh1997polynomial] present an efficient incremental algorithm for learning regular grammars from labeled examples and membership queries. This algorithm is an extension of Angluin's ID procedure to an incremental framework. The learning algorithm is intermittently provided with labeled examples and has access to a knowledgeable teacher capable of answering membership queries. Based on the observed examples and the teacher's responses to membership queries, the learner constructs a deterministic finite automaton (DFA) with which all examples observed thus far are consistent. When additional examples are observed, the learner modifies this DFA suitably to encompass the information provided by the new examples.

### 3.8 DeLeTe2 Algorithm [*[PDF]*](https://core.ac.uk/download/pdf/82264249.pdf) ###
Denis et al. [Denis2001Learning] design a new learning algorithm, DeLeTe2, based on the search of inclusion relations between residual languages, which produces a RFSA and have both good theoretical properties and good experimental performances.

Residual languages are important and natural components of regular languages. Most approaches in grammatical inference rely on this notion. Classical algorithms such as RPNI try to identify prefixes of positive learning examples which give rise to identical residual languages. Here, They study inclusion relations between residual languages. They lead experiments which show that when regular languages are randomly drawn using non deterministicrepresen tations, the number of inclusion relations is very important. They introduced in previous articles a new class of automata which is defined using the notion of residual languages: residual finite state automata (RFSA). RFSA representations of regular languages may have far less states than DFA representations. They prove that RFSA are not polynomially characterizable. However, they design a new learning algorithm, DeLeTe2, based on the search of inclusion relations between residual languages, which produces a RFSA and have both good theoretical properties and good experimental performances.


### 3.9 Estimation-Exploration Algorithm [*[PDF]*](http://cs.uvm.edu/~jbongard/papers/2005_JMLR_Bongard.pdf) ###
Bongard et al. [@Bongard2005Active] describes an active learning approach to the problem of grammatical inference, specifically the inference of deterministic finite automata (DFAs). They refer to the algorithm as the estimation-exploration algorithm (EEA). This approach differs from previous passive and active learning approaches to grammatical inference in that training data is actively proposed by the algorithm, rather than passively receiving training data from some external teacher. They show that this algorithm outperforms one version of the most powerful set of algorithms for grammatical inference, evidence driven state merging (EDSM), on randomly-generated DFAs. The performance increase is due to the fact that the EDSM algorithm only works well for DFAs with specific balances (percentage of positive labelings), while the EEA is more consistent over a wider range of balances. Based on this finding they propose a more general method for generating DFAs to be used in the development of future grammatical inference algorithms.

### 3.10 L* based Algorithm for Büchi automaton (Büchi automaton, ω-regular language) [*[PDF]*](http://www.cs.utoronto.ca/~azadeh/research/papers/learning.pdf) ###
Farzan et al. [@Farzan2008Extending] extend the automaton synthesis paradigm for the infinitary languages by presenting an algorithm to learn an arbitrary regular set of infinite sequences (an ω-regular language) over an alphabet Σ. Our main result is an algorithm to learn a nondeterministic Büchi automaton that recognizes an unknown ω-regular language. This is done by learning a unique projection of it on Σ* using the framework suggested by Angluin for learning regular subsets of Σ*.

### 3.11 TL\* Algorithm (DERAs) [*[PDF]*](http://www.it.uu.se/research/publications/reports/2008-013/2008-013-nc.pdf) ###
Grinchtein et al. [@Grinchtein2008Learning] [@Grinchtein2006Inference] presented a technique for learning timed systems that can be represented as event-recording automata. By considering the restricted class of event-deterministic automata, they can uniquely represent the automaton by a regular language of guarded words, and the learning algorithm can identify states by access strings that are untimed sequences of actions. This allows them to adapt existing algorithms for learning regular languages to the timed setting. The main additional work is to learn the guards under which individual actions will be accepted. The constructed automaton has a form of zone graph, which, in general, can be doubly exponentially larger than a minimal DERA representing the same language, but for many practical systems the zone graph construction does not lead to a severe explosion, as exploited by tools for timed automata verification [BDM+98, BLL+96]. They also present another algorithm for learning event-deterministic automata, which uses NP-hard procedure to construct smallest automaton which accepts timed language to be learned. Without the restriction of event-determinism, the problem of learning guards is significantly less tractable. They present an algorithm that learns general DERA. The drawback of the algorithm that it constructs a DERA in the form of a region graph, and, hence it has explosion in the number of states and transitions. Thus, it would be interesting to develop an algorithm for learning DERA which has better complexity than region graph based algorithm.

### 3.12 LM* and LM+ Algorithm (Mealy Machines) [*[PDF]*](https://link.springer.com/content/pdf/10.1007%2F978-3-642-05089-3_14.pdf) ###
Shahbaz et al. [@Shahbaz2009Inferring] have presented two algorithms for inferring Mealy machines, namely LM* and LM+. Their improvement in the algorithm for learning Mealy machines is inspired by Rivest & Schapire’s idea. 

The algorithm LM* is a straightforward adaptation from the algorithm L* . The algorithm LM+ is their proposal that contains a new method for processing counterexamples. The complexity calculations of the two algorithms shows that LM+ has a gain on the number of output queries over LM*.

### 3.13 NL* Algorithm (NFAs) [*[PDF]*](http://www.lsv.fr/Publis/PAPERS/PDF/BHKL-ijcai09.pdf) ###
Benedikt et al. [@Benedikt2009Angluin] introduces NL\* , a learning algorithm for inferring non-deterministic finite-state automata using membership and equivalence queries. More specifically, residual finite-state automata (RFSA) are learned similar as in Angluin’s popular L\* algorithm, which however learns deterministic finite-state automata (DFA). As RFSA can be exponentially more succinct than DFA, RFSA are the preferable choice for many learning applications. Their experiments carried out with implementation shows that this theoretical benefit is indeed the standard case in practice. Moreover, NL\* typically needs less membership and equivalence queries, especially for large automata, than the corresponding algorithm L\* for learning DFA. Thus, NL\* clearly outperforms L\*. 

However, this work suggested adding all suffixes of the counterexample to the table, which is inefficient.

### 3.14 IOA Algorithm (Mealy Machines for I/O Automata) [*[PDF]*](http://www.mbsd.cs.ru.nl/publications/papers/fvaan/LearningIOAs/paper.pdf) ###
Aarts et al. [Aarts2010Learning] have presented an approach for active learning of deterministic and output determined I/O automata. By eliminating the restriction from Mealy machines that inputs and outputs have to alternate, They have extended the class of models that can be learned. Their approach has been implemented on top of the LearnLib tool and has been applied successfully to three case studies. A new idea introduced in this paper is to use interface automata to focus the learning process to interesting/relevant parts of the behavior. Both in the passport and the SIP case study, the use of interface automata greatly reduced the number of queries. The efficiency of their learning approach can be improved by integrating this notion of interface automata within LearnLib: in this way it will be possible to further reduce the number of membership queries.

### 3.15 DHC and LM* Algorithm (Mealy Machines for realistic reactive systems) [*[PDF]*](http://www.falkhowar.de/papers/SFM2011-Introduction-to-Active-Automata-Learning-from-a-Practical-Perspective.pdf) ###
Steffen et al. [@Steffen2011Introduction] give an introduction to active learning of Mealy machines, an automata model particularly suited for modeling the behavior of realistic reactive systems.

### 3.16 RAL Algorithm (Register Automata) [*[PDF]*](http://www.falkhowar.de/papers/VMCAI2012-Inferring-Canonical-Register-Automata.pdf)###
Howar et al. [@Howar2012Inferring] [*[PDF]*](http://www.falkhowar.de/papers/VMCAI2012-Inferring-Canonical-Register-Automata.pdf) and Merten et al. [Merten2012Demonstrating] [*[PDF]*](http://www.falkhowar.de/papers/TACAS2012-Demonstrating-Learning-of-Register-Automata.pdf) present an extension of active automata learning to register automata, an automaton model which is capable of expressing the influence of data on control flow. Register automata operate on an infinite data domain, whose values can be assigned to registers and compared for equality. Their active learning algorithm is unique in that it directly infers the effect of data values on control flow as part of the learning process. This effect is expressed by means of registers and guarded transitions in the resulting register automata models. The application of their algorithm to a small example indicates the impact of learning register automata models: Not only are the inferred models much more expressive than finite state machines, but the prototype implementation also drastically outperforms the classic L* algorithm, even when exploiting optimal data abstraction and symmetry reduction.

### 3.17 Ll Algorithm (finite cover automata) [*[PDF]*](http://www.ifsoft.ro/~florentin.ipate/publications/JCSS%202011%20Learning%20finite%20cover%20automata%20from%20queries.pdf)###
Ipate et al. [@Ipate2012Learning] presents an algorithm, called Ll, for learning finite cover automata. As the size of a minimal cover automaton of a finite language may be much smaller than the size of the minimal automaton that accepts the language, the application of the Ll algorithm can provide substantial savings over existing automata inference methods.

### 3.18 The TTT Algorithm [*[PDF]*](http://learnlib.de/wp-content/uploads/2013/05/ttt.pdf) ###
Isberner et al. [@Isberner2014TTT] present TTT, a algorithm for actively inferring DFA. TTT is an active automata learning algorithm which stores the essential data in three tree-like data structures: a spanning tree defining unique access sequences, embedded into the hypothesis’ transition graph, a discrimination tree for distinguishing states, and a discriminator trie for storing the suffixclosed set of discriminators. This leads to an extremely compact representation, as it strips all the information down to the essentials for learning.

The TTT algorithm typically generates more intermediate hypotheses than the L* algorithm. This suggests that the number of input symbols used in membership queries alone may not be an appropriate metric for comparing learning algorithms: they also need to take into account the number of test queries required to implement equivalence queries. The total number of input symbols in membership and test queries appears to be a sensible metric to compare learning approaches in practice.

### 3.19 Heerdt's Algorithm (Mealy Machines) [*[PDF]*](http://www.cs.ru.nl/bachelorscripties/2014/Gerco_van_Heerdt___4167503___Efficient_Inference_of_Mealy_Machines.pdf) ###
Heerdt et al. [@Heerdt2014Efficient] have presented a reconstruction of the algorithms by Angluin and Rivest and Schapire adapted to Mealy machines, along with a full formal correctness proof. Furthermore, they have shown that equivalence queries are not required for non-minimal hypotheses, since a counterexample can be found in the observation table whenever the hypothesis is not minimal. In particular, this overcomes problems induced by the counterexample processing method of Rivest and Schapire . Their solution enables the direct use of standard conformance testing methods for equivalence query approximations while retaining the efficiency of the algorithm.

### 3.20 SL* Algorithm (EFSM, Register Automata) ###
- Learning Extended Finite State Machines [*[PDF]*](http://user.it.uu.se/~bengt/Papers/Full/sefm14.pdf)
- Learning Component Behavior from Tests : Theory and Algorithms for Automata with Data [*[PDF]*](http://uu.diva-portal.org/smash/get/diva2:865360/FULLTEXT01.pdf)
- Active Learning for Extended Finite State Machines [*[PDF]*](http://www.it.uu.se/research/publications/reports/2015-032/2015-032-nc.pdf)

Cassel et al. [Cassel2014Learning] present an active learning algorithm for inferring extended finite state machines (EFSM)s, combining data flow and control behavior.

Cassel et al. [Cassel2015Learning] have presented work in two main areas: automata theory, and algorithms for automata learning. They outline their contributions and conclusions for each area separately, followed by a section on future work.

Cassel et al. [@Cassel2016Active] present a black-box active learning algorithm for inferring extended finite state machines (EFSM)s by dynamic black-box analysis. EFSMs can be used to model both data flow and control behavior of software and hardware components. Different dialects of EFSMs are widely used in tools for modelbased software development, verification, and testing. This algorithm infers a class of EFSMs called register automata. Register automata have a finite control structure, extended with variables (registers), assignments, and guards. Their algorithm is parameterized on a particular theory, i.e., a set of operations and tests on the data domain that can be used in guards.

The key to this learning technique is a novel learning model based on so-called tree queries. The learning algorithm uses tree queries to infer symbolic data constraints on parameters, e.g., sequence numbers, time stamps, identifiers, or even simple arithmetic. They describe sufficient conditions for the properties that the symbolic constraints provided by a tree query in general must have to be usable in their learning model. They also show that, under these conditions, their framework induces a generalization of the classical Nerode equivalence and canonical automata construction to the symbolic setting. They have evaluated their  algorithm in a black-box scenario, where tree queries are realized through (black-box) testing. Their case studies include connection establishment in TCP and a priority queue from the Java Class Library.

### 3.21 FDFAs-based Algorithm (Regular ω-languages) [*[PDF]*](http://www.seas.upenn.edu/~fisman/documents/AF_ALT14.pdf) ###
Angluin et al. [Angluin2014Learning] provide an algorithm for learning an unknown regular set of infinite words, using membership and equivalence queries. Three variations of the algorithm learn three different canonical representations of omega regular languages, using the notion of families of dfas. One is of size similar to L$, a dfa representation recently learned using L*. The second is based on the syntactic forc, introduced in. The third is introduced herein. We show that the second can be exponentially smaller than the first, and the third is at most as large as the first two, with up to a quadratic saving with respect to the second.

### 3.22 A Mapper-Based Algorithm (Register Automata) [*[PDF]*](http://www.cs.ru.nl/~pfiteraubrostean/publications/2015-ICTAC.pdf)###
Aarts et al. [@Aarts2015Learning] have presented a mapper-based algorithm for active learning of  register automata that may generate fresh output values. Their algorithm uses counterexample-guided abstraction refinement to automatically construct a component which maps (in a history dependent manner) the large set of actions of an implementation into a small set of actions that can be handled by a Mealy machine learner. The class of register automata that is handled by their algorithm extends previous definitions since it allows for the generation of fresh output values. This feature is crucial in many real-world systems (e.g. servers that generate identifiers, passwords or sequence numbers). They have implemented their active learning algorithm in the Tomte tool.

### 3.23 LearnLTS Algorithm [*[PDF]*](https://journal.ub.tu-berlin.de/eceasst/article/download/1008/1006.pdf) ###

Volpato et al. [@Volpato2015Approximate] presented an algorithm for learning nondeterministic input-output transition systems by using an active learning L* style approach. The observation table has been modified to handle nondeterminism and unknown behaviour. They defined two different hypotheses, that can be derived from the modified observation table, which are able to describe the unknown behaviour in two different ways. They also adapted the properties that the table must satisfy for successfully inferring such hypotheses. The hypotheses are an under and an over-approximation of the SUL according to a newly defined relation ioco(S,E,T). They uncoupled the membership and equivalence queries, used in classic L\*, by following a learning process based on preciseness of the observation table: the learning stops, always with an ioco(S,E,T) conforming model, when the table is considered precise enough, otherwise some actions can be taken to increase its preciseness. Thus, a conformance test, analogue of the equivalence query, is not used directly in the learning process, but only as a mechanism to increase the preciseness. Stopping without reaching an isomorphism of the system under learning, contrary to L⋆, allows to obtain a valid, conforming, but approximate model which can be used to start (regression) testing.

### 3.24 Medhat's Algorithm (Hybrid Automata) [*[PDF]*](https://uwaterloo.ca/embedded-software-group/sites/ca.embedded-software-group/files/uploads/files/emsoft-trace-mining.pdf) ###
Medhat et al. [@Medhat2015A] have presented a general framework for inference of hybrid automata models from black box system implementations by mining their input/output traces. The framework outlines an iterative process for the inference and refinement of hybrid automata. They presented specific techniques in they case studies for the purpose of demonstration but the framework is general enough to allow replacement/improvements of these techniques. They introduced two case studies that demonstrate the applicability of the approach (and also some limitations). The results are highly encouraging and they believe would spur more work on hybrid automata inference.

### 3.25 learning VPA (Visibly Pushdown Automata) [*[PDF]*](https://eldorado.tu-dortmund.de/bitstream/2003/34282/1/Dissertation.pdf) ###
Isberner et al. [@Isberner2015Foundations] developed a model learning algorithm for visibly pushdown automata (VPAs), a restricted class of pushdown automata. This result is in a sense orthogonal to the results on learning register automata: using register automata learning, a stack with a finite capacity storing values from an infinite domain can be learned, whereas using VPA learning it is possible to learn a stack with unbounded capacity storing data values from a finite domain.

### 3.26 learning WFAS [*[PDF]*](https://cs.nyu.edu/~mohri/pub/cai.pdf) ###
Balle et al. [@Balle2015Learning] presented a detailed survey of modern algorithms for learning WFAs. They highlighted the key role played by the notion of Hankel matrix and its properties in the design of these learning algorithms which are designed for different scenarios. These properties and the algorithms they described could inspire other variants of these algorithms as well as other algorithms.

### 3.27 MooreMI Algorithm (Moore Machines) [*[PDF]*](http://people.eecs.berkeley.edu/~stavros/papers/fm2016.pdf) [*[PPT]*](http://research.cs.aalto.fi/cl/clday16/slides/giantamidis.pdf) ###
Giantamidis et al. [@Giantamidis2016Learning] formalized the problem of learning Moore machines for input-output traces and developed three algorithms to solve this problem. They showed that the most advanced of these algorithms, MooreMI, has desirable theoretical properties: in particular it satisfies the characteristic sample requirement and achieves identification in the limit. They also compared the algorithms experimentally and showed that MooreMI is also superior in practice.

### 3.28 νL* and νNL* Algorithm (Nominal Automata) [*[PDF]*](http://www.mimuw.edu.pl/~klin/papers/popl17.pdf) ###
Moerman et al. [@Moerman2017Nominal] present an Angluin-style algorithm to learn nominal automata,
which are acceptors of languages over infinite (structured) alphabets. In this paper, staying in the realm of active learning, They extend Angluin's algorithm to a richer class of automata, nomimal automata. They introduce vL\* ,a learning algorithm for Nominal DFAs. They also introduce a variant of νL\* , which call νNL\* , where the learnt automata is non-deterministic nominal automata.

### 3.29 Product - L* Algorithm (Moore Machines) [*[PDF]*](http://xueshu.baidu.com/s?wd=paperuri%3A%2891de43e91d57c853324f41f4fd5768c5%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http%3A%2F%2Farxiv.org%2Fpdf%2F1705.02850&ie=utf-8&sc_us=14469089663250886455) ###
Moerman et al. [@Moerman2017Product] give an optimization for active learning algorithms, applicable to learning Moore machines where the output comprises several observables. These machines can be decomposed themselves by projecting on each observable, resulting in smaller components. These components can then be learnt with fewer queries. This is in particular interesting for learning software, where compositional methods are important for guaranteeing scalability.

### 3.30 A Tree-based Algorithm for FDFAs [*[PDF]*](https://link.springer.com/content/pdf/10.1007%2F978-3-662-54577-5_12.pdf) [*[PPT]*](http://www.swu-rise.net.cn/SETSS2018/download/LearningBuchiAutomataandItsApplications-Prof.LijunZhang.pdf)###
Li et al. [@Li2017A] propose a novel algorithm to learn a B¨uchi automaton from a teacher who knows an ω-regular language. The algorithm is based on learning a formalism named family of DFAs (FDFAs) recently proposed by Angluin and Fisman. The main catch is that they use a classification tree structure instead of the standard observation table structure. The worst case storage space required by our algorithm is quadratically better than the table-based algorithm proposed in. They implement the first publicly available library ROLL (Regular Omega Language Learning), which consists of all ω-regular learning algorithms available in the literature and the new algorithms proposed in this paper. Experimental results show that our tree-based algorithms have the best performance among others regarding the number of solved learning tasks.


## 4. Tools ##

### 4.1 LearnLib [*[PDF]*](https://www.researchgate.net/publication/281438717_The_Open-Source_LearnLib_A_Framework_for_Active_Automata_Learning) [*[PPT]*](https://learnlib.de/wp-content/uploads/2013/08/learnlib.pdf)
Web page:
 
- [https://learnlib.de/](https://learnlib.de/) 
- [http://ls5-www.cs.tu-dortmund.de/projects/learnlib/index.php](http://ls5-www.cs.tu-dortmund.de/projects/learnlib/index.php)

Papers:

- LearnLib: A Library for Automata Learning and Experimentation [@Raffelt2006LearnLib] [*[PDF]*](https://www.researchgate.net/publication/221115203_LearnLib_A_Library_for_Automata_Learning_and_Experimentation)
- The Open-Source LearnLib: A Framework for Active Automata Learning [@Isberner2015The] [*[PDF]*](https://www.researchgate.net/publication/281438717_The_Open-Source_LearnLib_A_Framework_for_Active_Automata_Learning)

LearnLib is a library for active automata learning. The current, open-source version of LearnLib was completely rewritten from scratch, incorporating the lessons learned from the decade-spanning development process of the previous versions of LearnLib. Like its immediate predecessor, the open-source LearnLib is written in Java to enable a high degree of flexibility and extensibility, while at the same time providing a performance that allows for large-scale applications. Additionally, LearnLib provides facilities for visualizing the progress of learning algorithms in detail, thus complementing its applicability in research and industrial contexts with an educational aspect.

Currently, the following learning algorithms are supported:

- Angluin’s L* (extended, configurable implementation for DFA and Mealy machines)
- Direct Hypothesis Construction (Mealy)
- Maler/Pnueli (DFA, Mealy)
- Kearns/Vazirani (DFA, Mealy)
- Rivest/Schapire (DFA, Mealy)
- TTT (DFA, Mealy)
- NL* (NFA)

| FEATURE | OPEN-SOURCE LEARNLIB | OLD LEARNLIB (PUBLIC RELEASE) | OLD LEARNLIB (INTERNAL) |
|:-----------------------------------------:|:-------:|:------:|:----------:|
| TTT algorithm	                            | √       | X      | X          |
| Wp-method                                 | √       | X      | X          |
| Random walk           	                | √       | X      | X          |
| Reuse filter	                            | √       | X      | √          |
| Register Automata learning                | X       | X      | √          |
| Generic design                            | √       | X      | X          |
| Graphical modeling tool (LearnLib studio)	| X       | √      | √          |


### 4.2 Next Generation LearnLib (NGLL) [*[PDF]*](http://www.falkhowar.de/papers/TACAS2011-Next-Generation-LearnLib.pdf) ###

Papers:
   
- Next generation learnlib [@Merten2011Next] [*[PDF]*](http://www.falkhowar.de/papers/TACAS2011-Next-Generation-LearnLib.pdf)
- Model-Driven Active Automata Learning with LearnLib Studio [@Bauer2014Model] [*[PDF]*](https://link.springer.com/content/pdf/10.1007%2F978-3-319-51641-7_8.pdf)

The Next Generation LearnLib (NGLL) is a framework for model-based construction of dedicated learning solutions on the basis of extensible component libraries, which comprise various methods and tools to deal with realistic systems including test harnesses, reset mechanisms and abstraction/refinement techniques. Its construction style allows application experts to control, adapt, and evaluate complex learning processes with minimal programming expertise.

Bauer et al. [@Bauer2014Model]  present their reboot of LearnLib Studio, formerly being a part of the Next Generation LearnLib (NGLL) framework for modelbased construction of automata learning solutions. The new version of LearnLib Studio is a from-scratch re-implementation, which is based on an improved open-source realization of LearnLib as well as their latest version of the jABC framework (jABC4 ) for model-driven, service-oriented development of applications with recently added support for type-safe higher-order process modeling. Their all new version of LearnLib Studio provides an easy way to enable even users who do not necessarily have programming expertise to use and extend dedicated learning solutions with minimal manual effort. They illustrate the tool by applying automata learning to a concrete web service following the Representational State Transfer (REST) paradigm.

### 4.3 Libalf [*[PDF]*](http://libalf.informatik.rwth-aachen.de/files/libalf09-extendedAbstract.pdf) ###
Web page: [http://libalf.informatik.rwth-aachen.de/index.php?page=home](http://libalf.informatik.rwth-aachen.de/)

Papers:

- libalf: the automata learning framework [@Bollig2010libalf] [*[PDF]*](http://www.lsv.ens-cachan.fr/Publis/PAPERS/PDF/BKKLNP-cav10.pdf)

The libalf library is a comprehensive, open-source library for learning finite-state automata covering various well-known learning techniques (such as Angluin's L*, Biermann's learning approach, and RPNI), as well as novel learning algorithms (e.g. for NFA and visibly one-counter automata).

libalf is highly flexible and allows for facilely interchanging learning algorithms and combining domain-specific features in a plug-and-play fashion. Its modular design and its implementation in C++ make it the ideal platform for adding and engineering further, efficient learning algorithms for new target models (e.g., Büchi automata, timed automata, or probabilistic automata).

Currently, the following learning algorithms are supported:

| Algorithm                                           | offline | online | target model |
|:---------------------------------------------------:|:-------:|:------:|:------------:|
| Angluin's L*	                                      | √       | X      | DFA          |
| L* (adding counter-examples to columns)	          | √       | X      | DFA          |
| Kearns / Vazirani           	                      | √       | X      | DFA          |
| Rivest / Schapire	                                  | √       | X      | DFA          |
| NL*	                                              | √       | X      | NFA          |
| Regular positive negative inference (RPNI)          | X       | √      | DFA          |
| DeLeTe2	                                          | X       | √      | NFA          |
| Biermann & Feldman's algorithm                      | X       | √      | NFA          |
| Biermann & Feldman's algorithm (using SAT-solving)  | X	 	| √      | DFA          |

### 4.4 RALT ###

Papers: 

- Reverse Engineering Enhanced State Models of Black Box Software Components to support Integration Testing [@Shahbaz2008Reverse] [*[PDF]*](http://lig-membres.imag.fr/muzammil/documents/thesis/thesis.pdf)

### 4.5 Tomte ###
Web page: [http://tomte.cs.ru.nl/](http://tomte.cs.ru.nl/)

Papers:

- Automata Learning through Counterexample Guided Abstraction Refinement [@Aarts2012Automata] [*[PDF]*](http://www.sws.cs.ru.nl/publications/papers/fvaan/CEGAR12/FM.pdf)
- Tomte: bridging the gap between active learning and real-world systems [@Aarts2014Tomte] [*[PDF]*](http://repository.ubn.ru.nl/bitstream/handle/2066/130428/130428.pdf?sequence=1)
- Learning Register Automata with Fresh Value Generation [@Aarts2015Learning] [*[PDF]*](http://www.cs.ru.nl/~pfiteraubrostean/publications/2015-ICTAC.pdf)

Tomte is a tool that fully automatically constructs abstractions for automata learning. Usually, a component implementing the learning algorithm (the learner) is directly connected to the SUT (the teacher). By observing how the SUT responds to queries sent by the learner, a model of the behavior of the SUT can be constructed. This is not enough for learning models of realistic software components which, due to the presence of program variables and data parameters in messages, typically have much larger state spaces.

**Comparison of LearnLib:**

- Algorithms fro Inferring Register Automata, A Comparison of Existing Approaches [Aarts2014Algorithms] [*[PDF]*](https://pdfs.semanticscholar.org/7edc/91b7df02515e045c348ba4224c49ef77765c.pdf?_ga=2.228503645.1112651840.1517208804-57204035.1516950992)

### 4.6 RALib ###
Papers: 

- RALib: A LearnLib extension for inferring EFSMs [@cassel2015ralib] [*[PDF]*](http://www.it.uu.se/katalog/socas172/thesis/difts-final.pdf)

cassel et al. [@cassel2015ralib] have presented RALib, an extension to the LearnLib framework for automata learning. RALib contains a stable implementation of the SL* algorithm, along with additional features and optimizations aimed at increasing performance and at making SL* more useful in realistic scenarios. Also included are tools for directly inferring Java classes, as well as models with input and output.

### 4.7 ROLL ###
Web page: [http://iscasmc.ios.ac.cn/roll/doku.php](http://iscasmc.ios.ac.cn/roll/doku.php)

Papers:

- **A Novel Learning Algorithm for B¨uchi Automata based on Family of DFAs and Classification Trees** [@Li2017A] [*[PDF]*](https://link.springer.com/content/pdf/10.1007%2F978-3-662-54577-5_12.pdf)


ROLL is a Library of learning algorithms for ω-regular languages. It consists of all ω-regular learning algorithms available in the literature, namely

the learning algorithm for FDFAs

- the algorithm in [5] learns three canonical FDFAs using observation tables, which is based on results in [3],
- the algorithm in [6] learns three canonical FDFAs using classification trees;

and the learning algorithm for Büchi automata

- the algorithm in [4] learns a Büchi automaton by combining L* algorithm [1] and results in [2],
- the algorithm in [6] learns the Büchi automata via learning three canonical FDFAs.

new features:

- Interactive mode for education purpose.
- Büchi automata complementation based on learning [7].
- Büchi automata inclusion testing based on word sampling and learning.
- PAC-learning for Büchi automata based on Monte-Carlo word sampling (our improved version of [8]).
- Hanoi omega-automata format.

The ROLL library is implemented in JAVA. Its DFA operations are delegated to the dk.brics.automaton package. We use RABIT tool to check the equivalence of two Büchi automata.

[1] Dana Angluin. “Learning regular sets from queries and counterexamples.” Information and computation 75.2 (1987): 87-106.  
[2] Hugues Calbrix, Maurice Nivat, and Andreas Podelski. “Ultimately periodic words of rational ω-languages.” In MFPS. Springer Berlin Heidelberg, 1993: 554-566.  
[3] Oded Maler and Ludwig Staiger. “On syntactic congruences for ω-languages.” In STACS. Springer Berlin Heidelberg, 1993: 586-594.  
[4] Azadeh Farzan, Yu-Fang Chen, Edmund M. Clarke, Yih-Kuen Tsay, Bow-Yaw Wang. “Extending automated compositional verification to the full class of omega-regular languages.” In TACAS. Springer Berlin Heidelberg, 2008: 2-17.  
[5] Dana Angluin, and Dana Fisman. “Learning regular omega languages.” In ALT. Springer International Publishing, 2014: 125-139.  
[6] Yong Li, Yu-Fang Chen, Lijun Zhang, and Depeng Liu. “A Novel Learning Algorithm for Büchi Automata based on Family of DFAs and Classification Trees.” In TACAS. Springer Berlin Heidelberg, 2017: 208-226. preprint  
[7] Yong Li, Andrea Turrini, Lijun Zhang and Sven Schewe. “Learning to Complement Büchi Automata.” In VMCAI. Springer, Cham, 2018:313-335.  
[8] Radu Grosu, Scott A. Smolka. “Monte carlo model checking.” In TACAS. Springer-Verlag Berlin, Heidelberg, 2005:271-286.

### 4.8 Scikit-SpLearn ###

Web page: [http://pageperso.lif.univ-mrs.fr/~remi.eyraud/scikit-splearn/](http://pageperso.lif.univ-mrs.fr/~remi.eyraud/scikit-splearn/)


Papers:

- **Scikit-SpLearn : a toolbox for the spectral learning of weighted automata compatible with scikit-learn** [arrivault2017sp2learn] [*[PDF]*](https://hal.archives-ouvertes.fr/hal-01399412/document)


Scikit-SpLearn is a python toolbox implementing spectral learning algorithms for weighted automata. The toolbox is compliant with the well-known scikit-learn machine learning platform. In particular, all data tools of sk-learn, like cross-validation and gridsearch, can be used with scikit-splearn.

### 4.9 Symbolicautomata ###

Web page: [https://github.com/lorisdanto/symbolicautomata](https://github.com/lorisdanto/symbolicautomata)

Symbolicautomata is an efficient automata library that allows you to represent large (or infinite) alphabets succinctly.


## 5. Application ##

|                                               | Algorithms            |Learning Tool            | Application                          |
|:----------------------------------------------|:----------------------|:------------------------|:-------------------------------------|
| Cobleigh et al. 2003 [@Cobleigh2003Learning]  | L*                    | --                      | Compositional Verification           |
| Margaria et al. 2004 [@Margaria2004Efficient] | L* , L* for Mealy     | --                      | Legacy Software                      |
| Chaki et al. 2005 [@Chaki2005Automated]       | LT for DTA            | --                      | verification                         |
| Alur et al. 2008 [@alur2005symbolic]			| L*                    | --                      | Symbolic compositional verification  |
| Raffelt et al. 2007 [@Raffelt2007Dynamic]     | --                    | LearnLib                | Dynamic Testing                      |
| Chen et al. 2009[@Chen2009Learning]           | L-seq                 | --                      | Compositional Verification           |
| Aarts et al. 2010 [@Aarts2010Generating]      | L*                    | LearnLib                | Protocol (SIP and TCP)               |
| Meinke et al. 2011 [@Meinke2011Incremental]   | IKL algorithm (new)   | --                      | Learning Kripke Structures           |
| Feng et al. 2011 [@Feng2011Automated]         | Adapted L* algorithm  | --                      | Compositional Verification           |
| Aarts et al. 2012 [@Aarts2012Learning]        | adapted L* for Mealy  | LearnLib, Tomte         | Protocol Conformance Testing         |
| Aarts et al. 2013 [@Aarts2013Formal]          | adapted L* for Mealy  | LearnLib                | SmartCard                            |
| Feng et al. 2013 [@Feng2013Case]              | --                    | LBTest(newly implements)| Learning-based testing               |
| Neubauer et al. 2013 [@Neubauer2013Active]         | LM*                   | LearnLib                | evolving behavior of Application     |
| Aarts et al. 2014 [@Aarts2014Improving]       | adapted L* for Mealy  | LearnLib, Tomte         | Protocol Conformance Testing         |
| Chalupar et al. 2014 [@Chalupar2014Automated] | adapted L* for Mealy  | LearnLib                | SmartCard                            |
| Fiter et al. 2014 [@Fiter2014Learning]        | Improved L* for Mealy | LearnLib, Tomte         | TCP Network Protocol                 |
| Tijssen et al. 2014 [@Tijssen2014Automatic]   | L* for Mealy          | LearnLib                | SSH implementations                  |
| Shahbaz et al. 2014 [@Shahbaz2014Analysis]    | LM+                   | RALT(newly implements)  | testing black-box system             |
| Lin et al. 2014 [@Lin2014Learning]            | TL* Algorithm         | --                      | Verification For Timed System        |
| Xiao et al. 2014 [@Xiao2014TzuYu]             | L* with Lazy Alphabet | TzuYu(newly implements) | Learn Stateful Typestates            |
| Ruiter et al. 2015 [@Ruiter2015Protocol]      | L*                    | LearnLib                | TLS Protocol state fuzzing           |
| Smeenk et al. 2015 [@Smeenk2015Applying]      | Lee & Yannakakis'     | LearnLib                | Embedded Control Software            |
| Ipate et al. 2015 [@Ipate2015Model]           | Ll                    | --                      | Testing                              |
| Meller et al. 2015 [@Meller2015Learning]           | L*                    |                         | Model Checking of UML Systems        |
| He et al. 2015 [@He2015Leveraging]            | L* based for MTBDD’s  | --                      | Verification                         |
| Fiter et al. 2016 [@Fiter2016Combining]       | L* for Mealy          | LearnLib                | TCP Implementations                  |
| Schuts et al. 2016 [@Schuts2016Refactoring]   | L* , TTT              | LearnLib                | Legacy Software                      |
| Chen et al. 2017 [@Chen2017PAC]                    | various learning A    | libalf                  | PAC verification and model synthesis |
| Weiss et al. 2017 [@Weiss2017Extracting]      | L*                    | --                      | RNN                                  |
| Aichernig et al. 2017 [@Aichernig2017Learning]| improved L* Mealy     | LearnLib                | Mutation Testing                     |
| Tappler et al. 2017 [@Tappler2017Model]       | TTT                   | LearnLib                | Testing IoT Communication            |

### 5.1 Potocol ###
- Generating Models of Infinite-State Communication Protocols Using Regular Inference with Abstraction [@Aarts2010Generating] [*[PDF]*](http://www.sws.cs.ru.nl/publications/papers/fvaan/AJUV/main.pdf)
- Learning and Testing the Bounded Retransmission Protocol [@Aarts2012Learning] [*[PDF]*](http://www.mbsd.cs.ru.nl/publications/papers/fvaan/BRP12/BRP.pdf)
- Improving active Mealy machine learning for protocol conformance testing [@Aarts2014Improving] [*[PDF]*](http://www.ita.cs.ru.nl/publications/papers/fvaan/BRPfull/paper.pdf)
- Learning Fragments of the TCP Network Protocol [@Fiter2014Learning] [*[PDF]*](http://www.sws.cs.ru.nl/publications/papers/fvaan/TCP/main.pdf)
- Automatic modeling of SSH implementations with state machine learning algorithms [@Tijssen2014Automatic] [*[PDF]*](http://www.cs.ru.nl/bachelorscripties/2014/Max_Tijssen___4116925___Automatic_modeling_of_SSH_implementations_with_state_machine_learning_algorithms.pdf)
- Protocol state fuzzing of TLS implementations [@Ruiter2015Protocol] [*[PDF]*](http://www.cs.kun.nl/E.Poll/papers/tls_fuzzing.pdf)
- Combining Model Learning and Model Checking to Analyze TCP Implementations [@Fiter2016Combining] [*[PDF]*](http://repository.ubn.ru.nl/bitstream/handle/2066/159606/159606.pdf?sequence=1)

Aarts et al. [@Aarts2010Generating] developed a mathematical theory of such intermediate abstractions, with links to predicate abstraction and abstract interpretation.

Aarts et al. [@Aarts2012Learning] [@Aarts2014Improving] show how active learning can be used to establish the correctness of protocol implementation *I* relative to a given reference implementation *R*, using a well-known industrial case study from the verification literature, the bounded retransmission protocol. Using active learning, they learn a model *MR* of reference implementation *R*, which serves as input for a model based testing tool that checks conformance of implementation *I* to *MR*. In addition, they also explore an alternative approach in which they learn a model *MI* of implementation *I*, which is compared to model MR using an equivalence checker. Their work uses a unique combination of software tools for model construction (Uppaal), active learning (LearnLib, Tomte), model-based testing (JTorX, TorXakis) and verification (CADP, MRMC). They show how these tools can be used for learning models of and revealing errors in implementations, present the new notion of a conformance oracle, and demonstrate how conformance oracles can be used to speed up conformance checking.

Fiterău-Broştean et al. [@Fiter2014Learning] apply automata learning techniques to learn fragments of the TCP network protocol by observing its external behaviour. They show that different implementations of TCP in Windows 8 and Ubuntu induce different automata models, thus allowing for fingerprinting of these implementations. In order to infer their models they use the notion of a mapper component introduced by Aarts, Jonsson and Uijen, which abstracts the large number of possible TCP packets into a limited number of abstract actions that can be handled by the regular inference tool LearnLib. Inspection of the learned models reveals that both Windows 8 and Ubuntu 13.10 violate RFC 793.

Tijssen et al. [@Tijssen2014Automatic] note that a protocol like SSH is specified in RFCs which state how it should behave under different circumstances. Not only are these RFCs not complete or totally unambiguous, but there is also no guarantee that a implementation actually follows them. This research presents a way to automatically extract models from the transport layer of different SSH server implementations by using a learning algorithm. This learning algorithm will produce a model of a finite state machine which shows how well the implementation follows the RFC standard and possibly discover other unexpected behaviour. They modelled three different server implementations in this way and found only one out of three following the RFC standard. Besides this result they also uncovered some unique behaviour
for each server.

De Ruiter and Poll [@Ruiter2015Protocol] analyzed both server- and client-side implementations of the TLS protocol with a test harness that supported several key exchange algorithms and the option of client certificate authentication. They showed that model learning (or protocol state fuzzing, as they call it) can catch an interesting class of implementation flaws that is apparently common in security protocol implementations: in three out of nine tested TLS implementations new security flaws were found. For the Java Secure Socket Extension, for instance, a model was learned for Java version 1.8.0.25. The authors observed that the model contained two paths leading to the exchange of application data: the regular TLS protocol run and another unexpected run. By exploiting this behavior, an attack was possible in which both the client and the server application would think they were talking on a secure connection, where in reality anyone on the line could read the client’s data and tamper with it. A fix was released as part of a critical security update, and by learning a model of JSSE version 1.8.0.31, the authors were able to confirm that indeed the problem was solved. Due to a manually constructed abstraction/mapper, the learned Mealy machines were all quite small, with 6–16 states. As the analysis of different TLS implementations resulted in  different and unique Mealy machines for each one, model learning could also be used for fingerprinting TLS implementations.

Fiter et al. [@Fiter2016Combining] combined model learning and model checking in a case study involving Linux, Windows, and FreeBSD implementations of TCP servers and clients. Model learning was used to infer models of different components and then model checking was applied to fully explore what may happen when these components (for example a Linux client and a Windows server) interact. The case study revealed several instances in which TCP implementations do not conform to their RFC specification.

##### 5.1.0 Special topic: State machine inference for security protocols #####
***The following content comes from [http://www.cs.ru.nl/~joeri/StateMachineInference.html](http://www.cs.ru.nl/~joeri/StateMachineInference.html)***

This section tries to give a overview of experiments with using **state machine inference** (aka **active automata learning**) to analyse implementations of standard security protocols.

##### 5.1.1 Background #####
Implementing a security protocol usually involves implementing a **finite state machine** (aka a Deterministic Finite Automata or **DFA**) to correctly handle different protocol flows. If the happy flow is not implemented correctly then you quickly notice, as the implementation simply will not work, but logical flaws in handling non-standard protocol flows can be harder to spot. Such flaws can introduce security vulnerabilities, as has for instance been the case for TLS (see ****[here](https://mitls.org/pages/attacks/SMACK)**** and ****[here](http://www.cs.ru.nl/~joeri/papers/usenix15.pdf)****).

State machine inference is a technique to automatically infer the state machine from an implementation by means of black-box testing. This means we obtain **formal models for free**! Inspecting the state machines obtained - either manually or using a model-checker - can reveal flaws in implementations. It may also reveal differences between implementations of the same protocol that allow fingerprinting of particular implementations. Such differences are also an indication that the specifications may be unclear or ambiguous.

Often protocol state machines are not well-specified or left largely implicit in specifications. Paying explicit attention to these state machines can be seen as adhering to the principles of ****[LangSec](http://langsec.org/)**** (**language-theoretic security**), as discussed in more detail ****[here](https://doi.org/10.1109/SPW.2015.32)****. Active state machine learning can also be regarded as a form of **fuzzing**, where you do not fuzz individual messages, but rather sequences of messages.

##### 5.1.2 Further reading #####
The idea of state machine learning goes back to ****[the work by Angluin](https://www.sciencedirect.com/science/article/pii/0890540187900526?via%3Dihub)**** from the 1980s. A standard software library for inferring state machines which we have used extensively in our own work is LearnLib. One of the first tools to apply the idea of state machine learning to security protocols was ****[Prospex](https://doi.org/10.1109/SP.2009.14)****, which specifically targetted botnet protocols, with the aim to automatically reverse engineer these. Overviews of automated tools for protocol reverse engineering, not just using active state machine learning but also other techniques, is given in this ****[survey paper](https://doi.org/10.1145/2840724)**** or ****[this one](https://link.springer.com/article/10.1007/s11416-016-0289-8)****.

##### 5.1.3 Overview of protocols analysed #####
Below an overview of research into the use of using active learning to analyse implementations of standardised security protocols. (So we exclude e.g. botnet protocols in this overview.) This wiki provides additional information about some of the experiments listed below. ****[This wiki](http://automata.cs.ru.nl/Overview#Allbenchmarks)**** also includes information n some case studies with protocols that are not security-related, such as ****[TCP](http://automata.cs.ru.nl/BenchmarkTCP)****, ****[MQTT](http://automata.cs.ru.nl/BenchmarkMQTT)****, and ****[SIP](http://http//automata.cs.ru.nl/BenchmarkSIP)****.

##### 5.1.4 Network security protocols #####
- **TLS**
	- Joeri de Ruiter, A Tale of the OpenSSL State Machine: a Large-scale Black-box Analysis, NordSec 2016, LNCS volume 10014, pp. 169-184, Springer, 2016. [[*Models*](http://www.cs.ru.nl/~joeri/download/nordsec16_models.tgz), [*Scripts*](http://www.cs.ru.nl/~joeri/download/nordsec16_scripts.tgz)] ****[*[PDF]*](http://www.cs.ru.nl/~joeri/papers/nordsec16.pdf)****
	- Joeri de Ruiter and Erik Poll, Protocol state fuzzing of TLS implementations, USENIX Security, USENIX, 2015. [[*Code*](https://github.com/jderuiter/statelearner), [*Models*](http://www.cs.ru.nl/~joeri/download/usenix15.zip)]  ****[*[PDF]*](http://www.cs.ru.nl/~joeri/papers/usenix15.pdf)****

- **SSH**
	- Paul Fiterau-Brostean, Frits Vaandrager, Erik Poll, Joeri de Ruiter, Toon Lenaerts and Patrick Verleg, Model Learning and Model Checking of SSH Implementations, Paul Fiterau-Brostean, Frits Vaandrager, Erik Poll, Joeri de Ruiter, Toon Lenaerts and Patrick Verleg, SPIN 2017, p. 142-151, ACM, 2017. [[*Models*](https://gitlab.science.ru.nl/pfiteraubrostean/Learning-SSH-Paper/tree/master/models)] ****[*[PDF]*](http://dl.acm.org/citation.cfm?id=3092289)****
	- Toon Lenaerts, Improving-protocol state fuzzing of SSH, Bachelor thesis, Radboud University, 2017. ****[*[PDF]*](http://www.cs.ru.nl/bachelorscripties/2017/Toon_Lenaerts___4321219___Improving-protocol-state-fuzzing-of-SSH.pdf)****
	- Patrick Verleg, Inferring SSH state machines using protocol state fuzzing, Master thesis, Radboud University Nijmegen, 2016. ****[*[PDF]*](http://www.ru.nl/publish/pages/769526/z07_patrick_verleg.pdf)****

- **OpenVPN**
	- Lesly-Ann Daniel, Inferring OpenVPN State Machines Using Protocol State Fuzzing, Internship report, University of Rennes 1 and ENS Rennes, 2017 [[*Code*](https://framagit.org/leslyann/statelearner)] ****[*[PDF]*](http://www.cs.ru.nl/~joeri/StateMachineInference.html)**** 

- **IPSEC**
	- Bart Veldhuizen, Automated state machine learning of IPsec implementations, Bachelor thesis, Radboud University Nijmegen, 2017. ****[*[PDF]*](http://www.cs.ru.nl/bachelorscripties/2017/Bart_Veldhuizen___4492765___Automated_state_machine_learning_of_IPsec_implementations.pdf)****

##### 5.1.5 Smartcards and banking tokens #####

- **EMV (EuroPay-Mastercard-Visa)**
	- Fides Aarts, Joeri de Ruiter and Erik Poll, Formal Models of Bank Cards for Free, SECTEST 2013, pp. 461-468, IEEE, 2013. [[*Code & models*](http://www.cs.ru.nl/~joeri/download/sectest13.zip)] (More info and models [here](http://automata.cs.ru.nl/BenchmarkBankcard).) ****[*[PDF]*](http://www.cs.ru.nl/~joeri/papers/sectest13.pdf)****
- **EMV-based banking tokens**
	- Georg Chalupar, Stefan Peherstorfer, Erik Poll and Joeri de Ruiter, Automated Reverse Engineering using Lego, WOOT'14, USENIX, 2014. ****[*[PDF]*](http://www.cs.ru.nl/~joeri/papers/woot14.pdf)****
- **e-passport**
	- Fides Aarts, Julien Schmaltz, Frits Vaandrager, Inference and abstraction of the biometric passport, ISoLa 2010, LNCS volume 6415, pp. pp 673-686, Springer, 2010. (More info and models [here](http://automata.cs.ru.nl/BenchmarkPassport).) ****[*[PDF]*](http://www.sws.cs.ru.nl/publications/papers/fvaan/passport/)****

##### 5.1.6 Industrial Control Systems #####

- **IEC 60870-5-104**
	- Max Kerkers, Assessing the Security of IEC 60870-5-104 Implementations using Automata Learning, MSc thesis, University of Twente, 2017 [[*Source code*](https://github.com/mkerkers/mealy104)]  ****[*[PDF]*](http://essay.utwente.nl/72277/1/Kerkers_MA_EEMCS.pdf)****


### 5.2 SmartCard ###
- Formal Models of Bank Cards for Free [@Aarts2013Formal] [*[PDF]*](http://www.cs.ru.nl/E.Poll/papers/emvlearning_sectest13.pdf) [*[PPT]*](http://www.spacios.eu/sectest2013/pdfs/DeRuiter.pdf)
- Automated Reverse Engineering using Lego® [@Chalupar2014Automated] [*[PDF]*](http://www.cs.ru.nl/E.Poll/papers/legopaper_woot14_final.pdf) [*[PPT]*](http://www.usenix.org/sites/default/files/conference/protected-files/woot14_slides_chalupar.pdf)

Aarts et al. [@Aarts2013Formal] learned models of implementations of the EMV protocol suite on bank cards issued by several Dutch and German banks, on MasterCard credit cards issued by Dutch and Swedish banks, and on one UK Visa debit card. To learn the models, LearnLib performed between 855 and 1,696 membership and test queries for each card and produced models with four to eight states. All cards resulted in different models, only the applications on the Dutch cards were identical. The models learned did not reveal any security issues, although some peculiarities were noted. The authors argue that model learning would be useful as part of security evaluations.

Chalupar et al. [@Chalupar2014Automated] used model learning to reverse engineer the e.dentifier2, a smartcard reader for Internet banking. To be able to learn a model of the e.dentifier2, the authors constructed a Lego robot, controlled by a Raspberry Pi that can operate the keyboard of the reader. Controlling all this from a laptop, they then could use LearnLib to learn models of the e.dentifier2. They learned a four-state Mealy machine of one version of the e.dentifier2 that revealed the presence of a security flaw, and showed that the flaw is no longer present in a three-state model for the new version of the device.


### 5.3 Legacy software ###
- Efficient test-based model generation for legacy reactive systems [@Margaria2004Efficient] [*[PDF]*](https://www.researchgate.net/publication/4143885_Efficient_test-based_model_generation_for_legacy_reactive_systems)
- Refactoring of Legacy Software Using Model Learning and Equivalence Checking: An Industrial Experience Report [@Schuts2016Refactoring] [*[PDF]*](http://www.mbsd.cs.ru.nl/publications/papers/fvaan/SHV16/SchutsHoomanVaandrager2016.pdf) [*[PPT]*](http://www.esi.nl/innovation-support/documents/symposium-2016/3-MA_Refactoring-of-Legacy-Software.pdf)

Legacy systems have been defined as “large software systems.

Margaria et al. [@Margaria2004Efficient] were the first to point out that model learning may help to increase confidence that a legacy component and a refactored implementation have the same behavior.  They present the effects of using an efficient algorithm for behavior-based model synthesis which is specifically tailored to reactive (legacy) system behaviors. Conceptual backbone is the classical automata learning procedure L\* , which They adapt according to the considered application profile. The resulting learning procedure L*Meal , which directly synthesizes generalized Mealy automata from behavioral observations gathered via an automated test environment, drastically outperforms the classical learning algorithm for deterministic finite automata. Thus it marks a milestone towards opening industrial legacy systems to model-based test suite enhancement, test coverage analysis, and online testing.

Schuts et al. [@Schuts2016Refactoring] used model learning to support the rejuvenation of legacy embedded software in a development project at Philips. The project concerned the introduction of a new hardware component, the Power Control Component (PCC), which is used to start-up and shutdown an interventional radiology system. All computers in the system have a software component, the Power Control Service (PCS) which communicates with the PCC over an internal control network during the execution of start-up and shutdown scenarios. To deal with the new hardware of the PCC, which has a different interface, a new implementation of the PCS was needed. Since different configurations had to be supported, with old and new PCC hardware, the old and new PCS software needed to have exactly the same external behavior.

### 5.4 LBT & Testing Finite-State Machines ###
- Principles and methods of testing finite state machines-a survey [@Lee1996Principles] [*[PDF]*](https://www.researchgate.net/publication/2985061_Principles_and_Methods_of_Testing_Finite_State_Machines-a_Survey)
- Testing Finite-State Machines: State Identification and Verification [@Lee1994Testing] [*[PDF]*](http://www.it.uu.se/katalog/socas172/autol/slides3b.pdf)
- Model Generation by Moderated Regular Extrapolation [@Hagerer2002Model] [*[PDF]*](http://rd.springer.com/content/pdf/10.1007%2F3-540-45923-5_6.pdf)
- Dynamic Testing Via Automata Learning [@Raffelt2007Dynamic] [*[PDF]*](https://www.researchgate.net/publication/221471873_Dynamic_Testing_Via_Automata_Learning)
- Incremental Learning-Based Testing for Reactive Systems [@Meinke2011Incremental] [*[PDF]*](http://www.nada.kth.se/~sindhu/tap2011.pdf)
- Case Studies in Learning-based Testing [@Feng2013Case] [*[PDF]*](http://www.hats-project.eu/sites/default/files/D5.4/paper4.pdf)
- Active continuous quality control [@Neubauer2013Active] [*[PDF]*](http://delivery.acm.org/10.1145/2470000/2465469/p111-windmuller.pdf?ip=116.7.245.185&id=2465469&acc=ACTIVE%20SERVICE&key=BF85BBA5741FDC6E%2E5FBA890B628FA01E%2E4D4702B0C3E38B35%2E4D4702B0C3E38B35&__acm__=1517323085_34da67c0e1191737528013e4d22bfe17)
- Analysis and testing of black-box component-based systems by inferring partial models [@Shahbaz2014Analysis] [*[PDF]*](http://onlinelibrary.wiley.com/doi/10.1002/stvr.1491/epdf)
- Applying Automata Learning to Embedded Control Software [@Smeenk2015Applying]  [*[PDF]*](http://sws.cs.ru.nl/publications/papers/fvaan/ESM/main.pdf) 
- Model Learning and Test Generation Using Cover Automata [@Ipate2015Model] [*[PDF]*](http://www.ifsoft.ro/~florentin.ipate/publications/Model%20Learning%20and%20Test%20Generation.pdf)

Hagerer et al. [@Hagerer2002Model] use model learning on regression testing of telecommunication systems at Siemens. They  introduce regular extrapolation, a technique that provides descriptions of systems or system aspects a posteriori in a largely automatic way.

Raffelt et al. [@Raffelt2007Dynamic] presents dynamic testing, a method that exploits automata learning to systematically test (black box) systems almost without prerequisites. Based on interface descriptions, Their method successively explores the system under test (SUT), while it at the same time extrapolates a behavioral model. This is in turn used to steer the further exploration process. Due to the applied learning technique, Their method is optimal in the sense that the extrapolated models are most concise in consistently representing all the information gathered during the exploration. Using the LearnLib, Their framework for automata learning, Their method can elegantly be combined with numerous optimizations of the learning procedure, various choices of model structures, and last but not least, with the option to dynamically/interactively enlarge the alphabet underlying the learning process. All these features will be illustrated using as a case study the web application Mantis, a bug tracking system widely used in practice. They show how the dynamic testing procedure proceeds and how the behavioral models arise that concisely summarize the current testing effort. It has turned out that these models, besides steering the automatic exploration process, are ideal for user guidance and to support analyzes to improve the system understanding. 

Meinke et al. [@Meinke2011Incremental] show how the paradigm of learning-based testing (LBT) can be applied to automate specification-based black-box testing of reactive systems. Since reactive systems can be modeled as Kripke structures, They introduce an efficient incremental learning algorithm IKL for such structures. They show how an implementation of this algorithm combined with an efficient model checker such as NuSMV yields an effective learning-based testing architecture for automated test case generation (ATCG), execution and evaluation, starting from temporal logic requirements.

Feng et al. [@Feng2013Case] use model learning on testing requirements of a brake-by-wire system from Volvo Technology. They present case studies which show how the paradigm of learning-based testing (LBT) can be successfully applied to black-box requirements testing of reactive systems. For this they apply a new testing tool LBTest, which combines algorithms for incremental black-box learning of Kripke structures with model checking technology. They show how test requirements can be modeled in propositional linear temporal logic extended by finite abstract data types. They provide benchmark performance results for LBTest applied to two industrial case studies. Finally they present a first coverage study for the tool.

Neubaueret et al. [@Neubauer2013Active] use model learning on automatic testing of an online conference service of Springer Verlag. They present Active Continuous Quality Control (ACQC), a novel approach that employs incremental active automata learning technology periodically in order to infer evolving behavioral automata of complex applications accompanying the development process. This way they are able to closely monitor and steer the evolution of applications throughout their whole life-cycle with minimum manual effort.

Shahbaz et al. [@Shahbaz2014Analysis] use model learning on integration testing at France Telecom. They present an iterative approach of combining model learning and testing techniques for the formal analysis of a system of black-box components. In the approach, individual components in the system are learned as finite state machines that (partially) model the behavioural structure of the components. The learned models are then used to derive tests for refining the partial models and/or finding integration faults in the system. The approach has been applied on case studies that have produced encouraging results.

Smeenk et al. [@Smeenk2015Applying] succeeded to learn a Mealy machine model of a piece of widely used industrial control software, using an extension of the Lee & Yannakakis algorithm for adaptive distinguishing sequences. Their extension of Lee & Yannakakis’ algorithm is rather obvious, but nevertheless it appears to be new. Preliminary evidence suggests that it outperforms existing conformance testing algorithms. They are currently performing experiments in which they compare the new algorithm with other test algorithms on a number of realistic benchmarks.

Ipate et al. [@Ipate2015Model] propose an approach which, given a state-transition model of a system, constructs, in parallel, an approximate automaton model and a test suite for the system. The approximate model construction relies on a variant of Angluin’s automata learning algorithm, adapted to finite cover automata.

### 5.5 Conformance Testing ###
- Improving active Mealy machine learning for protocol conformance testing [@Aarts2014Improving] [*[PDF]*](http://www.ita.cs.ru.nl/publications/papers/fvaan/BRPfull/paper.pdf)
- Learning from Faults: Mutation Testing in Active Automata Learning [@Aichernig2017Learning] [*[PDF]*](http://www.ist.tugraz.at/aichernig/publications/papers/nfm17.pdf)

Aarts et al. [@Aarts2014Improving] show how active learning can be used to establish the correctness of protocol implementation *I* relative to a given reference implementation *R*, using a well-known industrial case study from the verification literature, the bounded retransmission protocol. Using active learning, they learn a model *MR* of reference implementation *R*, which serves as input for a model based testing tool that checks conformance of implementation *I* to *MR*. In addition, they also explore an alternative approach in which they learn a model *MI* of implementation *I*, which is compared to model MR using an equivalence checker. Their work uses a unique combination of software tools for model construction (Uppaal), active learning (LearnLib, Tomte), model-based testing (JTorX, TorXakis) and verification (CADP, MRMC). They show how these tools can be used for learning models of and revealing errors in implementations, present the new notion of a conformance oracle, and demonstrate how conformance oracles can be used to speed up conformance checking.

Aichernig et al. [@Aichernig2017Learning] address conformance testing in active automata learning. They describe a randomised conformance testing approach which they extend with fault-based test selection. To show its effectiveness they apply the approach in learning experiments and compare its performance to a well-established testing technique, the partial W-method. This evaluation demonstrates that their approach significantly reduces the cost of learning – in one experiment by a factor of more than twenty.

### 5.6 Model Checking & Black Box Checking ###
- Black Box Checking [@Peled1999Black] [*[PDF]*](https://www.cs.rice.edu/~vardi/papers/pstv99.pdf)
- Learning-Based Compositional Model Checking of Behavioral UML Systems [@Meller2015Learning] [*[PDF]*](http://www.cs.technion.ac.il/users/wwwb/cgi-bin/tr-get.cgi/2015/CS/CS-2015-05.pdf)
- Combining Model Learning and Model Checking to Analyze TCP Implementations [@Fiter2016Combining] [*[PDF]*](http://repository.ubn.ru.nl/bitstream/handle/2066/159606/159606.pdf?sequence=1)

In order to check properties of learned models, model checking can be used.

Peled et al. [@Peled1999Black] showed how model learning and model checking can be fully integrated in an approach called black box checking. The basic idea is to use a model checker as a “preprocessor” for the conformance testing tool. When the teacher receives a hypothesis from the learner, it first runs a model checker to verify if the hypothesis model satisfies all the properties from the SUL’s specification. Only if this is true the hypothesis is forwarded to the conformance tester. If one of the properties does not hold then the model checker produces a counterexample. Now there are two cases. The first possibility is that the counterexample can be reproduced on the SUL. This means they have demonstrated a bug in the SUL (or in its specification) and they stop learning. The second possibility is that the counterexample cannot be reproduced on the SUL. In this case the teacher returns the counterexample to the learner since it follows that the hypothesis is incorrect.

Peled et al. proposed black-box checking as a solution to this problem. This technique applies active automata learning to infer models of systems with unknown internal structure. They have shown that active automata learning can provide models of black-box systems to enable formal verification, this kind of learning has turned into an active area of research in formal methods. As one of the first to combine learning and formal verification, proposed to implement these queries via conformance testing. In particular, they suggested to use the conformance testing algorithm by Vasilevskii and Chow.

Meller et al. [@Meller2015Learning] presents a novel approach for applying compositional model checking of behavioral UML models, based on learning. The Unified Modeling Language (UML) is a widely accepted modeling language for embedded and safety critical systems. As such the correct behavior of systems represented as UML models is crucial. Model checking is a successful automated verification technique for checking whether a system satisfies a desired property.

Fiter et al. [@Fiter2016Combining] combined model learning and model checking in a case study involving Linux, Windows, and FreeBSD implementations of TCP servers and clients. Model learning was used to infer models of different components and then model checking was applied to fully explore what may happen when these components (for example a Linux client and a Windows server) interact. The case study revealed several instances in which TCP implementations do not conform to their RFC specification.

### 5.7 Improving Efficiency And Scalability of Verification ### 
- Learning assumptions for compositional verification [@Cobleigh2003Learning] [*[PDF]*](https://ti.arc.nasa.gov/m/profile/dimitra/publications/tacas03.pdf)
- Automated assume-guarantee reasoning for simulation conformance [@Chaki2005Automated] [*[PDF]*](http://www.contrib.andrew.cmu.edu/~schaki/old-website-2013-08-19/publications/CAV-2005.pdf)
- Symbolic Compositional Verification by Learning Assumptions [@alur2005symbolic] [*[PDF]*](http://www.personal.psu.edu/users/w/u/wun1/pubs/CAV05.pdf)
- Learning Minimal Separating DFA's for Compositional Verification [@Chen2009Learning] [*[PDF]*](http://www.cs.utoronto.ca/~azadeh/research/papers/tacas09.pdf)
- Automated Learning of Probabilistic Assumptions for Compositional Reasoning [@Feng2011Automated] [*[PDF]*](http://www.veriware.org/papers/fase11.pdf)
- Leveraging Weighted Automata in Compositional Reasoning about Concurrent Probabilistic Systems [He2015Leveraging] [*[PDF]*](http://www.iis.sinica.edu.tw/~bywang/papers/popl15.pdf)
- PAC learning-based verification and model synthesis [Chen2017PAC] [*[PDF]*](http://www.iis.sinica.edu.tw/~bywang/papers/icse16.pdf)


Cobleigh et al. [@Cobleigh2003Learning] presents a novel framework for performing assume-guarantee reasoning in an incremental and fully automated
fashion. To check a component against a property, their approach generates assumptions that the environment needs to satisfy for the property to hold. These assumptions are then discharged on the rest of the system. Assumptions are computed by a learning algorithm.

Chaki et al. [@Chaki2005Automated] address the issue of efficiently automating assume-guarantee reasoning for simulation conformance between finite state systems and specifications. They focus on a non-circular assume-guarantee proof rule, and show that there is a weakest assumption that can be represented canonically by a deterministic tree automata (DTA). They then present an algorithm LT that learns this DTA automatically in an incremental fashion, in time that is polynomial in the number of states in the equivalent minimal DTA.

Alur et al. [@alur2005symbolic] propose an automated solution for discovering assumptions based on the L∗ algorithm for active learning of regular
languages. They present a symbolic implementation of the learning algorithm, and incorporate it in the model checker NuSMV. Our experiments demonstrate significant savings in the computational requirements of symbolic model checking.

Chen et al. [@Chen2009Learning] propose an efficient learning algorithm, called L-Sep, that learns and generates a minimal separating DFA. Their algorithm has a quadratic query complexity in the product of sizes of the minimal DFA’s for the two input languages. In contrast, the most recent algorithm of Gupta et al. has an exponential query complexity in the sizes of the two DFA’s. Moreover, experimental results show that their learning algorithm significantly outperforms all existing algorithms on randomly-generated example problems. They describe how their algorithm can be adapted for automated compositional verification. 

Feng et al. [@Feng2011Automated] discuss recent developments in the area of automated compositional verification techniques for probabilistic systems. In particular, they describe techniques to automatically generate probabilistic assumptions that can be used as the basis for compositional reasoning. They do so using algorithmic learning techniques, which have already proved to be successful for the generation of assumptions for compositional verification of non-probabilistic systems. They also present recent improvements and extensions to this work and survey some of the promising potential directions for further research in this area.

He et al. [@He2015Leveraging] propose the first sound and complete learning-based compositional verification technique for probabilistic safety properties
on concurrent systems where each component is an Markov decision process. Different from previous works, weighted assumptions are introduced to attain completeness of our framework. Since weighted assumptions can be implicitly represented by multi-terminal binary decision diagrams (MTBDD’s), they give
an L* based learning algorithm for MTBDD’s to infer weighted assumptions.

Chen et al. [@Chen2017PAC] introduce a novel technique for verification and model synthesis of sequential programs. Their technique is based on learning an approximate regular model of the set of feasible paths in a program, and testing whether this model contains an incorrect behavior. Exact learning algorithms require checking equivalence between the model and the program, which is a difficult problem, in general undecidable. Their learning procedure is therefore based on the framework of probably approximately correct (PAC) learning, which uses sampling instead, and provides correctness guarantees expressed using the terms error probability and confidence. 

### 5.8 Testing IoT Communication ###
- Model-Based Testing IoT Communication via Active Automata Learning [@Tappler2017Model] [*[PDF]*](http://www.ist.tugraz.at/aichernig/publications/papers/icst17_mqtt.pdf)

Tappler et al. [@Tappler2017Model] present a learning-based approach to detecting failures in reactive systems. The technique is based on inferring models of multiple implementations of a common specification which are pair-wise cross-checked for equivalence. Any counterexample to equivalence is flagged as suspicious and has to be analysed manually. Hence, it is possible to find possible failures in a semi-automatic way without prior modelling. They show that the approach is effective by means of a case study. For this case study, they carried out experiments in which they learned models of five implementations of MQTT brokers/servers, a protocol used in the Internet of Things. Examining these models, they found several violations of the MQTT specification. All but one of the considered implementations showed faulty behaviour. In the analysis, they discuss effectiveness and also issues they faced.

### 5.9 Inferring Interface Programs of Systems At Runtime [*[PDF]*](http://eldorado.uni-dortmund.de:8080/bitstream/2003/29486/1/Dissertation.pdf) ###
Howar et al. [@Howar2012Interface] addresses the problem of inferring interface programs of systems at runtime using active automata learning techniques. Active automata learning uses a test-based and counterexample-driven approach to inferring models of black-box systems. The method has originally been introduced for finite automata (the popular L\* algorithm). Extending active learning to interface programs requires research in three directions: First, the efficiency of active learning algorithms has to be optimized to scale when dealing with data parameters. Second, techniques are needed for finding counterexamples driving the learning process in practice. Third, active learning has to be extended to richer models than Mealy machines or DFAs, capable of expressing interface programs.

The work presented in this thesis improves the state of the art in all three directions. More concretely, the contributions of this thesis are the following: first, an efficient active learning algorithm for DFAs and Mealy machines that combines the ideas of several known active learning algorithms in a non-trivial way; second, a framework for finding counterexamples in black-box scenarios, leveraging the incremental and monotonic evolution of hypothetical models characteristic of active automata learning; third, and most importantly, the technically
involved extension of the partition/refinement-based approach of active learning to interface programs.

The impact of extending active learning to interface programs becomes apparent already for small systems. They inferred a simple data structure (a nested stack of overall capacity 16) as an interface program in no more than 20 seconds, using less than 45,000 tests and only 9 counterexamples. The corresponding Mealy machine model, on the other hand, would have more than 10^9 states already in the case of a very small finite data domain of size 4 and require significantly more than 10^9 tests when being inferred using the classic L\* algorithm.

### 5.10 Program Structures And Interface Programs [*[PDF]*](https://www.researchgate.net/publication/271920241_Learning_register_automata_From_languages_to_program_structures) ###
Howar et al. [@Howar2014Learning] revisited the precursor techniques and influences, which in total span over more than a decade. A large share of this development was guided and motivated by the increasingly popular application of grammatical inference techniques in the field of software engineering. They specifically focus on a key problem to achieve practicality in this field: the adequate treatment of data values ranging over infinite domains, a major source of undecidability. Starting with the first case studies, in which data was completely abstracted away, They revisit different steps towards dealing with data explicitly at a model level: they discuss Mealy machines as a model for systems with (data) output, automated alphabet abstraction refinement techniques as a two-dimensional extension of the partition-refinement based approach of active automata learning to also inferring optimal alphabet abstractions, and Register Mealy Machines, which can be regarded as programs restricted to data-independent data processing as it is typical for protocols or interface programs. They are convinced that this development will significantly contribute to paving the road for active automata learning to become a technology of high practical importance.

### 5.11 Extracting Automata from Neural Networks [*[PDF]*](https://arxiv.org/abs/1711.09576) ###
Gail et al. [@Weiss2017Extracting] address the problem of extracting an automaton from a trained recurrent neural network (RNN). They present a novel algorithm that uses exact learning and abstract interpretation to perform efficient extraction of a minimal automaton describing the state dynamics of a given RNN. They use Angluin’s L* algorithm as a learner and the given RNN as an oracle, employing abstract interpretation of the RNN for answering equivalence queries. This technique allows automaton-extraction from the RNN while avoiding state explosion, even when the state vectors are large and fine differentiation is required between RNN states. A unique aspect of this approach is that it performs exact learning with abstraction of the teacher. In this setting, a disagreement between the teacher and the learner means that either the learner has to refine its representation (as typical in exact learning), or that the abstraction of the teacher is not precise enough and must be refined. 

They implemented the technique and show its ability to extract descriptive automata in settings where other approaches fail. Furthermore, they demonstrate the effectiveness of the method on modern and commonly used gated RNN types—multi-layer LSTM and multi-layer GRU—in contrast to previous work that were applied to the simpler Elman RNN or second-order RNNs

### 5.12 Active Automata Learning For Real-Life Applications [*[PDF]*](https://pdfs.semanticscholar.org/9cb9/74b6ece3e3fc2eab4f9cf0843bfc570df4a9.pdf) ###
Merten et al. [@Merten2013Active] discussed the development of an infrastructure for active automata learning that can drive the adoption of active automata learning techniques for real-life applications

Berg et al. [@Berg2005Insights] have implemented Angluin's L\* algorithm in a straightforward way to gain further insights to practical applicability. Furthermore, they have analyzed its performance on randomly generated as well as real-world examples. Their experiments focus on the impact of the alphabet size and the number of states on the needed number of membership queries. Additionally, they have implemented and analyzed an optimized version for learning prefix-closed regular languages. Memory consumption is one major obstacle when they attempted to learn large examples. They see that prefix-closed languages are relatively hard to learn compared to arbitrary regular languages. The optimization, however, shows positive results.

### 5.13 Learning Communicating Automata from MSCs [*[PDF]*](http://www4.in.tum.de/~leucker/Documents/Leucker/tse09_prelim.pdf) ###
Bollig et al. [Bollig2010Learning] investigates for which classes of MSC languages CFMs can be learned, presents an optimization technique for learning partial orders, and provides substantial empirical evidence indicating the practical feasibility of the approach. This paper is concerned with bridging the gap between requirements and distributed systems. Requirements are defined as basic message sequence charts (MSCs) specifying positive and negative scenarios. Communicating finite-state machines (CFMs), i.e., finite automata that communicate via FIFO buffers, act as system realizations. The key contribution is a generalization of Angluin’s learning algorithm for synthesizing CFMs from MSCs. This approach is exact—the resulting CFM precisely accepts the set of positive scenarions and rejects all negative ones—and yields fully asynchronous implementations.

### 5.14 Automated Compositional Verification For Timed Systems [*[PDF]*](http://www-lipn.univ-paris13.fr/~andre/documents/learning-assumptions-for-compositional-verification-of-timed-systems.pdf) ###
Lin et al. [@Lin2014Learning] propose a compositional verification framework using a learning algorithm for automatic construction of timed assumptions for AGR. They prove the correctness and termination of the proposed learning-based framework, and experimental results show that our method performs significantly better than traditional monolithic timed model checking. They show that Learning algorithm for timed automata can be developed for automated compositional verification for timed systems.

### 5.15  learn stateful typestates [*[PDF]*](http://pdfs.semanticscholar.org/61d6/893aa6ba0e18e71cb81cd869b1c8e0bf93d6.pdf) ###
Xiao et al. [@Xiao2014TzuYu] propose a fully automated approach to learn stateful typestates by extending the classic active learning process to generate transition guards (i.e.,propositions on data states). The proposed approach has been implemented in a tool called TzuYu and evaluated against a number of Java classes. The evaluation results show that TzuYu is capable of learning correct stateful typestates efficiently.

### 5.15 Fuzzy Learning Automata [*[PDF]*](http://ieeexplore.ieee.org/document/7724946/) ###
Gupta et al. [@Gupta2016Applications] show that fuzzy Learning automata is used in the operation of image sharpening, detection of edge, to efficiently control traffic signal, and acknowledge normal and abnormal human behavior in real time. Another interesting application is Multi Objective Reactive Power Planning. In web page ranking, summarizing of text and car navigation system fuzzy learning automata plays an important role.


## 6. Challenges And Discussion ##

即使模型学习已经在许多地方得到成功应用，但该研究领域仍处于起步阶段。模型学习的应用具有巨大的潜力，尤其是在传统控制软件领域，但是还需要对算法和工具进行更多的研究，以将模型学习从目前的学术原型水平转变为可实际使用的技术水平，从而方便应用于大量不同类型的系统。在这里，本文将讨论一些主要的研究挑战。

### 6.1 Predicates and operations on data ###

The recent extension of model learning algorithms to register automata is a breakthrough which, potentially, makes model learning applicable to a much larger class of systems. Due to the restriction that no operations on data are allowed, the class of systems that can be described as register automata is small, and mainly consists of academic examples such as the bounded retransmission protocol and some simple data structures. However, as pointed out by Cassel et al. 2016, using SMT solving the new learning algorithms for register automata can be extended to EFSM formalisms in which guards may contain predicates such as the successor and less than relation. A prototype implementation RALib is available and we are close to the point where we can learn models of real-world protocols such as TCP, SIP, SSH, and TLS automatically, without the need to manually define abstractions. Nevertheless, our understanding of algorithms for learning EFSMs with different predicates and operations is still limited, and there are many open questions.

Isberner et al. 2015 developed a model learning algorithm for visibly pushdown automata (VPAs), a restricted class of pushdown automata proposed by Alur and Madhusudan.5 This result is in a sense orthogonal to the results on learning register automata: using register automata learning, a stack with a finite capacity storing values from an infinite domain can be learned, whereas using VPA learning it is possible to learn a stack with unbounded capacity storing data values from a finite domain. From a practical perspective it would be useful to develop a learning algorithm for a class of models that generalizes both register automata and VPAs. There are many protocols in which messages may be buffered, and we therefore need algorithms that can learn queues with unbounded capacity.

### 6.2 Beyond Mealy machines ###

在Mealy机中，单个输入始终触发单个输出。然而，在实践中，系统可以用零个或多个输出来响应输入。此外，系统的行为通常是时序相关的，并且某个输出可能仅在某些输入未能在一定量的时间内得到时才发生。因此，模型学习的实际应用经常严重受限于Mealy机缺乏表达性。例如，为了将TCP实现为Mealy机，我们必须消除基于时序的行为以及重传。目前已经有一些初步的工作将学习算法扩展到I/O自动机、事件记录自动机等其它类型的自动机，但是仍然需要大量的工作将这些想法变成实际的工具。

由于一个输入序列可能导致在不同运行中会有不同的输出事件，因此从这种意义上来说，系统通常是非确定的。然而，现有的模型学习工具只能够学习确定性的Mealy机。在实际应用中，我们有时可以通过将不同的具体输出事件抽象为单个抽象输出以消除非确定性，但在许多情况下这是不可能的。Volpato和Tretmans提出了一种对L*的扩展算法，学习非确定性I/O自动机。他们的算法能够学习非确定性SUL，并且它允许我们构造部分或近似模型。同样，还需要进行大量工作以将这些想法纳入最先进的工具，如LearnLib、libalf、RALib 或 Tomte。

### 6.3 Quality of models ###

由于模型学习算法生成的模型是通过有限数量的测试获得的，所以我们不能确保它们是正确的。然而，从实践的角度来看，我们希望能够对学习模型的质量进行定量说明，例如，断言假设高概率地近似正确。Angluin根据Valiant的PAC学习方法提出了这样的一种设定。她的想法是假设在输入字母表I上一组单词的一些（未知）概率分布。为了测试假设，无论SUL的输出结果是否和假设相一致，一致性测试器选择指定数量的输入词（这些是统计独立的事件），并检查每个词。只有当完全一致时，一致性测试仪才会向learner返回答案'yes'。如果选择一个字符串所表现出来的差异的概率最多为ε，则该假设被认为是SUL的ε近似。给定SUL的状态数量的界限以及两个常数ε和δ，Angluin的多项式算法产生模型，使得该模型是SUL的近似的概率为至少1-δ。Angluin的结果是优雅的，但在反应系统的设置中不现实，因为我们通常在输入词上没有固定的分布。（输入受 SUL 环境的控制，且此环境可能会发生改变。）

我们使用传统一致性测试，可以设计出一个测试套件，该测试套件可在给定SUL状态数量上限的情况下保证学习模型的准确性。但是这种方法也不能令人满意，因为所需要的测试序列的数量会随着SUL的状态数量呈现指数型增长。因此，挑战在于如何在Angluin方法和传统的一致性测试之间建立一个折中。系统日志通常提供了一个输入词集合的概率分布，该输入词集合可被用来作为定义某个度量标准的启动点。

### 6.4 Opening the box ###

使用黑箱模型学习技术可以有很多原因。例如，我们可能想要了解组件的行为，但是不能访问代码。或者我们可以访问代码，但没有合适的工具来分析它（例如，在旧版软件的情况下）。即使在白箱的情况下，我们可以访问代码并有强大的代码分析工具，黑箱学习也是有意义的，例如：黑箱模型可以用于生成回归测试，用于检查是标准是否一致，或作为更大的基于模型开发的系统的一部分。一个具有挑战性的研究是结合黑箱和白箱模型提取技术，利用静态分析和concolic这些白箱方法帮助回答由黑箱模型学习中，学习者提出的等价性查询。


## 7. Reference ##

    @article{Angluin1987Learning,
      title={Learning regular sets from queries and counterexamples},
      author={Angluin, Dana},
      journal={Information & Computation},
      volume={75},
      number={2},
      pages={87-106},
      year={1987},
    }

    @book{Oncina1992INFERRING,
      title={INFERRING REGULAR LANGUAGES IN POLYNOMIAL UPDATED TIME},
      author={Oncina, J. and García, P.},
      pages={49-61},
      year={1992},
    }
   
    @article{Rivest1993Inference,
      title={Inference of Finite Automata Using Homing Sequences},
      author={Rivest, Ronald L. and Schapire, Robert E.},
      journal={Information & Computation},
      volume={103},
      number={103},
      pages={51-73},
      year={1993},
    }

    @book{Kearns1994An,
      title={An introduction to computational learning theory},
      author={Kearns, Michael J. and Vazirani, Umesh V.},
      publisher={MIT Press},
      pages={44-58},
      year={1994},
    }

    @article{Lee1994Testing,
      title={Testing Finite-State Machines: State Identification and Verification},
      author={Lee, D. and Yannakakis, M.},
      journal={Computers IEEE Transactions on},
      volume={43},
      number={3},
      pages={306-320},
      year={1994},
    }

    @article{Maler1995On,
      title={On the learnability of infinitary regular sets},
      author={Maler and Oded and Pnueli and Amir},
      journal={Information and Computation},
      volume={118},
      number={2},
      pages={316-326},
      year={1995},
    }

    @article{Dupont1996Incremental,
      title={Incremental regular inference},
      author={Dupont, Pierre},
      journal={Proceedings of the Third ICGI-96},
      volume={1147},
      pages={222-237},
      year={1996},
    }

    @article{Lee1996Principles,
      title={Principles and methods of testing finite state machines-a survey},
      author={Lee, D and Yannakakis, Mihalis},
      journal={Proceedings of the IEEE},
      volume={84},
      number={8},
      pages={1090-1123},
      year={1996},
    }

    @article{parekh1997polynomial,
      title={A polynomial time incremental algorithm for regular grammar inference},
      author={Parekh, Rajesh G and Nichitiu, Codrin and Honavar, Vasant},
      year={1997}
    }

    @article{Peled1999Black,
      title={Black Box Checking},
      author={Peled, Doron and Vardi, Moshe Y. and Yannakakis, Mihalis},
      journal={Journal of Automata Languages & Combinatorics},
      volume={7},
      number={2},
      pages={225-246},
      year={1999},
    }

    @book{Denis2001Learning,
      title={Learning regular languages using RFSAs},
      author={Denis, François and Lemay, Aurélien and Terlutte, Alain},
      publisher={Springer Berlin Heidelberg},
      pages={348-363},
      year={2001},
    }

    @inproceedings{Hagerer2002Model,
      title={Model Generation by Moderated Regular Extrapolation},
      author={Hagerer, Andreas and Hungar, Hardi and Niese, Oliver and Steffen, Bernhard},
      booktitle={International Conference on Fundamental Approaches To Software Engineering},
      pages={80-95},
      year={2002},
    }

    @inproceedings{Cobleigh2003Learning,
      title={Learning assumptions for compositional verification},
      author={Cobleigh, Jamieson M and Giannakopoulou, Dimitra and Reanu, Corina S},
      booktitle={TOOLS and Algorithms for the Construction and Analysis of Systems,  International Conference, Tacas 2003, Held As},
      pages={331-346},
      year={2003},
    }
    
    @inproceedings{Margaria2004Efficient,
      title={Efficient test-based model generation for legacy reactive systems},
      author={Margaria, T. and Niese, O. and Raffelt, H. and Steffen, B.},
      booktitle={High-Level Design Validation and Test Workshop, 2004. Ninth IEEE International},
      pages={95-100},
      year={2004},
    }

    @article{Bongard2005Active,
      title={Active Coevolutionary Learning of Deterministic Finite Automata},
      author={Bongard, Josh and Lipson, Hod},
      journal={Journal of Machine Learning Research},
      volume={6},
      number={2},
      pages={1651-1678},
      year={2005},
    }

    @inproceedings{Chaki2005Automated,
      title={Automated assume-guarantee reasoning for simulation conformance},
      author={Chaki, Sagar and Clarke, Edmund and Sinha, Nishant and Thati, Prasanna},
      booktitle={International Conference on Computer Aided Verification},
      pages={534-547},
      year={2005},
    }

    @article{Berg2005Insights,
      title={Insights to Angluin's Learning},
      author={Berg, Therese and Jonsson, Bengt and Leucker, Martin and Saksena, Mayank},
      journal={Electronic Notes in Theoretical Computer Science},
      volume={118},
      pages={3-18},
      year={2005},
    }

	@inproceedings{alur2005symbolic,
	  title={Symbolic compositional verification by learning assumptions},
	  author={Alur, Rajeev and Madhusudan, Parthasarathy and Nam, Wonhong},
	  booktitle={International Conference on Computer Aided Verification},
	  pages={548--562},
	  year={2005},
	  organization={Springer}
	}

    @inproceedings{Raffelt2006LearnLib,libalf the automata learn
      title={LearnLib: a library for automata learning and experimentation},
      author={Raffelt, Harald and Steffen, Bernhard},
      booktitle={International Conference on Fundamental Approaches To Software Engineering},
      pages={377-380},
      year={2006},

    @book{Grinchtein2006Inference,
      title={Inference of Event-Recording Automata Using Timed Decision Trees},
      author={Grinchtein, Olga and Jonsson, Bengt and Pettersson, Paul},
      publisher={Springer Berlin Heidelberg},
      year={2006},
    }

    @inproceedings{Raffelt2007Dynamic,
      title={Dynamic Testing Via Automata Learning},
      author={Raffelt, Harald and Steffen, Bernhard and Margaria, Tiziana},
      booktitle={Haifa Verification Conference},
      pages={136-152},
      year={2007},
    }

    @inproceedings{Farzan2008Extending,
      title={Extending Automated Compositional Verification to the Full Class of Omega-Regular Languages},
      author={Farzan, Azadeh and Chen, Yu Fang and Clarke, Edmund M. and Tsay, Yih Kuen and Wang, Bow Yaw},
      booktitle={TOOLS and Algorithms for the Construction and Analysis of Systems,  International Conference, Tacas 2008, Held As},
      pages={2-17},
      year={2008},
    }
    
    @article{Shahbaz2008Reverse,
      title={Reverse Engineering Enhanced State Models of Black Box Software Components to support Integration Testing},
      author={Shahbaz, Muzammil and Muhammad},
      journal={PhD thesis, Laboratoire Informat. de Grenoble},
      year={2008},
     }

    @article{Grinchtein2008Learning,
      title={Learning of Event-Recording Automata},
      author={Grinchtein, Olga and Jonsson, Bengt and Leucker, Martin},
      journal={Theoretical Computer Science},
      volume={411},
      number={47},
      pages={4029-4054},
      year={2008},
    }
    
    @inproceedings{Benedikt2009Angluin,
      title={Angluin-Style Learning of NFA.},
      author={Benedikt Bollig and Peter Habermehl and Carsten Kern and Martin Leucker},
      booktitle={International Jont Conference on Artifical Intelligence},
      pages={1004-1009},
      year={2009},
    }

    @inproceedings{Shahbaz2009Inferring,
      title={Inferring Mealy Machines},
      author={Shahbaz, Muzammil and Groz, Roland},
      booktitle={World Congress on Formal Methods},
      pages={207-222},
      year={2009},
    }

    @inproceedings{Chen2009Learning,
      title={Learning Minimal Separating DFA's for Compositional Verification},
      author={Chen, Yu Fang and Farzan, Azadeh and Clarke, Edmund M. and Tsay, Yih Kuen and Wang, Bow Yaw},
      booktitle={International Conference on TOOLS and Algorithms for the Construction and Analysis of Systems: Held As},
      pages={31-45},
      year={2009},
    }

    @inproceedings{Bollig2010libalf,
      title={libalf: the automata learning framework},
      author={Bollig, Benedikt and Katoen, Joost Pieter and Kern, Carsten and Leucker, Martin and Neider, Daniel and Piegdon, David R.},
      booktitle={International Conference on Computer Aided Verification},
      pages={360-364},
      year={2010},
    }

    @article{Bollig2010Learning,
      title={Learning Communicating Automata from MSCs},
      author={Bollig, Benedikt and Katoen, Joost Pieter and Kern, Carsten and Leucker, Martin},
      journal={IEEE Transactions on Software Engineering},
      volume={36},
      number={3},
      pages={390-408},
      year={2010},
    }

    @book{Aarts2010Learning,
      title={Learning I/O Automata},
      author={Aarts, Fides and Vaandrager, Frits},
      publisher={Springer Berlin Heidelberg},
      pages={71-85},
      year={2010},
    }

    @inproceedings{Aarts2010Generating,
      title={Generating Models of Infinite-State Communication Protocols Using Regular Inference with Abstraction},
      author={Aarts, Fides and Jonsson, Bengt and Uijen, Johan},
      booktitle={IFIP International Conference on Testing Software and Systems},
      pages={188-204},
      year={2010},
    }
    
    @inproceedings{Merten2011Next,
      title={Next generation learnlib},
      author={Merten, Maik and Steffen, Bernhard and Howar, Falk and Margaria, Tiziana},
      booktitle={International Conference on TOOLS and Algorithms for the Construction and Analysis of Systems},
      pages={220-223},
      year={2011},
    }

    @book{Feng2011Automated,
      title={Automated Learning of Probabilistic Assumptions for Compositional Reasoning},
      author={Feng, Lu and Kwiatkowska, Marta and Parker, David},
      publisher={Springer Berlin Heidelberg},
      pages={2-17},
      year={2011},
    }

    @article{Steffen2011Introduction,
      title={Introduction to Active Automata Learning from a Practical Perspective},
      author={Steffen, Bernhard and Howar, Falk and Merten, Maik},
      year={2011},
    }

    @inproceedings{Meinke2011Incremental,
      title={Incremental Learning-Based Testing for Reactive Systems},
      author={Meinke, Karl and Sindhu, Muddassar A.},
      booktitle={Tests and Proofs -  International Conference, Tap 2011, Zurich, Switzerland, June 30 - July 1, 2011. Proceedings},
      pages={134-151},
      year={2011},
    }

    @article{Aarts2012Automata,
      title={Automata Learning through Counterexample Guided Abstraction Refinement},
      author={Fides Aarts and Faranak Heidarian and Harco Kuppens and Petur Olsen and Frits Vaandrager},
      journal={Lecture Notes in Computer Science},
      volume={7436},
      pages={10-27},
      year={2012},
    }

    @article{Howar2012Inferring,
      title={Inferring Canonical Register Automata},
      author={Howar, Falk and Steffen, Bernhard and Jonsson, Bengt and Cassel, Sofia},
      journal={Lecture Notes in Computer Science},
      volume={7148},
      pages={251-266},
      year={2012},
    }


    @article{Howar2012Interface,
      title={Active learning of interface programs},
      author={Howar, Falk M.},
      journal={Fakultäten},
      year={2012},
    }

    @inproceedings{Aarts2012Learning,
      title={Learning and Testing the Bounded Retransmission Protocol},
      author={Aarts, Fides and Kuppens, Harco and Tretmans, Jan and Vaandrager, Frits and Verwer, Sicco and Heinz, Jeffrey and Higuera, Colin De La and Oates, Tim},
      pages={4-18},
      year={2012},
    }

    @book{Ipate2012Learning,
      title={Learning finite cover automata from queries},
      author={Ipate, Florentin},
      publisher={Academic Press, Inc.},
      pages={221-244},
      year={2012},
    }

    @inproceedings{Merten2012Demonstrating,
      title={Demonstrating Learning of Register Automata},
      author={Merten, Maik and Howar, Falk and Steffen, Bernhard and Cassel, Sofia and Jonsson, Bengt},
      booktitle={International Conference on TOOLS and Algorithms for the Construction and Analysis of Systems},
      pages={466-471},
      year={2012},
    }

    @article{Merten2013Active,
      title={Active automata learning for real life applications},
      author={Merten, Maik},
      journal={Fakultäten},
      year={2013},
    }

    @inproceedings{Feng2013Case,
      title={Case Studies in Learning-based Testing},
      author={Feng, Lei and Lundmark, Simon and Meinke, Karl and Niu, Fei and Sindhu, Muddassar A. and Wong, Peter Y. H.},
      booktitle={Ifip Ictss},
      pages={164-179},
      year={2013},
    }

    @inproceedings{Neubauer2013Active,
      title={Active continuous quality control},
      author={Neubauer, Johannes and Steffen, Bernhard and Howar, Falk and Bauer, Oliver},
      booktitle={International ACM Sigsoft Symposium on Component-Based Software Engineering},
      pages={111-120},
      year={2013},
    }

    @inproceedings{Aarts2013Formal,
      title={Formal Models of Bank Cards for Free},
      author={Aarts, Fides and Ruiter, Joeri De and Poll, Erik},
      booktitle={IEEE Sixth International Conference on Software Testing, Verification and Validation Workshops},
      pages={461-468},
      year={2013},
    }

    @inproceedings{Bauer2014Model,
      title={Model-Driven Active Automata Learning with LearnLib Studio},
      author={Bauer, Oliver and Neubauer, Johannes and Isberner, Malte},
      booktitle={International Symposium on Leveraging Applications of Formal Methods},
      pages={128-142},
      year={2014},
    }

    @article{Heerdt2014Efficient,
      title={Efficient Inference of Mealy Machines},
      author={Heerdt, Gerco Van},
      year={2014},
    }
    
    @inproceedings{Aarts2014Algorithms,
      title={Algorithms for Inferring Register Automata},
      author={Aarts, Fides and Howar, Falk and Kuppens, Harco and Vaandrager, Frits},
      booktitle={International Symposium On Leveraging Applications of Formal Methods, Verification and Validation},
      pages={202-219},
      year={2014},
    }

    @article{Aarts2014Tomte,
      title={Tomte : bridging the gap between active learning and real-world systems},
      author={Aarts, F. D},
      journal={Model Based System Development},
      year={2014},
    }

    @article{Aarts2014Improving,
      title={Improving active Mealy machine learning for protocol conformance testing},
      author={Aarts, Fides and Kuppens, Harco and Tretmans, Jan and Vaandrager, Frits and Verwer, Sicco},
      journal={Machine Learning},
      volume={96},
      number={1-2},
      pages={189-224},
      year={2014},
    }

    @article{Chalupar2014Automated,
      title={Automated Reverse Engineering using Lego®},
      author={Chalupar, Georg and Peherstorfer, Stefan and Poll, Erik and Ruiter, Joeri De},
      journal={Woot14 Usenix Workship on Offensive Technologies August San Diego Ca},
      pages={1-10},
      year={2014},
    }

    @inproceedings{Isberner2014TTT,
      title={The TTT Algorithm: A Redundancy-Free Approach to Active Automata Learning},
      author={Isberner, Malte and Howar, Falk and Steffen, Bernhard},
      booktitle={International Conference on Runtime Verification},
      pages={307-322},
      year={2014},
    }
    
    @article{Howar2014Learning,
      title={Learning register automata: from languages to program structures},
      author={Howar, Falk and Howar, Falk and Steffen, Bernhard},
      journal={Machine Learning},
      volume={96},
      number={1-2},
      pages={65-98},
      year={2014},
    }

    @inproceedings{Fiter2014Learning,
      title={Learning Fragments of the TCP Network Protocol},
      author={Fiterău-Broştean, Paul and Janssen, Ramon and Vaandrager, Frits},
      booktitle={International Workshop on Formal Methods for Industrial Critical Systems},
      pages={78-93},
      year={2014},
    }

    @article{Tijssen2014Automatic,
      title={Automatic modeling of SSH implementations with state machine learning algorithms},
      author={Tijssen, Max and Ruiter, Joeri De},
      year={2014},
    }

    @article{Shahbaz2014Analysis,
      title={Analysis and testing of black-box component-based systems by inferring partial models},
      author={Shahbaz, Muzammil and Groz, Roland},
      journal={Software Testing Verification & Reliability},
      volume={24},
      number={4},
      pages={253–288},
      year={2014},
    }

    @inproceedings{Cassel2014Learning,
      title={Learning Extended Finite State Machines},
      author={Cassel, Sofia and Howar, Falk and Jonsson, Bengt and Steffen, Bernhard},
      booktitle={International Conference on Software Engineering and Formal Methods},
      pages={250-264},
      year={2014},
    }

    @inproceedings{Angluin2014Learning,
      title={Learning Regular Omega Languages},
      author={Angluin, Dana and Fisman, Dana},
      booktitle={Algorithmic Learning Theory},
      pages={125-139},
      year={2014},
    }

    @inproceedings{Xiao2014TzuYu,
      title={TzuYu: Learning stateful typestates},
      author={Xiao, Hao and Sun, Jun and Liu, Yang and Lin, Shang Wei and Sun, Chengnian},
      booktitle={Ieee/acm  International Conference on Automated Software Engineering},
      pages={432-442},
      year={2014},
    }

    @article{Lin2014Learning,
      title={Learning Assumptions for CompositionalVerification of Timed Systems},
      author={Lin, Shang Wei and Étienne André and Liu, Yang and Sun, Jun and Dong, Jin Song},
      journal={IEEE Transactions on Software Engineering},
      volume={40},
      number={2},
      pages={137-153},
      year={2014},
    }

    @article{Ipate2015Model,
      title={Model Learning and Test Generation Using Cover Automata},
      author={Ipate, Florentin and Stefanescu, Alin and Dinca, Ionut},
      journal={Computer Journal},
      volume={58},
      number={5},
      year={2015},
    }

    @article{He2015Leveraging,
      title={Leveraging Weighted Automata in Compositional Reasoning about Concurrent Probabilistic Systems},
      author={He, Fei and Gao, Xiaowei and Wang, Bow Yaw and Zhang, Lijun},
      journal={Acm Sigplan Notices},
      volume={50},
      number={1},
      pages={503-514},
      year={2015},
    }

    @inproceedings{Meller2015Learning,
      title={Learning-Based Compositional Model Checking of Behavioral UML Systems},
      author={Meller, Yael and Grumberg, Orna and Yorav, Karen},
      booktitle={Revised Selected Papers of the  International Conference on Formal Aspects of Component Software},
      pages={275-293},
      year={2015},
    }

    @article{Isberner2015Foundations,
      title={Foundations of Active Automata Learning: An Algorithmic Perspective},
      author={Isberner, Malte},
      year={2015},
    }

    @inproceedings{Isberner2015The,
      title={The Open-Source LearnLib: A Framework for Active Automata Learning},
      author={Isberner, Malte and Howar, Falk and Steffen, Bernhard},
      booktitle={Computer Aided Verification},
      year={2015},
    }
    
    @article{Cassel2015Learning,
      title={Learning Component Behavior from Tests : Theory and Algorithms for Automata with Data},
      author={Cassel, Sofia},
      year={2015},
    }

	@article{Balle2015Learning,
	  title={Learning Weighted Automata},
	  author={Balle, Borja and Mohri, Mehryar},
	  year={2015},
	}

    @book{Smeenk2015Applying,
      title={Applying Automata Learning to Embedded Control Software},
      author={Smeenk, Wouter and Moerman, Joshua and Vaandrager, Frits and Jansen, David N.},
      publisher={Springer International Publishing},
      pages={67-83},
      year={2015},
    }

    @inproceedings{Medhat2015A,
      title={A framework for mining hybrid automata from input/output traces},
      author={Medhat, R and Ramesh, S and Bonakdarpour, B and Fischmeister, S},
      booktitle={International Conference on Embedded Software},
      pages={177-186},
      year={2015},
    }

    @book{Aarts2015Learning,
      title={Learning Register Automata with Fresh Value Generation},
      author={Aarts, Fides and Fiterau-Brostean, Paul and Kuppens, Harco and Vaandrager, Frits},
      publisher={Springer International Publishing},
      pages={165-183},
      year={2015},
    }

    @article{Volpato2015Approximate,
      title={Approximate Active Learning of Nondeterministic Input Output Transition Systems},
      author={Volpato, Michele and Tretmans, Jan},
      booktitle={15th InternationalWorkshop on Automated Verification of Critical Systems},
      year={2015},
    }

    @article{cassel2015ralib,
      title={RALib: A LearnLib extension for inferring EFSMs},
      author={Cassel, Sofia and Howar, Falk and Jonsson, Bengt},
      journal={DIFTS. hp://www. faculty. ece. vt. edu/chaowang/di s2015/papers/paper},
      volume={5},
      year={2015}
    }

    @inproceedings{Ruiter2015Protocol,
      title={Protocol state fuzzing of TLS implementations},
      author={Ruiter, Joeri De and Poll, Erik},
      booktitle={Usenix Conference on Security Symposium},
      pages={193-206},
      year={2015},
    }

    @book{Schuts2016Refactoring,
      title={Refactoring of Legacy Software Using Model Learning and Equivalence Checking: An Industrial Experience Report},
      author={Schuts, Mathijs and Hooman, Jozef and Vaandrager, Frits},
      publisher={Springer International Publishing},
      year={2016},
    }

    @article{Cassel2016Active,
      title={Active learning for extended finite state machines},
      author={Sofia Cassel and Falk Howar and Bengt Jonsson and Bernhard Steffen},
      journal={Formal Aspects of Computing},
      volume={28},
      number={2},
      pages={233-263},
      year={2016},
    }

    @inproceedings{Giantamidis2016Learning,
      title={Learning Moore Machines from Input-Output Traces},
      author={Giantamidis, Georgios and Tripakis, Stavros},
      booktitle={International Symposium on Formal Methods},
      pages={291-309},
      year={2016},
    }

    @inproceedings{Gupta2016Applications,
      title={Applications of fuzzy learning automata a review},
      author={Gupta, Vidushi and Aggarwal, Swati},
      booktitle={International Conference on Computing for Sustainable Global Development},
      year={2016},
    }

    @book{Fiter2016Combining,
      title={Combining Model Learning and Model Checking to Analyze TCP Implementations},
      author={Fiterău-Broştean, Paul and Janssen, Ramon and Vaandrager, Frits},
      publisher={Springer International Publishing},
      year={2016},
    }

    @article{Frits2017Model,
      title={Model Learning},
      author={Frits Vaandrager},
      journal={Communications of the ACM},
      volume={60},
      number={2},
      pages={86-95},
      year={2017},
    }

    @article{Weiss2017Extracting,
      title={Extracting Automata from Recurrent Neural Networks Using Queries and Counterexamples},
      author={Weiss, Gail and Goldberg, Yoav and Yahav, Eran},
      year={2017},
    }

    @article{Moerman2017Nominal,
      title={Learning nominal automata},
      author={Moerman, Joshua and Sammartino, Matteo and Silva, Alexandra and Klin, Bartek and Szynwelski, Micha},
      journal={Acm Sigplan Notices},
      volume={52},
      pages={613-625},
      year={2017},
    }

    @article{Moerman2017Product,
      title={Learning Product Automata},
      author={Moerman, Joshua},
      year={2017},
    }

    @book{Aichernig2017Learning,
      title={Learning from Faults: Mutation Testing in Active Automata Learning},
      author={Aichernig, Bernhard K. and Tappler, Martin},
      year={2017},
    }

    @inproceedings{Tappler2017Model,
      title={Model-Based Testing IoT Communication via Active Automata Learning},
      author={Tappler, Martin and Aichernig, Bernhard K. and Bloem, Roderick},
      booktitle={IEEE International Conference on Software Testing, Verification and Validation},
      pages={276-287},
      year={2017},
    }

    @inproceedings{Chen2017PAC,
      title={PAC learning-based verification and model synthesis},
      author={Chen, Yu Fang and Hsieh, Chiao and Lii, Tsung Ju and Tsai, Ming Hsien and Wang, Bow Yaw and Wang, Farn},
      booktitle={Ieee/acm  International Conference on Software Engineering},
      pages={714-724},
      year={2017},
    }

    @inproceedings{Li2017A,
      title={A Novel Learning Algorithm for Büchi Automata Based on Family of DFAs and Classification Trees},
      author={Li, Yong and Chen, Yu Fang and Zhang, Lijun and Liu, Depeng},
      booktitle={International Conference on Tools and Algorithms for the Construction and Analysis of Systems},
      pages={208-226},
      year={2017},
    }

	@inproceedings{arrivault2017sp2learn,
	  title={Sp2Learn: A Toolbox for the spectral learning of weighted automata},
	  author={Arrivault, Denis and Benielli, Dominique and Denis, Fran{\c{c}}ois and Eyraud, R{\'e}mi},
	  booktitle={International Conference on Grammatical Inference},
	  pages={105--119},
	  year={2017}
	}
