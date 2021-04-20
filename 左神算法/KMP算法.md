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
时间复杂度O（m*n）


**思路二**（序列化+KMP）

```java
    public static boolean containsTree(Node big, Node small){
        if(small == null){
            return true;
        }
        if(big == null){
            return false;
        }
        ArrayList<String> b = preSerial(big);
        ArrayList<String> s = preSerial(small);
        String[] str = new String[b.size()];
        for(int i=0; i<str.length; i++){
            str[i] = b.get(i);
        }
        String[] match = new String[s.size()];
        for(int i=0; i<match.length; i++){
            match[i] = s.get(i);
        }
        return getIndexOf(str, match) != -1;
    }

    public static ArrayList<String> preSerial(Node head){
        ArrayList<String> ans = new ArrayList<>();
        pres(head, ans);
        return ans;
    }

    //序列化，要加上null
    public static void pres(Node head, ArrayList<String> ans){
        if(head == null){
            ans.add(null);
        }else{
            ans.add(String.valueOf(head.value));
            pres(head.left, ans);
            pres(head.right, ans);
        }
    }

    public static int getIndexOf(String[] str1, String[] str2){
        if(str1 == null || str2 == null || str1.length < 1 || str1.length < str2.length){
            return -1;
        }
        int x = 0;
        int y = 0;
        int[] next = getNextArray(str2);
        while (x < str1.length && y < str2.length){
            if(isEqual(str1[x], str2[y])){
                x++;
                y++;
            }else if(next[y] = -1){
                x++;
            }else {
                y = next[y];
            }
        }
        return y == str2.length ? x - y : -1;
    }

    public static int[] getNextArray(String[] ms){
        if(ms.length == 1){
            return new int[]{-1};
        }
        int[] next = new int[ms.length];
        next[0] = -1;
        next[1] = 0;
        int i = 2;
        int cn = 0;
        while (i < next.length){
            if(isEqual(ms[i-1], ms[cn])){
                next[i++] == ++cn;
            }else if(cn > 0){
                cn = next[cn];
            }else {
                next[i++] = 0;
            }
        }
        return next;
    }

    public static boolean isEqual(String a, String b){
        if(a == null && b == null){
            return true;
        }else {
            if(a == null || b == null){
                return false;
            }else {
                return a.equals(b);
            }
        }
    }
```
