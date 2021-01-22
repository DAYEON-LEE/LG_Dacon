# LG_Dacon


|model|data|parameter tuning|auc_score|accuracy|precision|recall|f1_score|final_score|
|--|--|--|--|--|--|--|--|--|
|lgb+StratifiedKFold5+SMOTE|train_err|'objective': 'binary','boosting_type': 'dart','subsample_freq': 5,'num_leaves': 92, 'min_data_in_leaf': 64, 'subsample_for_bin': 23000, 'max_depth': -1,'feature_fraction': 0.302,'bagging_fraction': 0.904,'lambda_l1': 0.099, 'lambda_l2': 1.497,'min_child_weight': 38.011,'nthread': 32, 'metric': 'auc', 'learning_rate': 0.021, 'drop_rate': 0.846244, 'skip_drop': 0.792465, 'max_drop': 65,'seed': 42,'n_estimators': 1000|0.8997|0.8276|0.8806|0.7455|0.8153| |
|lgb+kfold5+dart+SMOTE|train_err|'boosting_type' : 'dart','objective': 'binary','metric': 'auc','learning_rate' : '0.02','seed': 1015, num_boost_round=1200,early_stopping_rounds=200|0.9021|0.8264|0.9611|0.6413|0.81127|0.81291|
|lgb+StratifiedKFold5+dart+SMOTE|train_err|'objective': 'binary','boosting_type': 'dart', 'subsample_freq': 5,'min_data_in_leaf': 64, 'max_depth': -1, 'feature_fraction': 0.302,'bagging_fraction': 0.904, 'nthread': 32,'metric': 'auc', 'learning_rate': 0.01, 'max_drop': 65,'seed': 1015,'n_estimators': 1000|0.8973|0.8249|0.9004|0.71|0.8063|0.80759|
