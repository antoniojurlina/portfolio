---
toc: true
layout: post
title: Economic Freedom and Growth
date: 2019-12-07 15:12:31 PDT
description: Using an index to determine potential economic relationships (with code in EViews).
categories: [projects, EViews]
---

<br> 

##### **Summary**

The Economic Freedom Index is published yearly by the Fraser Institute. Latest edition is comprised of five areas used to construct a scale of economic freedom, with each area rated on a scale of 1 to 10. In this project, I use this index along with GDP data for 52 countries, ranging between 1970 and 2015, to estimate the relationship between index components and GDP growth. I begin by running a preliminary OLS panel model, followed by several statistical tests designed to probe for the presence of multicollinearity, autocorrelation, heteroskedastisicty and effectiveness of my proposed approach. Eventually, I settled on a feasible generalized least squares model for panel data. The project concludes with a strong indication that the freedom to trade interantionally and sound money are important predictors of GDP growth. Included, also, is a link to the GitHub [repository](https://github.com/antoniojurlina/economic_freedom_and_growth) containing the data I worked with and the EViews files. I chose to work in EViews in order to learn how it works, while I was still builidng my econometrics toolset. After having dealt with everything it has to offer, it is clear to me that there is much better software and statistical languages out there. I would recommend R and Stata for this (and any other similar) project, over EViews.  

**1. Review**

Adam Smith[^1], with the publication of the Wealth of the Nations, instigated a debate around the causes of economic growth. Although mercantilism had dominated the period of late Renaissance in Europe and powerful merchants had built routes based on the belief that trade would benefit them greatly[^2], it wasn’t until Smith’s work had been published that attention turned towards free trade, production advantage, economies of scale and institutions (or lack thereof) intended to orchestrate this in unison. This work was further expanded by Ricardo[^3], who ushered the realization that benefits can be acquired even by those countries that are not the most efficient suppliers around. Eventually, economic growth was taken up by the likes of Solow[^4], who had expanded the Harrod-Domar model[^5] [^6] so that growth is represented as function of capital, labor and technology (with more optimistic limitations).

Following Solow’s work, Kuznets[^7] argued that, while necessary, technology itself wouldn’t suffice in producing measurable growth. He claimed that growth would induce change along the way (something along the lines of Schumpeter[^8]), and all the conflict would have to be resolved cost-effectively through institutions designed to do so. Only then, with conflict resolution costs smaller than benefits of growth, would long-term economic progress occur. Finally, there is Milton Friedman[^9] , crafting institutional approach along more libertarian lines, arguing for a government that is there to promote safety, monopolize violence and influence the economy through the money supply. He also argued for the removal of major trade barriers as the only way to introduce stable equilibria.

More recent work had found that property rights, monetary stability, and freedom to trade internationally all have visible impact on growth [^10] [^11] [^12] [^13] [^14]. Additionally, previously underdeveloped, closed-off, and/or countries with centralized economies had all undergone drastic economic changes (in the positive direction) upon loosening institutional grips, opening towards the world and openly stifling hierarchical corruption. This can be observed in the economies of Taiwan, Singapore and Hong Kong, with China following closely upon realizing its neighbors had adopted a slightly more laissez-faire approach and experienced significant growth[^15].

Market equilibria inside closed economies adjust themselves according to supply and demand interactions and institutional involvement. With constraints effectively placed to incorporate the costs of most inefficiencies, effective resource allocation is determined on aggregate, through interactions of all the individual participants. With current levels of globalization and economic interconnectedness, eliminating quotas and barriers results in a world- wide market place with freer price points. This, much like on a single-country level, is an amalgamation of countless interactions producing an inherent equilibrium. Depending on the institutional restrictions imposed by all the individual players (and their size), this equilibrium will inch towards comparative efficiency, fostering more growth. With the readjustment of the production possibility frontiers to accommodate the new demand and supply pressures, Mundell[^16] and Fleming[^17] identify certain factors affecting GDP levels of open-economies: fiscal policy, monetary policy, and foreign trade shifts. Even if Leonteif’s[^18] observations (the failure of H-O theorem) hold across countries, there is still an adjustment shift according to the world market.

Finally, Gwartney, Lawson, & Block[^19], have created an index consisting of all these factors influencing growth. The index rates the economic freedom of countries on a scale of 1 to 10, with 10 indicating a country that is completely free economically. The Economic Freedom Index (from here on referred to as EFI), is comprised of separate indices for the size of the government, legal system and property rights, freedom to trade internationally, stability of the monetary policy, and the number of regulatory obstacles. This index, and similar ones (such as the one produced by the Heritage Foundation) have been used in several ways in order to determine a possible causal link between economic freedom and economic growth[^20] [^21] [^22] [^23] [^24].

**2. Econometric Model**

The EFI is published yearly by the Fraser Institute. Latest edition [^25] is comprised of five areas used to construct a scale of economic freedom, with each area rated on a scale of 1 to 10. *Size of Government* focuses on individual choice-making through market interactions, as opposed to relying on policy making. Countries with low levels of government
spending, a smaller government enterprise sector, and lower tax rates earn the highest ratings in this area. *Legal System and Property Rights* focuses on unbiased judiciary systems, effective protection of private property and impartial enforcement of the law. Countries that satisfy these categories the best, score the highest in this area. *Sound Money* refers to money with a stable purchasing power over time. Countries that score high in this area, must follow policies and adopt institutions that lead to low rates of inflation and avoid regulations that limit the ability to use alternative currencies. *Freedom to Trade Internationally* focuses on the level and ease of interactions across the borders. To score high in this area, a country must have “low tariffs, easy clearance and efficient administration of customs, a freely convertible currency, and few controls on the movement of physical and human capital”[^25]. *Regulation* measures the access into markets and restrictions around economic interactions. To score high in this area, countries need to relax regulatory constraints around labor, product and credit markets. For more detail on the construction of each of these areas, see Appendix 1.

The model under consideration uses these areas as explanatory variables. This should help in determining the causal link, or at least, the sign of the relationship, between economic growth and factors determining classical assumptions around economic freedom. The model is as follows

$$GROWTH_{i,(t-5,t)} = \beta_0 + \beta_1GOV_{i,t-5} + \beta_2LEGAL_{i,t-5} + \beta_3MONEY_{i,t-5} + \beta_4TRADE_{i,t-5} + \beta_5REGULATION_{i,t-5} + \beta_6log(GDP)_{i,t-5} + \varepsilon_{i,t}$$

where the dependent variable represents growth of log GDP per capita over a 5-year period, $$\beta_1$$ through $$\beta_5$$ represent the five areas of EFI at the beginning of each 5-year period, $$\beta_6$$ represents the log of GDP per capita at the beginning of each 5-year period and $$\varepsilon$$ represents the error term. Each of these variables is defined across countries (*i*) and time (*t, t-5*), making this a fixed effects model. Country-level heterogeneity carries a lot of unobservable variables, so with fixed effects, the remaining variation can be used to causally identify the relationships of interest. The null hypotheses are that there is no significant effect between the five areas of EFI and growth of GDP per capita. The alternative hypotheses are that the higher a country scores in all five areas of EFI, the higher the log growth of GDP per capita, in accordance with classical assumptions.

**3. Data**

The data set used for this project was created using three different sources. The EFI was obtained from the Fraser Institute[^25], data on GDP per capita was obtained from the World Bank Group[^26], recorded in 2018 US dollars. GDP was chosen to be per capita specifically to avoid any issues with population size differences among countries. Finally, a dummy variable on whether nations are members of the OECD was created using the list of member nations from the OECD website[^27]. These variables are organized as panel data with 52 countries, ranging from 1970 to 2015. Countries were selected based on data availability, to avoid missing values that would result in omitted observations during estimation. Summary statistics (minimum, maximum, mean, and standard deviation) for all variables can be seen in Appendix 2 and graphs representing these variables, faceted by country, can be seen in Appendix 3.

<br>

*Figure 1 - Economic Freedom of the World 2015 Report (The Fraser Institute)*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/economic-freedom.jpeg" data-zoomable>
    </div>
</div>

<br>

**4. Preliminary Estimates**

The first step of the analysis revolved around estimating an OLS version of the model in Equation 1. Results were estimated in three different ways – by grouping all countries together, by using only OECD member countries and by using only non-member countries. This dummy variable was introduced due to the special economic relationships fostered by OECD member nations, to stabilize unobservable heterogeneity between members and non-members. The use of the dummy variable was also inspired by previous research, especially that of Mankiw, Romer and Weil[^28]. Detailed results are presented in Figure 2.

<br>

*Figure 2 - Panel Least Squares Regressions* <br>
*Dependent variable: log growth of GDP per capita (1975 - 2015)*

<table>
<colgroup>
<col width="50%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Pooled</th>
<th>non-OECD</th>
<th>OECD</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Intercept</td>
<td markdown="span">0.441 <br> (0.034)</td>
<td markdown="span">0.372 <br> (0.054)</td>
<td markdown="span">0.526 <br> (0.046)</td>
</tr>
<tr>
<td markdown="span">log GDP per capita</td>
<td markdown="span">__-0.071\*\*\*__ <br> (0.005)</td>
<td markdown="span">__-0.072\*\*\*__ <br> (0.009)</td>
<td markdown="span">__-0.072\*\*\*__  <br> (0.007)</td>
</tr>
<tr>
<td markdown="span">Size of Government</td>
<td markdown="span">0.005 <br> (0.004)</td>
<td markdown="span">0.006 <br> (0.006)</td>
<td markdown="span">0.002 <br> (0.005)</td>
</tr>
<tr>
<td markdown="span">Legal System & Property Rights</td>
<td markdown="span">-0.003 <br> (0.003)</td>
<td markdown="span">-0.005 <br> (0.005)</td>
<td markdown="span">-0.002 <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">Sound Money</td>
<td markdown="span">-0.004 <br> (0.002)</td>
<td markdown="span">__-0.006\*\*__ <br> (0.003)</td>
<td markdown="span">-0.000 <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">Regulation</td>
<td markdown="span">__0.024\*\*\*__ <br> (0.005)</td>
<td markdown="span">__0.024\*\*\*__ <br> (0.008)</td>
<td markdown="span">__0.025\*\*\*__ <br> (0.007)</td>
</tr>
<tr>
<td markdown="span">Freedom to Trade Internationally</td>
<td markdown="span">__0.013\*\*\*__ <br> (0.003)</td>
<td markdown="span">__0.017\*\*\*__ <br> (0.004)</td>
<td markdown="span">__0.008\*\*\*__ <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">Observations</td>
<td markdown="span">430</td>
<td markdown="span">189</td>
<td markdown="span">241</td>
</tr>
<tr>
<td markdown="span">Cross-sections <br> (periods)</td>
<td markdown="span">52 <br> (9)</td>
<td markdown="span">24 <br> (9)</td>
<td markdown="span">28 <br> (9)</td>
</tr>
<tr>
<td markdown="span">$$R^2$$</td>
<td markdown="span">0.44</td>
<td markdown="span">0.41</td>
<td markdown="span">0.46</td>
</tr>
<tr>
<td markdown="span">F-statistic <br> (p-value)</td>
<td markdown="span">5.03 <br> (0.000)</td>
<td markdown="span">3.88 <br> (0.000)</td>
<td markdown="span">5.4 <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Durbin-Watson statistic</td>
<td markdown="span">2.67</td>
<td markdown="span">2.41</td>
<td markdown="span">2.92</td>
</tr>
</tbody>
</table>

_* - 90 % significance / ** - 95 % significance / *** - 99 % significance_

<br>


As Figure 2 shows, *Freedom to Trade Internationally* and *Regulation* have a significant effect across three model runs, and *Sound Money* has a significant effect for non-OECD countries only. Any one-point increase in the *Freedom to Trade Internationally* index is correlated with a 1.3 % (pooled), 1.7 % (non-OECD) and 0.8% (OECD) increase in the growth rate of GDP per capita, on average. Any one-point increase in the *Regulation* index is correlated with a 2.4 % (pooled), 2.4 % (non-OECD) and 2.5% (OECD) increase in the growth rate of GDP per capita, on average. Finally, any one-point increase in the *Sound Money* index for non-OECD countries is correlated with a 0.6 % decrease in the growth rate of GDP per capita, on average. To reiterate, an increase in index score across all three of the variables mentioned indicates an increase in economic freedom, as per EFI design.

**5. Testing**

Although the results seem significant at first glance, there are many causes for concern regarding the validity of parameter identification in a simple OLS approach to this data set. This section is dedicated to discovering possible violations of Gauss-Markovian assumptions and checking the validity of model design.

###### *Multicollinearity*

One of the primary issues with deconstructing an index is the causal relationships between some of the subcomponents. It seems reasonable to assume that size of the government, amount of regulation and property rights are correlated with one another. This can result in multicollinearity among explanatory variables, affecting the robustness of the estimates. The correlation matrix in Figure 3 indicates strong correlation (over 0.5) between the five areas of the EFI. This serves as a rough estimate of multicollinearity present in the model, indicating that estimates need to be interpreted conservatively. For future reference, multicollinearity should be further confirmed by estimating the model and changing the data slightly, many times over, seeing how the estimates react. Also, dependent variables should be dropped, and model estimated without some, to see the effect on estimates. Significant estimate changes in both these approaches would indicate a presence of multicollinearity. Finally, a variance inflation factor should be calculated, as it gives an exact numeric value for evaluation. Since multicollinearity doesn’t change the BLUE properties of the model, and due to time limitations, the model will be left as is.

<br>

*Figure 3. Correlation Matrix*

<table>
<colgroup>
<col width="30%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>log growth of GDP per capita</th>
<th>GDP per capita</th>
<th>Size of Government</th>
<th>Legal System & Property Rights</th>
<th>Sound Money</th>
<th>Regulation</th>
<th>Freedom to Trade Internationally</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">**log growth of GDP per capita**</td>
<td markdown="span">1.000</td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">**GDP per capita**</td>
<td markdown="span">-0.089</td>
<td markdown="span">1.000</td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">**Size of Government**</td>
<td markdown="span">0.042</td>
<td markdown="span">-0.162</td>
<td markdown="span">1.000</td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">**Legal System & Property Rights**</td>
<td markdown="span">-0.007</td>
<td markdown="span">0.686</td>
<td markdown="span">-0.185</td>
<td markdown="span">1.000</td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">**Sound Money**</td>
<td markdown="span">-0.029</td>
<td markdown="span">0.533</td>
<td markdown="span">0.043</td>
<td markdown="span">0.583</td>
<td markdown="span">1.000</td>
<td markdown="span"> </td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">**Regulation**</td>
<td markdown="span">-0.035</td>
<td markdown="span">0.590</td>
<td markdown="span">0.239</td>
<td markdown="span">0.626</td>
<td markdown="span">0.649</td>
<td markdown="span">1.000</td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">**Freedom to Trade Internationally**</td>
<td markdown="span">0.030</td>
<td markdown="span">0.495</td>
<td markdown="span">0.079</td>
<td markdown="span">0.722</td>
<td markdown="span">0.644</td>
<td markdown="span">0.668</td>
<td markdown="span">1.000</td>
</tr>
</tbody>
</table>

<br>

###### *Autocorrelation*

Since the primary statistical software used in estimation doesn’t allow for direct autocorrelation testing, several indirect ways shall be explored, to detect any potential autocorrelation. First, simply observing graphs of residuals from the three OLS approaches, can be very indicative of any potential autocorrelation. Indeed, by looking at the attached graphs (see Appendix 4), it seems highly possible that autocorrelation is present. The order is harder to discern. Furthermore, Durbin-Watson statistic can be used for identification of first-order autocorrelation. The autocorrelation coefficient, $$\rho$$, (with stable errors) is located on the interval −1 < $$\rho$$ < 1, and the Durbin-Watson test statistic is approximately equal to 4, 2 and 0, for the $$\rho$$ values of -1, 0, and 1, respectively. Therefore, the d-statistic serves as a rough guide of first order autocorrelation. In the first (pooled) OLS estimate, d is 2.7. Being further from 2 (in the positive direction), indicates that autocorrelation is more likely. The same can be said for the d values of the second (non- OECD) and third (OECD) OLS estimates, for which d values are 2.4 and 2.9, respectively (see Figure 2). Finally, a feasible GLS model is estimated, with the addition of autocorrelation parameters. These are added individually, starting with a parameter for first order autocorrelation, up to the point where their p-values start to become insignificant at conventional confidence levels (see Figure 4). The results indicate a presence of first and second order autocorrelation, with any subsequent level added failing to pass as significant or reducing the number of observations past the optimal point.

<br>

*Figure 4. FGLS Regression - Sensitivity Testing for Autocorrelation* <br>
*Dependent variable: log growth of GDP per capita (1975 - 2015)* 

<table>
<colgroup>
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Pooled</th>
<th>non-OECD</th>
<th>OECD</th>
<th>Pooled</th>
<th>non-OECD</th>
<th>OECD</th>
<th>Pooled</th>
<th>non-OECD</th>
<th>OECD</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Intercept</td>
<td markdown="span">0.365 <br> (0.032)</td>
<td markdown="span">0.320 <br> (0.059)</td>
<td markdown="span">0.430 <br> (0.038)</td>
<td markdown="span">0.239 <br> (0.024)</td>
<td markdown="span">0.225 <br> (0.049)</td>
<td markdown="span">0.286 <br> (0.022)</td>
<td markdown="span">0.164 <br> (0.062)</td>
<td markdown="span">0.112 <br> (0.092)</td>
<td markdown="span">0.240 <br> (0.106)</td>
</tr>
<tr>
<td markdown="span">log GDP per capita</td>
<td markdown="span">-0.063*** <br> (0.006)</td>
<td markdown="span">-0.065*** <br> (0.011)</td>
<td markdown="span">-0.064*** <br> (0.007)</td>
<td markdown="span">-0.037*** <br> (0.004)</td>
<td markdown="span">-0.048*** <br> (0.010)</td>
<td markdown="span">-0.031*** <br> (0.004)</td>
<td markdown="span">-0.023*** <br> (0.009)</td>
<td markdown="span">0.016 <br> (0.017)</td>
<td markdown="span">-0.032*** <br> (0.009)</td>
</tr>
<tr>
<td markdown="span">Size of Government</td>
<td markdown="span">0.008 <br> (0.003)</td>
<td markdown="span">0.006 <br> (0.006)</td>
<td markdown="span">0.009 <br> (0.004)</td>
<td markdown="span">0.005* <br> (0.003)</td>
<td markdown="span">0.006 <br> (0.005)</td>
<td markdown="span">0.005** <br> (0.002)</td>
<td markdown="span">0.000 <br> (0.005)</td>
<td markdown="span">-0.002 <br> (0.008)</td>
<td markdown="span">0.003 <br> (0.005)</td>
</tr>
<tr>
<td markdown="span">Legal System & Property Rights</td>
<td markdown="span">0.001 <br> (0.003)</td>
<td markdown="span">-0.004 <br> (0.006)</td>
<td markdown="span">0.006 <br> (0.004)</td>
<td markdown="span">-0.001 <br> (0.003)</td>
<td markdown="span">-0.004 <br> (0.005)</td>
<td markdown="span">0.000 <br> (0.003)</td>
<td markdown="span">0.000 <br> (0.005)</td>
<td markdown="span">0.000* <br> (0.007)</td>
<td markdown="span">0.000 <br> (0.008)</td>
</tr>
<tr>
<td markdown="span">Sound Money</td>
<td markdown="span">-0.004** <br> (0.002)</td>
<td markdown="span">-0.007** <br> (0.003)</td>
<td markdown="span">-0.001 <br> (0.003)</td>
<td markdown="span">-0.009*** <br> (0.002)</td>
<td markdown="span">-0.010*** <br> (0.003)</td>
<td markdown="span">-0.006*** <br> (0.002)</td>
<td markdown="span">-0.008*** <br> (0.003)</td>
<td markdown="span">-0.006 <br> (0.004)</td>
<td markdown="span">-0.008* <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">Regulation</td>
<td markdown="span">0.019*** <br> (0.005)</td>
<td markdown="span">0.024*** <br> (0.009)</td>
<td markdown="span">0.014** <br> (0.006)</td>
<td markdown="span">0.009** <br> (0.004)</td>
<td markdown="span">0.019** <br> (0.009)</td>
<td markdown="span">0.003 <br> (0.003)</td>
<td markdown="span">0.003 <br> (0.007)</td>
<td markdown="span">0.012 <br> (0.016)</td>
<td markdown="span">0.005 <br> (0.006)</td>
</tr>
<tr>
<td markdown="span">Freedom to Trade Internationally</td>
<td markdown="span">0.013*** <br> (0.003)</td>
<td markdown="span">0.016*** <br> (0.005)</td>
<td markdown="span">0.008*** <br> (0.003)</td>
<td markdown="span">0.017*** <br> (0.002)</td>
<td markdown="span">0.018*** <br> (0.004)</td>
<td markdown="span">0.009*** <br> (0.002)</td>
<td markdown="span">0.022*** <br> (0.005)</td>
<td markdown="span">0.017* <br> (0.009)</td>
<td markdown="span">0.009 <br> (0.006)</td>
</tr>
<tr>
<td markdown="span">AR(1)</td>
<td markdown="span">__-0.351\*\*\*__ <br> (0.053)</td>
<td markdown="span">__-0.226\*\*\*__ <br> (0.089)</td>
<td markdown="span">__-0.478\*\*\*__ <br> (0.065)</td>
<td markdown="span">__-0.590\*\*\*__ <br> (0.050)</td>
<td markdown="span">__-0.406\*\*\*__ <br> (0.089)</td>
<td markdown="span">__-0.821\*\*\*__ <br> (0.054)</td>
<td markdown="span">__-0.517\*\*\*__ <br> (0.073)</td>
<td markdown="span">__-0.498\*\*\*__ <br> (0.121)</td>
<td markdown="span">__-0.586\*\*\*__ <br> (0.009)</td>
</tr>
<tr>
<td markdown="span">AR(2)</td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span">__-0.469\*\*\*__ <br> (0.049)</td>
<td markdown="span">__-0.447\*\*\*__ <br> (0.089)</td>
<td markdown="span">__-0.607\*\*\*__ <br> (0.051)</td>
<td markdown="span">__-0.513\*\*\*__ <br> (0.064)</td>
<td markdown="span">__-0.585\*\*\*__ <br> (0.095)</td>
<td markdown="span">__-0.552\*\*\*__ <br> (0.079)</td>
</tr>
<tr>
<td markdown="span">AR(5)</td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span">__-0.089\*\*\*__ <br> (0.049)</td>
<td markdown="span">__-0.262\*\*\*__ <br> (0.100)</td>
<td markdown="span">0.008 <br> (0.055)</td>
</tr>
<tr>
<td markdown="span">Observations</td>
<td markdown="span">377</td>
<td markdown="span">165</td>
<td markdown="span">212</td>
<td markdown="span">325</td>
<td markdown="span">141</td>
<td markdown="span">184</td>
<td markdown="span">168</td>
<td markdown="span">69</td>
<td markdown="span">100</td>
</tr>
<tr>
<td markdown="span">Cross-section <br> (periods)</td>
<td markdown="span">52 <br> (8)</td>
<td markdown="span">24 <br> (8)</td>
<td markdown="span">28 <br> (8)</td>
<td markdown="span">52 <br> (7)</td>
<td markdown="span">24 <br> (7)</td>
<td markdown="span">28 <br> (7)</td>
<td markdown="span">50 <br> (4)</td>
<td markdown="span">22 <br> (4)</td>
<td markdown="span">28 <br> (4)</td>
</tr>
<tr>
<td markdown="span">$$R^2$$</td>
<td markdown="span">0.44</td>
<td markdown="span">0.37</td>
<td markdown="span">0.54</td>
<td markdown="span">0.53</td>
<td markdown="span">0.38</td>
<td markdown="span">0.74</td>
<td markdown="span">0.71</td>
<td markdown="span">0.73</td>
<td markdown="span">0.78</td>
</tr>
<tr>
<td markdown="span">F-statistic <br> (p-value)</td>
<td markdown="span">4.3 <br> (0.000)</td>
<td markdown="span">2.66 <br> (0.000)</td>
<td markdown="span">6.03 <br> (0.000)</td>
<td markdown="span">5.03 <br> (0.000)</td>
<td markdown="span">2.17 <br> (0.002)</td>
<td markdown="span">11.79 <br> (0.000)</td>
<td markdown="span">4.72 <br> (0.000)</td>
<td markdown="span">3.49 <br> (0.000)</td>
<td markdown="span">6.18 <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Durbin-Watson statistic</td>
<td markdown="span">2.32</td>
<td markdown="span">2.22</td>
<td markdown="span">2.56</td>
<td markdown="span">1.80</td>
<td markdown="span">2.06</td>
<td markdown="span">1.60</td>
<td markdown="span">2.41</td>
<td markdown="span">2.61</td>
<td markdown="span">2.24</td>
</tr>
</tbody>
</table>

_Note: This approach assumes there might be first, second and fifth order autocorrelation. The fifth order autocorrelation is assumed because growth is studied through 5-year periods. Sensitivity analysis is looking for AR terms that are significant at standard confidence levels and that produce d-statistics closest to 2. Significance levels are as follows: \* - 90 % significance / \*\* - 95 % significance / \*\*\* - 99 % significance._

<br>

###### *Heteroskedasticity*

After scouring EViews help pages and related forums and blogposts, I have concluded that the current version of the statistical software just doesn’t provide support when it comes to testing for heteroskedasticity in panel data through direct tests. With that in mind, there were two options left for attempting to detect possible heteroskedasticity in the data. First approach is a common-sense (backed by econometrics textbooks[^29] [^30]) approach, that assumes there is high probability of heteroskedastic errors occurring in cross-sectional data. This seems intuitively reasonable as well – countries vary greatly in GDP per capita and economic freedom measures, indicating a strong possibility of errors having varying degrees of statistical dispersion. Furthermore, Figure 5 plots residuals against fitted values, for the three OLS estimates (pooled, non-OECD, and OECD). These graphs indicate that errors are indeed not uniformly dispersed and that heteroskedasticity is likely present between cross-sections (i.e. countries). Finally, plots showing within-country fitted values and residuals aren’t feasible since there are only 10 periods under consideration meaning that there aren’t enough points to visually estimate the shape of error dispersion.

<br>

*Figure 5 - Residuals versus Fitted Values plots*

<div class="row-grid">
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/hs-pooled.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/hs-non-oecd.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/hs-oecd.png" data-zoomable>
    </div>
</div>

<br>

###### *Redundant Fixed Effects*

Figure 6 shows the results of redundant fixed effects tests, performed on the three OLS models. With future revised estimation in mind, the tests were completed for fixed cross-sectional effects, fixed period effects, and both. In each case, across all the model version, tests confirm that all model specifications are supported, with significant p-values across multiple tests.

<br>

*Figure 6 - Redundant Fixed Effects Test* <br>

<table>
<colgroup>
<col width="50%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Pooled</th>
<th>non-OECD</th>
<th>OECD</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Cross-Section F</td>
<td markdown="span">__3.039\*\*\*__ <br> (0.000)</td>
<td markdown="span">__3.057\*\*\*__ <br> (0.000)</td>
<td markdown="span">__2.782\*\*\*__ <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Cross-Section Chi-Square</td>
<td markdown="span">__152.544\*\*\*__ <br> (0.000)</td>
<td markdown="span">__72.255\*\*\*__ <br> (0.000)</td>
<td markdown="span">__77.188\*\*\*__  <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Period F</td>
<td markdown="span">__38.593\*\*\*__ <br> (0.000)</td>
<td markdown="span">__13.747\*\*\*__ <br> (0.000)</td>
<td markdown="span">__54.386\*\*\*__  <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Period Chi-Square</td>
<td markdown="span">__264.110\*\*\*__ <br> (0.000)</td>
<td markdown="span">__103.413\*\*\*__ <br> (0.000)</td>
<td markdown="span">__279.292\*\*\*__  <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Cross-Section/Period F</td>
<td markdown="span">__10.400\*\*\*__ <br> (0.000)</td>
<td markdown="span">__6.794\*\*\*__ <br> (0.000)</td>
<td markdown="span">__16.360\*\*\*__  <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Cross-Section Chi-Square</td>
<td markdown="span">__424.806\*\*\*__ <br> (0.000)</td>
<td markdown="span">__165.049\*\*\*__ <br> (0.000)</td>
<td markdown="span">__326.592\*\*\*__  <br> (0.000)</td>
</tr>
</tbody>
</table>

_Note: * - 90 % significance / ** - 95 % significance / *** - 99 % significance_ <br>
_Null: Cross-Section/Period/Both Effects specifications are redundant,_ <br>
_Alternative: Cross-Section/Period/Both Effects specifications are valid_

<br>

###### *Hausman Test*

Hausman Test null hypothesis states that there is no correlation between unique errors and regressors (meaning that the random effects model is preferred) against an alternative that there is correlation between unique errors and regressors (meaning that the fixed effects model is preferred). This test could only be performed on cross-sectional fixed effects and not two-way fixed effects since EViews doesn’t estimate two-way random effects tests on unbalanced data for the purposes of further testing. Hausman test results (see Figure 7) reject the null hypothesis and the alternative is accepted – fixed effects model is appropriate.

<br>

*Figure 7 - Random versus Fixed Effects Test* <br>

<table>
<colgroup>
<col width="50%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Pooled</th>
<th>non-OECD</th>
<th>OECD</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Chi-Square statistic</td>
<td markdown="span">__146.276\*\*\*__ <br> (0.000)</td>
<td markdown="span">__52.771\*\*\*__ <br> (0.000)</td>
<td markdown="span">__37.882\*\*\*__ <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">degrees of freedom</td>
<td markdown="span">6</td>
<td markdown="span">6</td>
<td markdown="span">6</td>
</tr>
</tbody>
</table>

_Note: * - 90 % significance / ** - 95 % significance / *** - 99 % significance_

<br>

**6. Revised Estimates**

Following all the tests performed, I have decided to reformulate the previous cross-section fixed effects OLS model. For more precise parameter identification and robust standard errors, cross-sectional heteroskedasticity and first (and possibly second) order autocorrelation need to be addressed. Therefore, the revised model is structured as a cross-section fixed effects GLS model, with two terms for autocorrelation and cross-section weights which assume the presence of heteroskedasticity in the relevant dimension. Estimates for this model are presented in Figure 8, split up between estimates for all countries, only non-OECD countries, and only OECD countries.

<br>

*Figure 8. Panel EGLS Regression (Cross-Section Weights)* <br>
*Dependent variable: log growth of GDP per capita (1975 - 2015)* 

<table>
<colgroup>
<col width="30%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Pooled</th>
<th>Pooled</th>
<th>non-OECD</th>
<th>non-OECD</th>
<th>OECD</th>
<th>OECD</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span"> </td>
<td markdown="span">EGLS</td>
<td markdown="span">OLS</td>
<td markdown="span">EGLS</td>
<td markdown="span">OLS</td>
<td markdown="span">EGLS</td>
<td markdown="span">OLS</td>
</tr>
<tr>
<td markdown="span">Intercept</td>
<td markdown="span">__0.228\*\*\*__ <br> (0.014)</td>
<td markdown="span">__0.441\*\*\*__ <br> (0.034)</td>
<td markdown="span">__0.217\*\*\*__ <br> (0.028)</td>
<td markdown="span">__0.372\*\*\*__ <br> (0.054)</td>
<td markdown="span">__0.280\*\*\*__ <br> (0.016)</td>
<td markdown="span">__0.526\*\*\*__ <br> (0.046)</td>
</tr>
<tr>
<td markdown="span">log GDP per capita</td>
<td markdown="span">__-0.032\*\*\*__ <br> (0.003)</td>
<td markdown="span">__-0.071\*\*\*__ <br> (0.005)</td>
<td markdown="span">__-0.044\*\*\*__ <br> (0.007)</td>
<td markdown="span">__-0.072\*\*\*__ <br> (0.009)</td>
<td markdown="span">__-0.027\*\*\*__ <br> (0.003)</td>
<td markdown="span">__-0.072\*\*\*__ <br> (0.007)</td>
</tr>
<tr>
<td markdown="span">Size of Government</td>
<td markdown="span">__0.005\*\*\*__ <br> (0.003)</td>
<td markdown="span">0.005 <br> (0.004)</td>
<td markdown="span">0.005 <br> (0.003)</td>
<td markdown="span">0.006 <br> (0.006)</td>
<td markdown="span">0.002 <br> (0.001)</td>
<td markdown="span">0.002 <br> (0.005)</td>
</tr>
<tr>
<td markdown="span">Legal System & Property Rights</td>
<td markdown="span">0.001 <br> (0.002)</td>
<td markdown="span">-0.003 <br> (0.003)</td>
<td markdown="span">0.001 <br> (0.003)</td>
<td markdown="span">-0.005 <br> (0.005)</td>
<td markdown="span">-0.002 <br> (0.002)</td>
<td markdown="span">-0.002 <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">Sound Money</td>
<td markdown="span">__-0.007\*\*\*__ <br> (0.001)</td>
<td markdown="span">-0.004 <br> (0.002)</td>
<td markdown="span">__-0.009\*\*\*__ <br> (0.002)</td>
<td markdown="span">__-0.006\*\*__ <br> (0.003)</td>
<td markdown="span">__-0.006\*\*\*__ <br> (0.001)</td>
<td markdown="span">0.000 <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">Regulation</td>
<td markdown="span">0.003 <br> (0.003)</td>
<td markdown="span">__0.024\*\*\*__ <br> (0.005)</td>
<td markdown="span">__0.013\*\*__ <br> (0.006)</td>
<td markdown="span">__0.024\*\*\*__ <br> (0.008)</td>
<td markdown="span">0.001 <br> (0.002)</td>
<td markdown="span">__0.025\*\*\*__ <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">Freedom to Trade Internationally</td>
<td markdown="span">__0.014\*\*\*__ <br> (0.002)</td>
<td markdown="span">__0.013\*\*\*__ <br> (0.003)</td>
<td markdown="span">__0.017\*\*\*__ <br> (0.006)</td>
<td markdown="span">__0.017\*\*\*__ <br> (0.004)</td>
<td markdown="span">__0.010\*\*\*__ <br> (0.002)</td>
<td markdown="span">__0.008\*__ <br> (0.004)</td>
</tr>
<tr>
<td markdown="span">AR(1)</td>
<td markdown="span">-0.669*** <br> (0.038)</td>
<td markdown="span"> </td>
<td markdown="span">-0.498*** <br> (0.072)</td>
<td markdown="span"> </td>
<td markdown="span">-0.862*** <br> (0.039)</td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">AR(2)</td>
<td markdown="span">-0.538*** <br> (0.033)</td>
<td markdown="span"> </td>
<td markdown="span">-0.530*** <br> (0.071)</td>
<td markdown="span"> </td>
<td markdown="span">-0.663*** <br> (0.034)</td>
<td markdown="span"> </td>
</tr>
<tr>
<td markdown="span">Observations</td>
<td markdown="span">325</td>
<td markdown="span">430</td>
<td markdown="span">141</td>
<td markdown="span">189</td>
<td markdown="span">184</td>
<td markdown="span">241</td>
</tr>
<tr>
<td markdown="span">Cross-section <br> (periods)</td>
<td markdown="span">52 <br> (7)</td>
<td markdown="span">52 <br> (9)</td>
<td markdown="span">24 <br> (7)</td>
<td markdown="span">24 <br> (9)</td>
<td markdown="span">28 <br> (7)</td>
<td markdown="span">28 <br> (9)</td>
</tr>
<tr>
<td markdown="span">$$R^2$$</td>
<td markdown="span">0.71</td>
<td markdown="span">0.44</td>
<td markdown="span">0.56</td>
<td markdown="span">0.41</td>
<td markdown="span">0.85</td>
<td markdown="span">0.46</td>
</tr>
<tr>
<td markdown="span">F-statistic <br> (p-value)</td>
<td markdown="span">10.83 <br> (0.000)</td>
<td markdown="span">5.03 <br> (0.000)</td>
<td markdown="span">4.42 <br> (0.000)</td>
<td markdown="span">3.88 <br> (0.000)</td>
<td markdown="span">24.64 <br> (0.002)</td>
<td markdown="span">5.4 <br> (0.000)</td>
</tr>
<tr>
<td markdown="span">Durbin-Watson statistic</td>
<td markdown="span">1.84</td>
<td markdown="span">2.67</td>
<td markdown="span">2.08</td>
<td markdown="span">2.41</td>
<td markdown="span">1.74</td>
<td markdown="span">2.92</td>
</tr>
</tbody>
</table>

_Note: * - 90 % significance / ** - 95 % significance / *** - 99 % significance_

<br>

As Figure 8 shows, *Freedom to Trade Internationally* and *Sound Money* have a significant effect across three model runs, and *Regulation* has a significant effect for non-OECD countries only. Any one-point increase in the *Freedom to Trade Internationally* index is correlated with a 1.3 % (pooled), 1.7 % (non-OECD) and 1% (OECD) increase in the growth rate of GDP per capita, on average. Any one-point increase in the *Sound Money* index is correlated with a 0.7 % (pooled), 0.9 % (non-OECD) and 0.6% (OECD) increase in the growth rate of GDP per capita, on average. Any one-point increase in the *Regulation* index for non-OECD countries is correlated with a 1.3 % increase in the growth rate of GDP per capita, on average. Finally, any one-point increase in the *Size of Government* index for pooled countries is correlated with a 0.5 % increase in the growth rate of GDP per capita, on average. To reiterate, an increase in index scores across all three of the variables mentioned indicates an increase in economic freedom, as per EFI design.

<br>

*Figure 9 - Residuals versus Fitted Values plots*

<div class="row-grid">
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/hs-pooled2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/hs-non-oecd2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/hs-oecd2.png" data-zoomable>
    </div>
</div>

<br>

Moreover, to confirm that the revised model significantly diminishes heteroskedasticity and autocorrelation detected previously, Figure 8 reports the Durbin-Watson statistic and Figure 9 shows the residual plots. The Durbin-Watson (d) statistic went from 2.7 to 1.8 (pooled), 2.4 to 2.1 (non-OECD), and 2.9 to 1.7 (OECD). Since a d value of 2 indicates that $$\rho$$ is 0, this indicates a reduction in residual trend correlation. Additionally, Appendix 5 reports the trend of residuals across periods (compared with the OLS estimated ones), indicating a smoothening. Finally, Figure 9 reports the error dispersion (compared with the OLS estimated ones), indicating more uniform dispersion (homoskedasticity).

**7. Conclusion**

This project finds a significant relationship between freedom to trade internationally and economic growth. This freedom is reflected in lower tariffs, few regulations on movement of human and physical capital, easily convertible currency and simple customs clearance operations. This finding mirrors that of Gwartney [^23] and Torstensson [^14], and contradicts the findings of Ayal and Karras[^10], who find a negative relationship between freedom to trade and economic growth. This finding holds for all countries pooled together, only non-OECD countries, as well as OECD member nations. Furthermore, this research finds that pursuing low inflation and allowing free access to alternative currency use has a small negative impact on economic growth (between 0.6% and 0.9% for each EFI area unit increase). This result directly contradicts that of Ayal and Karras[^10] and Barro[^31]. There is also significant evidence that less stringent regulation positively affects economic growth for non-OECD countries only. This reflects the findings of Barro[^31], Torstensson[^14] and Knack & Keefer[^13]. Finally, there is significant evidence that smaller governments, that intervene economically less often, are positively correlated with economic growth. This has only been observed for the pooled data set and the effect was only 0.05%. This finding is mirrored in a more robust way in other research[^11] [^23] [^13].

Across all model formulations, freedom to trade internationally remained very robust. These results support classical economic theory[^3] [^1], as well as more modern assumptions[^9]. Monetary stability through low inflation and the freedom to use alternate currencies seemed likely to be correlated with economic growth. However, research did not support this hypothesis. There are a few reasons that might explain this finding. Subcomponents of this area of the index might be constructed out of elements with opposite effects. Also, most countries in the data set do not wield the economic power of the United States and are economically tied to the fluctuations of more influential currencies. Therefore, they are unable to produce sound monetary policy and often attempt to restrict the power of foreign currencies in the domestic marketplace. If these countries experience increased growth rates, this area of the index does not predict development as assumed. Free movement into markets, as pictured through the regulation variable, is only significant for non-OECD countries, most of which are underdeveloped. This could indicate that lax regulation fosters growth on the way to the status of a first-world country. However, countries that had already reached these levels of development are not experiencing such growth rates and often begin introducing new regulation when they become appropriately placed on the Kuznets curve to do so. This is often regulation that deals with various market inefficiencies (externalities, informational asymmetry, etc.).

###### *Limitations and future research*

This data set was somewhat unbalanced (missing periods for a few cross-sections), resulting in the omission of those observations. Along with balanced panel data, the set needs to extend over a longer time frame, given that in the process of correcting for autocorrelation, the number of observations got further reduced. It would also be useful to detect breakpoints in the time series, centered around significant economic events (like the Great Recession), and perform the estimation around them. Much like a longer time frame, more countries included in the set would be useful. The issue lies in procuring the necessary data, especially further into the past, given that some countries do not provide any data or provide data that is highly questionable.

**8. GitHub repository**

For data, code, and similar projects, visit [github.com/antoniojurlina/economic_freedom_and_growth](https://github.com/antoniojurlina/economic_freedom_and_growth).

##### Appendix 1 - Index Area Components

* <details>
    <summary> Size of Government: </summary>
        
    (a) Government Consumption
    <br>
    (b) Transfers and subsidies
    <br>
    (c) Government enterprises and investment
    <br>
    (d) Top marginal tax rate
    </details>

* <details>
    <summary> Legal System and Property Rights: </summary>
        
    (a) Judicial independence
    <br>
    (b) Impartial courts
    <br>
    (c) Protection of property rights
    <br>
    (d) Military interference in rule of law and politics
    <br>
    (e) Integrity of the legal system
    <br>
    (f) Legal enforcement of contracts
    <br>
    (g) Regulatory costs of the sale of real property
    <br> 
    (h) Reliability of police
    <br>
    (i) Business costs of crime
    </details>

* <details>
    <summary> Sound Money: </summary>
        
    (a) Money growth
    <br>
    (b) Standard deviation of inflation
    <br>
    (c) Inflation: most recent year
    <br>
    (d) Freedom to own foreign currency bank
    </details>

* <details>
    <summary> Freedom to Trade Internationally: </summary>
        
    (a) Tariffs
    <br>
    (b) Regulatory trade barriers
    <br>
    (c) Black-market exchange rates
    <br>
    (d) Controls of the movement of capital and people
    </details>

* <details>
    <summary> Regulation: </summary>
        
    (a) Credit market regulations
    <br>
    (b) Labor market regulations
    <br>
    (c) Business regulations
    </details>

##### Appendix 2 - Summary Statistics

<div class="row-grid">
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/gdp.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/gov.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/growth.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/legal.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/money.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/regulation.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/trade.png" data-zoomable>
    </div>
</div>

##### Appendix 3 - Summary Graphs

<div class="row-grid">
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/gdp2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/gov2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/growth2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/legal2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/money2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/regulation2.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/trade2.png" data-zoomable>
    </div>
</div>

##### Appendix 4 - Residuals Plots (before)

<div class="row-grid">
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/residuals_pooled_b.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/residuals_non_oecd_b.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/residuals_oecd_b.png" data-zoomable>
    </div>
</div>

##### Appendix 5 - Residuals Plots (after)

<div class="row-grid">
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/residuals_pooled_a.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/residuals_non_oecd_a.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/residuals_oecd_a.png" data-zoomable>
    </div>
</div>

##### Appendix 6 - EViews Code

{% highlight eviews %} 
spool results output(s) results
'creating variables necessary to introduce growth to the model
series pp = log(gdp)
series growth = (pp - pp(-1)) / 5

'''''' Creating summary statistics, graphs and a covariance matrix''''''''''''''''' ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' '''''''''''''''''''''''''''''''''''''''''''''''''
alpha country = countries
group dataset country year gdp gov growth*100 legal money oecd pp regulation trade
results.insert dataset
delete country
gdp.statby(max, min, nov, noa, p) countries
close gdp
graph ggdp.line(m, ab = histogram, panel = individual) gdp ggdp.options connect
results.insert ggdp
growth.statby(max, min, nov, noa, p) countries
close growth
graph ggrowth.line(m, ab = histogram, panel = individual) growth
ggrowth.options connect
results.insert ggrowth
gov.statby(max, min, nov, noa, p) countries
close gov
graph ggov.line(m, ab = histogram, panel = individual) gov ggov.options connect
results.insert ggov
legal.statby(max, min, nov, noa, p) countries
close legal
graph glegal.line(m, ab = histogram, panel = individual) legal glegal.options connect
results.insert glegal
money.statby(max, min, nov, noa, p) countries
close money
graph gmoney.line(m, ab = histogram, panel = individual) money
gmoney.options connect
results.insert gmoney
trade.statby(max, min, nov, noa, p) countries
close trade
graph gtrade.line(m, ab = histogram, panel = individual) trade
gtrade.options connect
results.insert gtrade
regulation.statby(max, min, nov, noa, p) countries
close regulation
graph gregulation.line(m, ab = histogram, panel = individual) regulation
gregulation.options connect
results.insert gregulation
group variables growth gdp gov legal money regulation trade matrix x = @cor(variables)
x.setcollabels growth gdp gov legal money regulation trade x.setrowlabels growth gdp gov legal money regulation trade x.displayname Correlation Matrix
x.label results.insert x
delete ggdp ggov glegal gtrade gmoney gregulation ggrowth world_gdp usd
delete variables rank economic_freedom_summary_index x
results.displayname untitled01 "Data Set" results.displayname untitled02 "GDP per capita" results.displayname untitled03 "GDP per capita" results.displayname untitled04 "Growth rate" results.displayname untitled05 "Growth rate" results.displayname untitled06 "Size of Government" results.displayname untitled07 "Size of Government" results.displayname untitled08 "Legal System & Property Rights"
results.displayname untitled09 "Legal System & Property Rights"
results.displayname untitled10 "Sound Money" results.displayname untitled11 "Sound Money" results.displayname untitled12 "Freedom to trade internationally"
results.displayname untitled13 "Freedom to trade internationally"
results.displayname untitled14 "Regulation" results.displayname untitled15 "Regulation" results.displayname untitled16 "Correlation Matrix" ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' '''''''''''''''''''''''''''''''''''''''''''''''''
''''''' end of summary statistics ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' ''''''''''''''''''

''''''' Fixed effects OLS models ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' ''''''''''''' ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' '''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
smpl @all
equation eq_a.ls(cx=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
results.insert eq_a
results.displayname untitled17 "OLS (pooled)"
smpl if oecd = 0
equation eq_b.ls(cx=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
results.insert eq_b
results.displayname untitled18 "OLS (non-OECD)"
smplifoecd=1
equation eq_c.ls(cx=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
results.insert eq_c
results.displayname untitled19 "OLS (OECD)" ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' '''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
'''''''''' End of OLS estimation ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' '

'''''' Tests ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' ''''''''
smpl @all
equation eq_d.ls(cx=f, per=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
eq_d.fixedtest(p)
results.displayname untitled20 "Redundancy Test a"
smpl if oecd = 0
equation eq_e.ls(cx=f, per=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
eq_e.fixedtest(p)
results.displayname untitled21 "Redundancy Test b"
smpl if oecd = 1
equation eq_f.ls(cx=f, per=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
eq_f.fixedtest(p)
results.displayname untitled22 "Redundancy Test c"
close eq_d
close eq_e
close eq_f
smpl @all
equation eq_d.ls(cx=r) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
eq_d.ranhaus(p)
close eq_d
results.displayname untitled23 "RE vs FE Test a"
smpl if oecd = 0
equation eq_e.ls(cx=r) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
eq_e.ranhaus(p)
close eq_e
results.displayname untitled24 "RE vs FE Test b"
smpl if oecd = 1
equation eq_f.ls(cx=r) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
eq_f.ranhaus(p)
close eq_f
results.displayname untitled25 "RE vs FE Test c"
smpl @all
EQ_A.makeresids resid_a
graph res_a.spike(m, panel = combine) resid_a res_a.options connect
results.insert res_a
results.displayname untitled26 "OLS (pooled) Residuals"
smpl if oecd = 0
EQ_B.makeresids resid_b
graph res_b.spike(m, panel = combine) resid_b res_b.options connect
results.insert res_b
results.displayname untitled27 "OLS (non-OECD) Residuals"
smpl if oecd = 1
EQ_C.makeresids resid_c
graph res_c.spike(m, panel = combine) resid_c res_c.options connect
results.insert res_c
results.displayname untitled28 "OLS (OECD) Residuals"
smpl @all
eq_a.forecast(e, g) growthf
graph hetero1.scat(panel = stack) growthf resid_a hetero1.axis(b) angle(auto)
hetero1.legend position(LEFT)
hetero1.setelem(1) symbol(CIRCLE) linepattern(none) linecolor(@rgb(57,106,177))
hetero1.setelem(1) legend(Fitted Values) hetero1.setelem(2) legend(Residuals) hetero1.setelem(1) axis(b)
results.insert hetero1
results.displayname untitled29 "OLS (pooled) Residuals Plot"
smpl if oecd = 0
eq_b.forecast(e, g) growthf
graph hetero2.scat(panel = stack) growthf resid_b hetero2.axis(b) angle(auto)
hetero2.legend position(LEFT)
hetero2.setelem(1) symbol(CIRCLE) linepattern(none) linecolor(@rgb(57,106,177))
hetero2.setelem(1) legend(Fitted Values) hetero2.setelem(2) legend(Residuals)
hetero2.setelem(1) axis(b)
results.insert hetero2
results.displayname untitled30 "OLS (non-OECD) Residuals Plot"
smpl if oecd = 1
eq_c.forecast(e, g) growthf
graph hetero3.scat(panel = stack) growthf resid_c hetero3.axis(b) angle(auto)
hetero3.legend position(LEFT)
hetero3.setelem(1) symbol(CIRCLE) linepattern(none) linecolor(@rgb(57,106,177))
hetero3.setelem(1) legend(Fitted Values)
hetero3.setelem(2) legend(Residuals)
hetero3.setelem(1) axis(b)
results.insert hetero3
results.displayname untitled31 "OLS (OECD) Residuals Plot"
delete res_a res_b res_c hetero1 hetero2 hetero3 resid_a resid_b resid_c growthf
smpl @all
equation eq_g.ls(cx=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1) ar(1) ar(2)
results.insert eq_g
results.displayname untitled32 "OLS (pooled)"
smpl if oecd = 0
equation eq_h.ls(cx=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1) ar(1) ar(2)
results.insert eq_h
results.displayname untitled33 "OLS (non-OECD)"
smpl if oecd = 1
equation eq_i.ls(cx=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1) ar(1) ar(2)
results.insert eq_i
results.displayname untitled34 "OLS (OECD)"
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''' '''''''''''''''''''''''''''''''''''''''''''''''''''''' '''''''''
'''' end of tests ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

smpl @all
equation eq_j.ls(cx=f, wgt=cxdiag, deriv=aa) growth c gov(- 1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1) ar(1) ar(2)
results.insert eq_j
results.displayname untitled35 "OLS (pooled)"
smpl if oecd = 0
equation eq_k.ls(cx=f, wgt=cxdiag, deriv=aa) growth c gov(- 1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1) ar(1) ar(2)
results.insert eq_k
results.displayname untitled36 "OLS (non-OECD)"
smpl if oecd = 1
equation eq_l.ls(cx=f, wgt=cxdiag, deriv=aa) growth c gov(- 1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1) ar(1) ar(2)
results.insert eq_l
results.displayname untitled37 "OLS (OECD)"
smpl @all
equation eq_m.ls(cx=f, per=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
results.insert eq_m
results.displayname untitled38 "OLS (pooled)"
smpl if oecd = 0
equation eq_n.ls(cx=f, per=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
results.insert eq_n
results.displayname untitled39 "OLS (non-OECD)"
smpl if oecd = 1
equation eq_o.ls(cx=f, per=f) growth c gov(-1) legal(-1) money(-1) trade(-1) regulation(-1) pp(-1)
results.insert eq_o
results.displayname untitled40 "OLS (OECD)"
smpl @all
eq_j.makeresids resid_j
graph res_j.spike(m, panel = combine) resid_j res_j.options connect
results.insert res_j
results.displayname untitled41 "OLS (pooled) Residuals"
smpl if oecd = 0
eq_k.makeresids resid_k
graph res_k.spike(m, panel = combine) resid_k res_k.options connect
results.insert res_k
results.displayname untitled42 "OLS (non-OECD) Residuals"
smpl if oecd = 1
eq_l.makeresids resid_l
graph res_l.spike(m, panel = combine) resid_l res_l.options connect
results.insert res_l
results.displayname untitled43 "OLS (OECD) Residuals"
smpl @all
eq_j.forecast(e, g) growthf
graph hetero1.scat(panel = stack) growthf resid_j hetero1.axis(b) angle(auto)
hetero1.legend position(LEFT)
hetero1.setelem(1) symbol(CIRCLE) linepattern(none) linecolor(@rgb(57,106,177))
hetero1.setelem(1) legend(Fitted Values) hetero1.setelem(2) legend(Residuals) hetero1.setelem(1) axis(b)
results.insert hetero1
results.displayname untitled44 "OLS (pooled) Residuals Plot"
smpl if oecd = 0
eq_k.forecast(e, g) growthf
graph hetero2.scat(panel = stack) growthf resid_k hetero2.axis(b) angle(auto)
hetero2.legend position(LEFT)
hetero2.setelem(1) symbol(CIRCLE) linepattern(none) linecolor(@rgb(57,106,177))
hetero2.setelem(1) legend(Fitted Values) hetero2.setelem(2) legend(Residuals)
hetero2.setelem(1) axis(b)
results.insert hetero2
results.displayname untitled45 "OLS (non-OECD) Residuals Plot"
smpl if oecd = 1
eq_l.forecast(e, g) growthf
graph hetero3.scat(panel = stack) growthf resid_l hetero3.axis(b) angle(auto)
hetero3.legend position(LEFT)
hetero3.setelem(1) symbol(CIRCLE) linepattern(none) linecolor(@rgb(57,106,177))
hetero3.setelem(1) legend(Fitted Values)
hetero3.setelem(2) legend(Residuals)
hetero3.setelem(1) axis(b)
results.insert hetero3
results.displayname untitled46 "OLS (OECD) Residuals Plot"
delete res_j res_k res_l hetero1 hetero2 hetero3 resid_j resid_k resid_l growthf
results.options displaynames

{% endhighlight %}

##### **Works Cited**

[^1]: Smith, A. (2003). An Inquiry into the Nature and Causes of the Wealth of Nations. Bantam Classics.

[^2]: McCusker, J., & Morgan, K. (Eds.). (2001). The Early Modern Atlantic Economy. Cambridge: Cambridge University Press. doi:10.1017/CBO9780511523878

[^3]: Ricardo, D. (1817). On the Principles of Political Economy and Taxation. London: Dover Publications.

[^4]: Solow, R. M. (1956). A Contribution to the Theory of Economic Growth. The Quarterly Journal of Economics, 70(1), 65. https://doi.org/10.2307/1884513

[^5]: Domar, E. D. (1946). Capital Expansion, Rate of Growth, and Employment. Econometrica, 14(2), 137–147. https://doi.org/10.2307/1905364

[^6]: Harrod, R. F. (1939). An Essay in Dynamic Theory. The Economic Journal, 49(193), 14–33. https://doi.org/10.2307/2225181

[^7]: Kuznets, S. (1973). Modern Economic Growth: Findings and Reflections. The American Economic Review, 63(3), 247–258.

[^8]: Schumpeter, J. (1942). Capitalism, Socialism and Democracy (Vol. 1). Routledge. Retrieved from https://www.goodreads.com/work/best_book/129884-capitalism-socialism-and-democracy

[^9]: Friedman, M. (1962). Capitalism and Freedom (1st ed.). University of Chicago Press. Retrieved from https://www.goodreads.com/work/best_book/1534488-capitalism-and-freedom

[^10]: Ayal, E. B., & Karras, G. (1998). Components of Economic Freedom and Growth: An Empirical Study. The Journal of Developing Areas, 32(3), 327–338.

[^11]: Barro, R. J. (1991). Economic Growth in a Cross Section of Countries. The Quarterly Journal of Economics, 106(2), 407–443. https://doi.org/10.2307/2937943

[^12]: Easterly, W. (1992). Marginal income tax rates and economic growth in developing countries (No. WPS1050) (p. 1). The World Bank. Retrieved from http://documents.worldbank.org/curated/en/432391468766196026/Marginal-income-tax-rates-and-economic-growth-in-developing-countries

[^13]: Knack, S., & Keefer, P. (1995). Institutions and Economic Performance: Cross-Country Tests Using Alternative Institutional Measures. Economics & Politics, 7(3), 207–227. https://doi.org/10.1111/j.1468-0343.1995.tb00111.x

[^14]: Torstensson, J. (1994). Property Rights and Economic Growth: An Empirical Study. Kyklos, 47(2), 231–247.

[^15]: Naughton, B. (2007). The Chinese economy: transitions and growth. Cambridge, Mass: MIT Press.

[^16]: Mundell, R. A. (1963). Capital Mobility and Stabilization Policy under Fixed and Flexible Exchange Rates. The Canadian Journal of Economics and Political Science / Revue Canadienne d’Economique et de Science Politique, 29(4), 475–485. https://doi.org/10.2307/139336

[^17]: J. M. Fleming, “Domestic Financial Policies under Fixed and Floating Exchange Rates,” IMF Staff Papers, Vol. 9, 1962, pp. 369-379. doi:10.2307/3866091

[^18]: Leontief, W. (1953). Domestic Production and Foreign Trade; The American Capital Position Re-Examined. Proceedings of the American Philosophical Society, 97(4), 332– 349.

[^19]: Gwartney, J. D., Lawson, R. A., & Block, W. (1996). Economic Freedom of the World:1975-1995 (p. 342). The Fraser Institute. Retrieved from http://bit.ly/2jUkBGR

[^20]: Berggren, N. (2003). The Benefits of Economic Freedom: A Survey. The Independent Review, 8(2), 193–211.

[^21]: Carlsson, F., & Lundström, S. (2002). Economic freedom and growth: Decomposing the effects. Public Choice, 112, 335–344. https://doi.org/10.1023/a:1019968525415

[^22]: de Haan, J., & Sturm, J.-E. (2000). On the relationship between economic freedom and economic growth. European Journal of Political Economy, 16(2), 215–241. https://doi.org/10.1016/S0176-2680(99)00065-8

[^23]: Gwartney, J. D., Lawson, R. A., & Holcombe, R. G. (1999). Economic Freedom and the Environment for Economic Growth. Journal of Institutional and Theoretical Economics, 155(4), 643–663.

[^24]: Nelson, M. A., & Singh, R. D. (1998). Democracy, Economic Freedom, Fiscal Policy, and Growth in LDCs: A Fresh Look. Economic Development and Cultural Change, 46(4), 677– 696. https://doi.org/10.1086/452369

[^25]: Economic Freedom of the World. (2015, December 22). Retrieved October 9, 2018, from http://bit.ly/2h5xBVI

[^26]: GDP per capita (current US$) Data. (2018). Retrieved October 9, 2018, from https://data.worldbank.org/indicator/NY.GDP.PCAP.CD

[^27]: OECD - Members and partners. (2018, July). Retrieved December 12, 2018, from http://www.oecd.org/about/membersandpartners/

[^28]: Mankiw, N. G., Romer, D., & Weil, D. N. (1992). A Contribution to the Empirics of Economic Growth. The Quarterly Journal of Economics, 407–437. https://doi.org/10.3386/w3541

[^29]: Guajarati, D. N. (1987). Basic Econometrics (4th ed.).

[^30]: Wooldridge, J. M. (2016). Introductory Econometrics: A Modern Approach (6th ed.). Thomson South-Western.

[^31]: Barro, R. J. (1996). Democracy and Growth. Journal of Economic Growth, 1(1), 1–27.