---
toc: true
layout: post
title: How does traffic congestion impact life satisfaction?
date: 2019-04-31 17:13:45 PDT
description: An inital econometric approach using a probit model written in Stata.
categories: [projects, Stata]
---

<br> 

##### **Summary**

In an attempt to venture solo, while an undergraduate student in economics, I worked on a probit model designed to study the effect traffic congestion has on satisfaction with work and place of residence. The question seemed straightforward, and the dependent variables were already either presented as binary or Likert scales that could be easily turned into one. Data came from the city of Cardiff, UK, in the form of an online questionnaire. I find that an increase in the perceived level of congestion makes individuals less happy with working and living in Cardiff. After adjusting for all the potential issues (like hetersokedasticity) this data might have, the results seem quite robust across two probit models. Included, also, is a link to the GitHub [repository](https://github.com/antoniojurlina/econometrics) containing the data I worked with and the Stata do-file.

##### **Part I**

**1. Data**

Council authority of Cardiff has, in cooperation with Cardiff Business Partnership, conducted an online survey, yielding 2,094 responses. After data clean-up process, 2,045 observations remained. While most variables were dropped, 17 were deemed significant enough to include (see Table 1). Missing values were filled with unconditional means, except in the case of the variables describing the primary method of transportation and proximity of a train station, where 19 and 32 observations with missing values were dropped, respectively. Variables regarding age, income and employment duration were acquired via drop-down answer menus on the survey, with each answer constituting a specific bracket. For the sake of continuity and interpretability, these answers were encoded as the midpoint value of each bracket. Since each set of brackets concluded with an open ended one (e.g. more than 10, 65+, etc.), and no middle point could be determined, each of those categories was encoded as the sum of the size of the largest bracket (for the relevant variable) and the bracket’s lower bound.

<br>

*Table 1 - Data Summary*

<table>
<colgroup>
<col width="30%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="30%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Average</th>
<th>Std. Dev.</th>
<th>Min</th>
<th>Max</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Quality of public transportation</td>
<td markdown="span">3.26</td>
<td markdown="span">1.09</td>
<td markdown="span">1</td>
<td markdown="span">5</td>
<td markdown="span">1 is very bad and 5 is very good</td>
</tr>
<tr>
<td markdown="span">Being able to get from place to place with little traffic (i.e. congestion)</td>
<td markdown="span">2.72</td>
<td markdown="span">1.04</td>
<td markdown="span">1</td>
<td markdown="span">5</td>
<td markdown="span">1 is very bad and 5 is very good</td>
</tr>
<tr>
<td markdown="span">Work proximity</td>
<td markdown="span">2.98</td>
<td markdown="span">1.28</td>
<td markdown="span">1</td>
<td markdown="span">5</td>
<td markdown="span">1 you like the most and 5 you dislike</td>
</tr>
<tr>
<td markdown="span">Travel within the city</td>
<td markdown="span">3.86</td>
<td markdown="span">0.96</td>
<td markdown="span">1</td>
<td markdown="span">5</td>
<td markdown="span">1 is not important and 5 is very important</td>
</tr>
<tr>
<td markdown="span">Ease of getting to work</td>
<td markdown="span">3.65</td>
<td markdown="span">1.02</td>
<td markdown="span">1</td>
<td markdown="span">5</td>
<td markdown="span">1 is not important and 5 is very important</td>
</tr>
<tr>
<td markdown="span">Overall satisfaction with life</td>
<td markdown="span">2.95</td>
<td markdown="span">0.98</td>
<td markdown="span">1</td>
<td markdown="span">5</td>
<td markdown="span">1 being highly satisfied and 5 being unsatisfied</td>
</tr>
<tr>
<td markdown="span">Number of years employed</td>
<td markdown="span">9.02</td>
<td markdown="span">5.46</td>
<td markdown="span">0.5</td>
<td markdown="span">15</td>
<td markdown="span">How long have you worked in your present employment</td>
</tr>
<tr>
<td markdown="span">Income</td>
<td markdown="span">25,748.18</td>
<td markdown="span">10,318.80</td>
<td markdown="span">5,720</td>
<td markdown="span">55,241</td>
<td markdown="span">How much do you get paid, before taxes</td>
</tr>
<tr>
<td markdown="span">Age</td>
<td markdown="span">41.50</td>
<td markdown="span">11.05</td>
<td markdown="span">21</td>
<td markdown="span">74</td>
<td markdown="span">How old are you</td>
</tr>
<tr>
<td markdown="span">Sex</td>
<td markdown="span">0.42</td>
<td markdown="span">0.49</td>
<td markdown="span">0</td>
<td markdown="span">1</td>
<td markdown="span">1 for male, 0 for female</td>
</tr>
<tr>
<td markdown="span">Relationship</td>
<td markdown="span">0.69</td>
<td markdown="span">0.46</td>
<td markdown="span">0</td>
<td markdown="span">1</td>
<td markdown="span">1 for in a relationship, 0 for single</td>
</tr>
<tr>
<td markdown="span">Children</td>
<td markdown="span">0.56</td>
<td markdown="span">0.49</td>
<td markdown="span">0</td>
<td markdown="span">1</td>
<td markdown="span">1 for having any number of children, 0 for none</td>
</tr>
<tr>
<td markdown="span">Overall satisfaction with place of residence</td>
<td markdown="span">0.66</td>
<td markdown="span">0.47</td>
<td markdown="span">0</td>
<td markdown="span">1</td>
<td markdown="span">1 for satisfied, 0 for unsatisfied</td>
</tr>
<tr>
<td markdown="span">Train station proximity</td>
<td markdown="span">2.72</td>
<td markdown="span">0.50</td>
<td markdown="span">1</td>
<td markdown="span">3</td>
<td markdown="span">Do you have a train station within 2 miles of your residence? 1 - Don't Know, 2 - No, 3 - Yes</td>
</tr>
<tr>
<td markdown="span">Education</td>
<td markdown="span">2.53</td>
<td markdown="span">1.10</td>
<td markdown="span">1</td>
<td markdown="span">6</td>
<td markdown="span">1 - High School, 2 - Associates degree, 3 – Bachelor’s Degree BA, BSc, 4 – Master’s Degree, 5 - Professional degree, 6 - PhD</td>
</tr>
<tr>
<td markdown="span">Primary mode of transportation</td>
<td markdown="span">1.36</td>
<td markdown="span">0.83</td>
<td markdown="span">0</td>
<td markdown="span">2</td>
<td markdown="span">0 - Hippie, 1 - Public, 2 - Drive</td>
</tr>
<tr>
<td markdown="span">Job satisfaction</td>
<td markdown="span">0.52</td>
<td markdown="span">0.50</td>
<td markdown="span">0</td>
<td markdown="span">1</td>
<td markdown="span">1 for satisfied, 0 for unsatisfied</td>
</tr>
</tbody>
</table>

<br>

**2. Methodology**

Literature review, focusing on measuring different sorts of satisfaction[^1] [^2] [^3], exemplifies various probit models and their appropriate usage. Binary nature of the dependent variables, together with the literature, was the main reason for choosing a probit-model approach. Given that the report attempts to clarify the effect congestion might have on satisfaction with place of residence (R) and working (J) in Cardiff, following two models represent its cornerstone:

$$J = \beta_0 + \beta_1 congestion + x \delta + \epsilon_1$$

$$R = \beta_0 + \beta_1 congestion + x \delta + \epsilon_2$$

Model 2 dependent variable was binary in the form the data was presented. However, Model 1 dependent variable had to be created from a set of Likert scale variables focusing on job satisfaction. Mean value for the set was created across individuals and compared to “3”, which is the neutral option on the answer sheet. Individuals above “3” were categorized as satisfied and the rest were categorized as unsatisfied. This way, a binary variable was generated, appropriate for use in a probit model. Cronbach’s alpha was used to determine the scale reliability coefficient (0.8471), indicating that the set of Likert-scale variables used are closely related as a group and representative of the shared concept.

Furthermore, for both Models 1 and 2, $$\beta$$ and $$\delta$$ represent coefficients to be estimated (see Table 1). Control variables were chosen based on two criteria: 1) those causally linked with the dependent variable through congestion and 2) those with a direct, causal link to satisfaction that were deemed as likely sources of severe endogeneity. Explanatory variables of interest consist of demographic characteristics[^4] [^5], congestion[^6] [^7] [^8] and those capturing satisfaction spillover[^9] [^10]. Stochastic terms are noted as $$\epsilon_1$$ and $$\epsilon_2$$. Also, since the probit models are being used, robust standard errors are reported and corrected for underlying (inherent) heteroskedasticity.

