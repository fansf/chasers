# 常见排序算法-liu  

## 常用排序算法性能比较
![](_v_images/_1517402113_13632.png)

不稳定的排序算法：快、选、希、堆。  
排序稳定性是指，通俗地讲就是能保证排序前2个相等的数其在序列的前后位置顺序和排序后它们两个的前后位置顺序相同。在简单形式化一下，如果Ai = Aj，Ai原来在位置前，排序后Ai还是要在Aj位置前。  
对于数组比较有序的情况，插入的效率比较好，对于数组特别混乱的情况，快排很好。  
堆排序、归并排序最坏情况都不会超过O(nlogn)；  


## 直接插入排序

基本思想：把一个记录直接插入到已经排好序的有序表中，从而得到一个新的，记录数增1的有序表。  
![直接插入排序](_v_images/_直接插入排序_1517405202_19854.gif)
```cpp
void StrInserSort(int *p, int length)  
{  
    int i, j;  
    for (i =1; i <length; i++)  
    {  
        if ( p[i]<p[i-1])  
        {  
          int tmp;  
          tmp = p[i];  
          for (j = i - 1; p[j] > tmp; j--)  
             p[j+1] = p[j];  
          p[j+1] = tmp;  
        }  
    }  
}  
```
插入排序的时间复杂度最好的情况是已经是正序的序列，只需比较(n-1)次，时间复杂度为O(n)，最坏的情况是倒序的序列，要比较n(n-1)/2次，时间复杂度为O(n^2 ) ，平均的话要比较时间复杂度为O(n^2 )。  

插入排序是一种稳定的排序方法，排序元素比较少的时候很好，大量元素便会效率低下。  

## shell排序

基本思想：把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止。   
![shell排序算法](_v_images/_shell排序算法_1517404482_20079.png)
```cpp
void ShellSort(int *p, int length)  
{  
    int i, j;  
    int increment = length;  
    do {  
         increment = increment / 3 +1;  
         for (i = increment ; i <= length - 1; i++)  
        {  
              if (p[i] < p[i - increment])  
              {  
                int tmp;  
                tmp = p[i];  
                for (j = i - increment; j >= 0 && tmp < p[j]; j -= increment)  
                  p[j + increment] = p[j];  
                p[j + increment] = tmp;  
              }  
            }  
    } while (increment > 1);  
}  
```

## 直接选择排序

基本思想：通过n-1次关键字间的比较，从n-i+1个记录中选择最小的记录，并和第i次(1≤i≤n)记录交换。    
```cpp
void SimSelSort(int *p, int length)  
{  
    int i, j, min;  
    for (i = 0; i < length - 1; i++)  
    {  
        min = i;                     //记录最小值的下标，初始化为i  
        for (j=i+1;j<=length-1;j++)  
        {  
            if (p[j] < p[min])  
                min = j;            //通过不断地比较，得到最小值的下标  
        }  
        swap(p[i], p[min]);  
    }  
}  
```

## 堆排序

基本思想：将待排序的序列构造成一个大顶堆。此时，整个序列的最大值就是堆顶的根结点。将它移走（其实就是将它与堆数组的末尾元素交换，此时末尾元素就是最大值），然后将剩余n-1个序列重新构造成一个堆，这样就会得到n个元素的次小值。如此反复执行，便能得到一个有序序列。  
![堆排序](_v_images/_堆排序_1517404714_30243.gif)
```cpp
//构造最大堆  
void MaxHeapFixDown(int *p, int i, int length)   
{  
    int j = 2 * i + 1;  
    int temp = p[i];  
    while (j<length) {  
        if (j + 1<length && p[j]<p[j + 1])  
            ++j;  
        if (temp>p[j])  
            break;  
        else {  
            p[i] = p[j];  
            i = j;  
            j = 2 * i + 1;  
        }  
    }  
    p[i] = temp;  
}  

//堆排序
void HeapSort(int *p, int length) {  
    for (int i = length / 2 - 1; i >= 0; i--)  
    {  
        MaxHeapFixDown(p, i, length);  
    }  
    for (int i = length - 1; i >= 1; i--)  
    {  
        swap(p[i], p[0]);  
        MaxHeapFixDown(p, 0, i);  
        cout << "i的值：" << i << " 排序：";  
        ergodic(p, 9);  
    }  
}  
```

## 冒泡排序  

基本思想：两两比较相邻记录的关键字，如果反序则交换，直到没有反序的记录为止。  
![冒泡排序](_v_images/_冒泡排序_1517404930_10465.gif)
```cpp
void BubbleSort(int *p, int length)  
{  
    for (int i = 0; i < length-1; i++)  
    {  
        for (int j =length-1; j>=i;j--)  
        {  
            if (p[j-1] > p[j])  
            {  
                swap(p[j-1], p[j]);  
            }  
        }  
    }  
}  
```

