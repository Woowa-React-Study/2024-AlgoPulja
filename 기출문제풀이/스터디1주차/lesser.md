## 기출 문제 링크

[2024 KAKAO WINTER INTERNSHIP > 가장 많이 받은 선물](https://school.programmers.co.kr/learn/courses/30/lessons/258712)

## 문제 분석

- friends의 길이를 N, gifts의 길이를 M이라고 할 때 N과 M의 크기가 크지 않아 시간복잡도에 구애 받지 않음을 알 수 있음.
- 결과 값을 구하는 데 '이름' 정보와 부가 정보가 필요하므로 Map으로 접근.(해시 문제)

## 문제 풀이법

일단 반성: 문제 끝까지 안 읽고 구현부터 하다가 중첩 Map을 만들어서 조금 복잡하게 풀었음.

### 시간 복잡도

O(M + N^2)

### 코드

```js
const calculatedPairs = new Set();
function isPairCalculated(a, b) {
  const pairKey = [a, b].sort().join('-');

  if (!calculatedPairs.has(pairKey)) {
    calculatedPairs.add(pairKey);
    return false;
  }
  return true;
}

function getInitialGiftBook(gifts) {
  const giftBook = new Map();

  gifts.forEach((record) => {
    const [giver, receiver] = record.split(' ');

    const initialValue = { given: new Map(), score: 0 };
    const giverInfo = giftBook.get(giver) ?? initialValue;
    const receiverInfo = giftBook.get(receiver) ?? initialValue;

    giverInfo.given.set(receiver, (giverInfo.given.get(receiver) || 0) + 1);
    giverInfo.score += 1;
    receiverInfo.score -= 1;

    giftBook.set(giver, giverInfo);
    giftBook.set(receiver, receiverInfo);
  });

  return giftBook;
}

function getNextReceiver(giftBook, a, b) {
  const aInfo = giftBook.get(a);
  const bInfo = giftBook.get(b);

  const givenAtoB = giftBook.get(a)?.given.get(b) || 0;
  const givenBtoA = giftBook.get(b)?.given.get(a) || 0;

  const giftDifference = Math.sign(givenAtoB - givenBtoA);
  if (giftDifference !== 0) return giftDifference > 0 ? a : b;

  const scoreDifference = Math.sign((aInfo?.score || 0) - (bInfo?.score || 0));
  if (scoreDifference !== 0) return scoreDifference > 0 ? a : b;

  return null;
}

function solution(friends, gifts) {
  const giftBook = getInitialGiftBook(gifts);
  const nextMonth = new Map(friends.map((name) => [name, 0]));

  friends.forEach((a) => {
    friends.forEach((b) => {
      if (a === b || isPairCalculated(a, b)) return;

      const receiver = getNextReceiver(giftBook, a, b);
      if (receiver) {
        nextMonth.set(receiver, nextMonth.get(receiver) + 1);
      }
    });
  });

  return Math.max(...nextMonth.values());
}
```
