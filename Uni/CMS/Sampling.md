- Sampling is a key method in compuational statistics for **selecting a representative subset of data from a larger population**
- Essential for estimating population characteristics without examining every individual case

## Sampling: key terms
![[CMS - sampling key terms.png]]
- ***Target population***: **entire group of individuals or elements** that a researcher wants to **study and draw conclusions about**
	- Usually very larget and often impossible to study completely
	- Examples:
		- All university students in Austria
		- All customers of Nike worldwide
		- All Vietnamese households using digital payment apps0
- **Sample population (sample)**: **smaller subset selected from the target population** that is actually studied
	- Chosen to represent the target population as accurately as possible
	- Examples:
		- 500 students selected from Austrian universities
		- 1,000 Nike customers surveyed online
		- 300 Vietnamese users of a mobile payment app
> Results from the sample are generalized to the target population

## Inductive Inference vs. Deductive Inference
### Inductive Inference
- Draws general conclusions from specific observations
- Examples:
	- Context: You want to know if new smartphone model has good battery life
	- Protocol:
		- You test 10 phones of this model and record battery life and then you conclude the average battery life of this model is around 12 hours. 
		- Your conclusion is result from inductive inference because, you go from specific observations.
		- The conclusion is likely, but maybe the test phones were usually good. Testing more phones could change the result
## Deductive Inference
- Applies general rules or theories to specific cases
- Examples: 
	- Context: You want to know if new smartphon model has good battery life
	- Protocol:
		- You accept the rule: "Any smartphone that lasts more then 10 hours has good battery life"
		- From the tests, the phone lasts 12 hours
		- You conclude: "This smartphone has good battery life". This conclusion is resulted from deductive inference because you apply a general rule to a specific case

## Types of sampling methods
1. **Random sampling**:
	- Each number of the population has an equal chance of being selected
	- Ensures unbiased representation of the population
2. **Systematic Sampling**:
	- Selects members from a larger population according to a fixed interval
	- Useful when dealing with large populations
3. **Stratified Sampling**:
	- Population is divided into subgroups (***strata***) based on shared characteristics
	- Random sample are then drawn from each stratum
4. **Cluster Sampling**:
	- The population is divided into clusters, and a random sample of clusters is selected
	- All members of chosen clusters are then studied
5. **Convenience Sampling**:
	- Samples are taken from a group that is easy to access or contact
	- Not representative of the general population

## Understanding Random Samples in Statistics
- A sequence of random variables $X_1, X_2, \dots, X_n$ is a **random sampel** of size $n$ froma  population if they ar  **independent and identically distributed** with a common density function

## Sampled population
- Let $X_1, X_2, \dots, X_n$ be a random sample from a population with density $f(\cdot)$; then this population is called the **sampled population**
### Distinction between sampled and target population
- With random samples, we can only make valid probability statements about the sampled population
- Statements about the target population are not valid
