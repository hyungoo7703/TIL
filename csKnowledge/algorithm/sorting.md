# 정렬

컴퓨터 프로그램에서 자료를 정렬하는 알고리즘으로, 입력된 자료를 비교하여 오름차순 또는 내림차순으로 정렬한다.

## 1. 버블 정렬(Bubble Sort)
```java
public void bubbleSort(int[] arr) {
    for (int i = 0; i < arr.length - 1; i++) {
        for (int j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```
+ 시간복잡도: O(n²)
+ 공간복잡도: O(1)
+ 인접한 두 원소를 비교하면서 정렬하는 알고리즘
+ 구현이 간단하지만, 비교 및 교환 횟수가 많아 대규모 데이터에 대해서는 비효율적이다.

## 2. 선택 정렬(Selection Sort)
```java
public void selectionSort(int[] arr) {
    for (int i = 0; i < arr.length - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        int temp = arr[minIdx];
        arr[minIdx] = arr[i];
        arr[i] = temp;
    }
}
```
+ 시간복잡도: O(n²)
+ 공간복잡도: O(1)
+ 주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하며 정렬하는 알고리즘
+ 비교 횟수는 버블 정렬과 같으나, 교환 횟수는 적은편이다. (데이터 교환 횟수가 정해져 있다)

## 3. 삽입 정렬(Insertion Sort)
```java
public void insertionSort(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}
```
+ 시간복잡도: O(n²), 최선의 경우 O(n)
+ 공간복잡도: O(1)
+ 각 원소를 적절한 위치에 삽입하여 정렬하는 알고리즘
+ 이미 정렬된 부분 리스트를 유지하면서 하나씩 새로운 원소를 올바른 위치에 삽입하는 방식이다.
+ 데이터가 이미 정렬되어 있는경우 다른 정렬 알고리즘보다 빠르게 동작 할 수 있다.

## 4. 퀵 정렬(Quick Sort)
```java
public void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

private int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = (low - 1);
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    return i + 1;
}
```
+ 시간복잡도: 평균 O(nlogn), 최악 O(n²)
+ 공간복잡도: O(logn)
+ <b>분할 정복(divide and conquer)</b>방법을 통해 리스트를 정렬하는 알고리즘
+ 리스트에서 하나의 원소를 선택하고 그 원소를 기준으로 리스트를 두개의 부분 리스트로 나눈뒤 재귀적으로 정렬을 수행한다.
+ 평균적으로 매우 빠르며 대부분의 상황에서 효과적으로 동작한다.(실제 상황에서 가장 빠른 정렬 알고리즘)

> #### 분할 정복이란?
매우 큰 문제를 작은 단위로 나누어(분할) 해결(정복)하는 알고리즘 설계 기법

## 5. 병렬 정렬(Merge Sort) 
```java
public void mergeSort(int[] arr, int l, int r) {
    if (l < r) {
        int m = (l + r) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}

private void merge(int[] arr, int l, int m, int r) {
    int n1 = m - l + 1;
    int n2 = r - m;
    int[] L = new int[n1];
    int[] R = new int[n2];
    
    for (int i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];
        
    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}
```
+ 시간복잡도: O(nlogn)
+ 공간복잡도: O(n)
+ 분할 정복(divide and conquer)방법을 통해 리스트를 정렬하는 알고리즘
+ 하나의 리스트를 작은 리스트로 분할하고, 각각을 병렬적으로 정립하여 합치는 방식으로 정렬
+ 대용량 데이터를 빠르게 정렬할 수 있는 효과적인 방법이다.

## 결론
선택 정렬과 삽입 정렬은 구현이 간단하지만 시간복잡도가 O(n²)로 비효율적이므로, <br>
**실제로는 퀵 정렬이나 병합 정렬, 또는 언어에서 제공하는 정렬 라이브러리를 사용하는 것이 권장**

> #### 자바에서 제공하는 주요 정렬 라이브러리

### 1. Arrays.sort()
```java
// 기본 정렬 (오름차순)
int[] arr = {5, 2, 3, 1, 4};
Arrays.sort(arr);

// 특정 범위만 정렬
Arrays.sort(arr, 1, 4);  // 인덱스 1부터 3까지만 정렬

// 내림차순 정렬 (Integer 배열만 가능)
Integer[] arr = {5, 2, 3, 1, 4};
Arrays.sort(arr, Collections.reverseOrder());
```

### 2. Collections.sort()
```java
// ArrayList 정렬
ArrayList<Integer> list = new ArrayList<>();
Collections.sort(list);  // 오름차순
Collections.sort(list, Collections.reverseOrder());  // 내림차순
```
