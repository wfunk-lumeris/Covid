# Lumeris' COVID-19 Predictive Analytics
 
We understand that you, your staff, and your community are under stress during this time and we wish to do our part to help you care for your sickest patients.  We know that in some communities, PCPs and clinic staff are being under-utilized; we see opportunities for those teams to support proactive management to keep people as healthy as possible and at home.  This of course helps reduce constraints on valuable hospital beds and medical equipment including ventilators.

This page describes Lumeris' COVID-19 predictive analytics program, provides links to our code repository, and offers the actual model for free within your own EHR.  The predictive model identifies people with a high risk of COVID-related adverse events such as emergency hospitalizations.  We are assessing the possibility of future updates that may include additional adverse events such as likelihood of ICU and death or other tools to aid with managing the COVID-19 crisis.  Medical and/or non-medical staff (such as social workers) at private and public health systems, provider offices, and clinics can outreach to these high-risk people proactively.  It is expected that through that engagement the staff can identify challenges the person may be facing that, if addressed, can reduce the likelihood of that person having an adverse event such as hospitalization and increase the likelihood that the person can remain at home sheltered-in-place. 

The engagement driven by these predictive analytics can be conducted via various channels including phone calls, text messages, telehealth, or messages via the EHR's patient portal. 

The analytics can be deployed within most EHR's population health platform including Cerner’s Millennium, Athena, eClinicalWorks, and Epic’s Healthy Planet.  In additional, this first model release has analytics that were simplified such that most authorized EHR users can enter the parameters; staff from the IT Department aren’t necessarily required.   

