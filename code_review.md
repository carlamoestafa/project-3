# Code Review: Project 3

[X] README.md included <br/>
[X] Slides Included <br/>
[X] Code Included <br/>

## Clean Code:
* When transforming/scaling only some columns, it's probably cleaner and easier to make a new DF of just those columns, transform the whole thing at once, then assign the result to the original columns. For example:
    
  scaled_features = data.copy()</br>
  col_names = ['Age', 'Weight']</br>
  features = scaled_features[col_names]</br>
  scaler = StandardScaler().fit(features.values)</br>
  features = scaler.transform(features.values)</br>
  scaled_features[col_names] = features
    
* Consider combining more of your cells into one if they are part of the same step, such as defining X's and y's. It's difficult to read dozens of consecutive lines and makes it much harder to navigate your code. 
* If you're repeately testing the same model with different features, you should probably write a single function to do all the repetitive steps. In the cross-validation/feature selection section, the exact same code is used in literally dozens of consecutive cells -- this should never happen. Write it once, save it as a function and then call it with the different data sets you want to try. This single section accounts for nearly SEVENTY cells and makes it extremely inconvenient to navigate the notebook. Additionally, when this is run in order, you're holding over 35 nearly identical dataframes in memory for no real reason. 
* 10 is probably excessive for cross-validation and may even make your results worse if you don't have enough observations to support it. It would also be much more efficient to use sklearn's KFold, fit the model to the folds and have them calculate all 4 scores at once. With your current method of using cross_val_score for each different score individually, you're fitting a model 40 times (10 folds for each of your 4 scores) instead of 10.

## Good Documentation:
* Minor grammatical errors in readme (e.g. "scalling", missing spaces)
* Readme is probably a little too detailed -- specific parameters and feature selections may be a bit too much information. 
* Markdown cells could be used more effectively to explain thought process and workflow. Don't be afraid to write more full sentences and paragraphs between sections. Many sections (e.g. resampling) have no explanation of reasoning or conclusions. 
* Try to avoid phrasing your thoughts as questions if you aren't going to directly attempt to answer them.

## Proper Data Science:
* If the GridSearch results seem incorrect, it's probably best not to use that model and not include it in the final ReadMe/results. 
* The conclusion doesn't seem to indicate what an acceptable threshold might be so it's difficult to know what would or would not be an acceptable False Positive or Recall rate. 
* The order of your scaling and train-test splits is difficult to follow. In general you should be splitting into train and test before doing any scaling or fitting any models. Do not fit a model to the entire data set. If you want to train on unscaled data (which you probably should not do for Logistic Regression anyway), this section of the notebook should come before you create the scaled dataframes. It becomes very difficult to keep track of what dataframe you're working with. 
* The correlation matrix should probably be analyzed earlier in the notebook -- it's a part of basic EDA that you should be doing before fitting and manipulating models. 
* It's not clear what the motivation for oversampling is or what your conclusion is about whether or not to resample your data for the final model.

## Comments:
* Avoid including your data (the csv file) in your repo, you already provide a link to the data source and it will often be too large to host on git and lead to potential commit issues. 
* I'd highly recommend splitting future projects into separate notebooks for processing, EDA and modeling.
* Consider renaming the repo to make it more presentable as an independent rather than an assigned project

