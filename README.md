# airline_passenger_satisfaction
Predicting airline passenger satisfaction from survey data

## Problem Formulation
The target audience of this project will be the management team of a commercial airline. This team is trying to find ways to improve customer satisfaction with the hope of retaining repeat business. I plan to discover actionable and focused ideas for changes with our marketing campaigns, and our service offerings to help increase customer satisfaction.

The nature of the data in this project provides flexibility for what type of learning to focus on (supervised vs. unsupervised). For example, unsupervised learning such as clustering can be applied to create clusters of customers and eventually translate those clusters into segments. I can try to understand which segments of customers exist and create marketing campaigns tailored to those segments of flyers. But, since this data provides a satisfied/dissatisfied outcome variable, I can also try supervised, predictive techniques such as logistic regression to predict if a customer will be satisfied or dissatisfied based on their demographic and survey data. The airline can try to operationalize this and predict if a customer will be dissatisfied before a flight occurs, understand what factors lead to dissatisfaction, and try to prevent the dissatisfaction from occurring.

This project will be significant to the airline since it will aim to reveal parts of their service that are important to overall customer satisfaction. Travel has been significantly impacted due to COVID, so this airline will need to try new and refreshed ways of keeping their customers satisfied so they’ll want to travel with the airline again.

## Data Description
### Source + Characteristics
The data set I’m using contains variables on airline passenger satisfaction collected via survey. I discovered the data set from Kaggle. This is a mixed data set of numeric and categorical variables. It contains an outcome variable showing whether a passenger of the airline was satisfied or dissatisfied. The data set in Kaggle contained two separate data sets (train and test). This data set can be used for both supervised and unsupervised learning, since we have a satisfaction/dissatisfaction outcome variable.
### Collection Procedures
The test and train data set combined contains a total of 155,856 observations along 24 variables; the train set alone had about 129,000 observations. However, when trying to run models I realized my computer didn’t have the capacity to run models with this large of a data set, so I took a random sample to limit the train data set to 15,000 observations.

The data only had missing values in the Arrival Delay in Minutes column, I manipulated this variable by replacing the missing values with the median of the values, which happened to be 0. For my analysis I also removed the Customer ID variable, as this has no explanatory power.
### Key Variables
The variables can be split into two categories: demographics, and survey results:
* Demographics about the passenger and their flight

  	Gender: Male, Female
	
	Customer Type: Loyal, Disloyal
	
	Age: Integer ranging from 7 to 85
	
	Type of Travel: Personal, Business
	
	Class: Eco, Eco Plus, Business
	
	Flight Distance: Integer ranging from 31 to 4,983
	
  	Departure Delay in Minutes: Integer ranging from 0 to 1,592
	
  	Arrival Delay in Minutes: Integer ranging from 0 to 1,584
