# 알고리즘

## Sorting(정렬)

정렬에는 2가지 종류가 있다. 비교를 하면서 진행하는 Comparisons 방식과, 그렇지 않은 Non-Comparisons 방식으로 나눌 수 있다.

`Selection Sort`, `Insertion Sort`, `Bubble Sort`, `Merge Sort`, `Quick Sort`, `Heap Sort`

### Comparisons Sorting Argorithm (비교 방식 알고리즘)


### Selection Sort(선택 정렬)

현재 위치에 들어갈 값을 찾아 정렬하는 방법. 현재 위치에 저장될 값의 크기가 Max, Min인지에 따라 최소 선택 정렬(오름차순), 최대 선택 정렬(내림차순)로 구분할 수 있다.

1. 인덱스의 맨 앞에서부터, 이를 포함한 그 이후의 배열값 중 최소의 값을 찾는다.
2. 가장 작은 값을 찾으면 현재 인덱스의 값과 바꿔준다.
3. 인덱스를 한칸 옮기고 위 과정을 반복한다.(배열의 마지막 요소 전을 가르킬 때 까지)

이 알고리즘은 첫 인덱스일 때 n-1개, 이후 n-2, ... 1개씩 비교를 반복한다. 따라서 **시간복잡도**는 `O(n^2)`이다. **공간복잡도**는 단 하나의 배열에서만 진행하므로 `O(n)`이다.

```cpp
void selectionSort(vector<int> v){
	for(int i = 0; i < v.size() - 1; i++){
		int maxIndex = i;
		for(int j = i+1; j < v.size(); j++){
			if(v[maxIndex] >= v[j]) maxIndex = j;
		}
	swap(v[i], v[maxIndex]);
	}
}
```

### Insertion Sort(삽입 정렬)

삽입정렬은 현재 위치에서, 그 이하의 배열들을 비교하여 자신이 들어갈 위치를 찾아, 그 위치에 삽입하는 알고리즘이다.

1. 두 번째 인덱스부터 시작한다. 현재 인덱스는 별도의 변수에 저장하고, 비교 인덱스를 현재 인덱스 -1 로 잡는다.
2. 별도로 저장해둔 삽입을 위한 변수와, 비교 인덱스의 배열 값을 비교한다.
3. 두 값을 비교하여 순서에 맞게 교체한다. 이후 비교 인덱스 -1 하여 반복한다(첫 인덱스 까지)

이 정렬 알고리즘은 **최악의 경우(역으로 정렬되어 있을 때)** 시간복잡도는 `O(n^2)`이다. **이미 정렬되어 있을 경우** 한번씩만 비교하기 때문에 `O(n)` 이다.
공간복잡도는 `O(n)`

```cpp
void insertionSort(vector<int> v){
	for(int i = 1; i < v.size(); i++){
		int key = v[i];
		int j = i - 1;
		while(j >= 0 && key < v[j]){
			swap(v[j], v[j+1]);
			j--;
		}
		v[j+1] = key;
	}
}
```

### Bubble Sort(버블 정렬)

버블 정렬은 매번 연속된 두 개의 인덱스를 비교하여, 정한 기준의 값을 뒤로 넘겨 정렬하는 방법이다.
오름차순일 경우 비교시마다 큰 값이 뒤로 이동하며, 1회전 종료시 가장 큰 값이 제일 뒤에 저장된다.
내림차순일 경우 반대로 비교시마다 작은 값들이 뒤로 이동하며, 1회전 종료시 가장 작은 값이 제일 뒤에 저장된다.
마지막 값은 정렬되어 있는 상태가 되므로, **전체 배열의 크기 - 현재까지 순환한 바퀴 수(마지막에 정렬된 원소 갯수)** 만큼 반복해주면 된다.

배열의 형태가 어떻든 전체 비교를 진행하게 되며, 시간복잡도는 `O(n^2)`이다.
공간복잡도는 `O(n)`

