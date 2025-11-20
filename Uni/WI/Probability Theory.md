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
$$
P(A) = \frac{|A|}{|\Omega|} \text{ is called "Wahrscheinlichkeitsmaß)}
$$

### (N) Normierung (Normalization)
- The probability of the entire sample space must be 1 ($P(\Omega) = 1$)
### (A) $\sigma-Additivität$ (Sigma-Additivity)
- If the events $A_i$ are **pairwise disjoint**, the probability of their union equals the sum of their probabilities
$$
P(\bigcup_{i \geq 1}A_i) = \Sigma_{i \geq 1}P(A_i)
$$

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
- a type random experiment where all possible outcomes are equally likely
$$
P(A) = \frac{|A|}{|\Omega|}
$$
Where:
- $A$ - event (a subset of outcomes) => $|A|$ - number of favorable outcomes
### Urnenmodelle
#### Ziehen ohne Zurücklegen
- Context:
	- Having n balls $1\dots N$
	- Balls $1\dots N_1$ -> color1 || Balls $N_1 + 1 \dots N_1 + N_2$ -> color 2
	- evey elementary event belongs to a subset of $\{1,\dots,N\}$
- Formalize:
	- $\Omega = \{\{j_1, \dots, j_n\} \subset \{1, \dots, N\}\} \Rightarrow |\Omega|= {{N}\choose{n}} = \frac{N!}{n!(N-n)!}$
	- $\mathcal{A} = \mathcal{P}(\Omega)$
	- $P(\{j_1\dots j_n\}) = \frac{1}{{N}\choose{n}} \Rightarrow = P(A) = \frac{|A|}{{N}\choose{n}}$
#### Hypergeometric Distribution
$$
P|\text{Color i}| = k_i, 1 \leq i \leq L) = \frac{{N_1\choose{k_1}}\times\dots\times{{N_L}\choose{k_L}}}{{N}\choose{n}}
$$
##### Examples
1. Choosing 6 numbers from $\{1\dots45\}$ without replacement. What is the probability, that we guess correctly 4 numbers? What is the probability that we guess correctly all 6 numbers?
	- When the result of a lotto is released, there are
		- 6 numbers to be considered **Successes**
		- 39 numbers to be considered **Un-successes**
	- So the probability of guessing correctly 4 of 6 numbers is:
	$$
	P(X = 4) = \frac{{6\choose4} \times {39 \choose 2}}{45 \choose 6}
	$$
	- The probability of guessing correctly all 6 numbers is:
	$$
	P(X = 6) = \frac{{6\choose6} \times {39 \choose 0}}{45 \choose 6} = \frac{1}{45 \choose 6} \approx \frac{1}{8.10^9}
	$$

2. For an exam, the professor has a 32-question catalogue. In the real exam, he picks randomly 5 questions out of 32 questions. Peter only studied 20 questions. What is the probability, that he AT LEAST 3 questions right?
	- $P(X \geq 3) = 1 - P(X < 3) = 1 - P(X = 0) - P(X = 1) - P(X = 1)$
	- $P(X = 0) = \frac{{20\choose 0} \times {12 \choose 5}}{32 \choose 5}$
	- $P(X = 1) = \frac{{20\choose 1} \times {12 \choose 4}}{32 \choose 5}$
	- $P(X = 2) = \frac{{20\choose 2} \times {12 \choose 3}}{32 \choose 5}$
	$\Rightarrow P(X \geq 3) = 1 - \frac{{20\choose 0} \times {12 \choose 5}}{32 \choose 5} - \frac{{20\choose 1} \times {12 \choose 4}}{32 \choose 5} - \frac{{20\choose 2} \times {12 \choose 3}}{32 \choose 5}$

#### Ziehen mit Zurücklegen
- Context:
	- Having n balls $1\dots N$
	- Balls $1\dots N_1$ -> color1 || Balls $N_1 + 1 \dots N_1 + N_2$ -> color 2
	- evey elementary event belongs to a vector $(j_1, \dots, j_n) \in \{1,\dots,N\}^n$

- Formalize:
	- $\Omega = \{\{j_1, \dots, j_n\} \subset \{1, \dots, N\}\} \Rightarrow |\Omega|= N^n$
	- $\mathcal{A} = \mathcal{P}(\Omega)$
	- $P(\{(j_1\dots j_n)\}) = N^{-n} \Rightarrow = P(A) = |A|\times N^{-n}$

#### Multinomial Distribution
$$
\Large
P(|\text{color i}| = k_i, 1 \leq i \leq L) = \underbrace{{n \choose {k_1, k_2, \dots , k_L}}}_{\frac{n!}{k_1!k_2!\dots k_L!}} \times p_1^{k_1} \times \dots \times p_L^{k_L} 
$$
##### Binomial Distribution
- a special case of **Multinomial Distrubtion**, where L = 2
	- $p = \frac{N_1}{N}$
	- $q = 1 - p = \frac{N_2}{N}$
Then we have:
$$
P(|Color 1| = k) = {n \choose k}\times p^k \times q^{n-k}
$$

#### Examples
1. We roll 5 dices. What is the probability that we roll exactly 2 dices with 4?
	- $P(X = 4) = \frac{1}{6}$
	- $P(X \neq 4) = \frac{5}{6}$
$\Rightarrow P(\text{rolling exactly 2 dices with 4}) = {5 \choose 2} \frac{1}{6}^2 \frac{5}{6}^3$ 

