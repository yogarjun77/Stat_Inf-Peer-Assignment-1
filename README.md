
Title:  Central Limit Theorem comparison for exponential distribution with simulations in R

Overview: An exponential distribution can be simulated in R using function rexp(n, lambda) where n is the rate parameter. Theoretically, both the mean and standard 

deviation of an exponential distribution is 1/lambda.  1000 simulations of the averages of 40 exponentials are generated and in this experiment and compared with the 

theoretical values and normal distribution.

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

Using a Q-Q (quantile-quantile) plot is more effective way to test if a sample is normally distributed. The resulting plots show that the normalized exp_2 (circles) 

closely fit the theoretical default normal (line).

The histogram and Q-Q plot positively indicate that the 1000 samples of the average of 40 exponentials exhibit characteristics of a normal distribution according to 

the Central Limit Theorem.















Project 2

Title: Studying effect of vitamin C on teeth growth of guinea pigs

Description of data:

The data consists of measurements of the mean length of the odontoblast cells harvested from the incisor teeth of a population of 60 guinea pigs. These animals were 

divided into 6 groups of 10 and consistently fed a diet with one of 6 Vitamin C supplement regimes for a period of 42 days. The Vitamin C was administered either in 

the form of Orange Juice (OJ) or chemically pure Vitamin C in aqueous solution (VC). Each animal received the same daily dosage of Vitamin C (either 0.5, 1.0 or 2.0 

milligrams) consistently. Since each combination of supplement type and dosage was given to 10 randomly selected animals this required a total of 60 animals for the 

study. After 42 days, the animals were euthanized, their incisor teeth were harvested and subject to analysis via optical microscopy to determine the length (in 

microns) of the odontoblast cells (the layer between the pulp and the dentine). The ToothGrowth data set therefore consists of 60 observations of the 3 variables - 

mean length of odontoblasts (microns), supplement type (OJ or VC) and Vitamin C dosage (milligrams/day).

Data Source:[Link]...


Preparation

Load libraries


1) Load the ToothGrowth data and perform some basic exploratory data analyses 

a) Quick overview of the data

#Load dataset
data(ToothGrowth)

#Preview dataset
head(ToothGrowth)

#Dataset dimension

dim(ToothGrowth)


#Dataset summary

str(ToothGrowth)
summary(ToothGrowth)


b) Exploratory data analysis


#Group data and arrange in order



> TG <- ToothGrowth %>%
 group_by(supp, dose)%>%
 arrange(dose, len)


#Boxplot of data side by side - note the outliers

p<- ggplot(ToothGrowth, aes(factor(dose), len)) + geom_boxplot(fill="lightblue")
p + facet_wrap(~supp, ncol = 2) + 
labs(x="dose", y="length", title = "Tooth Growth Data by Dose and Supplement")+
theme_bw()



2) Provide a basic summary of the data.

#Mean, var, sd, median, table , sum

Overall observation based on boxplot


3) Use confidence intervals and/or hypothesis tests to compare tooth growth by supp and dose. (Only use the techniques from class, even if there's other approaches 

worth considering)

a) by supp 
Since there are not control group in the dataset, our hypothesis for comparison would be as follows:


VC_a <- VC %>%
group_by(dose)%>%
summarize(count_VC = length(len), sum_VC = sum(len), mean_VC = mean(len), SD_VC = sd(len))

VC_a

  dose count   sum  mean       SD
1  0.5    10  79.8  7.98 2.746634
2  1.0    10 167.7 16.77 2.515309
3  2.0    10 261.4 26.14 4.797731



Comparing

a) supp_sum <- merge(VC_a, OJ_a)

dose count_VC sum_VC mean_VC    SD_VC count_OJ sum_OJ mean_OJ    SD_OJ
1  0.5       10   79.8    7.98 2.746634       10  132.3   13.23 4.459709
2  1.0       10  167.7   16.77 2.515309       10  227.0   22.70 3.910953
3  2.0       10  261.4   26.14 4.797731       10  260.6   26.06 2.655058



supp_sum$md <- supp_sum











95% confidence interval

mean



OJ_a <- OJ %>%
	group_by(dose)%>%
	summarize(count_OJ = length(len), sum_OJ = sum(len), mean_OJ = mean(len), SD_OJ = sd(len))
OJ_a


  dose count   sum  mean       SD
1  0.5    10 132.3 13.23 4.459709
2  1.0    10 227.0 22.70 3.910953
3  2.0    10 260.6 26.06 2.655058




H0 - mean of difference in length between VC and OJ is equal to zero since there is no difference
Ha - mean of difference in length not equal to zero



b) by dose
Since there is no control group, our hypothesis for comparison would be as follows:

a) Compare 0.5 to 1.0
Ho - mean of difference is zero

b) Compare 1.0 to 2.0

