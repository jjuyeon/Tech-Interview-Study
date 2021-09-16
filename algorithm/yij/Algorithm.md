> [💡](#String) 문자열 비교 알고리즘들에 대해서 설명해주세요

# MST Algorithm

: Connected Graph에서 Minimum Spanning Tree를 구하는 알고리즘이다. 크루스칼, 프림 알고리즘 둘 다 **Greedy Algorithm**에 기반한다. 

<br>

> Greedy Algorithm
> : 알고리즘의 각 Step 마다 현재 상태에서 최적의 해를 만드는 것을 선택해 나가는 알고리즘. 일부 최적화 문제에 대해 global-optimal solution을 준다.

<br>

## Kruskal's Algorithm

: 현재 가지고 있는 Edge Set에 추가해도 **Cycle이 생기지 않는 Edge들 중 가장 Cost가 작은 Edge를 추가**해가며 MST를 생성하는 알고리즘.

👉 Edge Set T에 Edge e를 추가해도 spanning tree의 조건을 만족할 때, T + e는 feasible하다.

해당 과정을 통해 구해낸 결과물은 반드시 Connected Graph이다.

![](https://upload.wikimedia.org/wikipedia/commons/5/5c/MST_kruskal_en.gif)

<br>

> Greedy Algorithm에 의해 크루스칼 알고리즘의 성질은 모든 Step에서 두 가지 특성을 만족합니다.
> 1️⃣ Feasibility: 가용성 👉 중간 결과물이 원하는 성질을 해치지 않는가?
> 2️⃣ Optimality: 최적성 👉 중간 결과물에서의 선택이 실제 최적의 선택인가?

크루스칼 알고리즘의 결과물 Sub Graph G'은 항상 **1️⃣ Connected Graph** 이며, **2️⃣ 모든 Edge의 Cost가 다를 때 유일한 MST를 반환**한다.

<br>

### Pesudo Code
```
Sort edges by weight and assume w₁≤w₂≤..≤wⁿ
T is empty (T will store edges of a MST)
for i = 1 to m do
    if (T+i is feasible (does not contain a cycle))
    add i to T
return the set T
```

이 때, 시간복잡도는 O(m²)이다.

Union-Find 자료구조를 이용하면 시간 복잡도를 줄일 수 있다.

<br>

## Kruskal Algorithm With Union-Find

1. Kruskal's Algorithm의 중간 결과물을 Forest (tree들의 set)으로 저장한다.
2. 새로 추가할 edge가 Forest 상의 두 tree T1, T2를 이어줄 경우 해당 edge를 추가해서 두 Tree를 하나로 합친다.
3. Edge가 같은 Tree 상의 두 Vertex에 incident하면 해당 edge는 추가하지 않는다.

<br>


### Pesudo Code
```
procedure kruskal(G, w)
Input:      A connected undirected graph G=(V, E) with edge weights w(e)
Output:     A minimum spanning tree defined by the edges X

for all u ∈ V:
    makeset(u)

X = {}
Sort the edges E by weight
for all edges {u,v} ∈ E, in increasing order of weight:
    if find(u) ≠ find(v):
        add edge {u,v} to X
        union(u,v)
```

1. makeset() n번 -> O(n)
2. Edge Sorting -> O(m log n)
3. find 및 union 최대 m번 -> O(m log n)

Time Complexity: O(m log n) time


<br>

## Prim's Algorithm

### Cut Property

: Edge e = (u, v)가 MST의 edge set T에 속해있다고 할 때, e는 **vertex u를 포함한 Vertex set S와 (vertex v가 속한 vertex set)V(G)-S를 이어주는 어떠한 edge들 중에서도 가장 작은 cost**를 가지고 있다.

Cut Property에 의해, 어떤 임의의 vertex set S ⊂ V(G)에 대해, MST는 S와 V(G)-S를 이어주는 edge들 중 cost가 가장 작은 edge를 포함한다.

따라서 **S에 속한 vertex를 하나씩 늘려가면서 cut property를 만족시키도록 edge를 고른다**면 MST를 만들 수 있다.

<br>

![](https://kjaer.io/images/algorithms/dijkstra.gif)


<br>

### Pesudo Code

```
S={v}
T is empty (T will store edges of a MST)
while S≠V(G)
    pick e=(v,w) in E(G) such that, v ∈ S and w ∈ V(G)-S, and e has minimum cost
    T=T ∪ {e}
    S=S ∪ {w}
return the set T
```

- 총 n번 반복
- 해당 조건을 만족하는 edge를 찾는데 최대 m시간 소모

Time Complexity = O(nm)

<br>

## Prim's Algorithm with Priority Queue

```
procedure prim(G,w)
Input:      A connected undirected graph G=(V, E) with edge weights w(e)
Output:     A minimum spanning tree defined by the edges X

for all u ∈ V:
    cost(u)=∞
    prev(u)=nil
Pick any initial node w₁
cost(w₁)=0

H=makequeue(V)  (priority queue, using cost-values as keys)
while H is not empty:
    v=deletemin(H)
    for each {v,z} ∈ E:
        if cost(z)>w(v,z):
            cost(z)=w(v,z)
            prev(z)=v
            decreasekey(H,z)
```

- makequeue, insert n번 + deletemin n번 -> O(n log n)
- 각 edge를 최대 2번 scan하여 decrease key는 최대 m번 수행하게 된다 -> O(m log n)

Time Complexity: O((m+n)log n)


<br>

# String Algorithm

: 문자열 S에서 특정 패턴 P를 찾아내는 알고리즘이다.

## String Search Algorithm

### Naive String Search

: 이름 그대로 1 ~ m번째 글자 까지, 2 ~ m + 1번째 글자까지, 같은 식으로 문자열을 일일히 찾아가며 탐색하는 알고리즘.

길이가 각각 n, m인 문자열&패턴에 대해 O((n-m)m)의 시간 복잡도를 가진다. 매우 쉬운 구현 난이도를 가진다.

<details>
<summary>코드 보기</summary>

```cpp
#include <iostream>
#include <string>

using namespace std;

void find_pattern(const string &, const string &);

int main(const int argc, const char **argv)
{
    const string haystack = "hello hello hello hellchosun";
    const string needle = "hell";

    find_pattern(haystack, needle);

    return 0;
}

void find_pattern(const string &haystack, const string &needle)
{
    const auto haystack_size = haystack.size();
    const auto needle_size = needle.size();
    size_t i;
    size_t j;
    bool unmatched_flag = true;

    cout << "Begin to find pattern \"" << needle << "\" at target string \"" << haystack << "\"" << endl;

    for (int i = 0; i < haystack_size - needle_size; ++i)
    {
        for (int j = 0; j < needle_size; ++j)
        {
            if (haystack[i + j] == needle[j])
            {
                continue;
            }

            break;
        }

        if (j == needle_size)
        {
            cout << "Pattern \"" << needle << "\" matched at " << i + 1 << " ~ " << i + 1 + needle_size << endl;

            unmatched_flag = false;
        }
    }

    if (unmatched_flag)
    {
        cout << "Pattern unmatched" << endl;
    }
}
```

</details>

<br>

### KMP 알고리즘
![image](https://user-images.githubusercontent.com/30489264/132952247-1cd48c3a-22e5-4ca1-818e-3a84a31e8389.png)

> "HelloWorld"라는 문자열이 있다고 할 때, 
> 0. i = -1, j = 0으로 시작하고, 시작 위치의 상태 함수에는 -1이 들어간다.
> 1. i와 j를 한 칸씩 전진시킨 뒤 비교한다.
> 2. i와 j가 매치되면, 혹은 i==-1일 때 한칸씩 전진한 뒤, j 위치에 i를 저장한다.
> 3. 만약 i와 j가 매치되지 않는다면 i는 상태전이함수에 있는 값으로 전환시킨 뒤 2로 돌아간다.
> 4. j가 n보다 커질 때 까지 반복한다.

O(n+m)의 시간 복잡도를 가진다.

<details>
<summary>코드 보기</summary>

```cpp
#include <iostream>
#include <string>
#include <cstdio>
#include <vector>
using namespace std;
vector<int> getPi(string p){
    int m = (int)p.size(), j=0;
    vector<int> pi(m, 0);
    for(int i = 1; i< m ; i++){
        while(j > 0 && p[i] !=  p[j])
            j = pi[j-1];
        if(p[i] == p[j])
            pi[i] = ++j;
    }
    return pi;
}
vector<int> kmp(string s, string p){
    vector<int> ans;
    auto pi = getPi(p);
    int n = (int)s.size(), m = (int)p.size(), j =0;
    for(int i = 0 ; i < n ; i++){
        while(j>0 && s[i] != p[j])
            j = pi[j-1];
        if(s[i] == p[j]){
            if(j==m-1){
                ans.push_back(i-m+1);
                j = pi[j];
            }else{
                j++;
            }
        }
    }
    return ans;
}
int main(){
    string s, p;
    getline(cin, s);
    getline(cin, p);
    auto matched = kmp(s,p);
    printf("%d\n", (int)matched.size());
    for(auto i : matched)
        printf("%d ", i+1);
    return 0;
}
```
</details>

## String Compare Algorithm

### LCS(Longest Common Subsequence/Substring) Algorithm

: 최장 공통 부분수열/부분문자열 알고리즘이다. Subsequence/Substring의 차이는 문자의 연속 여부이다.

> ABCDEF
> GBCDFE
> 두 문자열이 있다고 할 때
> Longest Common Subsequence = 'BCDF' or 'BCDE'
> Longest Common Substring = 'BCD'

<details>
<summary>코드 보기</summary>

#### Longest Common Subsequence

```
function LCSLength(X[1..m], Y[1..n])
    C = array(0..m, 0..n)
    for i := 0..m
        C[i,0] = 0
    for j := 0..n
        C[0,j] = 0
    for i := 1..m
        for j := 1..n
            if X[i] = Y[j]
                C[i,j] := C[i-1,j-1] + 1
            else
                C[i,j] := max(C[i,j-1], C[i-1,j])
    return C[m,n]

function backtrackAll(C[0..m,0..n], X[1..m], Y[1..n], i, j)
    if i = 0 or j = 0
        return {""}
    if X[i] = Y[j]
        return {Z + X[i] for all Z in backtrackAll(C, X, Y, i-1, j-1)}
    R := {}
    if C[i,j-1] ≥ C[i-1,j]
        R := backtrackAll(C, X, Y, i, j-1)
    if C[i-1,j] ≥ C[i,j-1]
        R := R ∪ backtrackAll(C, X, Y, i-1, j)
    return R
```

#### Longest Common Substring

```python


def LCSubStr(X, Y, m, n):
 
    # Create a table to store lengths of
    # longest common suffixes of substrings.
    # Note that LCSuff[i][j] contains the
    # length of longest common suffix of
    # X[0...i-1] and Y[0...j-1]. The first
    # row and first column entries have no
    # logical meaning, they are used only
    # for simplicity of the program.
 
    # LCSuff is the table with zero
    # value initially in each cell
    LCSuff = [[0 for k in range(n+1)] for l in range(m+1)]
 
    # To store the length of
    # longest common substring
    result = 0
 
    # Following steps to build
    # LCSuff[m+1][n+1] in bottom up fashion
    for i in range(m + 1):
        for j in range(n + 1):
            if (i == 0 or j == 0):
                LCSuff[i][j] = 0
            elif (X[i-1] == Y[j-1]):
                LCSuff[i][j] = LCSuff[i-1][j-1] + 1
                result = max(result, LCSuff[i][j])
            else:
                LCSuff[i][j] = 0
    return result
 
 
# Driver Code
X = 'OldSite:GeeksforGeeks.org'
Y = 'NewSite:GeeksQuiz.com'
 
m = len(X)
n = len(Y)
 
print('Length of Longest Common Substring is',
      LCSubStr(X, Y, m, n))
```

</details>

<br>

### + Edit Distance

![image](https://user-images.githubusercontent.com/30489264/132952627-1be38e13-6034-421e-9635-3158f1a96bff.png)

특정 문자열 A에서 문자열 B로 변환하기 위해 몇 번의 연산(삽입, 수정, 삭제)이 필요한 지 구하는 DP 알고리즘

<br>

# 참고 문서

> https://namu.wiki/w/%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98
> https://bowbowbow.tistory.com/6
> https://velog.io/@junhok82/KMP