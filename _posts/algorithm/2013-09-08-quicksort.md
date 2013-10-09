---
layout: post
title: "快排 QuickSort"
categorist: [algorithm, sort]
---

快排算法及变形

快速排序普遍被认为是平均性能最好的基于比较的排序，平均时间复杂度是`O(NlogN)`，当序列已经有序的时候，复杂度最高为`O(N^2)`

###第一次的代码

    /**
	 * 快排，第一次的思路，虽然一个函数搞定，但是思路和代码不够清晰简练
	 */
	public void binSort2(Integer arr[], int s, int e){
		int val = arr[s];
		int idx = s; //明显可以优化掉的。。。
		for(int i=s, j=e;i<=j; ){
			while(i<=j && arr[j]>=val)
				j--;
			if(j>=i){
				arr[idx] = arr[j];
				idx = j;
				j--;
			}else
				break;
			while(i<=j && arr[i]<=val)
				i++;
			if(j>=i){
				arr[idx] = arr[i];
				idx = i;
				i++;
			}else
				break;
		}
		arr[idx] = val;
		if(idx>s+1)
			binSort2(arr, s, idx-1);
		if(idx<e-1)
			binSort2(arr, idx+1, e);
		
	}

### 看书后的代码

    public int binSplit(Integer arr[], int s, int e){
		int val = arr[s];
		while(e>s){
			while(e>s && arr[e]>=val)
				e--;
			if(e>s)
				arr[s] = arr[e];
			while(e>s && arr[s]<=val)
				s++;
			if(e>s)
				arr[e] = arr[s];
		}
		arr[s] = val;
		return s;
	}
	/**
	 * 看数据结构书上的思路，其实差不多，关键是其将快排显式地分为两个子过程，增强了代码的可读性和可理解性
	 * */
	public void binSort(Integer arr[], int s, int e){
		int mid = binSplit(arr, s, e);
		if(mid>s+1)
			binSort(arr, s, mid-1);
		if(mid<e-1)
			binSort(arr, mid+1, e);
	}

### 精简后的代码

    /**
	 * 精简后的代码
	 */
	public void binSort0(int A[], int s, int e) {
		if (e <= s)
			return;
		int val = A[s], i = s, j = e;
		while (i < j) {
			while (A[j] >= val && j > i)
				j--;
			A[i] = A[j];
			while (A[i] <= val && i < j)
				i++;
			A[j] = A[i];
		}
		A[i] = val;
		binSort0(A, s, i - 1);
		binSort0(A, i + 1, e);
	}

###变形1： 找前k个最小值并排序

    /**
	 * find and sort the first k minimum value in arr
	 */
	public void binSortKMinimum(Integer arr[], int s, int e, int k){
		int mid = binSplit(arr, s, e);
		if(mid>s+1)
			binSortKMinimum(arr, s, mid-1, k);
		if(mid<k-1 && mid<e-1)
			binSortKMinimum(arr, mid+1, e, k);
	}

###变形2： 找前k个最小值，不用排序
    
    /**
	 * find the first k minimum value in arr(without sorting)
	 */
	public void binFindKMinimum(Integer arr[], int s, int e, int k){
		int mid = binSplit(arr, s, e);
		if(mid>k-1 && mid>s+1)
			binFindKMinimum(arr, s, mid-1, k);
		if(mid<k-1 && mid<e-1)
			binFindKMinimum(arr, mid+1, e, k);
	}

### 测试代码

    /**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		QuickSort qs = new QuickSort();
		Integer[] arr = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20};
		ArrayList<Integer> list = new ArrayList<Integer>();
		Collections.addAll(list, arr);
		int maxIter = 1000;
		while(maxIter-->0){
			Collections.shuffle(list);
//			System.out.println(list);
			list.toArray(arr);
			int s=0, e=arr.length-1, k=e/2;
//			qs.binSort(arr, 0, e);
//			qs.binSort2(arr, 0, e);
//			qs.binFindKMinimum(arr, s, e, k);
			qs.binSortKMinimum(arr, s, e, k);
			if(!isAscend(arr, s, k))
				System.out.println("FAIL");
		}
		System.out.println("Over");
	}
	
	public static boolean isAscend(Integer[] arr, int s, int e){
		while(s<e){
			if(arr[s+1]<arr[s])
				return false;
			else
				s++;
		}
		return true;
	}
