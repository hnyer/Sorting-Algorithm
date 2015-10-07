# Sorting-Algorithm
###java实现的常用排序算法
[查阅更多](http://note.youdao.com/share/?id=a016e22b423e02ec15d8f4a7da02a367&type=note) 
```java
/**
 * 1.插入排序算法
 * @param int[] 未排序数组
 * @return int[] 排完序数组
 *插入排序的基本思想是在遍历数组的过程中，假设在序号 i 之前的元素即 [0..i-1] 都已经排好序， 本趟需要找到 i
 *对应的元素 x 的正确位置 k ，并且在寻找这个位置 k 的过程中逐个将比较过的元素往后移一位， 为元素 x “腾位置”，最后将
 * k 对应的元素值赋为 x 一般情况下，插入排序的时间复杂度和空间复杂度分别为 O(n2 ) 和 O(1)
 */
public int[] sortInsert(int[] array) {
    for (int i = 1; i < array.length; i++) {
        int temp = array[i];
        int j;
        for (j = i - 1; j >= 0 && temp < array[j]; j--) {
            array[j + 1] = array[j];
        }
        array[j + 1] = temp;
    }
    return array;
}

/**
 * 2.选择排序算法
 * @param int[] 未排序数组
 * @return int[] 排完序数组
 *选择排序的基本思想是遍历数组的过程中，以 i 代表当前需要排序的序号，则需要在剩余的 [i…n-1] 中找出其中的最小值，
 *然后将找到的最小值与 i 指向的值进行交换。因为每一趟确定元素的过程中都会有一个选择最大值的子流程，所以人们形象地称之为选择排序。
 *选择排序的时间复杂度和空间复杂度分别为 O(n2 ) 和 O(1)
 */
public int[] sortSelect(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        int miniPost = i;
        for (int m = i + 1; m < arr.length; m++) {
            if (arr[m] < arr[miniPost]) {
                miniPost = m;
            }
        }
 
        if (arr[i] > arr[miniPost]) {
            int temp;
            temp = arr[i];
            arr[i] = arr[miniPost];
            arr[miniPost] = temp;
        }
    }
    return arr;
}

/**
 * 3.冒泡排序算法
 * @param int[] 未排序数组
 * @return int[] 排完序数组
 * 冒泡排序是將比較大的數字沉在最下面，较小的浮在上面
 */
public int[] sortBubble(int[] array) {
    int temp;
    // 第一层循环:表明比较的次数, 比如 length 个元素,比较次数为 length-1 次（肯定不需和自己比）
    for (int i = 0; i < array.length - 1; i++) {
        for (int j = array.length - 1; j > i; j--) {
            if (array[j] < array[j - 1]) {
                temp = array[j];
                array[j] = array[j - 1];
                array[j - 1] = temp;
            }
        }
    }
    return array;
}
    /**
     * 4.快速排序算法
     * @param int[] 未排序数组
     * @return int[] 排完序数组 通过一趟排序将待排记录分割成独立的两部分,其中一部分记录的关键字均比另一部分的关键字小,
     *则可以分别对这两部分记录继续进行排序,已达到整个序列有序的目的
     *本质就是,找一个基位(枢轴,分水岭,作用是左边的都比它小,右边的都比它大.可随机,取名base
     *首先从序列最右边开始找比base小的
     *,如果小,换位置,从而base移到刚才右边(比较时比base小)的位置(记为临时的high位),这样base右边的都比base大
     *然后,从序列的最左边开始找比base大的
     *,如果大,换位置,从而base移动到刚才左边(比较时比base大)的位置(记为临时的low位),这样base左边的都比base小
     *循环以上两步,直到 low == heigh, 这使才真正的找到了枢轴,分水岭.
     *返回这个位置,分水岭左边和右边的序列,分别再来递归
     */
    public int[] sortQuick(int[] array) {
        return quickSort(array, 0, array.length - 1);
    }
 
    private int[] quickSort(int[] arr, int low, int heigh) {
        if (low < heigh) {
            int division = partition(arr, low, heigh);
            quickSort(arr, low, division - 1);
            quickSort(arr, division + 1, heigh);
        }
        return arr;
    }
 
    // 分水岭,基位,左边的都比这个位置小,右边的都大
    private int partition(int[] arr, int low, int heigh) {
        int base = arr[low]; // 用子表的第一个记录做枢轴(分水岭)记录
        while (low < heigh) { // 从表的两端交替向中间扫描
            while (low < heigh && arr[heigh] >= base) {
                heigh--;
            }
            // base 赋值给 当前 heigh 位,base 挪到(互换)到了这里,heigh位右边的都比base大
            swap(arr, heigh, low);
            while (low < heigh && arr[low] <= base) {
                low++;
            }
            // 遇到左边比base值大的了,换位置
            swap(arr, heigh, low);
        }
        // now low = heigh;
        return low;
    }
 
    private void swap(int[] arr, int a, int b) {
        int temp;
        temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }

/**
 * 5.合并排序算法
 * @param int[] 未排序数组
 * @return int[] 排完序数组
 *归并排序采用的是递归来实现，属于“分而治之”，将目标数组从中间一分为二，之后分别对这两个数组进行排序，
 *排序完毕之后再将排好序的两个数组“归并”到一起，归并排序最重要的也就是这个“归并”的过程，
 *归并的过程中需要额外的跟需要归并的两个数组长度一致的空间
 */
private int[] sort(int[] nums, int low, int high) {
    int mid = (low + high) / 2;
    if (low < high) {
        // 左边
        sort(nums, low, mid);
        // 右边
        sort(nums, mid + 1, high);
        // 左右归并
        merge(nums, low, mid, high);
    }
    return nums;
}
 
private void merge(int[] nums, int low, int mid, int high) {
    int[] temp = new int[high - low + 1];
    int i = low;// 左指针
    int j = mid + 1;// 右指针
    int k = 0;
    // 把较小的数先移到新数组中
    while (i <= mid && j <= high) {
        if (nums[i] < nums[j]) {
            temp[k++] = nums[i++];
        } else {
            temp[k++] = nums[j++];
        }
    }
    // 把左边剩余的数移入数组
    while (i <= mid) {
        temp[k++] = nums[i++];
    }
    // 把右边边剩余的数移入数组
    while (j <= high) {
        temp[k++] = nums[j++];
    }
    // 把新数组中的数覆盖nums数组
    for (int k2 = 0; k2 < temp.length; k2++) {
        nums[k2 + low] = temp[k2];
    }
}
 
public int[] sortMerge(int[] array) {
    return sort(array, 0, array.length - 1);
}

/**
 * 6.希尔排序算法
 * @param int[] 未排序数组
 * @return int[] 排完序数组
 *希尔排序的诞生是由于插入排序在处理大规模数组的时候会遇到需要移动太多元素的问题。希尔排序的思想是将一个大的数组“分而治之”，
 *划分为若干个小的数组，以 gap 来划分，比如数组 [1, 2, 3, 4, 5, 6, 7, 8] ，如果以 gap = 2
 *来划分， 可以分为 [1, 3, 5, 7] 和 [2, 4, 6, 8] 两个数组（对应的，如 gap = 3 ，
 *则划分的数组为： [1, 4, 7] 、 [2, 5, 8] 、 [3, 6] ）然后分别对划分出来的数组进行插入排序，
 *待各个子数组排序完毕之后再减小 gap 值重复进行之前的步骤，直至 gap = 1 ，即对整个数组进行插入排序，
 *此时的数组已经基本上快排好序了，所以需要移动的元素会很小很小，解决了插入排序在处理大规模数组时较多移动次数的问题
 *希尔排序是插入排序的改进版，在数据量大的时候对效率的提升帮助很大，数据量小的时候建议直接使用插入排序就好了。
 */
public int[] sortShell(int[] array) {
    // 取增量
    int step = array.length / 2;
    while (step >= 1) {
        for (int i = step; i < array.length; i++) {
            int temp = array[i];
            int j = 0;
            // 跟插入排序的区别就在这里
            for (j = i - step; j >= 0 && temp < array[j]; j -= step) {
                array[j + step] = array[j];
            }
            array[j + step] = temp;
        }
        step /= 2;
    }
    return array;
}
/**
 * 7.堆排序算法
 * @param int[] 未排序数组
 * @return int[] 排完序数组
 *本质就是先构造一个大顶堆,parent比children大,root节点就是最大的节点
 *把最大的节点(root)与尾节点(最后一个节点,比较小)位置互换
 *剩下最后的尾节点,现在最大,其余的,从第一个元素开始到尾节点前一位,构造大顶堆递归
 */
public int[] sortHeap(int[] array) {
    buildHeap(array);// 构建堆
    int n = array.length;
    int i = 0;
    for (i = n - 1; i >= 1; i--) {
        swap(array, 0, i);
        heapify(array, 0, i);
    }
 
    return array;
}
 
private void buildHeap(int[] array) {
    int n = array.length;// 数组中元素的个数
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(array, i, n);
}
 
private void heapify(int[] A, int idx, int max) {
    int left = 2 * idx + 1;// 左孩子的下标（如果存在的话）
    int right = 2 * idx + 2;// 左孩子的下标（如果存在的话）
    int largest = 0;// 寻找3个节点中最大值节点的下标
    if (left < max && A[left] > A[idx])
        largest = left;
    else
        largest = idx;
    if (right < max && A[right] > A[largest])
        largest = right;
    if (largest != idx) {
        swap(A, largest, idx);
        heapify(A, largest, max);
    }
}
```
