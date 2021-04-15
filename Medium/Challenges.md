# 학생들의 챌린지 수 출력하기

#### `최대 과제 수를 한 학생을 제외하고 중복된 과제 수를 제출한 학생들을 결과에서 빼기` <br/>
     최대 과제 개수 학생들 + 중복되지 않은 과제 개수 학생들 = 전체 학생들 - 최대 과제 개수 학생들이 아닌 중복된 과제 개수 학생들

### 방법
#### 1. 해커 아이디, 해커 이름, 수행한 과제 개수 출력
```MySQL
SELECT H.HACKER_ID, H.NAME, COUNT(H.HACKER_ID) AS CHALLENGES_CREATED
```
</br></br>
#### 2. 각 테이블 INNER JOIN 해주기</br>
   HACKERS와 CHALLENGES 조인
```MySQL
FROM HACKERS AS H 
    INNER JOIN CHALLENGES AS C ON H.HACKER_ID = C.HACKER_ID
```
</br></br>
#### 3. 해커 아이디와 이름으로 그룹핑</br>
   (해커 아이디 당 제출한 챌린지 수 구하기 위함)
```MySQL
GROUP BY H.HACKER_ID, H.NAME
```
</br></br>
#### 4. HAVING을 통해 조건 설정 </br>
   1) 가장 많은 챌린지 수를 제출한 경우 </br>
        CHALLENGES_CREATED가 가장 많은 챌린지 수 인 경우</br>
        CHALLENGES 테이블에서 HACKER_ID로 그룹핑 하여 챌린지 수로 정렬하여 맨 위의 값(최대 값) 구하기
      
```MySQL
HAVING CHALLENGES_CREATED = (SELECT COUNT(*)
                             FROM CHALLENGES AS C1
                            GROUP BY C1.HACKER_ID
                            ORDER BY COUNT(*) DESC
                            LIMIT 1)
```
</br>

  2) 가장 많은 챌린지 수는 아니지만 중복된 챌린지 수가 아닌 경우</br>
     CHALLENGES_CREATED가 중복되지 않은 챌린지 수인 경우</br>
     CHALLENGES 테이블에서 HACKER_ID로 그룹핑하여 챌린지 수를 구한 후, 다시 챌린지 수(CH.NUM)으로 그룹핑하여 1인 경우 구하기
  
```MySQL
OR CHALLENGES_CREATED IN (SELECT CH.NUM
                         FROM (SELECT COUNT(*) AS NUM
                               FROM CHALLENGES AS C2
                               GROUP BY HACKER_ID) CH
                        GROUP BY CH.NUM
                        HAVING COUNT(CH.NUM) = 1)
```
</br>
</br>

#### 5. 챌린지 수 별로 내림차순 정렬, 챌린지 수가 같다면 해커 아이디로 정렬</br>
```MySQL
ORDER BY CHALLENGES_CREATED DESC, H.HACKER_ID ASC
```

</br>
</br>
</br>

#### 전체 코드
```MySQL
SELECT H.HACKER_ID, H.NAME, COUNT(H.HACKER_ID) AS CHALLENGES_CREATED
FROM HACKERS AS H 
    INNER JOIN CHALLENGES AS C ON H.HACKER_ID = C.HACKER_ID
GROUP BY H.HACKER_ID, H.NAME
HAVING CHALLENGES_CREATED = (SELECT COUNT(*)
                             FROM CHALLENGES AS C1
                            GROUP BY C1.HACKER_ID
                            ORDER BY COUNT(*) DESC
                            LIMIT 1)
    OR CHALLENGES_CREATED IN (SELECT CH.NUM
                             FROM (SELECT COUNT(*) AS NUM
                                   FROM CHALLENGES AS C2
                                   GROUP BY HACKER_ID) CH
                            GROUP BY CH.NUM
                            HAVING COUNT(CH.NUM) = 1)
ORDER BY CHALLENGES_CREATED DESC, H.HACKER_ID ASC
```

---
문제 출처 : [HackerRank](https://www.hackerrank.com/challenges/challenges/problem)
