## 常见排序算法

### 直接插入排序
直接插入插排的基本思想是：当插入第i(i >= 1)时，前面的V[0]，V[1]，……，V[i-1]已经排好序。这时，用V[I]的值与V[i-1]，V[i-2]，…的值按顺序进行比较，当V[I]的值大于比较的值时，即找到插入位置将V[i]插入，
原来位置上的元素向后顺移。Java实现代码如下：
```Java
public void insertionSort(int[] nums) {
    for (int i = 1; i < nums.length; i++) {
        int insertNum = nums[i];
        int insertIndex;
        for (insertIndex = i - 1; insertIndex >= 0 && nums[insertIndex] > insertNum; insertIndex--) {
            nums[insertIndex + 1] = nums[insertIndex]; //每当前面的元素比待插入元素大，就向后移动
        }
        nums[insertIndex + 1] = insertNum; //当退出循环，表明已经找到了待插入位置，即index + 1
    }
}
```
直接插入没有利用到要插入的序列已有序的特点，插入第 i 个元素时可以通过二分查找找到插入位置 insertIndex，再把 i~insertIndex 之间的所有元素后移一位，把第 i 个元素放在插入位置上。
```Java
public void binaryInsertionSort(int[] nums) {
    for (int i = 1; i < nums.length; i++) {
        int insertNum = nums[i];
        int insertIndex = -1;
        int start = 0;
        int end = i - 1;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (insertNum > nums[mid])
                start = mid + 1;
            else if (insertNum < nums[mid])
                end = mid - 1;
            else {
                insertIndex = mid + 1;
                break;
            }
        }
        if (insertIndex == -1)
            insertIndex = start;
        if (i - insertIndex >= 0)
            System.arraycopy(nums, insertIndex, nums, insertIndex + 1, i - insertIndex);
        nums[insertIndex] = insertNum;
    }
}
```
## 希尔排序
又称缩小增量排序，是对直接插入排序的改进，不稳定，平均时间复杂度 O(n^1.3^)，最差时间复杂度 O(n²)，最好时间复杂度 O(n)，空间复杂度 O(1)。
希尔排序目的为了加快速度改进了插入排序，交换不相邻的元素对数组的局部进行排序，并最终用插入排序将局部有序的数组排序。
在此我们选择**增量 gap=length/2，缩小增量以 gap = gap/2 的方式**，用序列 {n/2,(n/2)/2...1} 来表示。java代码实现如下：
```Java
public void shellSort(int[] nums) {
  for(int b=nums.length/2;b>0;b/2){
    for(int i=b;i<nums.length;i++){
      int insertNum=nums[i];
      int insertIndex;
      for(insertIndex=i-b;insertIndex >= 0 && nums[insertIndex] > insertNum; insertIndex-=b){
        nums[insertIndex + d] = nums[insertIndex];
      }
      nums[insertIndex + d] = insertNum;
    }
  }
}
```
## 直接选择排序
不稳定，时间复杂度 O(n²)，空间复杂度 O(1)。

每次在未排序序列中找到最小元素，和未排序序列的第一个元素交换位置，再在剩余未排序序列中重复该操作直到所有元素排序完毕。
```Java
public void selectSort(int[] nums) {
  int minIndex;
  for(index=0;index<nums.length-1;index++){
    minIndex=index;
    for(i=index+1;i<nums.length;i++){
      if(nums[i]<nums[minIndex]){
        minIndex=i;
      }
      if(index!=minIndex){
        swap(nums,index,minIndex);//交换两个数字，简单实现
      }
    }
  }
}

```
## 堆排序
是对直接选择排序的改进，不稳定，时间复杂度 O(nlogn)，空间复杂度 O(1)。

将待排序记录看作完全二叉树，可以建立大顶堆或小顶堆，大顶堆中每个节点的值都不小于它的子节点值，小顶堆中每个节点的值都不大于它的子节点值。

