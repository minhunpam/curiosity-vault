## Declarative Sentence & Non-declarative Sentence (a.k.a Proposition)
### Declarative Sentence
- states a fact
- can be `true` or `false`
#### Examples:
![[LC - Declarative Sentence Examples.png]]

### Non-declarative Sentence
- Questions -> "What time is it?"
- Commands -> "Do your homework!"
- Exclamations -> "Oh my god!"
- Various others

### Model Declarative Sentences in Propositional Logic
![[LC - Model Declarative Sentences.png]]

## Syntax & Semantics
- **Syntax** of logical language: 
	- set of symbols
	- rules for combining symbols
	to form formulas of the language

- **Semantics** of logic:
	- provides ***meaning***
	- In propositional logic, the meaning is given by the **truth values**
		- $\top$ : true
		- $\bot$: false
	- The semantics of propositional logic assigns $\top$ and $\bot$ to formulas

### Syntax of Propositional Logic
- defines symbols and rules to form formulas in propositional logic
#### Symbols
- Truth symbols ($\top$ and $\bot$)
- Propositional variable symbols (lowercase character)
- Logical operators (logical connectors, Boolean connectors)
	- Conjunction:
	- Dis

## Satisfiability (SAT)
- At least one model satisfies the formula
- $\varphi$ is SAT iff there exists a model $\mathcal{M}$ such that $\mathcal{M} \models \varphi$ 

## Validity - Tautology
- ***All*** models satisfy the formula
- $\varphi$ is valid iff for all models $\mathcal{M}, \mathcal{M} \models \varphi$ 

## Unsatisfiability - UNSAT
- A formula that is UNSAT iff for all models $\mathcal{M}, \mathcal{M} \not\models M$
> SAT and validity are ***dual*** concepts: $\varphi \text{ is valid iff } \neg \varphi \text{ is } UNSAT$ 

## Semantic Entailment
- $\varphi$ semantically entails $\psi$ if $\varphi$ is a special case of $\psi$ 
	- $\varphi \models \psi$ iff $\mathcal{M} \models \varphi \Rightarrow \mathcal{M} \models \psi$  
![[LC - Semantic Entailment.png]]