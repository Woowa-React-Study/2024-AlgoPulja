# 1주차 기출

## 기출 문제 정보

- [2024 KAKAO WINTER INTERNSHIP 가장 많이 받은 선물](https://school.programmers.co.kr/learn/courses/30/lessons/258712)

## 기출 문제 분석

### 문제 분석

- `friends`: 친구들의 이름 목록.
- `gifts`: 선물을 주고받은 기록. 각 기록은 "보낸 사람 받은 사람" 형식의 문자열로 제공됨.

### 코드 설명

1. 친구 이름과 인덱스를 매핑하는 `nameIndexMap` 생성.
2. 선물 매트릭스 (`giftMatrix`) 생성.
3. 친구들이 받은 선물 수 (`giftNum`) 초기화.
4. 선물 기록을 순회하며 선물 매트릭스와 선물 수를 업데이트.
5. 각 친구에 대해 다른 친구들과 비교하여 더 많은 선물을 받은 횟수를 계산.
6. 가장 많이 받은 선물 수 반환.

### 시간 복잡도

- `friends` 배열을 순회하며 인덱스 매핑을 생성하는 작업: **O(N)**.
- `gifts` 배열을 순회하며 선물 매트릭스를 업데이트하는 작업: **O(M)** (`M`은 선물의 수).
- 두 번의 중첩 반복문을 통해 모든 친구들을 비교하는 작업: **O(N^2)**.

### 코드

```js
function solution(friends, gifts) {
  const nameIndexMap = {};
  friends.forEach((name, index) => {
    nameIndexMap[name] = index;
  });

  const N = friends.length;
  const giftMatrix = Array.from({ length: N }, () => Array(N).fill(0));

  const giftNum = friends.reduce((obj, name) => {
    obj[name] = 0;
    return obj;
  }, {});

  gifts.map((gift) => {
    const gi = gift.split(' ');

    giftNum[gi[0]] += 1;
    giftNum[gi[1]] -= 1;

    const senderIndex = nameIndexMap[gi[0]];
    const receiverIndex = nameIndexMap[gi[1]];

    giftMatrix[senderIndex][receiverIndex] += 1;
  });

  let maxGift = 0;
  for (let i = 0; i < N; i += 1) {
    let nextGift = 0;
    for (let j = 0; j < N; j += 1) {
      if (giftMatrix[i][j] > giftMatrix[j][i]) {
        nextGift += 1;
      } else if (giftMatrix[i][j] == giftMatrix[j][i]) {
        if (giftNum[friends[i]] > giftNum[friends[j]]) {
          nextGift += 1;
        }
      }
    }
    if (nextGift > maxGift) {
      maxGift = nextGift;
    }
  }

  return maxGift;
}
```