以大顶堆为例，在建堆时首先将最后一个节点作为当前节点，如果当前节点存在父节点且值大于父节点，就将当前节点和父节点交换。在移除时首先暂存根节点的值，然后用最后一个节点代替根节点并作为当前节点，如果当前节点存在子节点且值小于子节点，就将其与值较大的子节点进行交换，调整完堆后返回暂存的值。
#### 建立堆函数
```Java
void heapify(int[] arr[],int n,int i){
    int largest=i; // 将最大元素设置为堆顶元素
    int l=(2*i)+1;
    int r=(2*i)+2;
    // 如果 left 比 root 大的话
    if(l<n && arr[l]>nums[largest]){
        largest=l;
    }
    // 如果 right 比 root 大的话
    if(r<n && arr[r]>nums[largest]{
        largest=r;
    }
    if (largest != i) 
    { 
        swap(arr[i], arr[largest]); 
  
        // 递归地定义子堆
        heapify(arr, n, largest); 
    } 
}
```
堆排序的方法如下，把最大堆堆顶的最大数取出，将剩余的堆继续调整为最大堆，再次将堆顶的最大数取出，这个过程持续到剩余数只有一个时结束。

堆排序函数：
```Java
void heapSort(int[] arr,int n){
    //建立堆
    for(int i=(n-1)/2;i>=0;i--){
        heapify(arr,n,i);
    }
    // 一个个从堆顶取出元素
    for(int i=n-1;i>=0;i--){
        swap(arr[0],arr[i]);
        heapify(arr,i,0);
    }
}
```

## 冒泡排序
稳定，平均/最坏时间复杂度 O(n²)，元素基本有序时最好时间复杂度 O(n)，空间复杂度 O(1)。

比较相邻的元素，如果第一个比第二个大就进行交换，对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对，每一轮排序后末尾元素都是有序的，针对 n 个元素重复以上步骤 n -1 次排序完毕。
```Java
public void bubbleSort(int[] nums){
    for(int i=0;i<nums.length;i++){
        for(int idex=0;index<nums.length-1-i;index++){
            if (nums[index] > nums[index + 1])
                swap(nums, index, index + 1);//交换数据
        }
    }
}
```
当序列已经有序时仍会进行不必要的比较，可以设置一个标志记录是否有元素交换，如果没有直接结束比较。
```Java
public void betterBubbleSort(int[] nums) {
    boolean swap;
    for (int i = 0; i < nums.length - 1; i++) {
        swap = true;
        for (int index = 0; index < nums.length - 1 - i; index++) {
            if (nums[index] > nums[index + 1]) {
                swap(nums, index ,index + 1);
                swap = false;
            }
        }
        if (swap) break;
    }
}
```

## 快速排序
是对冒泡排序的一种改进，不稳定，平均/最好时间复杂度 O(nlogn)，元素基本有序时最坏时间复杂度 O(n²)，空间复杂度 O(logn)。

首先选择一个基准元素，通过一趟排序将要排序的数据分割成独立的两部分，一部分全部小于等于基准元素，一部分全部大于等于基准元素，再按此方法递归对这两部分数据进行快速排序。

快速排序的一次划分从两头交替搜索，直到 low 和 high 指针重合，一趟时间复杂度 O(n)，整个算法的时间复杂度与划分趟数有关。

最好情况是每次划分选择的中间数恰好将当前序列等分，经过 log(n) 趟划分便可得到长度为 1 的子表，这样时间复杂度 O(nlogn)。

最坏情况是每次所选中间数是当前序列中的最大或最小元素，这使每次划分所得子表其中一个为空表 ，这样长度为 n 的数据表需要 n 趟划分，整个排序时间复杂度 O(n²)。

