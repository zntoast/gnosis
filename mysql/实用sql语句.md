##### 按天分组统计用户
```
SELECT 
  DATE_FORMAT(FROM_UNIXTIME(created_at / 1000), '%Y-%m-%d') AS date,
  COUNT(DISTINCT user_id) AS num_participants
FROM 
  c_disney_lottery_draw_record
GROUP BY 
  DATE_FORMAT(FROM_UNIXTIME(created_at / 1000), '%Y-%m-%d')
ORDER BY 
  date ASC;
```


##### 获取用户的卡牌去向
```
SELECT DISTINCT  
        t1.order_no AS 订单号,  
        t1.goods_title AS 卡池,  
        t1.goods_num AS 抽数,  
        t1.payment_price AS 支付金额分,  
        t1.created_at AS 创建时间,  
        t2.card_mask_id AS 用户卡牌唯一编号,  
        t10.card_overall_id AS cardUid,  
        t3.card_mask_id AS 用户背包,  
  t4.card_mask_id AS 物流发货,  
        t5.card_mask_id AS 卡牌转欧气,  
        t6.card_mask_id AS 卡牌过期,  
        t7.card_mask_id AS 卡牌转能量   
FROM  
        c_order t1  
        LEFT JOIN c_order_item_detail t2 ON t1.id = t2.order_id  
        LEFT JOIN c_user_card_detail t3 ON t2.card_mask_id = t3.card_mask_id  
        LEFT JOIN c_logistics_card_detail t4 ON t2.card_mask_id = t4.card_mask_id  
        LEFT JOIN c_lucky_convert_card_detail t5 ON t2.card_mask_id = t5.card_mask_id  
        LEFT JOIN c_user_expire_card_detail t6 ON t2.card_mask_id = t6.card_mask_id  
        LEFT JOIN c_donate_detail t7 ON t2.card_mask_id = t7.card_mask_id  
        LEFT JOIN c_card t10 ON t2.card_id = t10.id   
WHERE  
        t1.user_id = 1019334
```