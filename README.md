# warm up

- 注意溢出

```
// n = Integer.MIN_VALUE;
n = -n;

```

- 快排
```
private int partition(int[] nums, int left, int right) {
    int tmp = nums[left];
    while(left < right) {
        while(left<right && nums[right]>=tmp) {
            right--;
        }
        nums[left] = nums[right];
        while(left<right && nums[left]<=tmp) {
            left++;
        }
        nums[right] = nums[left];
    }
    nums[left] = tmp;
    return left;
}
```
- 快排的优化版本

```

```
- 树的先序遍历

1. 递归版
```java
/**
 * TreeNode: left, right, val
 *
 **/
public void preOrder(TreeNode root) {
    if(root == null) {
        return;
    }
    System.out.println(root.val);
    preOrder(root.left);
    preOrder(root.right);
}
```

2. 非递归版

```java
public void preOrder(TreeNode root) {
    if(root == null) {
        return;
    }
    Stack<TreeNode> stack = new Stack();
    stack.push(root);
    while(!stack.isEmpty()) {
        TreeNode node = stack.pop();
        System.out.println(node.val);
        if(node.right != null) {
            stack.push(node.right);
        }
        if(node.left != null) {
            stack.push(node.left);
        }
    }
}
```

- 树的中序遍历

1. 递归版
```java
/**
 * TreeNode: left, right, val
 *
 **/
public void midOrder(TreeNode root) {
    if(root == null) {
        return;
    }
    midOrder(root.left);
    System.out.println(root.val);
    midOrder(root.right);
}
```
2. 非递归版
```java
public void midOrder(TreeNode root) {
    if(root == null) {
        return;
    }
    Stack<TreeNode> stack = new Stack();
    stack.push(root);
    Set<TreeNode> meet = new HashSet<>();
    while(!stack.isEmpty()) {
        TreeNode node = stack.pop();
        if(meet.contains(node)) {
            System.out.println(node.val);
            continue;
        }
        if(node.right != null) {
            stack.push(node.right);
        }
        if(!meet.contains(node)) {
            stack.push(node);
            meet.add(node);
        }
        if(node.left != null) {
            stack.push(node.left);
        }
    }
}
```

- 树的后序遍历

1. 递归版
```java
/**
 * TreeNode: left, right, val
 *
 **/
public void postOrder(TreeNode root) {
    if(root == null) {
        return;
    }
    postOrder(root.left);
    postOrder(root.right);
    System.out.println(root.val);
}
```
2. 非递归版
```java
public void postOrder(TreeNode root) {
    if(root == null) {
        return;
    }
    Stack<TreeNode> stack = new Stack();
    stack.push(root);
    Set<TreeNode> meet = new HashSet<>();
    while(!stack.isEmpty()) {
        TreeNode node = stack.peek();
        if(meet.contains(node)) {
            stack.pop();
            System.out.println(node.val);
            continue;
        } else {
            meet.add(node);
        }
        if(node.right != null) {
            stack.push(node.right);
        }
        if(node.left != null) {
            stack.push(node.left);
        }
    }
}
```

#### 根据遍历序列重建二叉树

- 广度遍历二叉树

- 递归版
```
public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans=new ArrayList<>();
        compute(ans,root,0);
        return ans;
}

public void compute(List<List<Integer>> ans,TreeNode curr,int level) {
    if(curr==null) return;
    
    if(ans.size()==level) 
        ans.add(new ArrayList<Integer>());
    
    ans.get(level).add(curr.val);
    
    compute(ans,curr.left,level+1);
    compute(ans,curr.right,level+1);
}
```

- 非递归版
```
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<List<Integer>>();
        List<TreeNode> nodes = new ArrayList<TreeNode>();
        nodes.add(root);
        
        int l = 0;
        while(nodes.size() > 0) {
            List<Integer> line = new ArrayList<Integer>();
            List<TreeNode> next = new ArrayList<TreeNode>();
            for(TreeNode n : nodes) {
                if(n != null) {
                    next.add(n.left);
                    next.add(n.right);
                    
                    line.add(n.val);
                }
            }
            if(line.size() > 0)
                ret.add(line);
            
            nodes = next;
            ++l;
        }
        
        return ret;
    }
```

#### 二分查找

- 递归版