**3. Hypotheses**

--------------------------------------------------------------------------------------------

$$H_{01}$$: *Congestion has no significant impact on job satisfaction for residents of Cardiff.*

$$H_{A1}$$: *Congestion has a negative impact on job satisfaction for residents of Cardiff.*

--------------------------------------------------------------------------------------------

$$H_{02}$$: *Congestion has no significant impact on satisfaction with place of residence.*

$$H_{A2}$$: *Congestion has a negative impact on satisfaction with place of residence.*

--------------------------------------------------------------------------------------------


Models are demonstrating the conditional probability of a specific outcome occurring ($$Y_i = 1$$ meaning satisfied with job for Model 1 and satisfied with place of residence for Model 2), for 3 which the marginal effects show how a unit change for __congestion__ increases (or decreases) the probability of the given outcome occurring (et ceteris paribus).

<br> 

##### **Part II**

**1. Satisfaction with working and living in Cardiff**

--------------------------------------------------------------------------------------------

*I am quite confident that an increase in perceived level of congestion makes individuals 4% less likely to be satisfied with working in Cardiff.*

--------------------------------------------------------------------------------------------

*I am quite confident that an increase in perceived level of congestion makes individuals 5% less likely to be satisfied with living in Cardiff.*

--------------------------------------------------------------------------------------------

