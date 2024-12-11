# 1주차 기출

## 기출 문제 정보

- [2024 KAKAO WINTER INTERNSHIP 가장 많이 받은 선물](https://school.programmers.co.kr/learn/courses/30/lessons/258712)

## 기출 문제 분석

### 알고리즘 : 해시맵

- 사람별로 '어떤 사람에게 몇번의 선물을 주었는 가', 선물 지수 계산을 위한 '선물 받은 횟수'와 '선물을 준 횟수'에 대한 정보가 필요 -> 이름을 key로하고 필요한 정보를 속성으로 하는 객체 활용
- 모든 친구와 비교해서, 다음달에 받을 선물이 필요함, 특정한 사람에게 선물을 얼마큰 주었는 가를 빠르게 탐색하기 위해 Map 객체 활용
- 친구들의 수는 최대가 50으로, 배열 순환을 어느 정도 이용해도 문제 없다고 생각

### 시간 복잡도 : O(m + n^2)

- n: 친구 수, m : 선물 기록 길이

## 기출 풀이

```js
function solution(friends, gifts) {
  const table = {};
  const result = Array(friends.length).fill(0);

  // 시간 복잡도 : O(n)
  friends.forEach(f => {
    table[f] = { receiverCount: 0, giverCount: 0, giverMap: new Map() };
  });
  // 시간 복잡도 : O(m)
  gifts.forEach(gift => {
    const [giver, receiver] = gift.split(" ");
    table[giver].giverCount += 1;
    table[giver].giverMap.set(
      receiver,
      (table[giver].giverMap.get(receiver) || 0) + 1
    );
    table[receiver].receiverCount += 1;
  });

  // 시간 복잡도 : O(n^2)
  for (let a = 0; a < friends.length; a++) {
    const friendA = friends[a];
    const aPoint = table[friendA].giverCount - table[friendA].receiverCount;

    for (let b = a + 1; b < friends.length; b++) {
      const friendB = friends[b];
      const bPoint = table[friendB].giverCount - table[friendB].receiverCount;

      const aTob = table[friendA].giverMap.get(friendB) || 0;
      const bToa = table[friendB].giverMap.get(friendA) || 0;

      // 선물 주고 받고 선물 개수가 다른 경우
      if (Math.max(aTob, bToa) && aTob !== bToa) {
        if (aTob > bToa) {
          result[a] += 1;
        } else {
          result[b] += 1;
        }
      } else {
        // 기록이 없거나 같은 경우
        if (aPoint !== bPoint) {
          if (aPoint > bPoint) {
            result[a] += 1;
          } else {
            result[b] += 1;
          }
        }
      }
    }
  }
  // 시간 복잡도 O(n)
  return Math.max(...result);
}
```
