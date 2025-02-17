# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Useful Resources
- [ScriptRunConfig Class](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py)
- [Configure and submit training runs](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-set-up-training-targets)
- [HyperDriveConfig Class](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriveconfig?view=azure-ml-py)
- [How to tune hyperparamters](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-tune-hyperparameters)


## Summary
**In 1-2 sentences, explain the problem statement: e.g "This dataset contains data about... we seek to predict..."**

The dataset contains data about clients and the result of marketing campaign (whether the client desided to subscribe to a term deposit or no (yes/no). We seek to predict, whether the client will subscribe to the term deposit based on client information such as age, job position, marital status and etc.

**In 1-2 sentences, explain the solution: e.g. "The best performing model was a ..."**

The best performing model was a VotingEnsemble model produced by AutoML, though, the performance of LogisticRegression model was not far away from VotingEnsemble (accuracy of 90.9 compared to 91.3 respectively). 

## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**

The pipeline starts off with loading dataset from given url and passed through cleaning step done in clean_data() function. In the cleaning step, NaN data points were droped. Then categorical features are one-hot encoded, whereas binary categorical features were converted to 0 and 1. Then trained data is sent to LogisticRegression classification model with hyperparameters passed by RandomParameterSampler. The hyperparameter tuning is stopped according to BanditPolicy. 

**What are the benefits of the parameter sampler you chose?**

The RandomParameterSampler is beneficial because it randomly samples from search space, which usually takes much less time compared to Grid Search yet it results in same performance result. So RandomParameterSamples is fast and effective parameter sampler. 

**What are the benefits of the early stopping policy you chose?**

The Bandit Policy is slack based early termination policy, meaning that it will evaluate model performance at evaluation_interval and if current performance (given slack_factor) is larger than previous result then hyperparameter tuning continues. This policy reduces time spent on redundant training. It allows fast and efficient hyperparameter tuning. 

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**

The AutoML run selected VotingEnemble as a best model, which is essentially an average of votes of other trained models. Generated hyperparameters vary for each model, however, what can be noticed is different scaler were used for models, meaning scaler methods were treated as hyperparameters.

In the automl_config we provided experiment timeout, means that it will constrain experiment run to this time. Then we selected machine learning task (in our case classification task) and selected primary metric for model evaluation (in our case it was accuracy). Then we provided with training data and target column. Finally, we selected number of cross validations. 

## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**

AutoML selected VotingEnsemble as best model, whereas HyperDrive was tuning only LogisticRegression. Even though VotingEnsemble is more complex model, its performance was not far better then simple logistic regression, where accuracy was 91.7 and 90.9 respectively. VotingEnsemble averages on voting of other classification models, which contributed on slightly better performance. However, the classification task is probably simple enough to be modelled by logistic regression, hence, its performance matches results of far complex models. 

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**

As in all data science tasks, more thorough explotary data analysis and feature generation may result improvement of overall model performance. New features may be more correlated with target value or produce more insights on the problem.

## Proof of cluster clean up
!['Proof'](https://github.com/bekiichone/nd00333_AZMLND_Optimizing_a_Pipeline_in_Azure-Starter_Files/blob/master/delete%20aml%20compute.PNG)