2. It is known that, around 5% of all passengers in a flight will request refund or miss the flight. Assume a flight has 200 passengers having booked seats. What is the probability, that **less than 180 passengers** will come?
	- $P(\text{out of all passenger in a flight will request refund or miss the flight}) = 0.05$
	- $P(\text{passengers will show up}) = 0.95$
	$$
	\begin{aligned}	
	P(< \text{ 180 passengers will come}) &= 1 - P(\geq \text{180 passengers will come})\\ &= 1 - P(\text{180 passengers will come}) -  \dots \ P(\text{200 passengers will come}) \\
	\end{aligned}
	$$
	- $\Rightarrow P(\text{k passengers will come}) = {200 \choose k} \times (0.95)^k \times (0.05)^{200 -k}$


## Discrete Probability Space (diskreter Wahrscheinlichkeitsraum)
- A Laplace-Experiment is a special case of **Finite Sample Space (endlicher Stichprobenraum)**
	- Let $\Omega \neq 0$ and $||\Omega| < \infty$ - a **finite and non-empty sample space**
	- Let $p_i: 1 \leq i \leq |\Omega|$ non-negative probabilities that satisfy $\Sigma_{k = 1}^{|\Omega|}p_k = 1$
	- We define the probability of an event $A \subseteq \Omega$ as $P(A) = \Sigma_{\omega_k \in A} p_k$ , where $P(\omega_k = p_k)$
$\Rightarrow \text{This is a probability measure}$ 

- Triple: $(\Omega, \mathcal{P}(\Omega), P)$ is defined as a **probability space**

> The sequence ($p_k$) is called the ***Probability Mass Function (PMF)***
> 	PMF refers to the probability of discrete random variable which is equal to a particular value. It is represented as $f(x) = P(X = x)$, where: 
> 		$X$ - discrete random variable 
> 		$x$ - is the specified value

### Examples
1. We have 2 dices and are interested in the sum of faces. We set $\{2,\dots, 12\}$. Determine the PMF for this experiment
	- Basic sample space: $\Omega = \{\omega = (\omega_1, \omega_2) \in \{1,\dots,6\}^2\}$
	- Let $X$ be the sum of 2 dices, whose possible values are $2,\dots, 12$
$$
\Rightarrow P(X = x) =
\begin{cases}
\frac{1}{36} \text{ if x} = 2 \text{ or } 12 \\
\frac{2}{36} \text{ if x} = 3 \text{ or } 11 \\
\frac{3}{36} \text{ if x} = 4 \text{ or } 10 \\
\frac{4}{36} \text{ if x} = 5 \text{ or } 9 \\
\frac{5}{36} \text{ if x} = 6 \text{ or } 8 \\
\frac{6}{36} \text{ if x} = 7 \\
\end{cases}
$$

2. We play Lotto (6 out of 45 numbers) and want to model the number of matching numbers. We set $\Omega = {0, 1,\dots, 6}$ determine the PMF 
$$
\Large
\Rightarrow P(X = x) = 
\begin{cases}
\frac{{6 \choose 0}{39 \choose 6}}{45 \choose 6} \text{ if } x = 0 \\
\frac{{6 \choose 1}{39 \choose 5}}{45 \choose 6} \text{ if } x = 1\\
\vdots \\
\frac{{6 \choose 6}{39 \choose 0}}{45 \choose 6} \text{ if } x = 6\\
\end{cases}
$$

## Poisson Distribution
- is a special case of binomial distribution
- is the discrete probability distribution of the number of events occuring in a given time period, given the average number of times the event occurs over that time period

### Derivation:

### Definition:
$$
	P(X = k) = \frac{\lambda^{k}e^{-\lambda}}{k!}
$$
- $X$ - discrete  random variable - represent the number of events observed over a given time period
- $\lambda$ - average number of events over a given time period
- $e$ - Euler's number

### Examples:
- Assume the number of goals of football club in a game follows Poisson Distrubtion with the average of goals is 2. What is the probability, that a team at least 2 games (a season has 50 games) scores $> 3$ goals

#### Solution:
- Let $X$ be the number of goals
$\Rightarrow P(X > 3) = 1 - P(X \leq 3) = 1 - (P(X = 0) + P(X = 1) + P(X = 2))$ 
	- $P(0) = \frac{2^0e^-2}{0!} \approx 0.1353$
	- $P(1) = \frac{2^1e^-2}{1!} \approx 0.2707$
	- $P(2) = \frac{2^2e^-2}{2!} \approx 0.2707$
	- $P(3) = \frac{2^3e^-2}{3!} \approx 0.1804$ 
	$\Rightarrow P(X \leq 3) = 0.8571$
	$\Rightarrow P(X > 3) = 0.1429$

- Let $Y$ be the number of matches in which the number of goals $> 3$
$\Rightarrow P(Y \geq 2) = 1 - P(Y < 2) = 1 - (P(Y = 0) + P(Y = 1))$
	- $P(0) = {50 \choose 0}(0.1429)^0(0.8571)^{50} = 0.0003$
	- $P(1) = {50 \choose 1}(0.1429)^1(0.8571)^{49} = 0.0025$

$\Rightarrow P(Y \geq 2) = 1 - (0.0003 + 0.0025) = 1 - 0.0028 = 0.9972$

## Geometric Distribution
- ***Bernoulli Trial/ Bernoulli experiment***, that satifies 2 key properties
	1. There are exactly 2 complementary outcomes, success and failure
	2. The probability of success is the same every time the experiment is repeated
- A series of Bernoulli trials is conducted until a success occurs

### Definition
For a geometric distribution with probability $p$ of success, the probability that exactly kk failures occur before the first success is
$$
P(X = k) = (1 - p)^kp
$$
### Example
- A die is rolled until 6 occurs. What is the probability, that we more than 20 times of rolling until 6 occurs
$$
P(X>20) = 1 - P(X \leq 20) = 1 - \sum_{k = 0}^{20}(\frac{5}{6})^{k}(\frac{1}{6})
$$