```cpp
void bubbleSort(vector<int> v){
	for(int i = 0; i < v.size() - 1; i++){
		for(int j = 1; j < v.size() - i; j++){
			if(v[j-1] > v[j]){
				swap(v[j-1], v[j]);
			}
		}
	}
}
```

### Merge Sort(합병 정렬)

합병정렬은 `분할 정복(Divide and conquer)` 방식으로 설계된 알고리즘이다. 분할정복은 큰 문제를 반으로 쪼개 문제를 해결해나가는 방식으로, 배열의 크기가 1보다 작거나 같을 때 까지 반복한다.
입력으로 하나의 배열을 받고, 연산 중에 두개의 배열로 계속 쪼게 나간뒤, 합치면서 정렬한다.
합병은 두개의 배열을 비교하여, 기준에 맞는 값을 다른 배열에 저장해나간다.
오름차순의 경우 배열 A, 배열 B를 비교하여 A에 있는 값이 더 작다면 새 배열에 저장해주고, A 인덱스를 증가시킨 후 다시 반복(B에 있는 값이 더 작다면 새 배열에 저장 후 B인덱스 증가)
A, B 중 하나가 모든 배열값들을 새 배열에 저장할 때 까지 반복하며, 남은 배열의 나머지 부분은 새 배열의 뒷 부분에 값을 옮겨준다.(이미 A, B는 정렬되어 있는 상태이므로 끝난 부분보다 무조건 크고 정렬되어 있다.)

**분할 과정**의 기본 로직은 다음과 같다.

1. 현재 배열을 반으로 쪼갠다. 배열의 시작 위치와 종료 위치를 받아 가운데 위치를 구한다. (시작위치 + 종료위치 / 2)
2. 이를 쪼갠 배열의 크기가 0이거나 1일때 까지 반복.

**합병 과정**

1. 두 배열 A, B의 크기를 비교한다. 각각의 배열의 현재 인덱스를 i, j로 가정하자.
2. i에는 A 배열 시작 인덱스를 저장, j에는 B 배열 시작 인덱스를 저장.
3. A[i], B[j]를 비교한다. 오름차순일 경우 두 값중 작은 값을 새배열 C에 저장한다. A[i]가 컸다면 A[i]의 값을 배열 C에 저장해주고, i 값을 하나 증가시킨다. 반대로 B[j]가 컸다면 B[j]의 값을 C에 저장해주고, j값을 하나 증가시킨다.
4. 이를 i나 j가 각자 배열의 끝에 도달할 때까지 반복한다.
5. 둘중 하나가 나면, 남은 배열의 나머지 부분을 C 배열 뒤에 저장한다.
6. C 배열을 원래 배열의 위치에 값을 저장해준다.

합병과정은 두 배열 A, B를 정렬하기 때문에, A 배열의 크기를 N1, B 배열의 크기를 N2라고 할 경우 `O(n1 + n2)`와 같다. 즉 전체적으로 `O(n)`이라고 할 수 있다.
분할 과정은 logN 만큼 일어난다. 매번 반씩 감소하므로 logN만큼 반복해야 크기가 1인 배열로 분할할 수 있다.
각 분할별로 합병을 진행하므로, 합병정렬의 시간 복잡도는 `O(n * log n)`이다.
사용하는 공간 복잡도는 정렬을 위한 배열을 하나 더 사용하기 때문에 `2N`개를 사용한다.

