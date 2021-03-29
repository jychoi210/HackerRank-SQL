# 중앙값(Median) 구하기

### `중앙값을 쿼리한 후 소수점 4자리로 반올림하기`

#### 방법
1.SET을 통해 총 행의 개수 구하기   

2.(행의 개수 / 2)를 통해 중간값 구하기   
  - 행의 개수가 짝수일 때 : 가운데 2개의 값 평균
  - 행의 개수가 홀수일 때 : 정 가운데 값

```MySQL
SET @Row=-1;
SELECT ROUND(AVG(LAT_N), 4)
FROM (SELECT @Row:=@Row+1 AS RowNum, LAT_N
     FROM STATION
     ORDER BY LAT_N) Num
WHERE RowNum IN (FLOOR(@Row/2), CEIL(@Row/2))
```
---
문제 출처 : [HackerRank](https://www.hackerrank.com/challenges/weather-observation-station-20/problem)
