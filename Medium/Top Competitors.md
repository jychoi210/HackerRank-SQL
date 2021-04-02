# 만점을 2개 이상 받은 해커들 출력하기

#### `챌린지 만점을 2개 이상 받은 해커들의 id와 이름 출력` <br/>
`(단, 동일한 수의 챌린지에서 만점을 받은 경우 hacker_id를 오름차순으로 정렬)`

#### 방법
1. Hacker 아이디와 이름 출력
```MySQL
SELECT H.HACKER_ID, H.NAME
```

2. 각 테이블 INNER JOIN 해주기</br>
   HACKERS : 해커 아이디와 이름을 출력하기 위함</br>
   SUBMISSIONS : 해커들이 받은 점수와 만점 점수를 비교하여 만점을 받았는지 알기 위함</br>
   CHALLENGES : 해커들이 도전한 문제들의 난이도를 알기 위함</br>
   DIFFICULTY : 해커들이 도전한 문제들의 난이도 별 만점 점수를 알기 위함</br>
```MySQL
SELECT H.HACKER_ID, H.NAME
FROM HACKERS AS H INNER JOIN SUBMISSIONS AS S ON H.HACKER_ID = S.HACKER_ID
    INNER JOIN CHALLENGES AS C ON S.CHALLENGE_ID = C.CHALLENGE_ID
    INNER JOIN DIFFICULTY AS D ON C.DIFFICULTY_LEVEL = D.DIFFICULTY_LEVEL
```

3. 해커들이 받은 점수와 만점 점수가 같다면, 챌린지에서 성공한 것(만점을 받은 것)으로 설정</br>
   단, 이때 각 챌린지의 난이도가 같아야 함
```MySQL
WHERE D.SCORE = S.SCORE AND C.DIFFICULTY_LEVEL = D.DIFFICULTY_LEVEL
```
4. 해커 아이디, 이름 별로 그룹핑 하여 개수 카운트, 2개 이상(1개 초과)인 것만 설정
```MySQL
GROUP BY H.HACKER_ID, H.NAME
HAVING COUNT(H.HACKER_ID) > 1
```

5. 해커들이 받은 만점의 개수를 세서 내림차순으로 정렬, 단 동일한 개수인 경우 해커 아이디 오름차순 정렬
```MySQL
ORDER BY COUNT(H.HACKER_ID) DESC, H.HACKER_ID ASC
```


#### 전체 코드
```MySQL
SELECT H.HACKER_ID, H.NAME
FROM HACKERS AS H INNER JOIN SUBMISSIONS AS S ON H.HACKER_ID = S.HACKER_ID
    INNER JOIN CHALLENGES AS C ON S.CHALLENGE_ID = C.CHALLENGE_ID
    INNER JOIN DIFFICULTY AS D ON C.DIFFICULTY_LEVEL = D.DIFFICULTY_LEVEL
WHERE D.SCORE = S.SCORE AND C.DIFFICULTY_LEVEL = D.DIFFICULTY_LEVEL
GROUP BY H.HACKER_ID, H.NAME
HAVING COUNT(H.HACKER_ID) > 1
ORDER BY COUNT(H.HACKER_ID) DESC, H.HACKER_ID ASC
```

---
문제 출처 : [HackerRank](https://www.hackerrank.com/challenges/full-score/problem)
