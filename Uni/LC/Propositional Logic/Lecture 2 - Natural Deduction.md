## Motivation
- There are facts we know are true, which is called **Premises**
	1. If the plane arrives late and there are no taxis at the airport, then Alice is late for her meet. $(p \wedge \neg t) \to l$
	2. Alice is not late for her meet. $\neg l$
	3. The plane did arrive late. $p$
- From those facts, we want to deduce new knowledge, which is called **Conclusion**
	4. Therefore, there were taxis at the airport $t$

> $p$ ... The plane arrives late
> $t$ ... There are taxis at the airport
> $l$ ... Alice is late for the appointment

## Natural Deduction
- Defines set of proof rules
	- **Syntactic rewriting rules**
	- Subsequently apply these rules to construct proof -> proves that conclusion follows from premises
- Create "watertight" proofs
	- No "Dark-Magic" Proofs
	- Proofs can be checked and generated automatically

## Sequents (Arguments)
$$
{\Large
\underbrace{\phi_1, \; \phi_2, \; \dots, \; \phi_n}_{\text{Premises}}
\;\vdash\;
\underbrace{\Psi}_{\text{Conclusion}}
}
$$
> $\vdash$ ... single turnstile - read: "entails", "proofs"

## Rules for Conjunction
### 1. AND-Introduction Rule ($\wedge i$)
$${
\Large
\frac{
  \varphi \quad \psi
}{
  \varphi \wedge \psi
}
}
$$
- If both $\varphi$ and $\psi$ are premises, you can introduce their conjunction $\varphi \wedge \psi$

### 2. AND-Elimination Rule ($\land e_1 \text{ and } \land e_2$)
- Once you have $\varphi \land \psi$ , you can extract either of its parts 

| $\land e_1$                                  | $\land e_2$                                |
| -------------------------------------------- | ------------------------------------------ |
| ${\Huge \frac{\varphi \land \psi}{\varphi}}$ | ${\Large \frac{\varphi \land \psi}{\psi}}$ |
## Rules for Double Negation
### 1. Introduction ($\neg \neg i$)
$$
{
\Large
\frac
{\varphi}
{\neg \neg \varphi}
}
$$
### 2. Elimination ($\neg \neg e$)
$$
{
\Large
\frac
{\neg \neg \varphi}
{\varphi}
}
$$

## Rule for Implication
### 1. Elimination (Modus Ponens) ($e$)
$$
{
\Large
\frac
{\varphi \quad \varphi \to \psi}
{\psi}
}
$$
- Premises:
	- "If it's raining, then the ground is wet" - $\varphi \to \psi$
	- "It is raining" - $\varphi$
- Conclusion:
	- "The ground is wet" - $\psi$

### 2. Derived Elimination (Modus Tollens) ($MT$)
$$
{
\Large
\frac
{\varphi \to \psi \quad \neg \psi}
{\neg \varphi}
}
$$
- $\varphi$ - "It is raining."
- $\psi$ - "The ground is wet."
- Premises: 
	- "If it is raining, then the ground is wet." - $\varphi \to \psi$
	- "The ground is **NOT** wet." - $\neg \psi$
- Conclusion:
	- "It is **NOT** raining." - $\neg \varphi$

### 3. Introduction Rule
$$
{\Large
\frac{
  \boxed{
    \begin{array}{c}
    \varphi \; \text{assumption} \\
    \vdots \\
    \psi
    \end{array}
  }
}{
  \varphi \to \psi
}}
$$
>> Hint: If conclusion is of form $\varphi \to \psi$ , apply introduction rule implication immediately 

##  Rules for Disjunction
### 1. Introduction ($\lor i$)

| $\lor i_1$                                   | $\lor i_2$                                   |
| -------------------------------------------- | -------------------------------------------- |
| ${\Large \frac{\varphi}{\varphi \lor \psi}}$ | ${\Large \frac{\varphi}{\psi \lor \varphi}}$ |
### 2. Elimination ($\lor e$)
$$
{
\Large
\frac{
	\varphi \lor \psi 
	\quad
	\boxed {
		\begin{array}{c}
	    \varphi \; \text{assum.} \\
	    \vdots \\
	    \chi
	    \end{array}
	}
	\quad
	\boxed {
		\begin{array}{c}
	    \psi \; \text{assum.} \\
	    \vdots \\
	    \chi
	    \end{array}
	}
}
{\chi}
}
$$

## Rules for Negation
## 1. (Not) elimination ($\neg e$)
$$
{
\Large
\frac
{\varphi \quad \neg \varphi}
{\bot}
}
$$
### 2. (Bottom) elimination ($\bot e$ )
$$
{
\Large
\frac
{\bot}
{\varphi}
}
$$
- Intuition: $\bot$ represents a contradiction - a situation that cannot be true. Once a contradiction has been reached, logic "breaks", and anything can be derived from it
#### Example:
- Suppose we have a contradiction $\bot$ , then by $\bot-elimination: \frac{\bot}{q}$ . We can conclude any statment q, for example: "The moon is hotter than the son", "2 + 2 = 5", etc. Things seem totally bullshit in a consistent system, but become provable when contradiction happens 
### 3. Introduction ($\neg i$)
$$
{
\Large
\frac
{
	\boxed {
		\begin{array}{c}
	    \varphi \; \text{assum.} \\
	    \vdots \\
	    \bot
	    \end{array}
	}
}
{\neg \varphi}
}
$$
> ***Hint: if conclusion is of form $\neg \varphi$, apply immediately $\neg i$***

#### Derived Rule: Proof by Contradiction ($PBC$)
$$
{
\Large
\frac
{
	\boxed {
		\begin{array}{c}
	    \neg \varphi \; \text{assum.} \\
	    \vdots \\
	    \bot
	    \end{array}
	}
}
{\varphi}
}
$$
## Other rules
### 1. Law of the Excluded Rule ($LEM$)
$$
{
\Large
\frac{}{\varphi \lor \neg \varphi}
}
$$
- The rule states that for every proposition, either this propsition or its negation is true, there is no third possibility
- This rule can be used at any time as a premise in classical proofs
> _Personal takeaway_: When it comes to logic expression, where there are no premises, this rule would be the starting point for the Natural Deduction
> For example:
$$ 
\Large 
\begin{aligned} 
	&\vdash (p \lor q) \land q \\ 
	&1. p \qquad \text{assumption} \\
	& \vdots \\
	&n. \neg p \qquad \text{assumption} \\
	& \vdots 
\end{aligned} 
$$

### 2. Copy Rule ($copy$)
$$
{
\Large
\frac{\varphi}{\varphi}
}
$$
- reuse a formula that's already been derived in the same scope. In other words, if you have $\varphi$ somewhere above in your proof, you can "copy" it again lower down when you need it


## Counter Examples for invalid Sequents
###  Soundness & Completeness
- An algorithm is sound, when it gives a correct solution
- An algorithm is complete, when it gives solutions to all inputs
- 

### Invalid sequents
- model $\mathcal{M}$ that is a counter example if
	- satisfies all premises 
	- does not satisfy the conclusion