```Java
    private static void quickSort(int[] arr, int low, int high) {

		if (low < high) {
			// 找寻基准数据的正确索引
			int index = getIndex(arr, low, high);

			// 进行迭代对index之前和之后的数组进行相同的操作使整个数组变成有序
			quickSort(arr, low, index - 1);
			quickSort(arr, index + 1, high);
		}
	}

	private static int getIndex(int[] arr, int low, int high) {
		// 基准数据
		int tmp = arr[low];
		while (low < high) {
			// 当队尾的元素大于等于基准数据时,向前挪动high指针
			while (low < high && arr[high] >= tmp) {
				high--;
			}
			// 如果队尾元素小于tmp了,需要将其赋值给low
			arr[low] = arr[high];
			// 当队首元素小于等于tmp时,向前挪动low指针
			while (low < high && arr[low] <= tmp) {
				low++;
			}
			// 当队首元素大于tmp时,需要将其赋值给high
			arr[high] = arr[low];

		}
		// 跳出循环时low和high相等,此时的low或high就是tmp的正确索引位置
		// 由原理部分可以很清楚的知道low位置的值并不是tmp,所以需要将tmp赋值给arr[low]
		arr[low] = tmp;
		return low; // 返回tmp的正确位置
	}

```
## 归并排序

归并排序基于归并操作，是一种稳定的排序算法，任何情况时间复杂度都为 O(nlogn)，空间复杂度为 O(n)。

基本原理：应用分治法将待排序序列分成两部分，然后对两部分分别递归排序，最后进行合并，使用一个辅助空间并设定两个指针分别指向两个有序序列的起始元素，将指针对应的较小元素添加到辅助空间，重复该步骤到某一序列到达末尾，然后将另一序列剩余元素合并到辅助空间末尾。

适用场景：**数据量大且对稳定性有要求的情况。**
```Java
int[] help; //辅助数组，拷贝原数组到辅助数组

public void mergeSort(int[] arr) {
    int[] help = new int[arr.length];
    sort(arr, 0, arr.length - 1);
}

public void sort(int[] arr, int start, int end) {
    if (start == end) return;
    int mid = start + (end - start) / 2;
    sort(arr, start, mid);
    sort(arr, mid + 1, end);
    merge(arr, start, mid, end);
}

public void merge(int[] arr, int start, int mid, int end) {
    if (end + 1 - start >= 0) System.arraycopy(arr, start, help, start, end + 1 - start); //分治完之后开始慢慢合并-> 第一次合并 tempLeft = 0 , right = 1 //第二次： tempLeft = 2 right = 3 // 最后一次 tempLeft = 0 right = arr.length-1
    int p = start;
    int q = mid + 1;
    int index = start; //指向help数组的当前索引
    while (p <= mid && q <= end) {
    // 如果左边的有序序列的当前元素，小于等于右边有序序列的当前元素
    // 即将左边的当前元素，填充到 arr数组
    // 然后 index++, p++
        if (help[p] < help[q]) 
            arr[index++] = help[p++];
        else // 反之,将右边有序序列的当前元素，填充到arr数组
            arr[index++] = help[q++];
    }
    // 把有剩余数据的一边的数据依次全部填充到arr
    while (p <= mid) arr[index++] = help[p++]; // 左边的有序序列还有剩余的元素，就全部填充到arr
    while (q <= end) arr[index++] = help[q++]; // 右边的有序序列还有剩余的元素，就全部填充到arr
}
```
## 排序算法选择

数据量规模较小，考虑直接插入或直接选择。当元素分布有序时直接插入将大大减少比较和移动记录的次数，如果不要求稳定性，可以使用直接选择，效率略高于直接插入。

数据量规模中等，选择希尔排序。

数据量规模较大，考虑堆排序（元素分布接近正序或逆序）、快速排序（元素分布随机）和归并排序（稳定性）。

一般不使用冒泡。

## 排序算法稳定性判定
如果在一个待排序的序列中，存在2个相等的数，在排序后这2个数的相对位置保持不变，那么该排序算法是稳定的；否则是不稳定的。

##### 举个例子
对五个同学（A,B,C,D,E）进行成绩排序，他们的成绩分别为：90，88，79，88，92，按成绩从高到低排（92,90,88,88,79）：
```C#
E,A,B,D,C——稳定的（B,D的相对位置没有变化）
E,A,D,B,C——不稳定的（B,D的相对位置发生了变化）
```
- 稳定的排序算法：冒泡排序、直接插入排序、归并排序、基数排序
- 不稳定的排序算法：选择排序、快速排序、希尔排序、堆排序
