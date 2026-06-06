```sql
SELECT binid,
	round(avg(cast(Fluo as float)),3) as Fluo,
	...
FROM(
	SELECT*
		cast(floor(ts) + floor((ts-floor(ts))*24*60/binsize)8binsize / (24*60) as datetime) as binid
	FROM (
		SELECT *,
			cast(timestamp as float) as ts,
			5.0 as binsize
		FROM Tokyo_4_merged_data_time
	)
) bins
GROUP BY binid
ORDER BY binid ASC
```

- 우선 11번째 줄의 ```FROM Tokyo_4_merged_data_time```부터 시작한다. 이는 원래 database이다. 이후 binsize와 ts를 정의 한다. 
- 이후 정의한 변수 ts와 binsize를 이용하여 binid를 정의 한다. 
- binid를 가져와서 이들로 여러 값들을 정의 하고(...으로 생략해둔 부분) 이를 binid를 기준으로 GROUP BY를 한 후 올림차순으로 정렬한다. 