All of the information here including in the associated support pages, and on Github, is being made freely-available under our [open source license](https://github.com/Lumeris-Health/Covid/blob/master/LICENSE).   

## Instructions
**This model was purposely made to be simple to use a points system analogous to Weight Watchers so that it can be typed into most EHRs manually by a non-technical person. For example, the model variables and points can be typed directly into Epic's Healthy Planet to create a patient registry (as well as most any EHR that has a module for population health).  A PDF that provides a brief overview and instructions, including the variables and points to create the registry in your EHR, is [here](https://github.com/Lumeris-Health/Covid/blob/master/howTo.pdf).**

## Detail
### Model 1.  Predicting COVID-19 related hospitalizations 

This is a simple model with just [18 variables](https://github.com/Lumeris-Health/Covid/blob/master/modelParameters.csv) that performs well at predicting the likelihood of all-cause emergency hospitalizations in the next 90 days for people particularly susceptible to COVID-19.  Emergency hospitalizations are defined as ED visits that are followed by an IP admission (the prediction is for ED visits that are followed by a discharge, not just standalone ED visits).  It predicts the hospitalizations for people especially susceptible to COVID-19 based on the latest data published by the CDC [here](https://www.cdc.gov/coronavirus/2019-ncov/index.html) and on March 11 in The Lancet [here](https://www.thelancet.com/journals/lancet/article/PIIS0140-6736(20)30566-3/fulltext). 

### Applicable Population 

Although the model was built on a small portion of the US population, in our judgment it can be applied to people of all ages (between 0 to 114 yo) and both males and females. 

The number of people in the validation dataset is 789k.  Data is from January 2018 thru March 24, 2020.  The demographics of the people utilized in the validation dataset are below: 

 | Characteristic | Value         |
| :-------------: |:-------------:|
| Age (median yo)      | 48 (range 0 to 114 yo) |
| Female      | 56.5%      |
| COPD | 1.4%      |
| Coronary Disease | 2.9% |
| Diabetes | 5.2% |
| Hypertension | 12.8% |
| Hospitalizations | 1.26% or 12.6 per 1000 |

Additional information on the demographics is available in Github [here](https://github.com/Lumeris-Health/Covid/blob/master/extendedDemographics.csv).  

### Model Details 

The model for this release was trained and tested on a collection of contemporary data from different health systems scattered across the US.  It is intended to work well on data available either in the EHR or from administrative claims data (all performance characteristics reported here are based on EHR data). 

We trained models on both the likelihood of hospitalization and the likelihood of hospitalization due to COVID-19 ([details of COVID data](https://github.com/Lumeris-Health/Covid/blob/master/covidCasesAlgorithm.pdf)). 

Additional information about this model are available in our [public Github repository](https://github.com/Lumeris-Health/Covid).   

### Model Performance 

Predictive modeling accuracy is an important factor and not academic:  Lower accuracy means that resources are wasted on people who are not truly high risk, and lower accuracy means that fewer people who are truly high risk will be correctly identified.  At the risk of being overly-dramatic, in our COVID-19 crisis this lower accuracy can translate to more deaths.

We evaluate performance using standard statistical tools applied to validation or “hold out” data sets – data that the computer algorithms did not have access to during the training of the model.  We do this in order to avoid overly optimistic estimates of accuracy.  Here we provide the overall accuracy (c-statistic, also known as area under the curve or AUC) together with sensitivity and positive predictive values (PPV) and sensitivities for the top 1% and 5% of the population for the prediction of COVID-19 related hospitalizations: 

 | Characteristic | Top 1% Sensitivity  | Top 1% PPV  | Top 5% Sensitivity | Top 5% PPV
| :-------------: |:-------------:|:-------------:|:-------------:|:-------------:|
| 0.833 |     0.141 | 0.138 | 0.387 | 0.075 |

The receiver operator characteristics (ROC) curve is below: 
![alt text][logo]

[logo]: https://github.com/Lumeris-Health/Covid/blob/master/roc.png "Covid Model 1 ROC"

The results are also available as a table [here](https://github.com/Lumeris-Health/Covid/blob/master/rocTable.csv). 

The performance of our model is relatively strong.  As a check we compared the performance of our model to one built from data published in that Lancet article mentioned [above](https://www.thelancet.com/journals/lancet/article/PIIS0140-6736(20)30566-3/fulltext).  These are the performance statistics for that model to predict hospitalizations:

 | Characteristic | Top 1% Sensitivity  | Top 1% PPV  | Top 5% Sensitivity | Top 5% PPV
| :-------------: |:-------------:|:-------------:|:-------------:|:-------------:|
| 0.732 |     0.058 | 0.047 | 0.202 | 0.032 |

We performed a good faith effort to apply that model in the best way possible, but nevertheless in comparing the performance between those two models, our model outpeformed the comparator model by a considerable amount.  This table shows the percent outperformance between our model and the one published in The Lancet:

| Characteristic | Top 1% Sensitivity  | Top 1% PPV  | Top 5% Sensitivity | Top 5% PPV
| :-------------: |:-------------:|:-------------:|:-------------:|:-------------:|
| +13% |     +131% | +259% | +84% | +186% |

There may be important details from that published paper that make such a comparison specious, but it nevertheless helped us believe that we were contributing positively to the healthcare community and society at large by publishing our model.

Given the model performance above, economic modeling of the breakeven costs shows that providers will “come out ahead” economically if they use our models.

We converted the breakeven analysis to answer the question, if an organization's effectiveness at reducing admission turns out to be low (they can reduce admissions 25%), moderate (50%), or high (75%), how much money can they spend on the proactive outreach and still come out ahead?  The **higher that dollar amount the better the predictive model** since there would be more money to spend and still be "profitable".  Our analysis showed that for an organization that's moderately effective at reducing admission (50% effective) for people in the Top 1% highest risk group, they can still come out ahead using our model if they spend up to $1,382 per person (the estimated breakeven for low and highly successful programs is $691 and $2,073, respectively).  

Said a different way, organizations using this predictive model to engage with the top 1% highest risk people in their population to reduce hospitalizations by 50%, can generally spend up to $1,382 per person to achieve that objective and still breakeven.  Of course, this economic analysis does not include the humanistic value of helping people stay safely at home avoiding adverse events, as well as the ability to free up system capacity for anticipated surges in hospitalizations during the COVID-19 epidemic.

The breakeven point for the cost of interventions toward the Top 1% and Top 5% are shown in the table below: 

 | Characteristic | Top 1%  | Top 5%  |
| :------------- |:-------------:| :-------------:|
| Clinical Success^: | | |
|          Low effectiveness (25% reduction in hospitalizations) | $691 | $225 |
|          Moderate (50% reduction in hospitalizations) | $1,382 | $450 |
|          High (75% reduction in hospitalizations) | $2,073 | $676 |

* ^ Clinical success refers to the percentage reduction in hospitalizations due to the outreach, assumed to be a reduction in hospitalizations by either 25%, 50%, or 75%.  For example, an organization that uses these predictive analytics to outreach to the Top 1% highest risk people, and has the ability to reduce hospitalizations by 50%, can spend up to $1,382 on achieving that outcome and still breakeven (this analysis does not accounting for reimbursement by payers, relieve relief funding from the US government, or other complexities, ; but this analysisit does provide a basis for decision-making regarding the amount of money to spend on outreach). 
* Assumes COVID-19 hospitalization costs $20,000 based on approximately $2,200/day for 9 days 
* All values in US dollars

For comparison we did the same economic analysis using model based on the data in The Lancet.  Not surprisingly, the lower predictive model accuracy meant that the organization had a smaller amount of money they could spend and still come out ahead.  For example, if an organization chooses to intervene with the Top 1% of highest risk people, they'll have about 46% less money available ($1382 vs $740) to achieve their 50% effectiveness rate.  The details for that comparator model are in the table below:
 | Characteristic | Top 1%  | Top 5%  |
| :------------- |:-------------:| :-------------:|
| Clinical Success: | | |
|          Low effectiveness (25% reduction in hospitalizations) | $370 | $148 |
|          Moderate (50% reduction in hospitalizations) | $740 | $295 |
|          High (75% reduction in hospitalizations) | $1,110 | $443 |


Predictive models in general, including ours, are far from perfect.  The lack of perfection falls into several categories including, overlooking patients who are truly high risk and misidentifying patients as high risk who are not truly high risk.  Some models provide results that are worse than merely guessing at who is high risk, while others vastly exceed the probabilities of guessing.  Fortunately, there are ways to quantify whether a model is better or worse than guessing.  We tested our model and it is far better than guessing.  Given this, we're confident that our model will improve the ability of providers to support proactive management to keep people as healthy as possible and at home.




**If you have questions or suggestions please send an email to: [Covid Predictive Analytics](mailto:info@lumeris.com?subject=[GitHub]%20Covid%20Predictive%20Analytics).**



**Do you want model updates?  Click the following link if you want updates for when we make improvements to this model or have new ones available:  [UPDATES](mailto:info@lumeris.com?subject=[GitHub]%20Covid%20Predictive%20Analytics%20UPDATES).**





****Note: The COVID-19 predictive analytics program and the information described herein is not intended or implied to be a substitute for professional medical advice, diagnosis or treatment.****
