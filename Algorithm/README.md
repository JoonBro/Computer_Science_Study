# 알고리즘

## Sorting(정렬)

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

---

REFERENCE

[https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html](https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html)

https://hsp1116.tistory.com/33