Tables 2 and 3 present more detailed results for these claims and state the possibility of a mistake being made. Numbers showing the impact of congestion on satisfaction with working and living in Cardiff are positive. This is a consequence of the way the survey was designed (1 is very bad and 5 is very good) and should be interpreted with an opposite sign. Literature review supports this finding. High levels of traffic congestion are associated with mental and physical stress[^11] [^12], which are further aggravated with the inability to complete daily routines [^13]. Furthermore, long work commutes cause residual stress in the workplace [^7] [^14] [^15]. Kahneman et al.[^8] found that work commutes were most frequently associated with negative feelings, out of all daily habits.

<br>

*Table 2 - Model 1 marginal effects*

<table>
<colgroup>
<col width="50%" />
<col width="10%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th> </th>
<th>Marginal effect</th>
<th>Standard error</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Quality of public transportation</td>
<td markdown="span"> </td>
<td markdown="span">0.049__***__</td>
<td markdown="span">0.011</td>
</tr>
<tr>
<td markdown="span">Being able to get from place to place with little traffic (i.e. congestion)</td>
<td markdown="span"> </td>
<td markdown="span">0.041__***__</td>
<td markdown="span">0.011</td>
</tr>
<tr>
<td markdown="span">Work proximity</td>
<td markdown="span"> </td>
<td markdown="span">0.023__***__</td>
<td markdown="span">0.008</td>
</tr>
<tr>
<td markdown="span">Travel within the city</td>
<td markdown="span"> </td>
<td markdown="span">0.007</td>
<td markdown="span">0.014</td>
</tr>
<tr>
<td markdown="span">Ease of getting to work</td>
<td markdown="span"> </td>
<td markdown="span">0.015</td>
<td markdown="span">0.013</td>
</tr>
<tr>
<td markdown="span">Overall satisfaction with life</td>
<td markdown="span"> </td>
<td markdown="span">-0.027__**__</td>
<td markdown="span">0.011</td>
</tr>
<tr>
<td markdown="span">Number of years employed</td>
<td markdown="span"> </td>
<td markdown="span">-0.001</td>
<td markdown="span">0.002</td>
</tr>
<tr>
<td markdown="span">Income</td>
<td markdown="span"> </td>
<td markdown="span">0.000__***__</td>
<td markdown="span">0.000</td>
</tr>
<tr>
<td markdown="span">Age</td>
<td markdown="span"> </td>
<td markdown="span">-0.008</td>
<td markdown="span">0.007</td>
</tr>
<tr>
<td markdown="span">Sex</td>
<td markdown="span"> </td>
<td markdown="span">-0.072__***__</td>
<td markdown="span">0.022</td>
</tr>
<tr>
<td markdown="span">Relationship</td>
<td markdown="span"> </td>
<td markdown="span">-0.015</td>
<td markdown="span">0.025</td>
</tr>
<tr>
<td markdown="span">Children</td>
<td markdown="span"> </td>
<td markdown="span">0.039</td>
<td markdown="span">0.026</td>
</tr>
<tr>
<td markdown="span">Overall satisfaction with place of residence</td>
<td markdown="span"> </td>
<td markdown="span">0.094__***__</td>
<td markdown="span">0.023</td>
</tr>
<tr>
<td markdown="span">Train station proximity</td>
<td markdown="span">Yes <br> No</td>
<td markdown="span">-0.204__\*\*\*__ <br> -0.219__***__</td>
<td markdown="span">0.058 <br> 0.027</td>
</tr>
<tr>
<td markdown="span">Education</td>
<td markdown="span"> </td>
<td markdown="span">0.024__**__</td>
<td markdown="span">0.010</td>
</tr>
<tr>
<td markdown="span">Primary mode of transportation</td>
<td markdown="span">Public <br> Drive</td>
<td markdown="span"> 0.010 <br> 0.076__***__</td>
<td markdown="span">0.033 <br> 0.027</td>
</tr>
</tbody>
</table>

