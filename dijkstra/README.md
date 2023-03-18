# Dijkstra's Algorithm

核心是最短路径上的任意两点也是最短路径

```mermaid
flowchart LR
  A -- "3" --> B
  A -- "2" --> B
  A -- "5" --> B
```

$$D(A, B) = \min (3, 2, 5)$$

---

```mermaid
flowchart LR
  subgraph AO
    AO1("AO1(4)")
    AO2("AO2(2)")
    AO3("AO3(3)")
  end
  AO1 -- "3" --> B
  AO2 -- "2" --> B
  AO3 -- "5" --> B
```

$$D(A, B) = \min \{AO_i + O_i B \}$$

---

注意到, 有$AO_i, O_i B$也是最短的

$$D(A, B) = \min \{D(A, O_i) + D(O_i, B) \}$$

```mermaid
flowchart LR
  A -- "3" --> O -- "1" --> B
  A -- "2" --> O -- "3" --> B
  A -- "5" --> O -- "2" --> B
```

---

用二部图思考

```mermaid
flowchart LR
  subgraph 1
    direction LR
    subgraph N
      direction TB
      AO1
      AO2
      AOn
    end
    subgraph M
      direction TB
      B1
      B2
      Bn
    end
    AO1 -- "3" --> B1
    AO1 -- "2" --> B2
    AOn -- "5" --> Bn
  end
  subgraph 2
    direction LR
    subgraph N'
      direction TB
      AO1'
      AO2'
      AOn'
      B2'
    end
    subgraph M'
      direction TB
      B1'
      Bn'
    end
    AO1' -- "3" --> B1'
    AOn' -- "5" --> Bn'
    B2' -- "2" --> Bn'
  end

  1 -- "Next" --> 2
```

没有连接的是无穷

可以证明$D(A, B_2) = AO_1 \rightarrow B_2$

整个过程, 有 N 不断增大, M 不断减小, 直到结束

---

有几点观察:

- 找到 NM 之间的最短, 即图中的'2', 很像$\min \{D(A, O_i) + D(O_i, B) \}$的 min, 连接起两个图的结构
- 算法的过程, 图的结构大体结构不变, 符合递归

  因此, 感觉递归具有结构不变性

- 二部图结构, 让我想到二元运算

  ```hs
  f (A, B) = (A', B')
  ```

  图的结构在变化, 在某种程度上有没有变

- 贪心的过程, N 不断增大, M 不断减小, 刻画了一种探索方式, N 是已知, M 是未知
