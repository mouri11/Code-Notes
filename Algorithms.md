# Algorithms

Contents:

- [Binary Search](#binary-search)
- [Quick Sort](#quick-sort)

## Binary Search
```
int binarySearch (vector<int> v, int key) {
	int low = 0;
	int high = v.size() - 1;
	while (low < high) {
		int mid = (low + high) / 2;
		if (v[mid] == key) return mid;
		else if (key < v[mid]) high = mid - 1;
		else low = mid + 1;
	}
	return -1;
}
```
Time Complexity: T(n) = T(n / 2) + c => O(logn)

## Quick Sort
```
int partition (vector<int> &v, int low, int high) {
	int pivot = v[high];
	int i = (low - 1);
	
	for (int j = low;j < high; j++) {
		if (v[j] < pivot) swap(v[++i],v[j]);
	}
	
	swap(v[i + 1], v[high]);
	return (i + 1);
}

void quickSort(vector<int> &v, int low, int high) {
	if (low < high) {
		int pivot = partition(v, low, high);
		
		quickSort(v,low, pivot - 1);
		quickSort(v, pivot + 1, high);
	}
}
```
Time Complexity: T(n) = T(k) + T(n - k - 1) + Theta(n) (k = no. of elements smaller than pivot)
Worst Case: T(n) = T(n - 1) + Theta(n) => O(n^2)
Best and Avg. Case: O(n logn)