* Survey Results: integers ranging from 0 (didn't answer question) to 5 (most satisfied)

  	Inflight Wi-Fi Service
	
	Departure/Arrival Time Convenience
	
	Ease of Online booking
	
	Gate location
	
	Food and drink
	
	Online boarding
  	Seat comfort
	
	Inflight entertainment
	
	On-board service
	
	Leg room service
	
	Baggage handling 
	
	Check-in service 
	
	Inflight Service
	
	Cleanliness

### Descriptive Information
* Categorical

![image](https://user-images.githubusercontent.com/44247793/130680761-5996969c-d8b6-40c7-8a98-f5a5871ac375.png)

Looking at the categorical variables, the data is skewed towards business flyers who are considered loyal customers and contains slightly more dissatisfied/neutral flyers than satisfied. The data set is almost evenly distributed in terms of gender.

* Numeric

![image](https://user-images.githubusercontent.com/44247793/130680834-7561e9d5-7998-4064-a043-2cabf98f4ad8.png)

For numeric variables, we have mostly younger passengers in the data set, there are many passengers in their mid-20s to 30s. We have more data on relatively shorter flights in terms of distance and are heavily skewed to short arrival and departure delays. Looking at the survey answers, I can see off the bat that ease of online booking and inflight Wi-Fi service has relatively higher rates of low satisfaction than many other survey questions. 

![image](https://user-images.githubusercontent.com/44247793/130680878-f5be93a6-a022-4983-ada3-74403dd2bd49.png)

The correlation plot doesn’t have a lot of interesting info; however, survey ratings on cleanliness and food are positively correlated, which seems to make sense intuitively. As I mentioned earlier, it appears that survey ratings on inflight Wi-Fi and ease of online booking are also positively correlated. Lastly, and most expected, departure and arrival delays are highly positively correlated with one another.

### Marketing Implications
For marketing, this data set is straightforward in terms of its usefulness and is very applicable to the marketing field. Marketing many times focuses on customer satisfaction, this data contains survey results of just that. We can use this to understand which customers are dissatisfied and try to see which variables within the data lead to their dissatisfaction. 

### Advantages and Disadvantages
I mentioned above how this data set is advantageous for the marketing practice and is useful for many of the models we’ve learned. While the data set is useful for marketing, there are some disadvantages. First off, there isn’t a lot of demographic data in the data set. It’d be nice to have more demographic data to use for clustering analysis to aid in creating segments of our customers. While we have lots of data on survey satisfaction, we only have a small view into who that customer is. Additionally, there are some variables where I’m unsure how those variables were decided. What allowed the airline to categorize someone as either a loyal or disloyal customer? My guess is that there are more data points available that were used to get to that outcome that could’ve been used for demographic clustering. 
## Model Building
### Associations between variables/meaningful marketing models
When building models, I first began with **logistic regression**. I wanted to use logistic regression because I have a binary outcome variable in my data set (satisfied vs. dissatisfied), and logistic regression helps predict an outcome variable utilizing a mixed data set of both numeric and categorical (turned into factor) variables, so this type of model suits the data that I’m working with. Also, I wanted to try a **decision tree** model, since the output of these models can be clearly and easily communicated with leadership. I felt that a high accuracy decision tree model would help me share visuals with leadership on which variables are important in deciding which customers will be satisfied. I decided on logistic regression and decision tree models because I wanted to get an understanding of which variables were highly correlated with satisfaction or dissatisfaction so I could pay closer attention to these variables when moving to an unsupervised model, such as clustering. I wanted to try **clustering** to create segments of customers for specialized campaigns. I planned to end my analysis with clustering. After I know which variables most affect satisfaction, I will look at how these variables fit into certain clusters and try to create market segments from these clusters to push targeted campaigns to.

## Models created and reasons why
* Model 1: Logistic Regression (All Variables)
The first model I tried was logistic regression using all variables. I wanted to see which variables were considered statistically significant, and the odds ratio for all variables before limiting the input variables. 

* Model 2: Stepwise Logistic Regression
After running logistic regression with all variables, I ran stepwise logistic regression to help get a “best” version of the logistic regression model, utilizing significant variables determined by the model instead of every variable available. 

* Model 3: Decision Tree
Next, I ran a decision tree model. First, I wanted to see if the decision tree results showing which variables were important aligned with the logistic regression results. Also, I wanted a simple visualization to share with leadership if the results were aligned to show which variables impacted satisfaction. 

* Model 4: Random Forest
After running a decision tree, I noticed that the variables of importance in logistic regression seemed to also be important for the decision tree. I also decided to run random forest to create prediction using the decision tree method, but trying to avoid the model from overfitting, and to create a variable importance plot visualization.

* Model 5: PAM Clustering
Lastly, I ran PAM clustering. With PAM, I wanted to focus on the mixed demographic data to try and create clusters of customers that we can create campaigns for, which is why I decided to try clustering with PAM instead of Hierarchical or K-Means clustering. I ran PAM with multiple clusters but ended on 5 clusters, using only demographic data. The reason I didn’t want to use survey result data was because I wanted to create clusters of customers based on their demographics so we can create segments of people to send targeted campaigns to.

### Envisioned Outcomes/Benefits
With these models I can help the airline predict whether a customer will be satisfied or dissatisfied. I will explain which variables likely contribute to the outcome, and I can create clusters of customers. We can use these results to improve services that are the most important to satisfaction, and then push updated campaigns to customers who are likely to be dissatisfied. Once we can predict whether a customer will be satisfied or dissatisfied, and which variables lead to those outcomes, I envision the airline company can understand where to spend development money to improve their service offerings. Once they feel comfortable with the with the improvement of these services, we can create targeted marketing campaigns to new customers and previously dissatisfied customers lauding our new and improved services. With these models I can help the airline predict whether a customer will be satisfied or dissatisfied. I will explain which variables likely contribute to the outcome, and I can create clusters of customers. We can use these results to improve services that are the most important to satisfaction, and then push updated campaigns to customers who are likely to be dissatisfied. Once we can predict whether a customer will be satisfied or dissatisfied, and which variables lead to those outcomes, I envision the airline company can understand where to spend development money to improve their service offerings. Once they feel comfortable with the with the improvement of these services, we can create targeted marketing campaigns to new customers and previously dissatisfied customers lauding our new and improved services. 

## Analysis Methods
### Available methods to analyze the modes + instruments/tools used
To analyze supervised learning models, one simple way to compare performance between models is accuracy: how many times your model predicted the correct outcome variable. To get a gauge of model accuracy, I split the data into test and train data sets. I built the models with the train data, then ran it with the test data to check accuracy. I then created a Confusion Matrices to compare the predicted results of the test data to the actual results. These confusion matrices will look the same for any unsupervised learning model so you can compare prediction accuracy across many models (which I did to compare my logistic regression models vs. the decision tree and random forest models)

There isn’t as clear of a method to analyze unsupervised models, such as clustering, this is where knowledge of the company and field come into play. As I created clusters, I must look at the description of variables within these clusters, such as the cluster centers or the cluster means. I created visualizations that compare cluster means across certain outcomes, which I will show later in the paper, to see if some of these clusters paint a story and there are useful segments that can be created from the clusters.  

Another way to think about models and their strengths/weaknesses is knowing what you want to do with it. For example, if you want to create predictions but then make the results of your model very simple to share with other people, a Decision Tree model may meet your needs. However, these models may overfit the data, so you may give up the fit of the model for the easily explanatory nature of the results. 

### Choices + procedures used to analyze the data
After running both logistic regression models, I used the summary function and odds ratio to analyze variable importance and effect on the outcome variable. The summary function provides detail on the coefficients, but the odds ratio translates the log odds into odds, which is easier to interpret. For example, I looked for variables that had odds ratios greater than or less than 1 to understand their relationship to the satisfaction outcome compared to the inverse of a categorical variable or compared to a one unit change in a numeric variable. The summary function provides p values to get an idea of which variables are statistically significant. I used the confusion matrix to analyze accuracy, this told me how predictions on the test set were correct. I will explain the results of this evaluation in more detail in the results and discussion section.

For stepwise logistic regression, I also checked multicollinearity using Variation Inflation Factor (VIF) to see if any variables were highly correlated with one another and there was redundancy in the model. This helped me decide if I should make any further adjustments to the model. 

For the random forest model, I also used a confusion matrix to analyze accuracy. On top of this, I was able to create variable importance plots to understand which variables were most important to understand the variable’s importance to the accuracy of the model.

For PAM clustering, I used the silhouette method to determine the best number of clusters. After running the cluster model, I reviewed cluster means to analyze the output. This simply provides a list of all the clusters and the mean observation of each variable within that cluster. After reviewing the means, I also created bar plots to review each cluster against certain variables that I deemed were important after running logistic regression and decision tree models. I would see the mix of satisfaction vs. dissatisfaction between each cluster, as well as the mix of ratings for some of the survey results.

## Results and Discussion
### Stepwise Logistic Regression
I will start with the supervised learning models. I will first discuss the findings from my stepwise logistic regression model, I will skip detailed explanation of the logistic regression model with all variables, because the results were very similar to the stepwise model, but it included some insignificant variables, and the accuracy was slightly lower. With stepwise logistic regression, the model kept many, but not all variables. Some variables it kept were customer type, age, type of travel, class, flight distance, inflight Wi-Fi service, ease of online booking, and online boarding ratings. Almost all values in the model were significant. When looking at the odds ratios, I saw the variables causing the highest likelihood of satisfaction were a one unit increase in the online boarding rating, a one unit increase in the inflight Wi-Fi rating, and a one unit increase in the check-in service rating. After checking the VIF, no variable had a score close to 5, so I felt confident that there wasn’t much redundancy in the model caused by highly correlated variables. When I ran the model on test data, the model was getting almost 87% accuracy in predicting customer satisfaction. In my opinion, the most important output of this model was understanding that one unit increases in certain ratings were linked to high chances of satisfaction. I wanted to start focusing on inflight Wi-Fi ratings and online boarding ratings for the rest of my analysis. 

Stepwise logistic regression odds ratios:

![image](https://user-images.githubusercontent.com/44247793/130681633-82203b37-9261-4cd4-81d6-5686af7d0698.png)

Stepwise VIF:

![image](https://user-images.githubusercontent.com/44247793/130681652-b79df6dd-7e50-4955-beb6-cad59a4e48b8.png)

Stepwise confusion matrix and accuracy:

![image](https://user-images.githubusercontent.com/44247793/130681692-45db43da-cb4c-4257-bec1-4bf2e03794a4.png)

### DecisionTree and Random Forest
The decision tree model only used three variables in tree construction: inflight Wi-Fi service, online boarding, and type of travel. The first split was whether the online boarding rating was less than or greater than 4 out of 5. If their rating was greater than 4 and they were not a personal traveler (they were a business traveler), the model predicts they will be a satisfied customer. Like the stepwise logistic regression model, I’m starting to see a pattern that the online boarding and inflight Wi-Fi ratings are important variables for determining customer satisfaction. If the online boarding was greater than 4 and they were a personal traveler, it then splits on the inflight Wi-Fi rating, which if it was less than perfect, 5, the model predicts the customer will not be satisfied. I also ran a prediction on this model, which got 88% accuracy, but I didn’t put much weight into the accuracy rating as it could be overfitting, I’ll put more importance on random forest accuracy. I like the decision tree model due to its simplicity. In a presentation to management, I’d be able to put this visualization right into a power point and show which variables were the best indicators for predicting customer satisfaction. 

Decision Tree:

![image](https://user-images.githubusercontent.com/44247793/130682124-2bf068f5-882f-47dc-887c-b252bab25faf.png)

Decision Tree Confusion Matrix + Accuracy:

![image](https://user-images.githubusercontent.com/44247793/130682151-93053c80-5527-43ca-aa82-830ad11f0fd2.png)

When running the random forest model, I got much higher accuracy, almost 96%. Since random forest runs many decision trees, it makes sense that it had higher accuracy, and I also have fewer concerns the model is overfitting. I couldn’t t plot a tree with the random forest model; however, I created a plot of the mean decrease accuracy, which measures the extent a variable improves the accuracy of predicting the outcome. And a I plotted the mean decrease Gini, which factors the contribution the variable makes to accuracy, and the degree of misclassification. Like previous supervised learning models suggested, online boarding and inflight Wi-Fi service ratings had the highest scores in both plots. 

Random Forest mean decrease accuracy and mean decrease Gini

![image](https://user-images.githubusercontent.com/44247793/130682200-712bc6b2-3f5a-4127-9f99-92adafb38f8f.png)

Random Forest confusion matrix + accuracy

![image](https://user-images.githubusercontent.com/44247793/130682229-4ee79ead-c2d7-45fe-acb2-05bd1e16e6b1.png)

Of all the supervised learning models I ran, I’d recommend the random forest model if the airline wanted to create and operationalize a model to predict customer satisfaction. It had the highest accuracy of any of the models I ran, and I could clearly see which variables were important to the model. At this point, I felt confident that these two variables were important contributors to customer satisfaction. Next, I moved to clustering. 

### PAM Clustering
I ran both K-Means and PAM clustering originally but decided to only use PAM clustering and limit the data to demographic data. I did this because I wanted to create segments based off demographics, and then see how those segments rated inflight Wi-Fi and online boarding in their survey results. This way, the airline would know which demographics to push marketing campaigns to in the future. The reason I stuck with PAM was because many of the demographic data available were categorical variables that could be turned into factors, and PAM works well with factors. 

When creating the silhouette plot to estimate the best number of clusters, the plot recommended 10 clusters. I felt this was too high and would be difficult to create segments for 10 different types of customers. The plot increased as cluster count increased, so I decided to go with 5 clusters. This was the highest number of segments I felt the airline would want to work with.

Silhouette Chart

![image](https://user-images.githubusercontent.com/44247793/130682281-2ce9cdc9-2a46-4ca1-988c-99780910acf3.png)

After creating 5 clusters, the distribution of clusters was even across all clusters. I’ve summarized some of the clusters below:
* Cluster 1: Cluster 1 contained all female passengers who were mostly considered loyal. These passengers fly business class and for business travel. They also flew the longest flights in terms of distance. This was the oldest cluster by age and had a high percentage of satisfied customers.
* Cluster 2: Like cluster 1, cluster 2 was another cluster of all female, loyal passengers. However, this cluster mostly flew personal travel in economy class, was slightly younger in age, and had a high percentage of dissatisfied passengers.
* Cluster 3: Cluster 3 was the only cluster with a mix of male and females. This was the youngest cluster by far and contained mostly disloyal customers who were travelling for business but flying economy. Customers in this cluster were mostly dissatisfied. 
* Cluster 4: Customers in cluster 4 were all male, loyal customers flying for personal reasons in economy class. This cluster had a high percentage of dissatisfied customers.
* Cluster 5: Cluster 5 was the opposite of cluster 4 in a few ways. These were still all male, loyal customers, but these customers were travelling for business purposes in business class and were mostly satisfied with their experience. Passengers in cluster 5 had an average flight distance more than 2x higher than cluster 4.

Cluster Means

![image](https://user-images.githubusercontent.com/44247793/130682338-6fbb73a1-c5a9-4b54-a9f1-c76dd98ed97e.png)

Satisfaction by Cluster

![image](https://user-images.githubusercontent.com/44247793/130682371-69a4ea18-d781-443c-8086-98cfc2a783d5.png)

As I mentioned when describing the clusters, clusters 1 and 5 had the highest proportion of satisfied customers, 2-4 were more dissatisfied. Next, I compared each cluster to their most common ratings for Inflight Wi-Fi service and online boarding. My logistic regression, decision tree, and random forest models all suggested these were important variables that contributed to satisfaction. As we can see, clusters 2-4 had the highest proportion of 2- and 3-star ratings for both variables, while clusters 1 and 5 had a significantly higher proportion of 5-star ratings than the rest of the clusters. 

![image](https://user-images.githubusercontent.com/44247793/130682396-ecc96d04-23df-4b3d-bc9c-ac4d9e36b476.png)

![image](https://user-images.githubusercontent.com/44247793/130682411-f1b5adf6-f228-46c9-97f2-e8f4b10bd841.png)


## Recommendations
Overall, these models helped me put together concrete recommendations for the management team. It’s apparent that increases in inflight Wi-Fi service ratings and online boarding ratings positively contribute to customer satisfaction. These are two straightforward areas of the business where the airline should focus future improvements. There were also some unexpected findings, for example, I found it strange that the odds ratio in my logistic regression model showed a one unit increase in ratings contributed negatively to customer satisfaction, such as the online boarding experience and the convenience of the departure or arrival time.

Service improvements and targeted campaigns for market segments
1.	Our company needs to invest in improvements to our inflight Wi-Fi service. We may want to commission a survey specifically for Wi-Fi to understand which parts of the service are causing passengers to be dissatisfied, for example, is it the speed of the internet or the cost of the internet? 
a.	After making improvements to the service, our airline needs to market the improvement to customers, so previously disloyal customers who were rated the service low may be tempted to try it again. I think customers that fit into cluster 3 are a unique customer that we should target our campaign to. Cluster 3 had younger, disloyal flyers who were flying for business purposes and were mostly dissatisfied with their experience. Business flyers were generally very satisfied with our airline, this cluster was the exception. So, the airline has a chance to turn these into loyal, satisfied customers. Since they are flying for business, they likely place a high importance on the Wi-Fi so they can work on their flight. Once we’ve made improvements to the service, I think we should target customers that fit into this “young, economy traveler for business purposes” segment and send them marketing ads promoting our new and improved inflight Wi-Fi. With these ads, we should offer them free Wi-Fi for one round-tip with our airline. This segment of customers may be willing to try our airline again and will be able to test the service for free, possibly increasing their satisfaction with the service and ultimately our airline. 
b.	Lastly, the airline should offer a free 20 minutes of Wi-Fi to all passengers. This way, a passenger who was previously dissatisfied with the inflight Wi-Fi can try it again, and potentially adjust their personal rating of the service. 
2.	Our company should invest in improvements in the online boarding experience. If we already have an online app that allows passengers to check in and get a boarding place in line in line, we should fund research to understand why passengers are unhappy, or happy, with this service. If this service currently doesn’t exist, we should invest R&D funds into a team to develop an app that would provide this service, since many airlines already provide this.
a.	For surveys, we should target our surveys to customers that fit into cluster 1 for a couple of reasons. Cluster 1 was our cluster of loyal, female passengers flying for business purposes. Cluster 1 had the second highest percent of 5-star ratings for the service, we could ask detailed questions to understand why they were satisfied. Cluster 1, however, also had the highest percent of 1-star ratings, so targeting survey results to customers in this cluster may provide us a wide range of reasoning behind both satisfaction and dissatisfaction of the service.


## Limitations
### Insights about the limitations of the current dataset
Some significant limitations of the data set are that it didn’t contain a lot of demographic data that could be used to create highly tailored market segments. I was able to create segments based on a customer’s age, and details on why they fly, but I couldn’t create segments based on the areas they live, the number of times they travel throughout the year, where they travel, etc. With more demographic data, I think I could create stronger market segments. 

Also, the data simply provides one rating for each important factor of the flight; it doesn’t provide more detail than that. I can’t investigate what specifically about the Wi-Fi service that customers were unhappy with, or what aspects of the online boarding process customers were dissatisfied with.

### Recommendations or plans for future analysis
While I believe the airline should make improvements to their Wi-Fi and online boarding offerings, they should collect more specific, detailed survey data on these two variables. I want to understand why customers are unhappy with the services, and what aspects of the service they are unhappy with. This would allow me to analyze reasons behind dissatisfaction of these services much deeper, and I could recommend the company to focus their development and improvement to areas of the service that truly need it or are worth it. 

Additionally, adding a time series element to the data set could help future analyses. I could review customer satisfaction and customer survey ratings over time. If the airline makes changes to these services, we will want to track how ratings and satisfaction changes as these services are improved and new campaigns are rolled out to understand the effectiveness of the airline’s efforts. 

