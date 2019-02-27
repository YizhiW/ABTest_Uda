# Udacity A/B Test Course Notes

## Lesson 1

Lesson 1 goes through the process of performing a A/B test. 

Say if you have a change for the product and you want to perform an A/B test to see if there is value to launch it. The A/B test should includes steps followed as:

* **Choose metrics**
	* Business driven. Funnel graph is one of the tool that could help with understanding how the work flows through the system and which level conversion you care most  
	* Common metrics used for E-commerce:
		1. Number of clicks
		2. Click-through-rate CTR, number of clicks/number of page view
		3. Click-through-probability CTP, unique visitors who click/ unique visitors who view
* **Review model**
	* Hypothesis test is usually used for analyzing A/B test. Most of the concepts are the same as hypothesis test in statistics. Here I list some of the important ones:
		1. Null/alternative hypothesis
		2. Underlying distribution. Student t for n < 30 and normal for n >= 30 when testing whether two sample means are equal 
		3. 	\alpha, significance level, which equals to P(reject|null hypothesis true)
		4. Confidence interval, which is calculated by 
		\begin{align}
		\left[ \mu - Z \times SE , \mu + Z \times SE \right]
		\end{align}
				