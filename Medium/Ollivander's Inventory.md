# 최적의 지팡이 찾기

#### `높은 힘, 높은 나이, 악마가 아닌 지팡이를 구입하는데 필요한 최소 금액` <br/>

#### 방법
1. 지팡이 코드, 나이, 금액, 힘 출력
```MySQL
SELECT W.ID, P.AGE, W.COINS_NEEDED, W.POWER
```

2. 각 테이블 INNER JOIN 해주기</br>
   WANDS와 WANDS_PROPERTY 조인
```MySQL
FROM WANDS AS W
    JOIN WANDS_PROPERTY AS P ON W.CODE = P.CODE
```

3. 악마가 없는 (is_evil = 0) 지팡이 선택, 가장 저렴한 가격 선택</br>
   가장 저렴한 가격 선택하기 위해 COINS_NEEDED를 MIN을 사용하여 구함</br>
   WANDS와 WANDS_PROPERTY를 조인하여 힘과 나이가 같은 지팡이에 대해 최소 가격 구함
```MySQL
WHERE P.IS_EVIL = 0 and W.COINS_NEEDED = (
    SELECT MIN(COINS_NEEDED)
    FROM WANDS AS W1 JOIN WANDS_PROPERTY AS P1 ON W1.CODE = P1.CODE
    WHERE W1.POWER = W.POWER AND P1.AGE = P.AGE)
```
4. 지팡이 힘으로 내림차순 정렬, 힘이 같다면 나이순으로 내림차순 정렬
```MySQL
ORDER BY POWER DESC, AGE DESC
```


#### 전체 코드
```MySQL
SELECT W.ID, P.AGE, W.COINS_NEEDED, W.POWER
FROM WANDS AS W
    JOIN WANDS_PROPERTY AS P ON W.CODE = P.CODE
WHERE P.IS_EVIL = 0 and W.COINS_NEEDED = (
    SELECT MIN(COINS_NEEDED)
    FROM WANDS AS W1 JOIN WANDS_PROPERTY AS P1 ON W1.CODE = P1.CODE
    WHERE W1.POWER = W.POWER AND P1.AGE = P.AGE)
ORDER BY POWER DESC, AGE DESC
```

---
문제 출처 : [HackerRank](https://www.hackerrank.com/challenges/harry-potter-and-wands/problem)