_* - 90 % significance / ** - 95 % significance / *** - 99 % significance_

<br>

Public transportation plays a major role in mitigating the effects of congestion on satisfaction[^16]. However, while it is the most accepted solution to congestion[^17], it only works if it is appropriately scaled with the needs of the public[^18]. In Cardiff, an increase in perceived quality of public transportation makes individuals 4% (on average) more likely to feel satisfied with their work and 6% (on average) more likely to feel satisfied with where they live, holding everything else the same. Moreover, utilizing public transportation systems decreases the likelihood of being satisfied with place of residence by 6% (on average), holding everything else the same. Those most likely to utilize public transportation live inside the urban area while those living in the suburbs are more likely to drive to work. Therefore, this decrease in likelihood of being satisfied with the place of residence, based on public transport utilization, indicates a preference for living outside of the city. Furthermore, driving decreases the likelihood of being satisfied with working in Cardiff by 8% (on average), holding everything else the same (see Tables 2 and 3 for more detailed results and the likelihood of mistakes being reported). Finally, living close to work increases the likelihood of being satisfied with working in Cardiff by 2% (on average), holding everything else the same.

<br>

*Table 3 - Model 2 marginal effects*

<table>
<colgroup>
<col width="50%" />
<col width="10%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th> </th>
<th>Marginal effect</th>
<th>Standard error</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Quality of public transportation</td>
<td markdown="span"> </td>
<td markdown="span">0.062__***__</td>
<td markdown="span">0.010</td>
</tr>
<tr>
<td markdown="span">Being able to get from place to place with little traffic (i.e. congestion)</td>
<td markdown="span"> </td>
<td markdown="span">0.052__***__</td>
<td markdown="span">0.010</td>
</tr>
<tr>
<td markdown="span">Work proximity</td>
<td markdown="span"> </td>
<td markdown="span">0.003</td>
<td markdown="span">0.008</td>
</tr>
<tr>
<td markdown="span">Travel within the city</td>
<td markdown="span"> </td>
<td markdown="span">0.016</td>
<td markdown="span">0.013</td>
</tr>
<tr>
<td markdown="span">Ease of getting to work</td>
<td markdown="span"> </td>
<td markdown="span">-0.021__\*__</td>
<td markdown="span">0.012</td>
</tr>
<tr>
<td markdown="span">Overall satisfaction with workplace</td>
<td markdown="span"> </td>
<td markdown="span">0.086__***__</td>
<td markdown="span">0.020</td>
</tr>
<tr>
<td markdown="span">Number of years employed</td>
<td markdown="span"> </td>
<td markdown="span">0.002</td>
<td markdown="span">0.002</td>
</tr>
<tr>
<td markdown="span">Income</td>
<td markdown="span"> </td>
<td markdown="span">0.000__***__</td>
<td markdown="span">0.000</td>
</tr>
<tr>
<td markdown="span">Age</td>
<td markdown="span"> </td>
<td markdown="span">-0.006</td>
<td markdown="span">0.006</td>
</tr>
<tr>
<td markdown="span">Sex</td>
<td markdown="span"> </td>
<td markdown="span">-0.035</td>
<td markdown="span">0.020</td>
</tr>
<tr>
<td markdown="span">Relationship</td>
<td markdown="span"> </td>
<td markdown="span">0.106__***__</td>
<td markdown="span">0.023</td>
</tr>
<tr>
<td markdown="span">Children</td>
<td markdown="span"> </td>
<td markdown="span">0.034</td>
<td markdown="span">0.024</td>
</tr>
<tr>
<td markdown="span">Overall satisfaction with life</td>
<td markdown="span"> </td>
<td markdown="span">0.002</td>
<td markdown="span">0.010</td>
</tr>
<tr>
<td markdown="span">Train station proximity</td>
<td markdown="span">Yes <br> No</td>
<td markdown="span">0.212__\*\*\*__ <br> 0.121</td>
<td markdown="span">0.075 <br> 0.077</td>
</tr>
<tr>
<td markdown="span">Education</td>
<td markdown="span"> </td>
<td markdown="span">0.019__***__</td>
<td markdown="span">0.009</td>
</tr>
<tr>
<td markdown="span">Primary mode of transportation</td>
<td markdown="span">Public <br> Drive</td>
<td markdown="span"> -0.055__\*__ <br> -0.030</td>
<td markdown="span">0.032 <br> 0.026</td>
</tr>
</tbody>
</table>

