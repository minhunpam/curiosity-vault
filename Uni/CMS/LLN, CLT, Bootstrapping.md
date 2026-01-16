## Weak Law of Large Numbers (WLLN)
> Let $X_1, X_2, \dots X_n$ be a random sample of size $n$ from a distribution with density $f(\cdot)$, and let $E(X_1) = \mu$ and $Var(X_1) = \sigma^2$ be finite. Then: $$\lim_{n \to \infty}P(\bar{X_n} - \mu \geq \epsilon) = 0, \forall \epsilon > 0$$     
- WLLN states that the sample mean $\bar{X_n}$ converges to the population mean $\mu$ in probability as the sample size $n$ increases. In practical terms, as we collect more samples, the probability of the sample mean begin far from $\mu$ becomes arbitrarily small
- This doesn't guarantte that $\bar{X_n}$ will equal $\mu$ in every sample, but rather that it will "cluster" around $\mu$ as $n$ grows
- The WLLN is the statement about **convergence in probability**. As n increases, the probability that the sample mean $\bar{X_n}$ deviates from the population mean $\mu$ by more than any positive amount $\epsilon$ tends to become zero
--> Hence, as we increase our sample size, our estimate of the population mean (using the sample mean) becomes more accurate

### Central Limit Theorem (CLT)
> Let $X_1, X_2, \dots, X_n$ be a random sample of size $n$ from a distribution with density $f(\cdot)$, where $E(X_1) = \mu$ and $Var(X_1) = \sigma^2$ are finite. The standardized sample mean is: $$Z_n = \frac{\bar{X_n} - \mu}{\sigma / \sqrt{n}}$$
> Then as $n \to \infty$, $Z_n$ converges in distribution to a standard normal random variable, $N(0, 1)$: $$\lim_{n \to \infty} F_n(z) = \Phi(z) = \frac{1}{\sqrt{2 \pi}} \int_{-\infty}^{z}e^{-\frac{x^2}{2}}$$

- As $n$ increases, probability statements about the sample mean $\bar{X_n}$ can be approximated using a standard normal distribution
#### Application in Software Bug Analysis
- Context and problem definition:
	- Consider a software program with $n = 100$ . Each file has a certain number of errors and we are interested in analyzing the total number of errors across all files 
- Variable definitions:
	- Let $X_i$ denote the number of errors in the $i^{th}$ file
	- Assume $X \sim Pois(1)$ <=> Each file follows a Poisson distribution with a mean of 1 error per file
	- Independence is assumed between the files
	- Define $Y = \sum_{i = 1}^{n}X_i$ as the total number of errors in the software
- The CLT states that the sume of the large number of independent random variables, each with finite mean and varian
-  Standardizin the total number of errors:
	- Mean of $Y$ (total errors): $\mu = 100$ (since each file has an average of 1 error)
	- Standard deviation of $Y$: $\sigma = 10$ (since standard deviation in Poisson is the square root of mean)
	--> Standardizing $Y$ into $Z$: $Z = \frac{Y - n \mu}{\sqrt{n \sigma^2}} = \frac{Y - 100}{10}$

##### Calculating the Z-Score for our example
- We approximate $P(Y \leq 80)$ by finding the corresponding Z-score and then using the standard normal distribution to find $P(Z \leq z)$
- In our case:
	- Total number of files, $n = 100$
	- Mean number of errors per file, $\mu = 1$
	- Standard deviation for Poisson distribution, $\sigma = 1$. Therefore, the standard deviation for $Y = \sqrt{n \sigma^2} = \sqrt{100 \times 1} = 10$ 
	- For $Y = 80$, the Z-score is $Z = \frac{80 - 100}{80} = -2$
- Intepretation:
	- A Z-score of 2 means that 80 errors are 2 standard deviation below the expected number of errors (100) for our software program
- The Z-table shows the cumulative probability from the left end of the curve to the Z-score. For a Z-score of -2, the Z-table gives the probability of being to the left -2. The Z-table value for -2 is approximately 0.228
	- Interpretation:
		- This means the probability of having 80 or few errors in our software program is about 2.28%
> **Remark: The CLT states that, for a large sample size, the sample mean will approximate a normal distribution, _regardless of the shape of the original distribution_. This holds provided that the original distribution has a finite variance**

### Z-scores
- A z-score measures how many standard deviations a data point $x$ is from the mean $\mu$. It standardizes data, making diffrent distributions comparable
	- A z-score of $0$: The data point equals the mean
	- A positive z-score: The data point is above the mean
	- A negative z-score: The data point is below the mean
![[CMS - z-score.png]]
$$
\Large
z = \frac{x - \mu}{\sigma}
$$
## Sampling Distribution of the Sample Mean
- The sampling distribution of the sample mean is the probability distribution of all possible sample means from all possible samples of a specific size drawn from a population
### Characteristics
- The mean of the sampling distribution of the sample mean == mean of the population
- The standard deviation of the sampling distribution, known as the **standard error**, is given by $\sigma_{\bar{x}} = \frac{\sigma}{\sqrt{n}}$, where:
	- $\sigma$ - population standard derivation
	- $n$ - sample size
- As the sample size increases, the stadard error decreases

### Example: Radar Gun Measurement Errors
- Consider the setup of $n$ radar guns along a road, each with a measurement error. We analyze the average reading $\bar{X}_n$ for a car passing ast speed $\mu$, under 2 diffrent error distributions
1. **Normal Error Distribution:**
	- Each radar gun has a normally distributed measurement error $N(0, \sigma^2)$
	- For $\mu = 45km/h, n=2$ radars, and $\sigma = 5km/h$, simulate and plot the distribution of $\bar{X}_n$
2. **Uniform Error Distribution:**
	- Each radar gun now has uniformly distributed measurement error with $E(X) = 0$ and $Var(X) = \sigma^2$
	- Using the same $\mu, n$ and $\sigma$, simluate and plot the distribution of $\bar{X}_n$ with the same error
![[CMS - radar guns.png]]