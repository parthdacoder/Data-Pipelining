# Pipelining
The repository aims to simplify pipelining for anyone who goes through this. Also highlight the aspect of automation as it reduces the time to apply the same features again on different datasets. 

1) Functions.ipynb - the file shows a basic outline in terms of pre-processing by creating functions and using them on a dataframe.
2) data_module.py - It contains the function load_data() which is called for the pipeline.
3) The pipeline.ipynb - It shows us how the functions made in Functions.ipynb can be converted to a pipeline so that it can be used for different dataframes again:
   from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.preprocessing import OneHotEncoder
from sklearn.impute import SimpleImputer
import pandas as pd


class NameDropper(BaseEstimator, TransformerMixin):
    
    def fit(self, X, y = None):
        return self
    
    def transform(self, X):
        return X.drop(['Name'], axis = 1)
    
class AgeImputer(BaseEstimator, TransformerMixin):
    
    def fit(self, X, y = None):
        return self
    
    def transform(self, X):
        imputer = SimpleImputer(strategy='mean')
        X['Age'] = imputer.fit_transform(X[['Age']])
        return X
    
class GenderTransformer(BaseEstimator, TransformerMixin):
    
    def fit(self, X, y = None):
        return self
    
    def transform(self, X):
        X = X.copy()
        X['Gender'] = X['Gender'].replace({'f':0, 'm':1})
        return X
        
class JobEncoder(BaseEstimator, TransformerMixin):
    
    def fit(self, X, y = None):
        return self
    
    def transform(self, X):
        X = pd.get_dummies(X, columns = ['Job'])
        return X
from sklearn.pipeline import Pipeline

pipe = Pipeline([
    ("dropper" , NameDropper()),
    ("imputer", AgeImputer()),
    ("transformer", GenderTransformer()),
    ("encoder", JobEncoder())   
]
)
   
