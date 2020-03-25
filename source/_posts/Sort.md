title: Sort
author: bellick
tags:
  - sort
  - c++
  - algorithm
  - ''
categories:
  - Algorithms
date: 2018-11-07 12:37:00
mathjax: true
---
# 常用排序算法总结(C++ 实现)
## 概述
在计算机科学中，排序算法是一种重要的操作。合理的排序算法能够大幅度提高计算机处理数据的性能。排序有内部排序和外部排序，内部排序是数据记录在内存中进行排序，而外部排序是因排序的数据很大，一次不能容纳全部的排序记录，在排序过程中需要访问外存。我们这里说说八大排序就是内部排序。
![sort](https://img-blog.csdn.net/20180414202737115?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
### 1.插入排序
#### 1.1直接插入排序
###### 算法思想
直接插入排序是一种简单的排序算法，由 n-1 趟排序组成。第 p 趟排序后保证第 0 个位置到第 p 个位置上的元素为有序状态。第 p+1 趟排序将第 p+2 个元素插入到前面 p+1个元素的有序表中。下图显示了直接插入排序算法的每一趟的排序情况。

|初始数据序列|32&nbsp;&nbsp;18 &nbsp;&nbsp;65&nbsp;&nbsp;48&nbsp;&nbsp;27&nbsp;&nbsp;9|
| ---------|:-------------------:|
|第1趟排序之后|**18**&nbsp;&nbsp;**32**&nbsp;&nbsp;65&nbsp;&nbsp;48&nbsp;&nbsp;27&nbsp;&nbsp;9|
|第2趟排序之后|**18**&nbsp;&nbsp;**32**&nbsp;&nbsp;**65**&nbsp;&nbsp;48&nbsp;&nbsp;27&nbsp;&nbsp;9|
|第3趟排序之后|**18**&nbsp;&nbsp;**32**&nbsp;&nbsp;**48**&nbsp;&nbsp;**65**&nbsp;&nbsp;27&nbsp;&nbsp;9|
|第4趟排序之后|**18**&nbsp;&nbsp;**27**&nbsp;&nbsp;**32**&nbsp;&nbsp;**48**&nbsp;&nbsp;**65**&nbsp;&nbsp;9|
|第5趟排序之后|**9**&nbsp;&nbsp;**18**&nbsp;&nbsp;**27**&nbsp;&nbsp;**32**&nbsp;&nbsp;**48**&nbsp;&nbsp;**65**|

###### 代码实现
	void DirectInsertSort(int *array,int n){
	    int p ,i;
	    for(p = 1;p<n;p++){
	        int temp = array[p];
	        i = p-1;
	        while(i>=0&&array[i]>temp){
	            array[i+1] = array[i];
	            i--;
	        }
	        array[i] = temp;
	    }
	}
###### 复杂度分析
直接插入排序算法主要应用比较和移动两种操作，从空间上来看，它只需要一个元素的辅助空间，用于位置的交换，有些教材也将这类排序算法称为原地(In Place)排序算法。
从时间上分析，首先外层循环要进行 n-1 次，但每一趟插入排序的比较和移动的次数并不相同。第 p 趟插入时最好的情况时数据已经排好序，每趟插入进行一次比较，两次移动；最坏的情况时比较 p 次，移动 p+2 次，(逆序）（p = 1，2，...,n-1)。记 M 为执行一次排序算法移动的次数，C 为比较次数，则：


&nbsp;&nbsp;C min = n-1;
&nbsp;&nbsp;M min = 2(n-1);

&nbsp;&nbsp;C max = 1+2+...+(n-1),

&nbsp;&nbsp;M max = 3+4+...+(n+1) = (n^2+3n-4)/2;

假设数据元素在各个位置的概率相等，即 1/n ，则平均的比较次数和移动次数为：

C ave = (n^2+n-2)/4,&nbsp;&nbsp;M ave = (n^2+5n-6)/4;

因此，直接插入排序算法的时间复杂度是 O(n^2)。对于随机顺序的数据来说，移动和比较的次数接近最坏情况。

由于直接插入算法的元素移动是顺序的，该算法是稳定的。
#### 1.2折半插入排序
###### 算法思想
直接插入排序算法是利用有序表的插入操作来实现对数据集合的排序。在进行第 p+1 趟插入排序时，需要在前面的有序序列 data[0],data[1],...data[p] 中找到 data[p+1] 第对应位置 i ，同时将 data[i],data[i+1],...data[p] 都向后移动一个位置。由于有序表是排好序的，故可以用折半查找（二分法）操作来确定 data[p+1]  的位置，这就是折半插入算法的思想。
###### 代码实现
	void HalfInsertSort(int *array,int n){
	    int left,right,middle,p;
	    for(p = 1;p<n;p++){
	        int temp = array[p];
	        left = 0;right = p-1;
	        while (left<=right) {
	            middle = (left+right)/2;
	            if(array[middle]>temp)
	                right = middle-1;
	            else
	                left = middle+1;
	        }
	        for(int i = p-1;i>=left;i--){
	            array[i+1] = array[i];
	            array[left] = temp;
	        }
	    }
	}
###### 复杂度分析
折半插入排序算法与直接插入算法相比，需要的辅助空间与直接插入排序算法基本一致，时间上，折半插入的比较次数比直接插入的最坏情况号，最好情况下，时间复杂度为 O(n log2 n);折半插入算法的元素移动与直接插入相同，复杂度为 O(n^2)。

折半插入和直接插入算法的元素移动一样是顺序的，因此该排序算法也是稳定的。
#### 1.3 希尔(shell)排序
##### 算法思想
希尔排序的基本思想是，先将待排序的数据序列划分成若干子序列，分别进行直接插入排序：待整个序列中的数据基本有序后，再对全部的数据进行一次直接插入排序。
对于子序列可以采用任意简单的排序算法。

例如，对于序列 {65,34,25,87,12,38,56,46,14,77,92,23},可以划分成如下 6 个子序列。
![shell](https://img-blog.csdn.net/20180414202844775?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![](/Users/bellick/Documents/MarkDown/sortFunction/屏幕快照 2017-12-18 上午10.06.30.png)

如果初始序列是 array[0],&nbsp;array[1],&nbsp;array[2], ... &nbsp;array[n-1],&nbsp;子序列间隔为 d ，则子序列可以描述为 &nbsp;array[i],&nbsp;array[i+d],&nbsp;array[i+2\*d], ...&nbsp;array[i+k\*d],（其中 0<=i<d,i+k*d<n)。希尔排序中通过不断地缩小增量，来将原始序列分成若干个子序列。例如，增量初始的时候可以选为待排序的元素个数的一半，即 n/2 的向下取整，在后来的迭代过程中不断缩小增量，下一次的增量为上一次的一半，即第二趟的选择增量为 n/4 的向下取整，以此类推，知道增量变为 1 为止。这时序列已经基本有序，对整个序列进行一次插入排序即可完成数据排序。

######希尔排序算法流程
![这里写图片描述](https://img-blog.csdn.net/20180414202942962?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180414203007577?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180414203020729?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180414203041317?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180414203056521?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180414203114194?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29uZV9pc19hbGxs/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 代码实现
	void ShellSort(int *array,int n){
	    int d = n/2;
	    while(d>=1){
	        for(int k = 0;k<d;k++){
	            for(int i = k+d;i<n;i++){
	                int temp = array[i];
	                int j = i-d;
	                while(j>=k&&array[j]>temp){
	                    array[j+d] = array[j];
	                    j -=d;
	                }
	                array[j+d] = temp;
	            }
	        }
	        d = d/2;
	    }
	}
##### 复杂度分析
希尔排序算法依赖于增量序列的选择，时间复杂度在 O(nlog2n) 和 O(n^2) 之间，大致为 O(n^1.3) 和直接插入排序算法相比，减少了算法复杂度。
希尔排序算法是不稳定的。
### 2.交换排序
#### 2.1冒泡排序
##### 2.1.1简单冒泡排序
###### 算法思想
以升序排序（不减排序）算法为例
冒泡排序算法一共要进行 n-1 趟排序，每一趟排序都要将待排序序列中最大的元素挤到最后。
第一趟：将第一个元素与第二个元素比较，若为逆序，则交换；然后比较第二个元素和第三个元素，若为逆序，则交换；以此类推，直到第 n-1 个元素和第 n 个元素比较，若为逆序，则交换，这样，经过第一趟排序，最大的元素被移动到了序列的最后。
第二趟排序，由于最大的元素已经在最右端了，因此只需要对记录{a[0],a[1],...a[n-1]}进行上述排序过程就可以了。
以此类推，进行 n-1 趟扫描。
###### 代码实现
	void bubbleSort(char *a,int n){
		char tmp;
		int i,j;
		for(i = 0;i<n-1;i++){
			for(j=0;j<n-i-1;j++){	
				if(a[j]>a[j+1]){
					tmp = a[j];
					a[j] = a[j+1];
					a[j+1] = tmp;
				}
			}
		}
	}
###### 复杂度分析
这种冒泡排序算法直观、简便。但时间复杂度长。
对任何序列，都需要进行 n-1 趟扫描，第 i 趟需要进行 n-i 次元素比较。造成了时间上的浪费。因此，大部分情况，我们都会选择改进的冒泡排序。
##### 2.1.2改进的冒泡排序
###### 算法思想
通过对每一趟排序进行监控，若中间某一趟排序过程中数没有进行交换，则说明序列已经排好，此时跳出循环即可
###### 代码实现
	void bubbleSort(char *a,int n){
		char tmp;
		int i,j;
		for(i = 0;i<n-1;i++){
			int flag=0;
			for(j=0;j<n-i-1;j++){	
				if(a[j]>a[j+1]){
					tmp = a[j];
					a[j] = a[j+1];
					a[j+1] = tmp;
					flag=1;
				}
			}
			if(flag==0)break;
		}
	}
###### 复杂度分析
显然，改进的冒泡排序算法的效率和待排序的初始顺序密切相关。若待排序的元素是正序，则是最好情况，此时只需要进行一趟排序，比较次数为 n-1 次，移动元素次数为 0 次；若初始待排序列为逆序，则是最坏情况，此时需要执行 n-1 趟排序，第 i 趟做了 n-i 次比较，执行 3(n-i) 次元素交换

所以最坏情况时间复杂度为 O(n^2).平均时间复杂度也是 O(n^2) 。

由于冒泡排序算法只是进行元素间的顺序移动，所以是稳定的算法。
#### 2.2快速排序
1. 分割：取序列的一个元素作为轴元素，利用这个轴元素，把序列分成三段，使所有小于等于轴元素的元素放在轴的左边，大于轴的元素放在轴的右边。此时，轴元素已经被放到的正确的位置。
2. 分治：对左段和右段中的元素递归调用（1）中的过程，分别对左端=段和右段中的元素进行排序。
3. 此时，排序完成

一般情况下，我们采用左边第一个元素作为轴元素的方法。
##### 分割策略1
###### 算法思想
首先用一个临时变量对首元素进行备份，取两个指针 left 和 right ，他们的初始值分别是待排序列的两端的下标，其中 left 指向最左边，right 指向最右边。在整个排序过程中保证 left 不大于 right ，用下面的方法不断移动两个指针:

1. 从 right 的位置向左搜索，找到第一个小于或等于轴的元素，移动到 left 的位置。
2. 再从 left 所指的位置向右搜索，找到第一个大于轴的元素，移动到 right 所指的位置。
3. 重复上面的过程，直到 left = right ，最后把轴元素放在 left 所指的位置。

经过上面的过程，所有大于轴的元素放在了轴的右边，小于轴的元素放在了轴的左边。


###### 代码实现
	int Partition(int *array,int left,int right){
	    int pivot = array[left];
	    while(left<right){
	        while (left<right&&array[right]>pivot) {
	            right--;
	        }
	        array[left] = array[right];
	        while (left<right&&array[left]<=pivot) {
	            left++;
	        }
	        array[right] = array[left];
	    }
	    array[left] = pivot;
	    return left;
	}

##### 分割策略2
###### 算法思想
分别从待排序序列两端相向扫描，从左边找到第一个大于轴的元素，从右边找到第一个小于轴的元素，然后交换二者。

然后把轴元素和 right 所指的元素交换。
###### 代码实现
	int Partition(int *array,int start,int end){
	    int pivot = array[start];
	    int left = start,right = end;
	    while (left<=right) {
	        while (left<=right&&array[left]<=pivot) {
	            left++;
	        }
	        while (right>=left&&array[right]>pivot) {
	            right--;
	        }
	        if(left<right){
	            swap(array[right], array[left]);
	            left++;right--;
	        }
	    }
	    swap(array[start], array[right]);
	    return right;
	}
##### 分治
###### 代码实现
	void QuickSort(int *array,int left,int right){
	    if(left<right){
	        int p = Partition(array, left, right);
	        QuickSort(array, left, p-1);
	        QuickSort(array, p+1, right);
	    }
	}
###### 复杂度分析

最好空间复杂度为 O(log n)， 最坏的空间复杂度为 O(n)。

### 3.选择排序
#### 3.1简单选择排序
###### 算法思想
简单选择排序算法是利用线性查找的方法从一个序列中找到最小的元素，即地 i 趟的排序操作为：通过 n-i 次关键字的比较，从 n-i+1 个元素中选出最小的元素，并和第 i-1 个元素交换。简单选择排序算法也称为直接选择排序算法。
###### 代码实现
	void SelectionSort(char*num,int n){
		char tmp;
		for(int i = 0;i<n-1;i++){
			int min = i;
			for(int j = i;j<n;j++){
				if(num[min]>num[j]){
					min = j;
				}
			}
			if(min!=i){
				tmp = num[i];
				num[i] = num[min];
				num[min] = tmp;
			}	
		}
	}
###### 复杂度分析
简单选择排序算法需要进行 n-1 趟操作，而且第 i 趟选择要进行 n-i 次比较，最多执行 1 次数据交换，最少进行 0 次，因此简单选择排序算法的时间效率是 O(n^2) 。简单选择排序算法比较次数较多，而移动次数较少。空间开销中，由于只需要使用一个临时变量来记录最小位置，因此空间负责度为 O(1) 。简单选择排序算法是不稳定的排序算法。
#### 3.2堆排序
###### 算法思想
以降序排序为例。

1. 将初始序列初始化为一个最大堆，初始化当前待排序列元素的个数 n 。
2. 将堆顶元素和当前最后一个元素交换，n = n-1;
3. 调整堆结构
4. 如果当前待排序列元素个数 n>1 则重复步骤 2），3）。

###### 代码实现
	void SiftDown(int *array,int i,int n){
		    int left = 2*i+1,right = 2*i+2,min = i;
		    if(left<n&&array[min]<array[left]){
		        min = left;
		    }
		    if(right<n&&array[min]<array[right]){
		        min = right;
		    }
		    if(min!=i){
		        int t = array[min];
		        array[min] = array[i];
		        array[i] = t;
		        SiftDown(array, min, n);
		    }
	}
	void BuildHeap(int *array,int n){
	    int p = n/2-1;
	    for(int i = p;i>=0;i--){
	        SiftDown(array, i, n);
	    }
	}
	void HeapSort(int *array,int n){
	    BuildHeap(array,n);
	    for(int i = n-1;i>0;i--){
	        int t = array[0];
	        array[0] = array[i];
	        array[i] = t;
	        SiftDown(array, 0, i);
	    }
	}
###### 复杂度分析
对于调整最大堆的操作 SiftDown 来说，最多执行 O(log2n) 次数据元素的交换，初始化堆的时间复杂度为 O(n) 。堆排序中一共调用了 n-1 次 SiftDown 操作。以及一次初始化操作，所以堆排序的时间复杂度为 O(log2n) 。排序过程中只需要，临时变量来进行交换操作，故空间开销为   O(1) 。堆排序算法是不稳定的算法。当数据量较大的时候堆排序的效率体现得很明显，在小数据集上，堆排序算法的优势并不明显。
### 4.基数排序
###### 算法思想
###### 代码实现
	//MergeSort
	//array是待归并数组，
	//其中对 array[start,mid] 和 array[mid+1,end]
	//之间的数据进行合并
	void Merge(int *array,int start,int mid,int end){
	    int len1 = mid-start+1;
	    int len2 = end-mid;
	    int i,j,k;
	    int *left = new int[len1];//临时用数组来存放 array[start,mid]的数据
	    int *right = new int[len2];//临时用数组来存放 array[mid+1,end]
	    for(i = 0;i<len1;i++){
	        left[i] = array[i+start];
	    }

	    for(i = 0;i<len2;i++){
	        right[i] = array[i+mid+1];
	    }
	
	    i = 0;j = 0;//执行归并
	    for(k = start;k<=end;k++){
	        if(i==len1 || j==len2){
	            break;
	        }
	        if(left[i]<=right[j]){
	            array[k] = left[i++];
	        }
	        else{
	            array[k] = right[j++];
	        }
	    }
	    
	    while (i<len1) {
	        array[k++] = left[i++];
	    }
	    while (j<len2) {
	        array[k++] = right[j++];
	    }
	    delete [] left;
	    delete [] right;
	}
	void MergeSort(int *array,int start,int end){
	    if(start<end){
	        int mid = (start+end)/2;
	        MergeSort(array, start, mid);
	        MergeSort(array, mid+1, end);
	        Merge(array, start, mid, end);
	    }
	}
###### 复杂度分析

### 5.归并排序

	###### 算法思想
	###### 代码实现
	//基数排序
	int  getMaxBit(int *array,int n){//得到元素序列中最大数的位数
	    int max = 1;
	    int k = 10;
	    for(int i = 0;i<n;i++){
	        while(array[i]>=k){
	            k*=10;
	            max++;
	        }
	    }
	    return max;
	}
	void RadixSort(int *array,int size)
	{
	    int n;
	    int max = getMaxBit(array, size);
	    int maxNum = 1;
	    for(int i = 1;i<max;i++){
	        maxNum *=10;
	    }
	    for(int i=1;i<=maxNum;i=i*10)
	    {
	        int tmp[15][10]={0};//分配操作:建立一个15行，10列的数组，每一列分别代表0~9位数，15行代表能存放的总个数
	        for(int j=0;j<size;j++)
	        {
	            n=(array[j]/i)%10;
	            tmp[j][n]=array[j];
	        }
	        int k=0;//收集操作：将二维数组中的数据自左至右、自上至下收集到数组中
	        for(int p=0;p<10;p++)
	            for(int q=0;q<size;q++)
	            {
	                if(tmp[q][p]!=0)
	                    array[k++]=tmp[q][p];
	            }
	    }
	}
###### 复杂度分析