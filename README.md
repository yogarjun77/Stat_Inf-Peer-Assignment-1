Description of data:

The data consists of measurements of the mean length of the odontoblast cells harvested from the incisor teeth of a population of 60 guinea pigs. These animals were divided into 6 groups of 10 and consistently fed a diet with one of 6 Vitamin C supplement regimes for a period of 42 days. The Vitamin C was administered either in the form of Orange Juice (OJ) or chemically pure Vitamin C in aqueous solution (VC). Each animal received the same daily dosage of Vitamin C (either 0.5, 1.0 or 2.0 milligrams) consistently. Since each combination of supplement type and dosage was given to 10 randomly selected animals this required a total of 60 animals for the study. After 42 days, the animals were euthanized, their incisor teeth were harvested and subject to analysis via optical microscopy to determine the length (in microns) of the odontoblast cells (the layer between the pulp and the dentine). The ToothGrowth data set therefore consists of 60 observations of the 3 variables - mean length of odontoblasts (microns), supplement type (OJ or VC) and Vitamin C dosage (milligrams/day).

1. Exploratory Data Analysis
a) Load the ToothGrowth data, necessary libraries and perform some basic analysis.
library(datasets)
library(dplyr)
library(ggplot2)

#Load Dataset
data(ToothGrowth)  

#Preview dataset
head(ToothGrowth)  

#Dataset dimension
dim(ToothGrowth)

#Dataset summary
str(ToothGrowth) 
summary(ToothGrowth)

b) Exploratory data analysis
Group data and arrange in order
TG <- ToothGrowth %>% group_by(supp, dose)%>% arrange(dose, len)
head(TG)
Full table can be seen in appendix 1

Boxplot of data side by sideBoxplot to summarize the data
p <- ggplot(ToothGrowth, aes(factor(dose), len)) + geom_boxplot(fill="lightblue") 
p + facet_wrap(~supp, ncol = 2) + labs(x="dose", y="length", title = "Tooth Growth Data by Dose and Supplement")+ theme_bw()
The boxplot compares OJ and VC data side by side 
2) Provide a basic summary of the data.
60 rows of data - 10 subjects for each case with tooth length measurement after 42 days and varying by:
i) supplement - OJ or VC and dose (30 rows)
ii) dose - 0.5, 1.0 and 2.0 mg (30 rows)
Overall observation based on boxplot shows....

3) Use confidence intervals and/or hypothesis tests to compare tooth growth by supp and dose. 

a) by supp 
 - Create vectors of each supp/dose combination
OJ_05 <- ToothGrowth %>%
	filter(supp=="OJ", dose == 0.5)%>%
   	select(len)%>%
	arrange(len)

OJ_1 <- ToothGrowth %>%
	filter(supp=="OJ", dose == 1.0)%>%
   	select(len)%>%
	arrange(len)


OJ_2 <- ToothGrowth %>%
	filter(supp=="OJ", dose == 2.0)%>%
   	select(len)%>%
	arrange(len)


VC_05 <- ToothGrowth %>%
	filter(supp=="VC", dose == 0.5)%>%
   	select(len)%>%
	arrange(len)

VC_1 <- ToothGrowth %>%
	filter(supp=="VC", dose == 1.0)%>%
   	select(len)%>%
	arrange(len)


VC_2 <- ToothGrowth %>%
	filter(supp=="VC", dose == 2.0)%>%
   	select(len)%>%
	arrange(len)

T-Tests OJ vs VC
t.test(OJ_05, VC_05, var.equal = FALSE)
t.test(OJ_1, VC_1, var.equal = FALSE)
t.test(OJ_2, VC_2, var.equal = FALSE)
Summary of results
Table - mean1, 2, n - 1, 2, t low, t high, p value, df

b) by dose 
s_05 <- ToothGrowth %>%
	filter(dose == 0.5)%>%
   	select(len)%>%
	arrange(len)

s_1 <- ToothGrowth %>%
	filter(dose == 1.0)%>%
   	select(len)%>%
	arrange(len)


s_2 <- ToothGrowth %>%
	filter(dose == 2.0)%>%
   	select(len)%>%
	arrange(len)

T-Tests
t.test(s_1, s_05, var.equal = FALSE)
t.test(s_2, s_1, var.equal = FALSE)




4) State your conclusions and the assumptions needed for your conclusions.
Summarized data in table
Conclusions -
Up to dose of 1 mg better than .5 mg shows that it can help. 2mg not much difference - meaning beyond optimum value possibly no effect.
Supp wise, OJ seems to yield better result compared to  VC.
Some criteria that you will be evaluated on Did you perform an exploratory data analysis of at least a single plot or table highlighting basic features of the data? Did the student perform some relevant confidence intervals and/or tests? Were the results of the tests and/or intervals interpreted in the context of the problem correctly? Did the student describe the assumptions needed for their conclusions?



