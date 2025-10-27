## Stichprobenraum (Ereignisraum, Ergebnisraum, ...)
- sample space
- set of **all possible outcomes** of a random experiment
- Symbol: $\Omega$
## Elementarereignis
- elementary event
- **one single outcome** from the sample space
- Symbol: $\omega \in \Omega$

| Experiment             | Sample Space                                  | Elementary event |
| ---------------------- | --------------------------------------------- | ---------------- |
| Tossing 1 coin once    | {Kopf, Zahl}                                  | "Kopf"           |
| Rolling a dice once    | $\{1, 2, \ldots , 6\}$                        | 4                |
| Rolling a dice 3 times | $\{1, 2, \ldots, 6\}^3$ (_Cartesian product_) | (1, 2, 3)        |
## Ereignis(se)
- event
- a set of outcomes ($A \subset \mathcal{A}$) == a collection of _elementary event_ 
- If $\omega \in A$ , event $A$ occurs
![[WI - Events.png]]
### Examples
### 1. Roulette
- 37 fields:
	- 18x Black = $\{19, 20, \ldots, 36\}$
	- 18x Rot = $\{1, 2, \ldots, 18\}$
	- 1x Green = $\{0\}$
- Rolling 2 times
- $\Omega = \{0, 1, 2, \ldots, 36\}$
- 2x Red: $A_1 = \{1, 2, \ldots, 18\}^2$
- first 1-12, then 13-24: $A_2 = \{1, \ldots, 12\} \bigtimes \{13,\ldots,24\}$
- only 1 black: $$A_3 = \{19, 20,\ldots,36\} \bigtimes \{0,1,\ldots,18\} \cup \{0,1,\ldots,18\} \bigtimes \{19, 20,\ldots,36\}$$
### 2. Bridge
![[WI - Bridge Example.png]]
- $\Omega = \{0, 1, 2, \ldots, 36\}$

## Wahrscheinlichkeiten
- The main goal of Probability Theory is to map events to propabilities. That means: We need to find a function $P$ such that:
$$A\mapsto P(A) \in [0,1]$$


### Wahrscheinlichkeitsraum 
- is a triple $$(\Omega, \mathcal{F}, P)$$ consisting 3 components
1. $\Omega$ - _Stichprobenraum_ (Sampe Space)
2. $\mathcal{F}$ - _Ereignisalgebra_ (Event Algebra/ $\sigma$-Algebra )
3. $P$ - _Wahrscheinlichkeitsmaß_ (Probability Measure)
 