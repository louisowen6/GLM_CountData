library(pscl)
library(MASS)
library(lmtest)
library(gmodels)
library(logspline)
library(vcd)
library(AER)
library(ggplot2)
library(plyr)

Smoking$age[Smoking$age=="45-59"]<-"45-49"
Smoking$age<-as.factor(Smoking$age)
Smoking$smoke<-as.factor(Smoking$smoke)


#Descriptive Statistics
str(Smoking)
summary(Smoking)

#Mosaic Plot of number of man
df<-data.frame(age=c("40-44","45-49","50-54","55-59","60-64","65-69","70-74","75-79","80+"),agecpt=c(14,12,9,14,16,13,10,7,5),cigarrettePlus=c(4531,3030,2267,4682,6052,3880,2033,871,345),cigarretteOnly=c(3410,2239,1851,3270,3791,2421,1195,436,113),no=c(656,359,249,632,1067,897,668,361,274),cigarPipeOnly=c(145,104,98,372,846,949,824,667,537))
df$xmax<-cumsum(df$agecpt)
df$xmin<-df$xmax-df$agecpt
df$agecpt<-NULL
require(reshape2)
dfm<-melt(df,id=c("age","xmin","xmax"))
dfm1<-ddply(dfm,.(age),transform,ymin=cumsum(value)-value,ymax=cumsum(value))
dfm1$xtext<-with(dfm1,xmin+(xmax-xmin)/2)
dfm1$ytext<-with(dfm1,ymin+(ymax-ymin)/2)

p<-ggplot(dfm1,aes(ymin=ymin,ymax=ymax,xmin=xmin,xmax=xmax,fill=variable))

p1<-p+geom_rect(colour=I("grey"))
plot(p1)

#Mosaic Plot of response variable
sf<-data.frame(age=c("40-44","45-49","50-54","55-59","60-64","65-69","70-74","75-79","80+"),agecpt=c(6,7,7,13,17,16,14,11,9),cigarrettePlus=c(149,169,193,576,1001,901,613,337,189),cigarretteOnly=c(124,140,187,514,778,689,432,214,63),no=c(18,22,19,55,117,170,179,120,120),cigarPipeOnly=c(2,4,3,38,113,173,212,243,253))
sf$xmax<-cumsum(sf$agecpt)
sf$xmin<-sf$xmax-sf$agecpt
sf$agecpt<-NULL
require(reshape2)
sfm<-melt(sf,id=c("age","xmin","xmax"))
sfm1<-ddply(sfm,.(age),transform,ymin=cumsum(value)-value,ymax=cumsum(value))
sfm1$xtext<-with(sfm1,xmin+(xmax-xmin)/2)
sfm1$ytext<-with(sfm1,ymin+(ymax-ymin)/2)

sp<-ggplot(sfm1,aes(ymin=ymin,ymax=ymax,xmin=xmin,xmax=xmax,fill=variable))

sp1<-sp+geom_rect(colour=I("grey"))
plot(sp1)


#Determining Base Level
Smoking$age<-relevel(Smoking$age,ref="60-64")
Smoking$smoke<-relevel(Smoking$smoke,ref="cigarrettePlus")


#Model Alternative 1
model_poi_1<-glm(dead~age+smoke+offset(log(pop)),family=poisson(link=log),data=Smoking)

summary(model_poi_1) 
par(mfrow=c(1,2))
scatter.smooth(predict(model_poi_1,Smoking,type="response"),Smoking$dead,col=Smoking$age,xlab="model_poi_1 prediction",ylab="Number of Deaths",main="Age Predictor")
scatter.smooth(predict(model_poi_1,Smoking,type="response"),Smoking$dead,col=Smoking$smoke,xlab="model_poi_1 prediction",ylab="Number of Deaths",main="Smoke Type Predictor")

#Pearson Chi-Square Goodness of Fit Test , H0: Poisson Disributed
a<-(predict(model_poi_1,type="response")-Smoking$dead)^2
b<-sum(a/predict(model_poi_1,type="response"))
pchisq(b,24,lower.tail=FALSE) 
#it can be seen that H0 is not rejected, so Poisson model is suitable

model_nb_1<-glm.nb(dead~age+smoke+offset(log(pop)),data=Smoking)
summary(model_nb_1)
#it can be seen that theta (1/k) -> infinity, which implies k -> 0. This means that Poisson model is more suitable