## 快速排序

基本思想：通过一趟排序将待排记录分割成独立的两部分，其中一部分记录的关键字均比另外一部分记录的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序的目的。   
![快速排序](_v_images/_快速排序_1517405002_31499.gif)
```cpp
void QuickSort(int *p, int l, int r)  
{  
    if (l< r)  
    {  
        int i = l, j = r, x = p[l];  
        while (i < j)   
        {  
            while (i < j && p[j] >= x) // 从右向左找第一个小于x的数   
                j--;  
            if (i < j)  
                p[i++] = p[j];  
            while (i < j && p[i]< x) // 从左向右找第一个大于等于x的数  
                i++;  
            if (i < j)  
                p[j--] = p[i];  
        }  
        p[i] = x;  
        QuickSort(p, l, i - 1); // 递归调用  
        QuickSort(p, i + 1, r);  
    }  
}  
```

## 归并排序
基本思想：将若干个序列进行两两归并，直至所有待排序记录都在一个有序序列为止。  
![归并排序](_v_images/_归并排序_1517405419_8813.gif)
```cpp
void Merge(int arr[], int reg[], int start, int end) {  
    if (start >= end)return;  
    int len = end - start, mid = (len >> 1) + start;  
    //分成两部分  
    int start1 = start, end1 = mid;  
    int start2 = mid + 1, end2 = end;  
    //然后合并
    Merge(arr, reg, start1, end1);  
    Merge(arr, reg, start2, end2);  
    int k = start;  
    //两个序列一一比较,哪的序列的元素小就放进reg序列里面,然后位置+1再与另一个序列原来位置的元素比较
    //如此反复,可以把两个有序的序列合并成一个有序的序列  
    while (start1 <= end1 && start2 <= end2)  
        reg[k++] = arr[start1] < arr[start2] ? arr[start1++] : arr[start2++];  

    //然后这里是分情况,如果arr2序列的已经全部都放进reg序列了然后跳出了循环  
    //那就表示arr序列还有更大的元素(一个或多个)没有放进reg序列,所以这一步就是接着放  
    while (start1 <= end1)  
        reg[k++] = arr[start1++];  

    //这一步和上面一样  
    while (start2 <= end2)  
        reg[k++] = arr[start2++];  
    //把已经有序的reg序列放回arr序列中  
    for (k = start; k <= end; k++)  
        arr[k] = reg[k];  
}  
void MergeSort(int arr[], const int len) {  
    //创建一个同样长度的序列,用于临时存放  
    int  reg[len];  
    Merge(arr, reg, 0, len - 1);  
}  

```

## 通排序
基本思想：设待排序序列的元素取值范围为0到m，则我们新建一个大小为m+1的临时数组并把初始值都设为0，遍历待排序序列，把待排序序列中元素的值作为临时数组的下标，找出临时数组中对应该下标的元素使之+1；然后遍历临时数组，把临时数组中元素大于0的下标作为值按次序依次填入待排序数组，元素的值作为重复填入该下标的次数，遍历完成则排序结束序列有序。   
```cpp
void BucketSort(int intArray[], int Count_intArray, int max)  
{  
    //待排序序列intArray的元素都是非负整数  
    //设待排序序列intArray的元素有Count_intArray个  
    //其取值范围为0到max，则我们新建一个大小为max+1的临时数组并把初始值都设为0  
    //此处是开辟max+1而不是max，因为比如给909,...0排序，是有1000个数，需要开辟999+1长度的数组  
    int *TmpArray = (int*)malloc(sizeof(int)*(max+1));//开辟一个新数组，这个数组即为桶；  
    for (int *p = TmpArray; p < TmpArray + max+1 ; p++) *p = 0;//初始化桶，是桶中的每个元素为0；  
    for (int *p = intArray; p < intArray + Count_intArray; p++) TmpArray[*p] += 1;///将TmpArray下标中等于intArray[i]的元素+1  
    int InsertIndex = 0;//指向intArray的指标  
    for (int j=0; j < max + 1; j++)  
    {  
        for (int k = 1; k <= TmpArray[j]; k++)//需要插入的元素的个数必须>1  
            intArray[InsertIndex++] = j;  
    }  
    free(TmpArray);  
}  
```
桶排序是一种效率很高的排序算法，它的时间复杂度为O(N+M)，(N个元素，范围为0--M)，但桶排序有一定的限制，必须为非负整数，而且元素不宜过大。  

    所有图片均来自百度百科
