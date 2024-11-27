## 기출 문제 링크

[2024 KAKAO WINTER INTERNSHIP > 도넛과 막대 그래프](https://school.programmers.co.kr/learn/courses/30/lessons/258711)

## 문제 분석

간선의 개수 E가 최대 100,000개이므로 E^2 풀이가 나오지 않도록 유의한다.

- 임의의 정점과 연결된 정점을 바탕으로 그래프를 따로 분리해 dfs로 탐색해 이루는 정점의 수와 간선의 수를 구할 수도 있다. 하지만 이는 연산이 복잡하다.
- 그래프 마다 특징적인 정점이 존재하기 때문에 이를 바탕으로 그래프를 판별한다면, 각 그래프를 분리하지 않아도 도니다.

## 문제 풀이법

1. 들어오는 게 없고 나가는 간선만 있다면 임의로 추가한 정점이다.
2. 임의의 정점과 연결된 간선의 개수가 그래프의 총 개수이다.
3. 임의의 정점을 삭제하고, 각 정점의 들어온 간선과 나간 간선을 계산한다.
4. 그래프를 판단한다.
   - 들어오는 간선이 한 개 혹은 0개이고, 나가는 간선이 없으면 막대 그래프다.
   - 들어오는 간선과 나가는 간선이 각각 2개씩 존재하면 8자 모양 그래프다.
   - 그래프의 총 개수에서 막대 그래프와 8자 모양 그래프의 수를 빼면 도넛 그래프의 수다.

### 시간 복잡도

O(E + V)

### 코드

```js
function getGraphInfo(edges) {
  const graph = new Map();

  edges.forEach(([from, to]) => {
    if (!graph.has(from)) {
      graph.set(from, { from: [], to: [] });
    }
    if (!graph.has(to)) {
      graph.set(to, { from: [], to: [] });
    }

    graph.get(from).to.push(to);
    graph.get(to).from.push(from);
  });

  let tempNode;
  for (const [node, { from, to }] of graph.entries()) {
    if (from.length === 0 && to.length >= 2) {
      console.log(tempNode, node);
      tempNode = node;
      graph.delete(node);
      break;
    }
  }

  return [graph, tempNode];
}

function solution(edges) {
  let line = 0;
  let eight = 0;

  const [graph, tempNode] = getGraphInfo(edges);
  const totalGraphs = Array.from(graph).reduce((sum, [_, { from }]) => {
    if (from.includes(tempNode)) return sum + 1;
    return sum;
  }, 0);

  for (const [node, info] of graph.entries()) {
    const from = info.from.filter((v) => v !== tempNode);
    const to = info.to;

    if ((from.length === 1 || from.length === 0) && to.length === 0) line++;
    if (from.length === 2 && to.length === 2) eight++;
  }

  return [tempNode, totalGraphs - (line + eight), line, eight];
}
```
