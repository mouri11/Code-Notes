# Algorithms

Contents:

- [Binary Search](#binary-search)
- [Quick Sort](#quick-sort)
- [Merge Sort](#merge-sort)
- [KMP String Search](#knuth-morris-pratt-algorithmkmp)

[top](#contents)
## Binary Search
```c++
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
O(1) extra space (in-place sort). Not stable.
```c++
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


## Merge Sort
Divide and Conquer. O(n) extra space. Stable.
```c++
void merge (vector<int> &v, int low, int mid, int high) {
	vector<int> left, right;
	
	for (int i = low; i<= mid;i++) left.push_back(v[i]);
	
	for (int i = mid + 1;i <= high;i++) right.push_back(v[i]);
	
	int i = 0, j = 0,k = low;
	
	while (i < left.size() && j < right.size()) {
		if (left[i] <= right[j]) v[k] = left[i++];
		else v[k] = right[j++];
		k++;
	}
	
	while (i < left.size()) v[k++] = left[i++];
	while (j < right.size()) v[k++] = right[j++];
}

void mergeSort(vector<int> &v, int low, int high) {
	if (low < high) {
		int mid = low + (high - low) / 2;
		
		mergeSort(v, low, mid);
		mergeSort(v, mid + 1, high);
		merge (v, low, mid, high);
	}
}
```
Time Complexity: T(n) = 2T(n/2) + Theta(n) => O(n logn) in all cases


## Knuth-Morris-Pratt Algorithm(KMP)
The KMP matching algorithm uses degenerating property (pattern having same sub-patterns appearing more than once in the pattern) of the pattern and improves the worst case complexity to O(n). The basic idea behind KMP’s algorithm is: whenever we detect a mismatch (after some matches), we already know some of the characters in the text of the next window. We take advantage of this information to avoid matching the characters that we know will anyway match.


Examples of lps[ ] construction:
For the pattern “AAAA”, 
lps[ ] is [0, 1, 2, 3]

For the pattern “ABCDE”, 
lps[ ] is [0, 0, 0, 0, 0]

For the pattern “AABAACAABAA”, 
lps[ ] is [0, 1, 0, 1, 2, 0, 1, 2, 3, 4, 5]

For the pattern “AAACAAAAAC”, 
lps[ ] is [0, 1, 2, 0, 1, 2, 3, 3, 3, 4] 

For the pattern “AAABAAA”, 
lps[ ] is [0, 1, 2, 0, 1, 2, 3]

txt[] = "AAAAABAAABA" 
pat[] = "AAAA"
lps[] = {0, 1, 2, 3} 

i = 0, j = 0
txt[] = "AAAAABAAABA" 
pat[] = "AAAA"
txt[i] and pat[j] match, do i++, j++

i = 1, j = 1
txt[] = "AAAAABAAABA" 
pat[] = "AAAA"
txt[i] and pat[j] match, do i++, j++

i = 2, j = 2
txt[] = "AAAAABAAABA" 
pat[] = "AAAA"
pat[i] and pat[j] match, do i++, j++

i = 3, j = 3
txt[] = "AAAAABAAABA" 
pat[] = "AAAA"
txt[i] and pat[j] match, do i++, j++

i = 4, j = 4
Since j == M, print pattern found and reset j,
j = lps[j-1] = lps[3] = 3

Here unlike Naive algorithm, we do not match first three 
characters of this window. Value of lps[j-1] (in above 
step) gave us index of next character to match.
i = 4, j = 3
txt[] = "AAAAABAAABA" 
pat[] =  "AAAA"
txt[i] and pat[j] match, do i++, j++

i = 5, j = 4
Since j == M, print pattern found and reset j,
j = lps[j-1] = lps[3] = 3

Again unlike Naive algorithm, we do not match first three 
characters of this window. Value of lps[j-1] (in above 
step) gave us index of next character to match.
i = 5, j = 3
txt[] = "AAAAABAAABA" 
pat[] =   "AAAA"
txt[i] and pat[j] do NOT match and j > 0, change only j
j = lps[j-1] = lps[2] = 2

i = 5, j = 2
txt[] = "AAAAABAAABA" 
pat[] =    "AAAA"
txt[i] and pat[j] do NOT match and j > 0, change only j
j = lps[j-1] = lps[1] = 1 

i = 5, j = 1
txt[] = "AAAAABAAABA" 
pat[] =     "AAAA"
txt[i] and pat[j] do NOT match and j > 0, change only j
j = lps[j-1] = lps[0] = 0

i = 5, j = 0
txt[] = "AAAAABAAABA" 
pat[] =      "AAAA"
txt[i] and pat[j] do NOT match and j is 0, we do i++.

i = 6, j = 0
txt[] = "AAAAABAAABA" 
pat[] =       "AAAA"
txt[i] and pat[j] match, do i++ and j++

i = 7, j = 1
txt[] = "AAAAABAAABA" 
pat[] =       "AAAA"
txt[i] and pat[j] match, do i++ and j++

We continue this way...

```c++
void computeLPS(string pattern, int m, vector<int> &lps) {
	int len = 0;
	
	lps[0] = 0;
	
	int i = 1;
	while (i < m) {
		if (pattern[i] == pattern[len]) lps[i++] = ++len;
		else {
			if (len != 0) len = lps[len - 1];
			else lps[i++] = 0;
		}
	}
}

void KMPSearch(string text, string pattern) {
	vector<int> lps(pattern.size());
	
	computeLPS(pattern, pattern.size(), lps);
	
	int i = 0,j = 0;
	
	while (i < text.size()) {
		if (pattern[j] == text[i]) i++,j++;
		
		if (j == pattern.size()) {
			cout<<i - j<<" ";
			j = lps[j - 1];
		}
		
		else if (i < text.size() && pattern[j] != text[i]) {
			if (j != 0) j = lps[j - 1];
			else i++;
		}
	}
}
```
