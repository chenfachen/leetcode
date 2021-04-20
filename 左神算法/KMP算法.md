# KMP算法

## 1.判断两个字符串是否互为旋转字符串，比如“123456”和“345612”

**思路**：首先判断两个子串长度是否相等进行过滤，不相等直接返回false。如果长度相等，将两个str1拼接成新的字符串，如果str2是新字符串的子串，那么就是互为旋转字符串


## 2. 判断A树是否为B树的子树（注意是子树不是子结构，就是某个节点开始，它的子节点完全一样）

**思路一**：（暴力法）
```java
    //big做头节点的树，其中是否有某颗子树的结构，是和small为头的树，完全一样的
    public static boolean containsTree(Node big, Node small){
        if(small == null){
            return true;
        }
        //small != null
        if(big == null){
            return false;
        }
        //big != null  small != null
        if(isSameValueStructure(big, small)){
            return true;
        }
        return containsTree(big.left, small) || containsTree(big.right, small);
    }

    //head1为头的树，是否在结构对应上，完全和head2一样
    public static boolean isSameValueStructure(Node head1, Node head2){
        if(head1 == null && head2 != null){
            return false;
        }
        if(head1 != null && head2 == null){
            return false;
        }
        if(head1 == null && head2 == null){
            return true;
        }
        if(head1.value != head2.value){
            return false;
        }
        //head1.value == head2.value
        return isSameValueStructure(head1.left, head2.left) && isSameValueStructure(head1.right, head2.right);
    }
```
