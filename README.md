# Notes for Udacity A/B Test Course 

## Lesson 1

Lesson 1 goes through the process of performing a A/B test. 

Say if you have a change for the product and you want to perform an A/B test to see if there is value to launch it. The A/B test should includes steps followed as:

* **Choose metrics**
	* Business driven. Funnel graph is one of the tool that could help with understanding how the work flows through the system and which level conversion you care most  
	* Common metrics used for E-commerce:
		1. Number of clicks
		2. Click-through-rate CTR, number of clicks/number of page view
		3. Click-through-probability CTP, unique visitors who click/ unique visitors who view
* **Model design**
	* Hypothesis test is usually used for analyzing A/B test. Most of the concepts are the same as hypothesis test in statistics. Here I list some of the important ones:
		1. Null/alternative hypothesis
		2. Underlying distribution. Student t for n < 30 and normal for n >= 30 when testing whether two sample means are equal 
		3. Alpha, significance level, which equals to P(reject|null hypothesis true). Also called type 1 error
		4. Beta, P(failed to reject | null false), also called type 2 error
		5. Confidence intervals, which is calculated by [m - Z * SE, m + Z * SE]. SE is pooled standard error
		6. Size-power trade-off. Statistical power is defined as 1-beta, meaning the probability to reject null when null is false. 
		The more sample you have, the more power you have. Normally set to 80%.
		The power appropriately follows 1-C(Z - mr/SE), where C is the cumulative density function and mr is the mean from alternative hypothesis. 
		This means to increase power, you will need to have a larger n, meaning more samples
* **Evaluation**
	* First sanity check, see if the data makes sense to you
	* Then check if the result is significant
	* If it's significant, does it satisfy the practical significance level(business driven)
		1. If the lower bound of confidence interval greater than the practical significance level, we can say the result is significant
		2. If the practical significance level falls into confidence interval, you might want to do a test with a larger sample size
		3. If 0 falls into confidence interval, then it's likely that no significant effect, but you might also want to do a different test with a larger sample size
		4. It's always a good idea to dive down and dig deeper in the data, sometimes the difference from slices/time window is the key for you to understand the result
		
## Lesson 2

Policy and ethics behind the experiment. More details see:

[Link to lesson 2](https://classroom.udacity.com/courses/ud257/lessons/3998098714/concepts/40011586280923#)

## Lesson 3

More details about how to choose metrics

* Before choosing or create metrics, you will need to fully understand how the system works. For example, how the customers 
use your website, and what is the most important KPI that you want to improve.
* There are several alternative methods for collection user feedback, especially in E-commerce business. They are:
	1. UER(User Experience Research): Kind of time study in IE, you need several participants in the experiment and you analyze their behavior. 
	Can be best in experiment depth but with least scale.
	2. Focus group: Get feedbacks and opinions from a group of participants who could be potential customers or experienced customers
	More specific to a certain topic but can also be bias due to group think and minority dominance
	3. Survey: With largest scale but least specific
	4. Other study or paper
* **Make sure you understand what is the underlying distribution for your metrics**
* **Sensitivity**
	* The metrics need to be sensitive to the change. In other words it has to be able to represent the change of user behavior
	* In the example of Google testing video loading latency, only users with slow network connection are sensitive to the loading speed change. 
	So mean loading time is not sensitive enough for measuring the change, instead 95 quantile is a better option
* **Robustness**
	* The metrics also need to be invariant against the noise
	* A/A test is a way to test the robustness, also it's good for understanding what causes the change in the metrics.
* **Common type of metrics** 
	| Type | Distribution | Variance | 
	| ---- | ------------ | ------- | 
	| Sum  | normal | s^2/N |
	| probability | binomial | p(1-p)/N |
	| median/percentile | depends | depends |
	| count/difference | normal | Var(x) + Var(Y) | 
	| rates | poisson | x bar | 
	| ratio | depends | depends | 
* **Empirical methods to get variance**
	* Some funky metrics have complicated underlying distribution or don't even have analytical solution, then we have to 
	use empirical method to get variance
	* The process is like historical simulation in quantitative finance, repeat A/A test, and get historical variance and 
	confidence interval. It's also a good way to validate your analytical solution


	