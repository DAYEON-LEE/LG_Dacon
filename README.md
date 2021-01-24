# LG_Dacon


|model|data|parameter tuning|auc_score|accuracy|precision|recall|f1_score|final_score|
|--|--|--|--|--|--|--|--|--|
|lgb+StratifiedKFold5+SMOTE|train_err|'objective': 'binary','boosting_type': 'dart','subsample_freq': 5,'num_leaves': 92, 'min_data_in_leaf': 64, 'subsample_for_bin': 23000, 'max_depth': -1,'feature_fraction': 0.302,'bagging_fraction': 0.904,'lambda_l1': 0.099, 'lambda_l2': 1.497,'min_child_weight': 38.011,'nthread': 32, 'metric': 'auc', 'learning_rate': 0.021, 'drop_rate': 0.846244, 'skip_drop': 0.792465, 'max_drop': 65,'seed': 42,'n_estimators': 1000|0.8997|0.8276|0.8806|0.7455|0.8153| |
|lgb+kfold5+dart+SMOTE|train_err|'boosting_type' : 'dart','objective': 'binary','metric': 'auc','learning_rate' : '0.02','seed': 1015, num_boost_round=1200,early_stopping_rounds=200|0.9021|0.8264|0.9611|0.6413|0.81127|0.81291|
|lgb+StratifiedKFold5+dart+SMOTE|train_err|'objective': 'binary','boosting_type': 'dart', 'subsample_freq': 5,'min_data_in_leaf': 64, 'max_depth': -1, 'feature_fraction': 0.302,'bagging_fraction': 0.904, 'nthread': 32,'metric': 'auc', 'learning_rate': 0.01, 'max_drop': 65,'seed': 1015,'n_estimators': 1000|0.8973|0.8249|0.9004|0.71|0.8063|0.80759|
|lgb+kfold10+SMOTE|train_err| "objective" : "binary","metric" : "auc","boosting": 'gbdt',"max_depth":-1,"num_leaves":64,"learning_rate":0.01,"seed":1015,num_boost_round=1200,early_stopping_rounds=200|0.9101|0.8279|0.9945|0.5977|0.81749|0.81195|

## stacking model 

|num|model|data|parameter tuning|auc_score|accuracy|precision|recall|f1_score|final_score|
|--|--|--|--|--|--|--|--|--|--|
|1|lgb_gbdt+kfold12+SMOTE|train_err|'boosting_type' : 'gbdt','objective': 'binary','metric':'auc','learning_rate':'0.01','seed':1015,num_boost_round = 1000,early_stopping_rounds = 50|0.9098|0.8279|0.9978|0.5770|0.8163||
|2|lgb_dart+kfold5+SMOTE|train_err|'boosting_type':'dart','objective':'binary','metric': 'auc','learning_rate': '0.02','seed':1015,num_boost_round = 1200, early_stopping_rounds = 200|0.9021|0.8264|0.9611|0.6413|0.8112||
|3|xgb+kfold5+SMOTE|train_err|'objective':'binary:logistic','eval_metric' :'auc','early_stoppings':100,num_boost_round=1000|0.8886|0.8120|0.9390|0.6855|0.8034|0.8*(num1)+0.1*(num2)+0.1*(num3)=**0.81320**|
|-|- |- |same model as the one above |- | -|- |- |- |0.6*(num1)+0.2*(num2)+0.2*(num3)=**0.81226**|