#Model Alternative 2
model_poi_2<-glm(dead~age+offset(log(pop)),family=poisson(link=log),data=Smoking)
summary(model_poi_2)
par(mfrow=c(1,2))
scatter.smooth(predict(model_poi_2,Smoking,type="response"),Smoking$dead,col=Smoking$age,xlab="model_poi_2 prediction",ylab="Number of Dead",main="Age Predictor")
scatter.smooth(predict(model_poi_2,Smoking,type="response"),Smoking$dead,col=Smoking$smoke,xlab="model_poi_2 prediction",ylab="Number of Dead",main="Smoke Type Predictor")

#Pearson Chi-Square Goodness of Fit Test , H0: Poisson Disributed
a<-(predict(model_poi_2,type="response")-Smoking$dead)^2
b<-sum(a/predict(model_poi_2,type="response"))
pchisq(b,27,lower.tail=FALSE) 
#It can be seen that H0 is rejected, so Poisson model is not suitale

model_nb_2<-glm.nb(dead~age+offset(log(pop)),data=Smoking)
summary(model_nb_2)

#Log-Likelihood Ratio Test Overdispersion Poisson, H0:Poisson Distributed
odTest(model_nb_2)
#it can be seen that H0 is rejected, so Negative Binomial model is more suitable

#Model Alternative 3
 model_poi_3<-glm(dead~smoke+offset(log(pop)),family=poisson(link=log),data=Smoking)
summary(model_poi_3)
par(mfrow=c(1,2))
scatter.smooth(predict(model_poi_3,Smoking,type="response"),Smoking$dead,col=Smoking$age,xlab="model_poi_3 prediction",ylab="Number of Deaths",main="Age Predictor")
scatter.smooth(predict(model_poi_3,Smoking,type="response"),Smoking$dead,col=Smoking$smoke,xlab="model_poi_3 prediction",ylab="Number of Dead",main="Smoke Type Predictor")

#Pearson Chi-Square Goodness of Fit Test , H0: Poisson Disributed
a<-(predict(model_poi_3,type="response")-Smoking$dead)^2
b<-sum(a/predict(model_poi_3,type="response"))
pchisq(b,32,lower.tail=FALSE) 
#It can be seen that H0 is rejected, so Poisson model is not suitale

model_nb_3<-glm.nb(dead~smoke+offset(log(pop)),data=Smoking)
summary(model_nb_3)

#Log-Likelihood Ratio Test Overdispersion Poisson, H0:Poisson Distributed
odTest(model_nb_3)
#it can be seen that H0 is rejected, so Negative Binomial model is more suitable

lrtest(model_poi_3,model_nb_3)

#Choosing the Best Model

Diff_Dev_DF_Table<-cbind(abs(model_poi_1$deviance/model_poi_1$df.residual-1),abs(model_nb_2$deviance/model_nb_2$df.residual-1),abs(model_nb_3$deviance/model_nb_3$df.residual-1))
colnames(Diff_Dev_DF_Table)<-c("Model_Poi_1","Model_nb_2","Model_nb_3")
Diff_Dev_DF_Table

Pvalue_Res_Dev_Test_Table<-cbind(pchisq(model_poi_1$deviance,model_poi_1$df.residual,lower.tail=FALSE),pchisq(model_nb_2$deviance,model_poi_2$df.residual,lower.tail=FALSE),pchisq(model_nb_3$deviance,model_poi_3$df.residual,lower.tail=FALSE))
colnames(Pvalue_Res_Dev_Test_Table)<-c("Model_Poi_1","Model_nb_2","Model_nb_3")
Pvalue_Res_Dev_Test_Table

AIC_Table<-cbind(model_poi_1$aic,model_nb_2$aic,model_nb_3$aic)
colnames(AIC_Table)<-c("Model_Poi_1","Model_nb_2","Model_nb_3")
AIC_Table

Dev_Table<-cbind(model_poi_1$deviance,model_nb_2$deviance,model_nb_3$deviance)

Res_df_Table<-cbind(model_poi_1$df.residual,model_nb_2$df.residual,model_nb_3$df.residual)

Dev_Res_df_Table<-rbind(Dev_Table,Res_df_Table)
rownames(Dev_Res_df_Table)<-c("Deviance","df")
Dev_Res_df_Table

#Summary
exp(model_poi_1$coefficients)
predict(model_poi_1,Smoking,type="response")

