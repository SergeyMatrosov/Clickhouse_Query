```sql
SELECT userid,
       arrayJoin(campaign) AS campaign,
       arrayJoin(medium) AS medium
FROM
  (SELECT userid,
          groupArray(campaign) AS campaign,
          groupArray(medium) AS medium
   FROM
     (SELECT userid,
             CASE
                 WHEN name='utm_campaign' THEN value
                 ELSE NULL
             END AS campaign,
             CASE
                 WHEN name='utm_medium' THEN value
                 ELSE NULL
             END AS medium
      FROM
        (WITH '111' AS userid, 'utm_campaign' AS name, 'Search_Google' AS value 
         SELECT userid, name, value
        
         UNION ALL 
         WITH '111' AS userid, 'utm_medium' AS name, 'cpc' AS value 
         SELECT userid, name, value
         
         UNION ALL 
         WITH '222' AS userid, 'utm_campaign' AS name, 'Facebook' AS value 
         SELECT userid, name, value
         
         UNION ALL WITH '222' AS userid, 'utm_medium' AS name,'cpc' AS value 
         SELECT userid, name, value))
   GROUP BY userid)
```