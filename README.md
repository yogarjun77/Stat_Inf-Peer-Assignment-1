# Stat_Inf-Peer-Assignment-1

Title:  Central Limit Theorem comparison for exponential distribution with simulations in R

Overview: An exponential distribution can be simulated in R using function rexp(n, lambda) where n is the rate parameter. Theoretically, both the mean and standard deviation of an exponential distribution is 1/lambda.  1000 simulations of the averages of 40 exponentials are generated and in this experiment and compared with the theoretical values and normal distribution.

Simulations:

Pre-determined parameters: 

	lambda = 0.2
	n = 40
	sim = 1000 #total simulations
	seed = 333
			 
		
Create a vector exp_2 which contains 1000 values - each the mean of 40 random values from an exponential distribution.

exp_2 = NULL
set.seed(333)

for (i in 1 :1000){

exp_2 = c(exp_2, mean(rexp(40,lambda)))

}


1) Sample mean vs Theoretical Mean


a) Sample set of 1000 averages of 40 exponentials
		
		hist(exp_2, col = "lightblue")
		mean_exp2 <- mean(exp_2)
		abline (v=mean_exp2, col = "darkblue", lwd = 2)
		text(mean_exp2,250, "sample mean")
		mean(mean_exp2)
		
b) Theoretical mean of distribution = 1/0.2 = 5

The sample mean is almost equal to the theoretical value, fulfilling the first condition for Central Limit Theorem (E[x] = µ.


2) Sample Variance vs Theoretical Variance

a) Calculate the sample variance and standard deviation
	exp_2_var = var(exp_2)
	print(exp_2_var)
	

b) Theoretical variance of distribution
   t_var <- (1/0.2)^2/40
   t_sd <- 1/0.2/sqrt(40)
   
Again, the sample variance is almost equal to the theoretical value, fulfilling the second condition for CLT Var[x] = s^2/n

3) Proving that the distribution is approximately normal

a) Comparing with a distribution of 1000 exponentials

A distribution of 1000 exponentials

exp_th <- rexp(1000, 0.2)
hist(exp_th)

Mean of 1000 exponential, mean(exp_th) = 4.966
Variance		  var(exp_th) = 26.816


The histogram and variance values show that a distribution of 1000 exponentials do not form a normal distribution.

b) Histogram of 1000 averages of 40 exponentials mapped onto a normal distribution

	hist(exp_2, freq = FALSE)
	lines(density(exp_2))

The distribution exhibits a Gaussian shape almost similar to a normal distribution.


    sd <- 0.79
    m <- 5
    hist(exp_2, density=20, breaks=20, prob=TRUE, 
         xlab="x-variable", ylim=c(0, 2), 
         main="normal curve over histogram")
    curve(dnorm(exp_2, mean=m, sd=sd), 
          col="darkblue", lwd=2, add=TRUE, yaxt="n")

	z_t<- (exp_2-5/0.625)

c) 	Normal Q-Q plot

qqnorm(exp_2)
qqline(exp_2)

Using a Q-Q (quantile-quantile) plot is more effective way to test if a sample is normally distributed. The resulting plots show that the normalized exp_2 (circles) closely fit the theoretical default normal (line).

The histogram and Q-Q plot positively indicate that the 1000 samples of the average of 40 exponentials exhibit characteristics of a normal distribution according to the Central Limit Theorem.















Project 2

1) Load the ToothGrowth data and perform some basic exploratory data analyses 

2) Provide a basic summary of the data.

3) Use confidence intervals and/or hypothesis tests to compare tooth growth by supp and dose. (Only use the techniques from class, even if there's other approaches worth considering)

4) State your conclusions and the assumptions needed for your conclusions. 

Some criteria that you will be evaluated on
Did you  perform an exploratory data analysis of at least a single plot or table highlighting basic features of the data?
Did the student perform some relevant confidence intervals and/or tests?
Were the results of the tests and/or intervals interpreted in the context of the problem correctly? 
Did the student describe the assumptions needed for their conclusions?


The data consists of measurements of the mean length of the odontoblast cells harvested from the incisor teeth of a population of 60 guinea pigs. These animals were divided into 6 groups of 10 and consistently fed a diet with one of 6 Vitamin C supplement regimes for a period of 42 days. The Vitamin C was administered either in the form of Orange Juice (OJ) or chemically pure Vitamin C in aqueous solution (VC). Each animal received the same daily dosage of Vitamin C (either 0.5, 1.0 or 2.0 milligrams) consistently. Since each combination of supplement type and dosage was given to 10 randomly selected animals this required a total of 60 animals for the study. After 42 days, the animals were euthanized, their incisor teeth were harvested and subject to analysis via optical microscopy to determine the length (in microns) of the odontoblast cells (the layer between the pulp and the dentine). The ToothGrowth data set therefore consists of 60 observations of the 3 variables - mean length of odontoblasts (microns), supplement type (OJ or VC) and Vitamin C dosage (milligrams/day).



library(datasets)
data(ToothGrowth)


OJ <- ToothGrowth %>%
	group_by(supp)%>%
	filter(supp == "OJ")


VC <- ToothGrowth %>%
	group_by(supp)%>%
	filter(supp == "VC")



