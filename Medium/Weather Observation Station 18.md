# 맨해튼 거리 구하기
```MySQL
SELECT ROUND(ABS(MAX(LAT_N)-MIN(LAT_N)) + ABS(MAX(LONG_W)-MIN(LONG_W)) , 4) FROM STATION
```
---
문제 출처 : [HackerRank](https://www.hackerrank.com/challenges/weather-observation-station-18/problem)
