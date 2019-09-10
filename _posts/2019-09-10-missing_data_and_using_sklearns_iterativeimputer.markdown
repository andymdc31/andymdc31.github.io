---
layout: post
title:      "Missing Data and Using Sklearn's IterativeImputer"
date:       2019-09-10 16:21:11 +0000
permalink:  missing_data_and_using_sklearns_iterativeimputer
---


A majority of data science is cleaning or cleansing the data set you are given. You have to detect any values that may give you trouble later on for certain algorithms. You have to deal with any missing values. Sometimes you have to drop certain data if there is just too many missing values for a specific column or feature. Some data is filled with NaN's while other may have placeholder data like '?' or maybe a default value. It is important to check all of these.  

I'm sure most people reading this have dealt with the issue of missing data before. Every blog you read or paper seems to talk of a different way to handle missing data. For my first project over the King County dataset I wanted to approach missing data in a way that seemed "better." I have that in quotes because it seems a lot of people have personal preferences when it comes to the best way to handle missing data. Obviously you have to see why the data was missing in the first place. Is there a trend with the missing data that perhaps is useful in itself? Did your survey or other way of gathering information pose a question that most people shied away from? This could be useful and instead of filling the values in, you could have important data in front of you.

What are some of the common ways of replacing missing values or "NaN" though?  The roughest way in my opinion would be to just drop any rows or sections with missing data. Maybe this will have no affect on the outcome of your model or further analysis. This really depends on the size of the dataset, what percentage is missing, and what you are trying to deduce from the data. You really need to understand why you are looking at the data. What are you trying to learn from it? Dropping data usually will not aid you. Another way of dealing with missing data would be more ideal. 

You could replace all the values with either the mean, median, mode, or random values. Once again this really is a case by case approach. Any values you enter into the missing data will skew or bias your results in some way. If you have only 0's and 1's in a column maybe you can deduct that all the missing values are 0, or maybe you can't. You may have to test several approaches to filling your data. 

I thought that surely there was a more analytic approach to dealing with missing data. A way for an algorithm or function to determine missing values from its neighbors or discover patterns. This is how I came across Sklearn's [IterativeImputer]([https://scikit-learn.org/stable/modules/generated/sklearn.impute.IterativeImputer.html).

In statistics, imputation is the process of replacing missing values with substituted values. So what makes IterativeImputer better or worse than just filling in the values yourself? One warning is that it is still experimental and predictions or the API might change. IterativeImputer works much like a MICE algorithm in that it estimates each feature from all other features  in a round-robin fashion. If you have any experience with R  you may notice some similarities with missForest. You can choose how many iterations or rounds that you want the imputer to go through. There are some neat experiments on sklearn that show different ways of using IterativeImputer and what is "better", you can find those [here](https://scikit-learn.org/stable/auto_examples/impute/plot_iterative_imputer_variants_comparison.html#sphx-glr-auto-examples-impute-plot-iterative-imputer-variants-comparison-py)

There are some downsides though. Biases will be formed depending on the data in the other features since IterativeImputer is going through and 'fitting' the values. You may end up going back through the data and reworking it to make more sense for your dataset. For example, if you have only integers in your original data but now without rounding or any other modifications, you will suddenly find decimal values. Negative values are also possible so just keep that in mind. 

While there are some interesting implementations of IterativeImputer I couldn't find any article or stackoverflow over just the simple use of sklearn's IterativeImputer which was my main motivation for writing this. It really is only a few lines of code and you may have found a new way of imputing missing data.  

```
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer
df = pd.DataFrame(*some dataset with missing values to be imputed*)
imp = IterativeImputer(max_iter=10, verbose=0)
imp.fit(df)
imputed_df = imp.transform(df)
imputed_df = pd.DataFrame(imputed_df, columns=df.columns)
```

That is it. 

Now any missing values ('NaN') you have in your pandas dataframe ('df') will be filled in the new dataframe I have called ('imputed_df'). The steps are to create an IterativeImputer, imp and pass this any arguments you may need,  you can find our more on the sklearn website. The default iterations is already set to 10. Verbose is optional and is just related to the verbosity flag, which I will not go into. The imputer is then fitted to the original dataset with the missing values. After the imputer is fitted, it can then be transformed to a new IterativeImputer object; but you may want to get it back to a pandas dataframe, which is what the last line is for. 

There are other imputers out there and some are fun to play around with and see what values you may end up getting. Just do your research and see what works best for your dataset. Missing data isn't going anywhere anytime soon so just get comfortable with it!