```cpp
void merge(vector<int>& v, int startIndex, int endIndex, int midIndex){
	vector<int> ret;
	int i = startIndex;
	int j = midIndex + 1;
	int copyIndex = 0; // 정렬된 임시 벡터를 원래 벡터에 복사할 때 사용하는 인덱스
	
	// 결과를 저장할 배열에 하나씩 비교하여 저장한다.
	while(i <= m && j <= e){
		if(v[i] < v[j]) ret.push_back(v[i++]);
		else if(v[i] > v[j]) ret.push_back(v[j++]);
	}
	// i 와 j가 가르키는 부분배열은 이미 이전단계에서 정렬되어 들어와있다. 따라서 한 곳이 종료되면 남은 
	// 부분은 이미 정렬되어 있는 부분으로써 맨 뒤에 추가만 하면 된다.
	// 남아있는 배열은 인덱스가 종료 인덱스보다 작을 경우이기 때문에 while문 조건에 의해 걸린다.
	while(i <= m) ret.push_back(v[i++]);
	while(j <= e) ret.push_back(v[j++]);

	// 원래 배열에 복사
	for(int k = startIndex; k <= endIndex; k++){
		v[k] = ret[copyIndex++];
	}
}

void mergeSort(vector<int>& v, int startIndex, int endIndex){
	if(startIndex < endIndex){
		int minIndex = (startIndex + endIndex) / 2;
		/* divide 분할 */
		mergeSort(v, startIndex, midIndex);
		mergeSort(v, midIndex + 1, endIndex);
		/* conquer, 합병 */
		merge(v, startIndex, endIndex, midIndex);
	}
}
```

### Quick Sort(퀵 정렬)

퀵 정렬 또한 `분할정복(Divide and conquer)` 방식을 이용하여 정렬을 수행하는 알고리즘이다.

`Pivot Point` 라는 기준이 되는 값을 하나 설정하는데, 이 값을 기준으로 작은 값은 왼쪽, 큰 값은 오른쪽으로 옮기는 방식으로 정렬을 진행한다. 이를 반복하여 분할된 배열의 크기가 1이 되면 배열이 모두 정렬된 것이다.

**과정 설명**

1. 리스트 안에 있는 한 요소를 선택한다. 이렇게 고른 원소를 피벗(pivot) 이라고 한다.
2. 피벗을 기준으로 피벗보다 작은 요소들은 모두 피벗의 왼쪽으로 옮겨지고 피벗보다 큰 요소들은 모두 피벗의 오른쪽으로 옮겨진다. (피벗을 중심으로 왼쪽: 피벗보다 작은 요소들, 오른쪽: 피벗보다 큰 요소들)
3. 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬한다.
    - 분할된 부분 리스트에 대하여 순환 호출 을 이용하여 정렬을 반복한다.
    - 부분 리스트에서도 다시 피벗을 정하고 피벗을 기준으로 2개의 부분 리스트로 나누는 과정을 반복한다.
4. 부분 리스트들이 더 이상 분할이 불가능할 때까지 반복한다.
    - 리스트의 크기가 0이나 1이 될 때까지 반복한다.

- 하나의 리스트를 피벗(pivot)을 기준으로 두 개의 비균등한 크기로 분할하고 분할된 부분 리스트를 정렬한 다음, 두 개의 정렬된 부분 리스트를 합하여 전체가 정렬된 리스트가 되게 하는 방법이다.
- 퀵 정렬은 다음의 단계들로 이루어진다.
    - 분할(Divide): 입력 배열을 피벗을 기준으로 비균등하게 2개의 부분 배열(피벗을 중심으로 왼쪽: 피벗보다 작은 요소들, 오른쪽: 피벗보다 큰 요소들)로 분할한다.
    - 정복(Conquer): 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않으면 순환 호출 을 이용하여 다시 분할 정복 방법을 적용한다.
    - 결합(Combine): 정렬된 부분 배열들을 하나의 배열에 합병한다.
    순환 호출이 한번 진행될 때마다 최소한 하나의 원소(피벗)는 최종적으로 위치가 정해지므로, 이 알고리즘은 반드시 끝난다는 것을 보장할 수 있다.

**예제(과정)**