_* - 90 % significance / ** - 95 % significance / *** - 99 % significance_

<br>


While there are negative effects of congestion on satisfaction with working and living in Cardiff, those effects are smaller than anticipated. This could be because working during a period of recession or post-recession[^19] recovery causes enough satisfaction that lessens the burdens of daily commute[^13]. Additionally, as noted by Stokols et al. [^11], congestion is only relevant when it significantly differs from the expected traffic levels. If people are exposed to similar congestion levels every day, negative effects are diminished.

**2. Policy relevance**

Those that live within 2 miles of a train station and those that live further away, are about 20% (on average) less likely to feel satisfied with working in Cardiff, holding everything else the same. Moreover, those that live within 2 miles of a train station are about 20% (on average) more likely to be satisfied with their place of residence, holding everything else the same (see Tables 2 and 3). Since most of survey respondents (74%) live within 2 miles of a train station and more than half (59%) drive to work (not necessarily overlapping groups), it could be assumed that there is a negative perception of public transportation and a strong tendency to avoid it. This, in addition to reported congestion and public transportation effects,  indicates that policy under consideration by the council authority of Cardiff needs to address both public transportation and congestion. Building a new metro system as well as introducing a congestion charge for those driving into the center of Cardiff should be packaged together. Literature review supports this claim, as well[^20].

**3. Limitations**

Variability of answers provided was reduced when most missing values were filled with average values for the category. Further adding to this issue was the design of the survey itself. Answers that were selected from a drop-down menu were presented as brackets, chosen by the surveyor. This eliminated most of the effect that individuals with answers far away from the average would have. Moreover, for privacy reasons, answers stating the specific place of residence were excluded, introducing additional lack of clarity.

