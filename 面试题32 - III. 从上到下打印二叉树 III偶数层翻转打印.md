### 题目描述   
请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。
   
### 例如:   
给定二叉树: [3,9,20,null,null,15,7],

          3
         / \
        9  20
          /  \
         15   7     

返回其层次遍历结果：     
[   
  [3],   
  [20,9],   
  [15,7]   
]   

###解答：   
```java

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {//跟II类似，但是要把偶数层翻转，用i=res.size()判断奇偶数层，若i为奇数，说明已有奇数个层，
//下一层便是偶数层，需要翻转列表tmp，若i为偶数，说明下一层为奇数层，不用修改tmp
    public List<List<Integer>> levelOrder(TreeNode root) {
        LinkedList<TreeNode> queue=new LinkedList<TreeNode>();
        List<List<Integer>> res=new ArrayList<>();
        if(root!=null) queue.add(root);
        int k=1;int i=1;
        while(!queue.isEmpty()){
            ArrayList<Integer> tmp=new ArrayList<Integer>();
            while(k>0){//直到这一层的节点（队列里的）全部处理完，每一个的处理过程是把它的左右子节点放进队列
                TreeNode nroot=queue.poll();
                tmp.add(nroot.val);k--;
                if(nroot.left!=null) queue.add(nroot.left);
                if(nroot.right!=null) queue.add(nroot.right);
            } 
            if(i%2==1) {
                Collections.reverse(tmp);//反转列表
            }
            res.add(tmp);
            k=queue.size();//k为下一层节点数量
            i=res.size();//i为当前层的奇偶
        }
        return res;
    }
}
```   
```java 
class Solution {//双端队列，LinkedList也实现了deque接口
    public List<List<Integer>> levelOrder(TreeNode root) {
        LinkedList<TreeNode> queue=new LinkedList<TreeNode>();
        List<List<Integer>> res=new ArrayList<>();
        if(root!=null) queue.add(root);
        int k=1;int i=1;
        while(!queue.isEmpty()){
            LinkedList<Integer> tmp=new LinkedList<Integer>();//LinkedList是用双向链表结构存储数据的，很适合数据的动态插入和删除，随机访问和遍历速度比较慢。LinkedList实现了Deque和Queue接口，可以按照队列、栈和双端队列的方式进行操作
            if(i%2==0){//奇数层，不用管
            while(k>0){
                TreeNode nroot=queue.poll();
                tmp.add(nroot.val);k--;
                if(nroot.left!=null) queue.add(nroot.left);
                if(nroot.right!=null) queue.add(nroot.right);
            } 
            }
            if(i%2==1) {//偶数层，将队列中的节点入栈，即先进后出
               while(k>0){
                   TreeNode nroot=queue.poll();
                   tmp.addFirst(nroot.val);k--;//像栈一样插到前面
                   if(nroot.left!=null) queue.add(nroot.left);
                   if(nroot.right!=null) queue.add(nroot.right);
               }
            }
            res.add(tmp);
            k=queue.size();//k为下一层节点数量
            i=res.size();//i为当前层的奇偶
        }
        return res;
    }
}
```    
### 复杂度分析：   
O(N).
