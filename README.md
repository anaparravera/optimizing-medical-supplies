# optimizing-medical-supplies
Optimizing medical supply distribution for California counties as part of the MSBA COVID19 Analytics Challenge

## What is the Goal Here? Scope?
Beyond the basic idea that efficiently supplying medical supplies to the counties that need it to the most, people in the US are experiencing a unique effect on our capitalist economy. States are being forced into bidding wars for needed medical supplies that can mean life or death for someone with COVID-19. States are spending significant time negotiating and bidding for medical supplies, but that is only half the battle. Once they get the medical supplies, the question is, where would be the most efficient place to distribute said medical supplies throughout their state in order to minimize deaths?

The project will be focusing specifically on counties in California. California, being one the most populated state in the US, has an alarming amount of confirmed deaths, 758.  

1. Use analytics to determine the optimal distribution of newly incoming ventilators to counties. 
2. We want to minimize overall death (both COVID-19 and non-COVID-19 caused)

## Optimization Models
Since we want to minimize death across all 58 California counties, our team set up a minimization model with a function that is dependent on each counties current number of available ventilators and the additional ventilators added to each county which is the optimization decision. There are two constraints placed on our model which are the fact that the additional ventilators assigned to each county have to be greater or equal to zero and the fact that no more than the state purchased ventalors can be distributed within the state. Once we knew what we wanted to minimize, the constraints related to the problem, and roughly the theoretical variables needed to execute this model, we began thinking more about the function needed. 

### Medical Supplies - Ventilators
We started this adventure by looking into ventilator data we found for pre-pandemic per California Hospital. This data gave us the maximum number of licensed and staffed beds possible in each hospital which we aggregated per county as used as our maximum number of ventilators for each county. This data also provided us an average number of used ventilators per week for each hospital. We also aggregated this data by county and through some medical research we decided to estimate that the current level of ventilator per county at any given week was based on a steady income of ventilators based on the expended average used minus the total number used for non-covid case and covid cases. Through research we determined that roughly 15% of every covid case requires a ventilator so we used the number of cases per county from the ERSI Website. (15% estimation from Michigan study published on Michigan.gov)

Additional Notes:
ICU Beds Capacity = COVID-19 Related ICU Hospitalization/Num ICU Beds

Projected Ventilators are calculated based on Michigan Stats

Ventilator Availability = COVID-19 Related ICU Hospitalization/Projected Ventilators ----assume that each ICU would be having one ventilator. This is also the least amount of ventilators that hospital has to have.

### Risk Index
We initially attempted to tackle the rate of infection using Twitter data. We scraped tweets with the following hashtags, hoping to calculate the rate of the virus spread by finding the density of tweets in locations over time. Above, we can see that the cities with the highest cases also were proportionally distributed in terms of tweet density. However, because Twitter allows users to put anything in their location, this feat proved to exceed our time constraints. We therefore changed tactics.

We calculated an average weekly rate of infection for each county in California. To derive this metric, we first collected data on the number of cases in each county from the start of the first tracked cases in the United States. We then modeled the data to fit an exponential curve in order to better predict the trend of the virus spread. This fitted model allowed us to aggregate the data into weekly segments and calculate the average rate of infection per week per county over past weeks as well as the projected rate based on predicted case numbers for future weeks. These rates were then implemented into our optimization model.

In order to determine the average risk of a county’s population we utilized an open source indexing model that determined an individual’s risk based on preexisting conditions and age. The model used Medicare data which, unfortunately, given the time length of the challenge and the security measure required to obtain medicare data samples, we were unable to use California specific data for. Instead, assuming that the medicare data provided in the github repository is truly a random sample of the US, we found the population percentage of each California county compare to the country population and then randomly sampled with replacement 1,000 times and then averaged to find the estimated average for specific counties. 

## Moving Forward
Our team recognize that building this model required a number of significant assumptions. Unfortunately, due to the lack of accurate available date, these assumption were needed in order to create a starting off point. Moving forward, our team suggests the model to be reassessed as more data becomes available. More data would also allow for this model to be expanded to other states and evolve functional form, and potentially even include additional theoretically important variable.  Using Medicare Data, given the appropriate assess, would significantly improve the accuracy of the Risk Index as it could be calculated for patients originally indicated as from California. 

Lastly, since the United States has such a great inequality of health care asses, this would need to be accountable for in all estimates of the used variables . Originally our team set out with the goal of using a Natural Language Processing technique to estimate the rate of spread using tweets since we believe that Twitter is more equal opportunity than health care. However, this endeavour was not realistic in the time frame of this challenge to any significant degree for our team.

## References
1. State of Michigan: Statewide PPE and Bed Tracking
   https://www.michigan.gov/coronavirus/0,9753,7-406-98159-523641--,00.html <br/>
2. Latest County-Level COVID-19 Data in ArcGIS Business Analyst,      https://www.esri.com/arcgis-blog/products/bus-analyst/real-time/latest-county-level-covid-19-data-in-arcgis-business-analyst/ <br/>
3. COVID19 Planning Report Dashboard https://bao.arcgis.com/InfographicsPlayer/ArcGISPro/BA_Covid19/<br/>
4. Definitive Healthcare Hospital Capacity and Utilization at County Level https://coronavirus-resources.esri.com/datasets/definitivehc::definitive-healthcare-usa-hospital-beds?geometry=32.519%2C-16.820%2C-57.481%2C72.123 <br/>
5. California Department of Public Health Open Data
https://data.chhs.ca.gov/dataset/california-covid-19-hospital-data-and-case-statistics <br/>
6. DeCaprio, D., et al (2020), Building a COVID-19 Vulnerability Index. medRxiv 2020.03.16.20036723; doi: https://doi.org/10.1101/2020.03.16.20036723 <br/>