c) Compare 0.5 to 2.0




4) State your conclusions and the assumptions needed for your conclusions. 

Some criteria that you will be evaluated on
Did you  perform an exploratory data analysis of at least a single plot or table highlighting basic features of the data?
Did the student perform some relevant confidence intervals and/or tests?
Were the results of the tests and/or intervals interpreted in the context of the problem correctly? 
Did the student describe the assumptions needed for their conclusions?


The data consists of measurements of the mean length of the odontoblast cells harvested from the incisor teeth of a population of 60 guinea pigs. These animals were 

divided into 6 groups of 10 and consistently fed a diet with one of 6 Vitamin C supplement regimes for a period of 42 days. The Vitamin C was administered either in 

the form of Orange Juice (OJ) or chemically pure Vitamin C in aqueous solution (VC). Each animal received the same daily dosage of Vitamin C (either 0.5, 1.0 or 2.0 

milligrams) consistently. Since each combination of supplement type and dosage was given to 10 randomly selected animals this required a total of 60 animals for the 

study. After 42 days, the animals were euthanized, their incisor teeth were harvested and subject to analysis via optical microscopy to determine the length (in 

microns) of the odontoblast cells (the layer between the pulp and the dentine). The ToothGrowth data set therefore consists of 60 observations of the 3 variables - 

mean length of odontoblasts (microns), supplement type (OJ or VC) and Vitamin C dosage (milligrams/day).



library(datasets)
data(ToothGrowth)


OJ <- ToothGrowth %>%
	filter(supp == "OJ")%>%
	select(dose, len)


VC <- ToothGrowth %>%
	filter(supp == "VC")%>%
	select(dose, len)

head(ToothGrowth)

#Boxplot
p<- ggplot(ToothGrowth, aes(factor(dose), len)) + geom_boxplot(fill="lightblue")
p + facet_wrap(~supp, ncol = 2) + 
labs(x="dose", y="length", title = "Tooth Growth Data by Dose and Supplement")+
theme_bw()




> TG <- ToothGrowth %>%
 group_by(supp, dose)%>%
 arrange(dose, len)

View(TG)




dose count_VC sum_VC mean_VC    SD_VC count_OJ sum_OJ mean_OJ    SD_OJ
1  0.5       10   79.8    7.98 2.746634       10  132.3   13.23 4.459709
2  1.0       10  167.7   16.77 2.515309       10  227.0   22.70 3.910953
3  2.0       10  261.4   26.14 4.797731       10  260.6   26.06 2.655058







sp <- sqrt( ((n1 - 1) * sd(x1)^2 + (n2-1) * sd(x2)^2) / (n1 + n2-2))
md <- mean(g2) - mean(g1)
semd <- sp * sqrt(1 / n1 + 1/n2)
rbind(
md + c(-1, 1) * qt(.975, n1 + n2 - 2) * semd,
t.test(g2, g1, paired = FALSE, var.equal = TRUE)$conf,
t.test(g2, g1, paired = TRUE)$conf
)



OJ_1 <- TG$len[1:10]
OJ_2 <- TG$len[11:20]
OJ_3 <- TG$len[21:30]


VC_1 <- TG$len[31:40]
VC_2 <- TG$len[41:50]
VC_3 <- TG$len[51:60]

# merge(VC_a, OJ_a)
#supp$md <- supp$mean_OJ-supp$mean_VC
#supp$SP <- sqrt((supp$count_OJ-1)*(supp$SD_OJ)^2+(supp$count_VC-1)*(supp$SD_VC)^2)
#semd <- supp$SP*sqrt(1/supp$count_OJ + 1/supp$count_VC)
#supp$tl <- supp$md + qt(0.975, supp$count_VC+supp$count_OJ)*semd
#> supp$th <- supp$md + qt(0.975, supp$count_VC+supp$count_OJ)*semd
#> supp$tl <- supp$md - qt(0.975, supp$count_VC+supp$count_OJ)*semd



#> supp  #(this all not necessary)
  dose count_VC sum_VC mean_VC    SD_VC count_OJ sum_OJ mean_OJ    SD_OJ    md       SP     semd         tl       th
1  0.5       10   79.8    7.98 2.746634       10  132.3   13.23 4.459709  5.25 15.71296 7.027048  -9.408165 19.90816
2  1.0       10  167.7   16.77 2.515309       10  227.0   22.70 3.910953  5.93 13.94995 6.238606  -7.083503 18.94350
3  2.0       10  261.4   26.14 4.797731       10  260.6   26.06 2.655058 -0.08 16.45017 7.356738 -15.425887 15.26589

#this below is the easier way

t.test(OJ_3, VC_3, var.equal = FALSE)
t.test(OJ_2, VC_2, var.equal = FALSE)
t.test(OJ_1, VC_1, var.equal = FALSE)







