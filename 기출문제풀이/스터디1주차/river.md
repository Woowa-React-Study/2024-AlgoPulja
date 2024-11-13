# 1주차 기출

## 기출 문제 정보

- [2024 KAKAO WINTER INTERNSHIP 가장 많이 받은 선물](https://school.programmers.co.kr/learn/courses/30/lessons/258712)

## 기출 문제 분석

### 알고리즘

### 시간 복잡도 : O(g + n^2)

`getGiftScore` 함수: gifts 배열을 한 번 순회 → O(G) (G는 gifts의 길이)

`compareGiftExchanges` 함수:

gifts 배열을 한 번 순회 → O(G)
getGiftScore를 두 번 호출 → O(2G)
총 O(3G) = O(G)

`solution` 함수:

이중 `for` 루프로 모든 friends 쌍을 순회 → O(N²) (N은 friends의 길이)
각 쌍마다 `compareGiftExchanges` 호출 → O(G)
전체 시간 복잡도: O(N² × G)

따라서 전체 시간 복잡도는 O(N² × G)

N: friends 배열의 길이
G: gifts 배열의 길이

## 기출 풀이

```javascript
// 선물 지수를 계산하는 함수 (준 선물 수 - 받은 선물 수)
function getGiftScore(name, gifts) {
  let given = 0; // 준 선물 수
  let received = 0; // 받은 선물 수

  // 모든 선물 기록을 순회하며 해당 친구의 준/받은 선물 수 계산
  gifts.forEach(([giver, receiver]) => {
    if (giver === name) given++;
    if (receiver === name) received++;
  });

  return given - received;
}

// 두 친구 간의 선물 교환을 비교하여 다음 달에 선물을 받을 사람을 결정하는 함수
function compareGiftExchanges(name1, name2, gifts) {
  let name1ToName2 = 0; // name1이 name2에게 준 선물 수
  let name2ToName1 = 0; // name2가 name1에게 준 선물 수

  // 두 친구 간의 선물 교환 기록 계산
  gifts.forEach(([giver, receiver]) => {
    if (giver === name1 && receiver === name2) {
      name1ToName2++;
    } else if (giver === name2 && receiver === name1) {
      name2ToName1++;
    }
  });

  // 선물을 주고받은 수가 같다면
  if (name1ToName2 === name2ToName1) {
    // 선물 지수를 비교하여 결정
    const giftScoreA = getGiftScore(name1, gifts);
    const giftScoreB = getGiftScore(name2, gifts);
    return giftScoreA > giftScoreB
      ? name1
      : giftScoreB > giftScoreA
      ? name2
      : null; // 선물 지수도 같다면 null 반환
  }

  // 더 많은 선물을 준 사람을 반환
  return name1ToName2 > name2ToName1
    ? name1
    : name2ToName1 > name1ToName2
    ? name2
    : null;
}

function solution(friends, gifts) {
  // 문자열 형태의 선물 기록을 [giver, receiver] 배열로 변환
  const giftExchangePairs = gifts.map((g) => {
    return ([giver, receiver] = g.split(" "));
  });

  // 각 친구별로 다음 달에 받을 선물의 수를 저장할 객체
  const predictedGifts = {};

  // 초기화: 모든 친구의 예상 선물 수를 0으로 설정
  friends.forEach((friend) => {
    predictedGifts[friend] = 0;
  });

  // 모든 친구 쌍에 대해 선물 교환 비교
  for (let i = 0; i < friends.length - 1; i++) {
    for (let j = i + 1; j < friends.length; j++) {
      const name1 = friends[i];
      const name2 = friends[j];

      // 두 친구 간의 선물 교환을 비교하여 승자 결정
      const giftWinner = compareGiftExchanges(name1, name2, giftExchangePairs);
      if (giftWinner) {
        predictedGifts[giftWinner]++; // 승자의 예상 선물 수 증가
      }
    }
  }

  // 가장 많은 선물을 받을 것으로 예상되는 수 반환
  return Math.max(...Object.values(predictedGifts));
}
```