![quick-sort2](https://user-images.githubusercontent.com/44499629/120925063-afa30500-c711-11eb-8fa6-056f324dab39.png)

```cpp
int partition(vector<int> &v, int left, int right){
	int pivot, temp;
	int low, high;
	low = left;
	high = right + 1;
	pivot = v[left]; // 가장 왼쪽 원소를 pivot으로 설정
	
	/* low와 high가 교차할 때까지 반복(low < high) */
	do{
		/* v[low]가 피벗보다 작을때까지 low 증가 */
		do{
			low++;
		} while(low <= right && v[low] < pivot);
		
		/* v[high]가 피벗보다 클 때까지 high 감소 */
		do{
			high--;
		} while(low <= right && v[high] > pivot);

		// 만약 low와 high가 교차하지 않았다면 v[low], v[high] 교체
		if(low < high){
			swap(v[low], v[high]);
		}
	} while(low < high);
	
	/* low와 high가 교차하여 종료되었을 때 pivot(v[left]) 와 v[high]를 교체 */
	/* v[high]가 피봇 기준 왼쪽에서 정렬된(피봇보다 작은 값들) 부분 중 가장 오른쪽에 위치한 곳으로서*/
	/* 피봇이 그 중 가장 큰 값이므로 피봇이 가야할 곳을 가르키고 있다. */
	swap(v[left], v[high]);
	return high; // pivot 위치를 반환(pivot은 정렬된 위치에 정확히 저장되어 있는 곳)
}

void quick_sort(vector<int> &v, int left, int right){
	
	if(left < right){ // 정렬할 v의 범위가 2개 이상의 데이터라면(벡터의 크기가 0이나 1이 아니면)
		int q = partition(v, left, right);
		quick_sort(v, left, q - 1);
		quick_sort(v, q + 1, right);
	}
}
		
```

### Heap Sort(힙 정렬)

`Binary Heap` 자료구조를 활용하여 정렬하는 방식이다. 2가지 방법이 존재한다. 정렬의 대상인 데이터들을 힙에 넣었다가 꺼내는 원리로 Sorting 하는 방법과, 기존의 배열을 `heapify`(heap으로 만들어주는 과정)을 거쳐 꺼내는 원리로 정렬하는 방법이다. `heap`에 데이터를 저장하는 시간 복잡도는 `O(log n)`이고, 삭제 시간복잡도 또한 `O(log n)`이다. 때문에 힙 자료구조를 사용하여 Sorting 하는데 시간복잡도는 `O(log n)`이 된다. 정렬하는 대상이 n개라면 총 `O(n*log n)`이 된다.

### Non-Comparisons Sorting Argorithm

### Counting Sort(카운팅 정렬)

- 항목들의 순서를 정하기 위해 집합에 각 항목이 몇개씩 있는지 세는 작업을 한다. 이를 통해 각 항목의 Index 범위를 설정할 수 있다.
- 다만 정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능하다.
- 카운트들을 위한 충분한 공간을 할당하려면 집합 내의 가장 큰 정수를 알아야 한다.
- 시간복잡도 : O(n + k) / n : 리스트의 개수 , k : 정수의 최댓값

예시를 통해 설명이 잘 되어있는 링크 : https://blog.naver.com/hunii123/222326693534

---

## 최단거리 알고리즘

### Dijkstra 알고리즘

**"하나의 시작점에서 모든 정점까지 최단거리를 구하는 방법"**

- 간선의 가중치가 음수이면 안됨(음수가 있다면 벨만-포드 사용)
- greed 알고리즘이기 때문에 한 단계밖에 생각 못함. 이미 가중치의 합이 커졌을 때 나중에 다시 줄어들것을 고려 못함(음수 안되는 이유)
- dist[v]는 출발 정점부터 정점 v 까지의 거리 의미(Deafult = INF)

다익스트라의 동작 과정에 대해 설명하면, `너비 우선 탐색(BFS)`와 비슷합니다. 먼저 해당 노드까지의 최단 거리를 저장할 dist 배열이 필요합니다. 처음에 방문하지 않았기 때문에 -1로 초기화해줍니다. 너비 우선 탐색과 달리 `우선순위 큐`를 사용하여 **방문할 정점**과 **방문할 정점까지의 거리**를 넣어줘야 합니다.

**<예제>**

![image](https://user-images.githubusercontent.com/44499629/121199341-cf6f3000-c8ad-11eb-963e-30a2bc16ef7b.png)

처음 들어갈 노드를 시작노드 S를 1로 하고, 시작노드와 거리를 우선순위 큐에 넣어줍니다. 이때 S까지의 거리는 자기자신이기 때문에 0입니다.

따라서 dist[1] = 0을 넣어주고 우선순위 큐에 {0, 1}라는 값을 넣어줍니다.

그리고 너비 우선 탐색과 마찬가지로 자신과 연결된 정점들을 확인합니다. 현재 위치 1까지 이동한 거리는 0이기 때문에, 우선순위 큐에는 {0 + 10, 2}, {0 + 5, 3} 이라는 값이 들어가게 되고 dist[2] = 10, dist[3] = 5가 됩니다.

이때 우선순위 큐의 정렬방식이 `MinHeap`이라 하면 3번 정점을 먼저 방문하게 됩니다.

3번 정점에서 연결된 정점을 확인하면 2번 정점이며 현재 위치 3까지 이동한 거리는 5이고, 간선의 가중치는 3이므로, 1에서 3을거쳐 2로 이동하는 거리는 8이 되며, 이는 1에서 바로 2까지 이동한 10보다 가깝습니다.

따라서 dist[2] = 8로 갱신하고 우선순위 큐에 {5 + 3, 2}를 넣어줍니다.

그러면 우선순위 큐이기 때문에 {10,2}보다 {8, 2}가 먼저 꺼내게 됩니다.

2에서는 간선이 더이상 존재하지 않기 때문에 갱신되는 것은 없고 바로 다음으로 {10, 2}가 나오게 되는데 dist[2] 는 10보다 작은 8이므로 바로 continue해서 넘어갑니다.

이후 우선순위 큐가 비어 있어 과정이 종료됩니다.

- 시간복잡도는 모든 간선은 한번씩 확인해야 하기 때문에 간선 검사하는데 O(E)의 시간이 걸리며, 우선순위 큐에 들어가는 원소는 간선마다 최대 1번씩 들어가므로 O(E), 이를 한번 삽입, 삭제하는데 O(logN)만큼 걸린다. 총 O(E + ElogE) = `O(E Log E)` 이다.

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;
int N, M,u,v,d,S;
vector< vector<pair<int, int> > > edge;
vector<int> Dijkstra(int start) {
	vector<int> dist(N+1, -1);
	priority_queue<pair<int, int> > pq; 
	// first : dist , second : vertex_pos
	dist[start] = 0;pq.push(make_pair(-dist[start], start)); 
	// Min-Heap 처럼 사용하기 위해 앞의 거리 인자에 -값을 곱해줌.
	while (!pq.empty()) {
		int here = pq.top().second;
		int heredist = -pq.top().first;
		pq.pop();
		if (heredist > dist[here]) continue;
		for (int i = 0; i < edge[here].size(); i++) {
			int there = edge[here][i].first;
			int nextdist = heredist + edge[here][i].second;
			if (dist[there] == -1 || dist[there] > nextdist) { //최단 거리 갱신
				dist[there] = nextdist;
				pq.push(make_pair(-nextdist, there));
			}
		}
	}
	return dist;
}

int main() {
	cin >> N >> M >> S;
	edge.resize(N + 1);
	for (int i = 0; i < M; i++) {
		cin >> u >> v >> d;
		edge[u].push_back(make_pair(v, d));
	}
	vector<int> dist = Dijkstra(S);
	for (int i = 1; i <= N; i++) {
		cout << dist[i] << endl;
	}
	return 0;
}
```
---

REFERENCE

[https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html](https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html)

https://hsp1116.tistory.com/33