```
public static int binarySearch(int[] arr, int start, int end, int hkey){
    if (start > end)
        return -1;
    
    // 防止溢位
    // int mid = (start+end)/2;// 这种写法，start+end 的值可能大于 Integer.MAX_VALUE;
    int mid = start + (end - start)/2;    
    if (arr[mid] > hkey)
        return binarySearch(arr, start, mid - 1, hkey);
    if (arr[mid] < hkey)
        return binarySearch(arr, mid + 1, end, hkey);
    return mid;  

}
```

- 非递归版

```
public static int binarySearch(int[] arr, int start, int end, int hkey){
    int result = -1;

    while (start <= end){
        int mid = start + (end - start)/2;    //防止溢位
        if (arr[mid] > hkey)
            end = mid - 1;
        else if (arr[mid] < hkey)
            start = mid + 1;
        else {
            result = mid ;  
            break;
        }
    }

    return result;

}
```

- 二分查找：nums[i] < target <= nums[i+1]

```java
public int binarySearch(int[] nums, int left, int right, int target) {
    while(left < right) {
        int mid = left+(right-left)/2;
        if(nums[mid] < target) {
            left = mid+1;
        } else {
            right = mid;
        }
    }
    // nums[left] >= target > nums[left-1]
    return left;
}
```

- 大小堆

使用一维数组来存储堆，其中下标为 i 的节点其左子树下标为 2*i+1，其右子树下标为 2*i+2，root 节点下标为 1。

1. 先初始化堆，即将元素填充入数组中；
2. 调整堆，使其成为最大堆或最小堆

从第一个非叶子结点（也就是下标为 arr.length/2-1）从下至上，从右至左调整结构。

```java
import java.util.Arrays;

/**
 * https://www.cnblogs.com/chengxiao/p/6129630.html
 *
 **/
public class HeapSort {
    public static void main(String[] args){
        int []arr = {9,8,7,6,5,4,3,2,1};
        sort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void sort(int[] arr){
        //1.构建大顶堆
        for(int i=arr.length/2-1; i>=0; i--){
            //从第一个非叶子结点从下至上，从右至左调整结构
            adjustHeap(arr, i, arr.length);
        }
        //2.调整堆结构+交换堆顶元素与末尾元素
        for(int j=arr.length-1; j>0; j--){
            swap(arr, 0, j);//将堆顶元素与末尾元素进行交换
            adjustHeap(arr, 0, j);//重新对堆进行调整
        }
    }

    /**
     * 调整大顶堆（仅是调整过程，建立在大顶堆已构建的基础上）
     * @param arr
     * @param i
     * @param length
     */
    public static void adjustHeap(int[] arr, int i, int length){
        int temp = arr[i];//先取出当前元素i
        // 调整完当前节点后继续调整左右子树
        for(int k=i*2+1; k<length; k=k*2+1){//从i结点的左子结点开始，也就是2i+1处开始
            if(k+1<length && arr[k]<arr[k+1]){//如果左子结点小于右子结点，k指向右子结点
                // 后续调整右子树
                k++;
            }
            if(arr[k] > temp){//如果子节点大于父节点，将子节点值赋给父节点（不用进行交换）
                arr[i] = arr[k];
                i = k;
            } else {
                break;
            }
        }
        arr[i] = temp;//将temp值放到最终的位置
    }

    /**
     * 交换元素
     * @param arr
     * @param a
     * @param b
     */
    public static void swap(int[] arr, int a, int b){
        int temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }
}
```

# leetcode
LeetCode Recode

难度标识：

Easy：:blush:，Medium：:confused:，Hard：:pensive:

- [54 Spiral Matrix](./54.md) :confused:
- [454 4Sum II](./454.md) :confused:
- [11 Container With Most Water](./11.md) :confused:
- [41 First Missing Positive](./41.md) :pensive:
- [128 Longest Consecutive Sequence](./128.md) :pensive:
- [141 Linked List Cycle](./141.md) :blush:
- [142 Linked List Cycle II](./142.md) :confused:
- [287 Find the Duplicate Number](./287.md) :confused:
- [239 Sliding Window Maximum](./239.md) :pensive:
- [114 Flatten Binary Tree to Linked List](./114.md) :confused:
- [236 Lowest Common Ancestor of a Binary Tree](./236.md) :confused: