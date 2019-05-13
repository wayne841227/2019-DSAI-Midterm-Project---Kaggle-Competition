# Kaggle Predict Future Sales
參考kernel [Feature engineering, xgboost](https://www.kaggle.com/dhimananubhav/feature-engineering-xgboost)




## data exploration
去除Outliers
把價格大於100000以及銷量大於1000的data去除


## data preprocessing

合併重複的店名，修正train data及test data



## feature selection
使用的feature:

    'date_block_num',
    'shop_id',
    'item_id',
    'item_cnt_month',
    'city_code',
    'item_category_id',
    'type_code',
    'subtype_code',
    'item_cnt_month_lag_1',
    'item_cnt_month_lag_2',
    'item_cnt_month_lag_3',
    'item_cnt_month_lag_6',
    'item_cnt_month_lag_12',
    'date_avg_item_cnt_lag_1',
    'date_item_avg_item_cnt_lag_1',
    'date_item_avg_item_cnt_lag_2',
    'date_item_avg_item_cnt_lag_3',
    'date_item_avg_item_cnt_lag_6',
    'date_item_avg_item_cnt_lag_12',
    'date_shop_avg_item_cnt_lag_1',
    'date_shop_avg_item_cnt_lag_2',
    'date_shop_avg_item_cnt_lag_3',
    'date_shop_avg_item_cnt_lag_6',
    'date_shop_avg_item_cnt_lag_12',
    'date_cat_avg_item_cnt_lag_1',
    'date_shop_cat_avg_item_cnt_lag_1',
    #'date_shop_type_avg_item_cnt_lag_1',
    #'date_shop_subtype_avg_item_cnt_lag_1',
    'date_city_avg_item_cnt_lag_1',
    'date_item_city_avg_item_cnt_lag_1',
    #'date_type_avg_item_cnt_lag_1',
    #'date_subtype_avg_item_cnt_lag_1',
    'delta_price_lag',
    'month',
    'days',
    'item_shop_last_sale',
    'item_last_sale',
    'item_shop_first_sale',
    'item_first_sale',

## model selection
使用xgboost

## experiment result
1.原始參數

    model = XGBRegressor(
        max_depth=8,
        n_estimators=1000,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.8, 
        eta=0.3,    
        seed=42)
        
    Public Score 0.91946

2.改小max_depth,改小n_estimators
  發現效能提升

    model = XGBRegressor(
        max_depth=10,
        n_estimators=800,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.8, 
        eta=0.3,    
        seed=42)
        
    Public Score 0.91391

3.改小max_depth,改回原本n_estimators,改大subsample試試看
發現效能下降很多

    model = XGBRegressor(
        max_depth=5,
        n_estimators=1000,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.9, 
        eta=0.3,    
        seed=42)
        
    Public Score 0.94905
4.max_depth不動,改小n_estimators,改小subsample
發現效能提升一點

    model = XGBRegressor(
        max_depth=10,
        n_estimators=700,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.7, 
        eta=0.3,    
        seed=42)
        
    Public Score 0.91321
5.再改小n_estimators,改小subsample
發現效能又下降

    model = XGBRegressor(
        max_depth=10,
        n_estimators=600,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.6, 
        eta=0.3,    
        seed=42)
        
    Public Score 0.91473
    
另外有做測試更改輸入data,有加上原本註解掉的feature

6.比原始參數結果好一點

    model = XGBRegressor(
        max_depth=8,
        n_estimators=1000,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.8, 
        eta=0.3,    
        seed=42)
        
    Public Score 0.91777
7.使用上面"實驗4"得出0.913的參數
結果並沒有得到更好的結果

    model = XGBRegressor(
        max_depth=10,
        n_estimators=700,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.7, 
        eta=0.3,    
        seed=42)

    Public Score 0.91601

8.刪除許多feature,使用一樣的參數
結果大幅下降
    
    Public Score 0.94086

9.改大max_depth
結果效能反而下降

    model = XGBRegressor(
        max_depth=15,
        n_estimators=700,
        min_child_weight=300, 
        colsample_bytree=0.8, 
        subsample=0.8, 
        eta=0.3,    
        seed=42)
        
    Public Score 0.91948
    
## conclusion

試著調整一些參數及特徵
結果效能並沒有比參考的那個kernel還好