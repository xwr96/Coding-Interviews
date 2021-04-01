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