Due to model choice, differences between observed and predicted and expected and predicted values for variables of interest were not uniform across the data. This means that I was more likely to falsely perceive an observation as insignificant and reject its validity and explanatory power. Finally, I wish to point out the possibility that any variable not considered stands a chance of being correlated with variables I did consider (e.g. place of residence might be linked with public transport utilization, commute satisfaction and satisfaction with living and working in Cardiff). By omitting any such variable, I introduced the likelihood of overstating the effects considered variables have on overall satisfaction.

**4. GitHub repository**

For data, code, and similar projects, visit [https://github.com/antoniojurlina/econometrics](https://github.com/antoniojurlina/econometrics).

<br>

##### **Stata code**

{% highlight stata %} 
    cls
    clear //clear previous data
    
    use CBP_survey.dta //choose data

    // following commands represent my data clean-up process
    ////////////////////////////////////////////////////////////////////
    encode Q3, generate(yrs_employed)
    replace yrs_employed = 0.5 if Q3 == "Less than 1 year"
    replace yrs_employed = 2 if Q3 == "1 to 3 years"
    replace yrs_employed = 4 if Q3 == "3 to 5 years"
    replace yrs_employed = 7.5 if Q3 == "5 to 10 years"
    replace yrs_employed = 15 if Q3 == "More than 10 years"

    encode Q4, generate(income)
    replace income = 5720 if Q4 == "£0 - £11,440 per year"
    replace income = 12480.5 if Q4 == "£11,441 - £13,520 per year"
    replace income = 14820.5 if Q4 == "£13,521 - £16,120 per year"
    replace income = 17420.5 if Q4 == "£16,121 - £18,720 per year"
    replace income = 20540.5 if Q4 == "£18,721 - £22,360 per year"
    replace income = 25220.5 if Q4 == "£22,361 - £28,080 per year"
    replace income = 31720.5 if Q4 == "£28,081 - £35,360 per year"
    replace income = 40300.5 if Q4 == "£35,361 - £45,240 per year"
    replace income = 55241 if Q4 == "£45,241 or more per year"
    drop if Q5 == "25- 40" | Q5 == "25- 41" | Q5 == "25- 42" | Q5 == "25- 43"
    drop if Q5 == "25- 44" | Q5 == "25- 45" | Q5 == "25- 46" | Q5 == "25- 47" | Q5 == "25-
    48" | Q5 == "25- 49"

    encode Q5, generate(age)
    replace age = 21 if Q5 == "18- 24"
    replace age = 32 if Q5 == "25- 39"
    replace age = 47 if Q5 == "40- 54"
    replace age = 59.5 if Q5 == "55- 64"
    replace age = 74 if Q5 == "65"
    generate age_sq = age * age
    
    label define sex 1 "Male" 0 "Female"
    encode Q6, generate(sex)
    
    generate relationshipy = Q7
    replace relationshipy = "Yes" if Q7 == "Co-habiting"
    replace relationshipy = "Yes" if Q7 == "Married"
    replace relationshipy = "No" if Q7 == "Single"
    label define relationship 1 "Yes" 0 "No"
    encode relationshipy, generate(relationship)
    drop relationshipy Q7

    label define children 1 "Yes" 0 "No"
    encode Q8, generate(children)
    
    rename Q14h pub_trans_quality
    rename Q14i congestion

    generate satisfactiony = Q15
    label define satisfaction 1 "Satisfied" 0 "Unsatisfied"
    encode satisfactiony, generate(satisfaction)
    drop satisfactiony Q15

    encode Q19, generate(train)

    label define education 1 "High School" 2 "Associates degree" 3 "Bachelors Degree BA,
    BSc" 4 "Masters Degree" 5 "Professional degree" 6 "PhD"
    encode Q20, generate(education)

    rename Q30c travel_importance

    rename Q30f work_travel_ease

    rename Q31 life_satisfaction

    summarize pub_trans_quality
    replace pub_trans_quality = r(mean) if pub_trans_quality == .
    summarize congestion
    replace congestion = r(mean) if congestion == .
    summarize yrs_employed
    replace yrs_employed = r(mean) if yrs_employed == .
    summarize income
    replace income = r(mean) if income == .
    summarize age
    replace age = r(mean) if age == .
    summarize sex
    replace sex = r(mean) if sex == .
    summarize relationship
    replace relationship = r(mean) if relationship == .
    summarize satisfaction
    replace satisfaction = r(mean) if satisfaction == .
    summarize train
    drop if train == .
    summarize education
    replace education = r(mean) if education == .
    summarize life_satisfaction
    replace life_satisfaction = r(mean) if life_satisfaction == .
    summarize children
    replace children = r(mean) if children == .
    summarize work_travel_ease
    replace work_travel_ease = r(mean) if work_travel_ease == .
    summarize travel_importance
    replace travel_importance = r(mean) if travel_importance == .
    
    generate transport1 = Q16
    replace transport1 = "Hippie" if Q16 ==  "Walk" | Q16 == "Cycle"
    replace transport1 = "Public" if Q16 == "Train" | Q16 == "Bus"
    replace transport1 = "Drive" if Q16 == "Car /Motorcycle"
    label define trans1 0 "Hippie" 1 "Public" 2 "Drive"
    encode transport1, generate(trans1)
    drop if trans1 == .

    summarize Q28a
    replace Q28a = r(mean) if Q28a == .
    summarize Q28b
    replace Q28b = r(mean) if Q28b == .
    summarize Q28c
    replace Q28c = r(mean) if Q28c == .
    summarize Q28d
    replace Q28d = r(mean) if Q28d == .
    summarize Q28e
    replace Q28e = r(mean) if Q28e == .
    summarize Q28f
    replace Q28f = r(mean) if Q28f == .
    summarize Q28g
    replace Q28g = r(mean) if Q28g == .
    summarize Q29a
    replace Q29a = r(mean) if Q29a == .
    summarize Q29b
    replace Q29b = r(mean) if Q29b == .
    summarize Q29c
    replace Q29c = r(mean) if Q29c == .
    summarize Q29d
    replace Q29d = r(mean) if Q29d == .
    summarize Q29e
    replace Q29e = r(mean) if Q29e == .
    summarize Q29f
    replace Q29f = r(mean) if Q29f == .
    summarize Q29g
    replace Q29g = r(mean) if Q29g == .
    summarize Q29h
    replace Q29h = r(mean) if Q29h == .
    rename Q29c work_proximity

    generate job = (Q28a + Q28b + Q28c + Q28d + Q28e + Q28f + Q28g)/7
    generate job_satisfaction = job
    summarize job
    replace job_satisfaction = 1 if job > 3
    replace job_satisfaction = 0 if job <= 3

    alpha Q28a-Q28g
    drop Q3 Q4 Q5 Q6 Q8 Q19 Q20
    drop Q1 Q2 Q11 Q13 Q12 Q14a Q14b Q14c Q14d Q14e Q14f Q14g Q14j Q14k Q14l Q14m Q14n
    Q14o
    drop Q17 Q21 Q22 Q23a Q23b Q24 Q25 Q26 Q27 Q28a Q28b Q28c Q28d Q28e Q28f Q28g
    drop Q29a Q29b Q29d Q29e Q29f Q29g Q29h Q30a Q30b Q30d Q30e Q30g Q30h Q30i Q30j Q30k
    drop Q32 Q33
    drop job Q16 Q18 transport1
    ////////////////////////////////////////////////////////////////////
    // end of data clean-up process
    
    summarize

    probit job_satisfaction satisfaction pub_trans_quality children congestion
    life_satisfaction yrs_employed travel_importance work_travel_ease work_proximity
    income age age_sq sex relationship i.train education i.trans1,robust

    margins, dydx(*)

    probit satisfaction job_satisfaction pub_trans_quality children congestion
    life_satisfaction yrs_employed travel_importance work_travel_ease work_proximity
    income age age_sq sex relationship i.train education i.trans1,robust

    margins, dydx(*)

    save CBP_survey_clean.dta, replace
{% endhighlight %}

<br> 


##### **Works Cited**

[^1]:Blanchflower, D. G., & Oswald, A. J. (2004). Well-being over time in Britain and the USA. Journal of Public Economics, 88, 1359-1386. doi:10.1016/S0047-2727(02)00168-8

[^2]:Fetai, B., Abduli, S., & Qirici, S. (2015). An Ordered Probit Model of Job Satisfaction in the Former Yugoslav Republic of Macedonia. Procedia Economics and Finance, 33, 350- 357. doi:10.1016/S2212-5671(15)01719-0

[^3]:Rayton, B. A. (2006). Examining the interconnection of job satisfaction and organizational commitment: An application of the bivariate probit model. Int. J. of Human Resource Management, 17(1), 139-154. doi:10.1080/09585190500366649

[^4]:Richey, S. (2012). Determinants of Community Satisfaction and its Relative Importance for Life Satisfaction.

[^5]:Auh, S., & Cook, C. (2009). Quality of Community Life Among Rural Residents: An Integrated Model. Social Indicators Research, 94(3), 377-389.

[^6]:Lipsetz, D. A. (2000). Residential Satisfaction: Identifying the differences between suburban-ites and urbanites. Columbus: Ohio State University.

[^7]:Novaco, R. W., Stokols, D., & Milanesi, L. (1990). Objective and subjective dimensions of travel impedance as determinants of commuting stress. American Journal of Community Psychology, 18, 231–257.

[^8]:Kahneman, D., Krueger, A. B., Schkade, D., Schwarz, N., & Stone, A. (2004). A survey method for characterizing daily life experience: The day reconstruction method (DRM). Science, 306, 1776–1780.

[^9]:Shimon L. Dolan & Eric Gosselin, 2000. "Job satisfaction and life satisfaction: Analysis of a reciprocal model with social demographic moderators," Economics Working Papers 484, Department of Economics and Business, Universitat Pompeu Fabra.

[^10]:Ilies, R., Wilson, K. S., & Wagner, D. T. (2009). The Spillover of Daily Job Satisfaction Onto Employees' Family Lives: The Facilitating Role of Work-Family Integration. Academy of Management Journal, 52(1), 87-102. doi:10.5465/AMJ.2009.36461938

[^11]:Stokols, D., Novaco, R. W., Stokols, J., & Campbell, J. (1978). Traffic Congestion, Type A Behavior, and Stress. Journal of Applied Psychology, 63(4), 467-480. doi:10.1037/0021- 9010.63.4.467

[^12]:Novaco, R. W., & Gonzales, O. I. (2009). Commuting and well-being. In Y. Amichai- Hamburger (Ed.), Technology and well-being (pp. 174–205). New York: Cambridge University Press.

[^13]:Olsson, L. E., Garling, T., Ettema, D., Friman, M., & Fujii, S. (2013). Happiness and Satisfaction with Work Commute. Soc Indic Res, 111, 255-263. doi:10.1007/s11205-012- 0003-2

[^14]:Glass, D. C., Singer, J., & Pennebaker, J. Behavioral and physiological effects of uncontrollable environmental events. In D. Stokols (Ed.), Perspectives on environment and behavior. New York: Plenum, 1977.

[^15]:Sherrod, D. R. Crowding, perceived control, and behavioral aftereffects. Journal of Applied Social Psychology, 1974, 4, 171-186.

[^16]:Kottenhoff, K., & Freij, K. B. (2009). The role of public transport for feasibility and acceptability of congestion charging – The case of Stockholm. Transportation Research Part A, 43, 297-305. doi:10.1016/j.tra.2008.09.004

[^17]:Schlag, B., Teubel, U., 1997. Public acceptability of transport pricing. IATSS Research, 21(2).

[^18]:Transport for London (TfL), 2005. Central London Congestion Charging. Impacts monitoring, Third Annual Report, April 2005

[^19]:Gross Domestic Product Preliminary Estimate: Q4 2014 (pp. 1-20, Rep.). (2015). Office for National Statistics.

[^20]:Jaensirisak, S., Wardman, M., May, A.D., 2005. Explaining variations in public acceptability of road pricing schemes. Journal of Transport Economics and Policy 39, 127–154.



