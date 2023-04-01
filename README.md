
# Customer Segmentation in Online Retail

In this project, I analyzed various customer segments in Online Retail dataset using python. For this task, I employed cohort analysis, RFM Analysis and k-means clustering.

## Problem Statement

**Identify the customer segmengts in the dataset and thereby prescribe course of business acton for each segment.**

Example of a segment might be the customers who bring the max profit and visit frequently.

## Data Overview

Source: [The UCI Machine Learning Repository](https://www.kaggle.com/datasets/carrie1/ecommerce-data)

This data set contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a non-store online retail. 

## Data Snapshot

![Screenshot (516)](https://user-images.githubusercontent.com/121576163/229271541-059cf0f3-272b-41ba-a53c-fae854fbe90e.png)

## Data Exploration 

- Removed Null Values
- Removed duplicated Values
- Maximum transactions are from UK

![Screenshot (517)](https://user-images.githubusercontent.com/121576163/229271584-9185bbb3-0da0-44dc-8aa7-99940322b33d.png)

### Cohort Analysis

A cohort is a set of users who share similar characteristics over time. Cohort analysis groups the users into mutually exclusive groups and their behaviour is measured over time.

There are three types of cohort analysis:

- Time cohorts: It groups customers by their purchase behaviour over time.
- Behaviour cohorts: It groups customers by the product or service they signed up for.
- Size cohorts: Refers to various sizes of customers who purchase company's products or services. This categorization can be based on the amount of spending in some period of time.

For this project, I have chosen time cohorts. The steps are as follows:

1. Identified cohort month for each customer (The month when customer first transacted)
```
# First Transaction month (Cohort Month) for each customer
df3['Cohort Month']=df3.groupby('CustomerID')['InvoiceFormat'].transform(min)
```

2. Identified cohort index (difference between transaction month and cohort month) for each transaction.
```
# This function calculates difference between invoice format and cohort month
def diff(d,x1,y1):
    l=[]
    for i in range(0,len(d)):
        xyear=d[x1][i].year
        xmonth=d[x1][i].month
        yyear=d[y1][i].year
        ymonth=d[y1][i].month
        diff=((xyear-yyear)*12)+(xmonth-ymonth)+1
        l.append(diff)
    return l
```

3. Grouped data by cohort month and cohort index.
4. Developed a pivot table.

![Screenshot (518)](https://user-images.githubusercontent.com/121576163/229271650-bda075f9-bb60-4f31-a862-6286d2f35c6e.png)

5. Developed a time cohort heatmap

![Screenshot (519)](https://user-images.githubusercontent.com/121576163/229271678-4586ca52-ba15-4ff9-a155-3fa704b9a698.png)

### Summary

1. We are roughly left with 10% of new joiners after an year of use. Retention thereby is quite poor.

2. Every month we are adding roughly 250 new people. Marketing regarding this aspect is Ok.

## RFM Analysis

RFM is ***Recency, Frequency, Monetary***. It looks at what was the last time a customer transacted, how frequent they transacted and what monetary value they bring to the business as factors to assign score to customers. These scores can further be used to group customers.

1. **Recency**

- The last transaction in the datset was on 2011-12-09. Thus the recency score was calculated taking 2012-01-01 (New Year) as snapshot date.
- Recency score is the difference between snapshot date and last transaction date by each customer. It is reported in no. of days.

2. **Frequency**

```
freq=df6.groupby(["CustomerID"])[["InvoiceNo"]].count()
```

3. **Monetary**

```
df6["total"]=df6["Quantity"]*df6["UnitPrice"]
money=df6.groupby(["CustomerID"])[["total"]].sum()
```
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## Clustering

Before K means clustering, I removed data skewness.

- RFM Distribution

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

Clearly the data is left skewed. I used log transformation to remove skewness.

- After log transformation

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


### Implementing K means

```
inertia=[]

for i in np.arange(1,11):
    kmeans=KMeans(n_clusters=i)
    kmeans.fit(scaled)
    inertia.append(kmeans.inertia_)
```

```
plt.figure(figsize=(12,8))
plt.plot(inertia, marker="o");
```

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

From the graph, Ichose a cluster size of 3.
The cluster statistics are :

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

I then labeled the customer segments as :

1. Major
2. At risk 
3. Average Customers

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

The relative importance of cluster charactersitics are:

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## Suggestions and Cluster Interpretation: 

1. **At Risk Customers** : These Customers have transacted a long time ago and contribute least in monetary terms.
- ***Suggestion*** : These customers may have already exited from customer base. Try to understand why they left. Some sale and discount offers might help to bring a portion back.

2. **Average Standing customers**: These Customers have transacted a recently and regularly, and contribute appreciably in monetary terms.
- ***Suggestion*** : Need to handle them with care and convert them to best customers. Discount and Sale are highly desirable. Provide top customer support and services.

3. **Best customers** : These customer transacted recently, are incredibly frequent and bring massive money to the company.
- ***Suggestion*** : These customers can be a target of newly launched product. Repeated advertising can further increase revenue. Heavy discounts are not required.


















