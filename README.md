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
	* | Type | Distribution | Variance |
	| :--- | :---: | ---: | 
	| Sum  | normal | s^2/N |
	| probability | binomial | p(1-p)/N |
	| median/percentile | depends | depends |
	| count/difference | normal | Var(x) + Var(Y) | 
	| rates | Poisson | x bar | 
	| ratio | depends | depends | 
* **Empirical methods to get variance**
	* Some funky metrics have complicated underlying distribution or don't even have analytical solution, then we have to 
	use empirical method to get variance
	* The process is like historical simulation in quantitative finance, repeat A/A test, and get historical variance and 
	confidence interval. It's also a good way to validate your analytical solution

## Lesson 4

This lesson talks more details about how to perform the experiment, like how to decide the size, what unit of diversion you want to choose as 
your metric parameters. A lot of concepts discussed here are related to the __online business__.

* **Unit of diversion**
	* Unit of diversion is the base unit of your data collection. You want users to have consistent experience. For example, 
you want to test if a different layout would increase the CTR. The least wanted thing is to have your customers using the new layout 
at day 1 and then have another layout at day 2. So unit of diversion is the distinguished identifier which will be treated the same from 
beginning till end in your experiment. 
	* Common unit of diversion is:
		1. user ID
		2. cookie
		3. page view
	* Your metrics will have more variability then analytical if your unit of diversion is different from the unit of variability. Analytical solution 
	usually assume each event is independent, but the realty is not.
* **Population and Size**
	* Population controls who you are test against. It will reduce variability and noise from the test 
	if you know exactly what's your target population
	* **Cohort** is another common technique for choosing population. Cohort means you define two groups with similar entering time point to your experiment,
	and tracking/comparing their performance in the experiment. By doing cohort you will have two groups that share same attributes and are exposed to the experiment 
	same amount of time.
	* Population also will affect the size you need for the experiment. Correct population will reduce variability, thus lower the size requirement for the same power and 
	alpha.
* Duration
	* How long the test should go on. Depending on the scale you want for the test, more traffic exposed to the experiment means shorter duration. 
	Common practice is to start with less traffic exposure, then ramp up.

## Lesson 5

Lesson 5 is all about how to analyze the result from your experiment.

* No matter what you get, the first thing will be always sanity check. There are several aspects that you can take a look at:
	1. Exploratory data analysis: break down the data into subgroup/time range/behavior, and see if there is abnormality 
	2. Check the invariant variables: Think of metrics that won't be affected by your experiment, and check if they are actually invariant 
	across control group and experiment group
	3. Sign test: Check significance day by day, see if there is any patterns
* If the result didn't pass sanity check, then it's a no-go. Need to revisit data collection/design
* **Single metrics**
	* If your result is not significant, similar to what we do now, dig deeper
* **Multiple metrics**
	* **Multiple-comparison**: 
	The more metrics you have, the higher chance you will get a significant result. This is a large topic of how to properly do a 
	multiple comparison. Here is several simple solutions to it:
		1. Set a higher significance level:  a_all is the overall significance level you want, and a_ind is the significance level 
		for each metric
			* a_all = 1 - (1 - a_individual)^n
		2. Bonferroni correction: This formula provides significance level for each metric without any assumptions, but sometimes could be too conservative 
			* a_individual = a_all/n
		3. Other methods like Boole-Bonferroni bound, etc.
		4. Other strategy other than just use significance level
			* False Discovery Rate (FDR): controls the E[# false positive/# rejects] instead of significance level
		