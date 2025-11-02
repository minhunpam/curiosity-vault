## Stichprobenraum (Ereignisraum, Ergebnisraum, ...)
- sample space
- set of **all possible outcomes** of a random experiment
- Symbol: $\Omega$

### Ways to define the sample space
Context: You have a deck of cards, and draw 2 cards
1. Order does not matter
	- This means drawing Ace of Heart then King of Heart == King of Heart then Ace of Heart
	- We use set notation in this case
2. Order does matter
	- We use permutation notation in this case

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
### Important notations:
1. A OR B occurs: $\omega \in A \cup B$
2. A AND B occur: $\omega \in A \cap B$
3. A occurs, BUT B does NOT occur: $\omega \in A \setminus B$
4. A does NOT occur: $\omega \in A^C = \Omega \setminus A$ (say: complement of A)
5. A implies B: $A \subset B$
	- whenever event A occurs, event B also occurs
	- Example:
		- $A = \{2, 4, 6\}$ : event of rolling an even number
		- $B = \{2, 3, 4, 5, 6\}$: event of rolling a number greater than 1
		Then: $A \subset B$
		
### Examples
#### 1. Roulette
- 37 fields:
	- 18x Black = $\{19, 20, \ldots, 36\}$
	- 18x Rot = $\{1, 2, \ldots, 18\}$
	- 1x Green = $\{0\}$
- Rolling 2 times
- $\Omega = \{0, 1, 2, \ldots, 36\}$
- 2x Red: $A_1 = \{1, 2, \ldots, 18\}^2$
- first 1-12, then 13-24: $A_2 = \{1, \ldots, 12\} \bigtimes \{13,\ldots,24\}$
- only 1 black: $$A_3 = \{19, 20,\ldots,36\} \bigtimes \{0,1,\ldots,18\} \cup \{0,1,\ldots,18\} \bigtimes \{19, 20,\ldots,36\}$$
#### 2. Bridge
![[WI - Bridge Example.png]]
- Goal: we draw 2 cards
- Definition:
	- Heart: 1 - 13, where 1 is the Ace
	- Spade: 14 - 26, where 14 is the Ace
	- Club: 27 - 39 where 27 is the Ace
	- Diamond: 40 - 52, where 40 is the Ace

- $\Omega = \{\omega = (\omega_1, \omega_2) | 1 \leq \omega_1, \omega_2 \leq 52, \omega_1 \neq \omega_2\}$
- $\Omega' = \{\omega = \{\omega_1, \omega_2\} \subseteq \{1\dots52\}\}$

$$
\begin{aligned}
A_1 = \text{2x Heart} = {\omega = \{\omega_1, \omega_2\} \subseteq \{1\dots13\}} \\
\text{Under } \Omega' = \{\omega = (\omega_1, \omega_2) | 1 \leq \omega_1, \omega_2 \leq 13, \omega_1 \neq \omega_2\} 
\end{aligned}
$$
$$
A_2 = \text{only 1 Ace and only 1 Heart} = \{\omega \in \Omega | |\omega \cap \{1\dots13\}|=1 \land |\omega \cap \{1, 14, 27, 40\}|=1 \}
$$

#### 3. Rolling 10 dices
- $A = \text{sum all of faces equals to 40}$
- $\Omega = \{(\omega_1,\dots,\omega_{10}) \in {1,\dots,6}^{10}\}$

- $A_2 = \{(x_1,\dots,x_{10}) \in \Omega: \max_{1 \leq i \leq 10}x_i \geq 4  \}$ 
	- $A \subset A_2$
		- If all dices were $\leq 3$ , then the sum would be at most $30 < 40$. Hence to reach the sum 40, at least 1 $x_i$ must be $\geq$ 4
		- Therefore, every outcome from $A$ lies in $A_2$

- $A_3 = \{(x_1,\dots,x_{10}) \in \Omega: \Sigma^{9}_{i = 1} \in [34, 40]\}$
	- Consider A: 
	$$
	\begin{aligned}
	\Sigma^{10}_{i = 1}x_i = 	 \Sigma^{9}_{i = 1}x_i + x_{10} \\
	x_{10} \in [1,6] \\
	\Rightarrow \Sigma^{9}_{i = 1}x_i \in [34, 39]
	\end{aligned}
	$$
	Mean while: $A_3 = \{(x_1,\dots,x_{10}) \in \Omega:\Sigma^{9}_{i = 1}x_i \in [34, 40]\}$
- Therefore, evey outcome from A lies in $A_3$

#### 4. Drawing cards from a deck of 52 cards "mit Zurücklegen" and "ohne Zurücklegen"
##### "mit Zurücklegen" and "ohne Zurücklegen"
- **"mit Zurücklegen"** - with replacement: you draw the first card, and put it back before drawing the second
- **"ohne Zurücklegen"** - without replacement: you draw the first card and keep it out. The second draw is from only 51 remaining cards

#### Mit Zurücklegen (drawing 3 cards)
- $\Omega = \{1\dots52\}^3$ , where 1, 2, 3, 4 are Aces
- $A_i$: is the event where i-th drawn card is Ace $= \{\omega = (\omega_i, \omega_j, \omega_k) | \omega_i \in \{1,2,3,4\}\}$
- at least 1 Ace: $A_1 \cup A_2 \cup A_3 = (A_1^C \cap A_2^C \cap A_3^C)^C$    
- no Ace: $(A_1 \cup A_2 \cup A_3)^C = A_1^C \cap A_2^C \cap A_3^C$
- 3 Aces: $A_1 \cap A_2 \cap A_3$
- only 1 Ace: $(A_1 \cap A_2^C \cap A_3^C)  \cup (A_1^C \cap A_2 \cap A_3^C) \cup (A_1^C \cap A_2^C \cap A_3)$ 

##### Ohne Zurücklegen (drawing 5 cards)
- $\Omega = \{\omega = \{\omega_1\dots\omega_5\} \subseteq {1,\dots,52}\}$, where 1, 2, 3, 4 are Aces
- $A_i = \text{event, where there are i Aces} = \{\omega \in \Omega | |\omega \cap \{1, 2, 3, 4\}| = i\}$ 
- at least 1 Ace: $A_1 \cup A_2 \cup A_3 \cup A_4 = A_o^C$ 
- no Ace: $A_0 = (A_1 \cup A_2 \cup A_3 \cup A_4)^C$
- maximum 3 Aces: $A_0 \cup A_1 \cup A_2 \cup A_3 = A_4^C$  
## Wahrscheinlichkeiten
- The main goal of Probability Theory is to map events to propabilities. That means: We need to find a function $P$ such that:
$$A\mapsto P(A) \in [0,1]$$

## $\sigma-Algebra$
- a collection of subsets of a $\Omega$ that specifies the 3 specific properties
	1. Closure under complementation: If event A is in the collection, its complement must also lie inside the collection
	2. 

### Wahrscheinlichkeitsraum 
- is a triple $$(\Omega, \mathcal{F}, P)$$ consisting 3 components
1. $\Omega$ - _Stichprobenraum_ (Sampe Space)
2. $\mathcal{F}$ - _Ereignisalgebra_ (Event Algebra/ $\sigma$-Algebra )
3. $P$ - _Wahrscheinlichkeitsmaß_ (Probability Measure)


## Laplace Experiment
- a type random experiment with

## Poisson Distribution
