# BMIN503/EPID600 Final Project

# Project Name
Patient Characteristics and racial disparities in determining clinical outcomes among COVID-19 Patients

## Table of contents
* [General info](#general-info)
* [Screenshots](#screenshots)
* [Technologies](#technologies)
* [Setup](#setup)
* [Features](#features)
* [Status](#status)
* [Inspiration](#inspiration)


## General info
Coronavirus disease is caused by severe acute respiratory syndrome coronavirus 2 (SARS-CoV-2) and was first detected in Wuhan, China in 2019. The disease has impacted over 150 countries and emerged as a global pandemic infecting over 1.5 million patients in the United States as of early June 2020 and the current surge of COVID infection back again is a growing concern. The purpose of the current R project is to find the impact of racial disparities and patient charactertistics such as age, gender, smoking history, obesity and other existing clinical conditions of patients determine the length of hospitalization stay, ventilator utilization and mortality.

## Screenshots
![Example screenshot](./img/screenshot.png)

## Technologies
* The project is developed using R Studio Version 1.3


## Setup
There are no particular set up requirements except you need to install R Stuio and download all the libraries

## Code Examples

#Creating a new data set for analysis
COVID.clean <- Pat_Merged_Data %>% 
        dplyr::select(Age,Race,Gender,Ethnicity,Smoking_Status,LOS,ICU_Dept_YN,Ventilator_YN,Category,Deceased_Status,
        HF,HTN,Diabetes,Angina,Hyperlipidemia,Stroke,COPD,CKD,Obesity,Asthma) %>%
        rename(age = Age,race = Race,gender= Gender,ethnicity = Ethnicity,smoking.status = Smoking_Status,category = Category,
            deceased.status = Deceased_Status,length.of.stay = LOS, icu=ICU_Dept_YN, ventilator = Ventilator_YN) %>%   
        filter(!race  %in% c("Other","Unknown","Patient Declined")) %>% 
        filter(!category  %in% c("Ruled Out")) %>% 
        filter(smoking.status != 'Not Asked') 
        
#average length of stay of patients by race
theme_set(theme_bw())
ggplot(Avg_LOS, aes(x = race, y = AvG_Length_of_Stay)) + 
  geom_bar(stat="identity", width=.5, fill="tomato3") + 
  labs(title="Average length of stay by race", 
       subtitle="Race Vs LOS", 
       caption="Avg LOS") + 
  theme(axis.text.x = element_text(angle=65, vjust=0.6))
  
#find out top variables that are significant based on regression model
decease.glm <- glm(deceased.status ~ as.numeric(age) + gender + race + smoking.status + HF + HTN + Diabetes +
              +Angina + Stroke + COPD + CKD + Obesity + Asthma + ventilator + icu,data = COVID.clean,family = binomial(logit))
summary(decease.glm)
        
        
## Features
The project has cool features to determine the clinical outcomes such as:
* Logistic Regression Models
* Random Forest
* ROC Curves

To-do list:
* Need to run the model with more data


## Status
Project is done and ready to submit

## Inspiration
I have taken this project because of its current importance.


