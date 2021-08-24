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
* ![image](https://user-images.githubusercontent.com/44247793/130680834-7561e9d5-7998-4064-a043-2cabf98f4ad8.png)
* 
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

