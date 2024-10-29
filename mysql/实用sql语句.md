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