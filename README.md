# LG_Dacon 

## 모델, 사용한 데이터, 파라미터튜닝, 스코어 소개 


|모델|훈련데이터|파라미터튜닝|스코어|
|--|--|--|--|
|XGBOOST|train_err|threshold=.5 early_stoppings=100 num_boost_round=1000 eval_metric='auc' objective='binary:logistic' |0.702084|
|XGBOOST+Kfold5|train_err|threshold=.5 early_stoppings=50 num_boost_round=1000 eval_metric='auc' objective='binary:logistic' |0.711779|
|LGB+kfold5|train_err+train_quality|threshold=.5 early_stoppings=3 num_boost_round=1000 eval_metric='auc' objective='binary' boosting_type='gbdt' |0.658334|
|LGB+kfold5|train_err+train_quality|threshold=.35 early_stoppings=3 num_boost_round=1000 eval_metric='auc' objective='binary' boosting_type='gbdt' |0.728858|
|LGB+PCA35+Kfold5|train_err+train_quality|threshold=.5 early_stoppings=50 num_boost_round=1000 eval_metric='auc' objective='binary:logistic' |0.624817|
|RF|train_err|기본값|0.704615|
|LGB+Kfold+SMOTE|train_err|현재 파라미터 튜닝하며 성능 Check중...|0.81xxx|

### 1. 알게된 점
- train_err와 train_quality병합하여 결측치 처리하여 modeling하면 성능이 좋지않았다. 
- train_err이 레이블값에 미치는 영향이 큰 것으로 나타났다. 
- xgboost와 lgb가 그레디언트 부스팅에선 가장 성능이 좋았지만 그래도 0.7초반이라 낮았다. 
- random forest의 경우 세세한 파라미터 튜닝은 하지 않았지만 default로 분석하니 0.7의 성능이 나왔다. 
- 현재는 lgb+kfold에 레이블(1,0)의 불균형(imbalanced)이 심할 때 비율을 같게 맞춰주는 방법인데 성능효과에 탁월하다는 것을 발견했다.   
    ->  현재 파라미터 튜닝하면서 성능개선을 계속 진행중입니다. 
  
  
### 2. eda폴더

- train_err와 train_quality간의 관계가 어떤지 전처리 및 분석한 결론을 정리했습니다. 

## 
|model|data|parameter tuning|auc_score|accuracy|precision|recall|f1_score|final_score|
|--|--|--|--|--|--|--|--|--|
|lgb+StratifiedKFold5+SMOTE|train_err|'objective': 'binary','boosting_type': 'dart','subsample_freq': 5,'num_leaves': 92, 'min_data_in_leaf': 64, 'subsample_for_bin': 23000, 'max_depth': -1,'feature_fraction': 0.302,'bagging_fraction': 0.904,'lambda_l1': 0.099, 'lambda_l2': 1.497,'min_child_weight': 38.011,'nthread': 32, 'metric': 'auc', 'learning_rate': 0.021, 'drop_rate': 0.846244, 'skip_drop': 0.792465, 'max_drop': 65,'seed': 42,'n_estimators': 1000|0.8997|0.8276|0.8806|0.7455|0.8153| |
|lgb+kfold5+dart+SMOTE|train_err|'boosting_type' : 'dart','objective': 'binary','metric': 'auc','learning_rate' : '0.02','seed': 1015, num_boost_round=1200,early_stopping_rounds=200|0.9021|0.8264|0.9611|0.6413|0.81127|0.81291|
|lgb+StratifiedKFold5+dart+SMOTE|train_err|'objective': 'binary','boosting_type': 'dart', 'subsample_freq': 5,'min_data_in_leaf': 64, 'max_depth': -1, 'feature_fraction': 0.302,'bagging_fraction': 0.904, 'nthread': 32,'metric': 'auc', 'learning_rate': 0.01, 'max_drop': 65,'seed': 1015,'n_estimators': 1000|0.8973|0.8249|0.9004|0.71|0.8063|0.80759|
|lgb+kfold10+SMOTE|train_err| "objective" : "binary","metric" : "auc","boosting": 'gbdt',"max_depth":-1,"num_leaves":64,"learning_rate":0.01,"seed":1015,num_boost_round=1200,early_stopping_rounds=200|0.9101|0.8279|0.9945|0.5977|0.81749|0.81195|
|lgb+kfold5+SMOTE|train_err+train_quaility(median+impyute)|'boosting_type':'gbdt','objective':'binary','metric':'auc','learning_rate':'0.01','seed': 1015,num_boost_round = 2000,early_stopping_rounds=800|0.9052|0.8292|0.9666|0.6453|0.8155|ver3|
|lgb+kfold5+SMOTE+knnimputation|train_err+train_quality(median)|'boosting_type':'dart','objective':'binary','metric':'auc','learning_rate':'0.01','seed': 1015,threshold = 0.5,num_boost_round = 1200|0.8972|0.8208|0.9676|0.6322|0.8043|0.80736|


## stacking model 

|num|model|data|parameter tuning|auc_score|accuracy|precision|recall|f1_score|final_score|
|--|--|--|--|--|--|--|--|--|--|
|1|lgb_gbdt+kfold12+SMOTE|train_err|'boosting_type' : 'gbdt','objective': 'binary','metric':'auc','learning_rate':'0.01','seed':1015,num_boost_round = 1000,early_stopping_rounds = 50|0.9098|0.8279|0.9978|0.5770|0.8163||
|2|lgb_dart+kfold5+SMOTE|train_err|'boosting_type':'dart','objective':'binary','metric': 'auc','learning_rate': '0.02','seed':1015,num_boost_round = 1200, early_stopping_rounds = 200|0.9021|0.8264|0.9611|0.6413|0.8112||
|3|xgb+kfold5+SMOTE|train_err|'objective':'binary:logistic','eval_metric' :'auc','early_stoppings':100,num_boost_round=1000|0.8886|0.8120|0.9390|0.6855|0.8034|0.8*(num1)+0.1*(num2)+0.1*(num3)=**0.81320**|
|-|- |- |same model as the one above |- | -|- |- |- |0.6*(num1)+0.2*(num2)+0.2*(num3)=**0.81226**|



