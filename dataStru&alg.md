### 线性结构（线性表）

1. 顺序存储结构（顺序表）

   物理空间连续，随机查询效率高。

2. 链式存储结构（链表）

   物理空间不连续，存储相邻元素的地址，增删改效率高。

### 栈

1. 先进后出

2. 栈顶、栈底

3. pop弹栈、push压栈、peek获取栈顶

4. 数组：静态栈、链表：动态栈

5. 应用：完成表达式的计算

   一个数字栈，一个符号栈

   如果符号栈是空，直接入栈。

   如果符号栈中不为空，比较该符号与站内符号优先级，如果该符号优先级高，压栈，如果该符号优先级低或等于，弹出栈内符号运算。

### 堆

在顺序表中存储完全二叉树，左子树的下标是2n，右子树的下标是2n+1。

完全二叉树：如果从上到下，从左到右依次编号，每个节点的编号和满二叉树的编号相同，则为完全二叉树。

### 队列

1. 先进先出
2. 队头front，队尾rear
3. 防止混淆真假溢出，可以规定队尾比最大值小1为满队列。
3. 方法：poll, add, remove, peek

### 单向环形链表（约瑟夫问题）

设编号为1，2...，n的n个人围坐一圈，约定编号为k（1<=k<=n）的人从1开始报数，数到m的那个人出列，它的下一位，又从1开始报数，数到m的那个人出列，它的下一位又从1开始报数，数到m的那个人又出列，一次类推，知道所有人出列为止，由此产生一个出队编号的序列。

### 稀疏数组

将二维数组中的数字，放到一个顺序表中，第0行记录行数列数和数字的个数，其余行记录二维数组中所有数字所在的坐标和数字大小。（同时也存在链式存储）（从0行0列开始算）

### 递归

```java
//小球迷宫递归
public static boolean isRun(int[][] map,int i,int j){
    if(map[6][5]==2){
        return true;
    }else{
        if(map[i][j]==0){
            map[i][j] = 2;
            
            if(isRun(map,i+1,j)){
                return true;
            }else if(isRun(map,i,j+1)){
                return true;
            }else if(isRun(map,i-1,j)){
                return true;
            }else if(isRun(map,i,j-1)){
                return true;
            }else{
                map[i][j] = 3;
                return false;
            }
        }else{
            return false;
        }
    }
}
```

### 算法时间效率

1. 事后统计方法

   不可靠

2. 事前估算方法

   时间复杂度

3. 时间频度

   一个算法花费的时间与算法中语句执行次数成正比，次数越多，花费时间越多。

   一个算法中语句执行次数称之为语句频度或时间频度。记为T(n)

4. 时间复杂度

   常对幂指阶

   平均时间复杂度：所有可能的输入实例均以等概率的出现情况下得到算法的运行时间

   最坏时间复杂度：一般讨论的都是最坏情况下的复杂度，以此作为界限。

### 排序

1. 基数排序

   思想：将整数按位数切割成不同的数字，然后按每个位数分别比较。从地位到高位以此排序。

   时间复杂度：O (nlog(x)m)
   
   ```java
   public int[] sort(int[] array){
           //先计算出数组中最大数字的长度
           int max = 0;
           for(int i = 0; i < array.length; i++){
               if(array[i] > max){
                   max = array[i];
               }
           }
           int maxLength = (max + "").length();
   
           //除数
           int times = 1;
           for(int i = 0; i < maxLength; i++){
               //准备10个工具桶
               int[][] util = new int[10][maxLength];
               //准备一个辅助数组，用于计数
               int[] counter = new int[10];
               //先将数组按照相应位数在10个桶中排序
               for(int j = 0; j < array.length; j++){
                   int temp = array[j] / times % 10;
                   util[temp][counter[temp]++] = array[j];
               }
               //将相应位数排序好的数字放入原数组
               int count = 0;
               for(int j = 0; j < 10; j++){
                   for(int k = 0; k < counter[j]; k++){
                       array[count++] = util[j][k];
                   }
               }
               //除数乘10，用于排序下一位
               times *= 10;
           }
           //最大位数已排好序，可以返回
           return array;
       }
   ```
   
   

2. 冒泡排序

   思想：通过对数组从前往后依次比较相邻元素值，若发现逆序则交换，使值较大的元素从前往后逐步向后移动。

   ```java
   public int[] sort(int[] array){
           int temp;
           for(int i = 0; i < array.length - 1; i++){
               for(int j = 0; j < array.length - i -1; j++){
                   if(array[j] > array[j+1]){
                       temp = array[j+1];
                       array[j+1] = array[j];
                       array[j] = temp;
                   }
               }
           }
           return array;
   }
   ```

   

3. 快速排序

   思想：快速排序是对冒泡排序的一种改进，通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的数据都比另一部分所有的数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序的过程可以递归进行，依次达到整个数据变成有序序列。

   ```java
   public static void sort(int[] array, int l, int r){
           //如果左大于等于右，表明该部分已完成排序
           if(l >= r){
               return;
           }
           //因为左右参数后面还需要用到， 所以备份
           int left = l;
           int right = r;
           //将枢轴备份
           int pivot = array[left];
           //当左不小于右时，表明该枢轴左右都满足要求
           while(left < right){
               //从右往左找一个比枢轴大的数
               while(left < right && array[right] >= pivot){
                   right--;
               }
               //最开始枢轴已备份，所以可以覆盖
               array[left] = array[right];
               while(left < right && array[left] <= pivot){
                   left++;
               }
               array[right] = array[left];
           }
           //出循环后，左等于右，表明该位置为枢轴位置，左右都已经满足要求
           array[left] = pivot;
           //迭代枢轴左半部分和右半部分
           sort(array, l, left - 1);
           sort(array, left + 1, r);
       }
   ```

   

4. 插入排序

   思想：属于内部排序，从第二个元素开始，从左往右，寻找每个元素的位置，并插入，以达到排序的目的。

   时间复杂度：O(n^2)

   ```java
   public static int[] sort(int[] array){
           int temp;
           for(int i = 1; i < array.length; i++){
               int j = i;
               while(j > 0 && array[j-1] > array[j]){
                   temp = array[j];
                   array[j] = array[j-1];
                   array[j-1] = temp;
                   j--;
               }
           }
           return array;
       }
   ```

   

5. 选择排序

   属于内部排序，从前往后，给每个位置选择当前元素中最小的，以此排序。

   ```java
   public static int[] sort(int[] array){
           for(int i = 0; i < array.length; i++){
               int minIndex = i;
               int temp;
               for(int j = i; j < array.length; j++){
   
                   if(array[j] < array[minIndex]){
                       minIndex = j;
                   }
               }
               temp = array[i];
               array[i] = array[minIndex];
               array[minIndex] = temp;
           }
           return array;
       }
   ```

   

6. 希尔排序

   思想：是插入排序的一种，又称“缩小增量排序”，相对稳定。初始增量设置为数组长度的一半，使1和1+L/2为一组，2和2+L/2为一组...以插入排序的方式为每一个组排序，然后逐渐减小增量（减半），直到增量变为1，使所有数字变为一组进行插入排序。

   ```java
   void ShellSort(int a[]){
       int length = a.length;
       int d, i, j;
       //d为增量
       for(d = length / 2; d >= 1; d = d / 2){
           //i从第一组第二个元素开始，不同组之间交替进行，
           //直到最后一组最后一个元素
           for(i = d; i < length; i++){
               //如果比前面的小，说明需要插入排序
               if(a[i] < a[i - d]){
                   int temp = a[i];
                   //从前一个元素往前找，直到找到比i小的
                   for(j = i - d; j >= 0 && temp < a[j]; j = j - d){
                       a[j + d] = a[j];
                   }
                   a[j + d] = temp;
               }
           }
       }
   }
   ```
   
   

7. 归并排序

   将两个已经排好序的数组进行合并。（时间复杂度为nlog2n）n为每次，log2n为树高

   ```java
   void merge(int[] a, int low, int mid, int high){
       int i, j, k;
       int[] b = new int[a.length];
       
       for(i = low; i <= high; i++){
           b[i] = a[i];
       }
       
       for(i = low, j = mid + 1, k = low; i <= mid && j <= high; k++){
           if(b[i] <= b[j]){
               a[k] = b[i++];
           }else{
               a[k] = b[j++];
           }
           while(i < mid){
               a[k++] = b[i++];
           }
           while(j < mid){
               a[k++] = b[j++];
           }
       }
   }
   
   void mergeSort(int[] a, int low, int high){
       if(low < high){
           mid = (low + high) / 2;
           mergeSort(a, low, mid);
           mergeSort(a, mid + 1, high);
           merge(a, low, mid, high);
       }
   }
   ```

8. 二分查找

   思想：先对数组进行排序，使用三个整形指针，分别指向最小，中间，最大。当中间大于要查找的值，最大指向中间-1，中间重新计算......

### 哈希表

1. 属于线性结构
2. 根据关键码值（Key value）而直接进行访问的数据结构。通过把关键码值映射到表中的一个位置来访问记录，以加快查找的速度。
3. 哈希碰撞：
   1. 开发地址法：碰撞，即放到该地址后一个地址内。
   2. 拉链法：数组结合链表

### 树

1. 二叉树：是树形结构的一个重要类型，特点是每个节点最多只能有两棵子树，且有左右之分。节点的权重遵循左小右大。

2. 满二叉树：所有的叶子节点都在最后一层，并且节点总数是2^(n-1) , n是层数。

3. 完全二叉树：所有的叶子节点都在最后一层或者倒数第二层，最后一层左边连续，倒数第二层右边连续。

4. 前中后序遍历是相对于父节点而言的。

   ```java
   //前序遍历
   public void preSelect(){
       System.out.println(this);
       if(this.left != null){
           this.left.preSelect();
       }
       if(this.right != null){
           this.right.preSelect();
       }
   }
   ```

   ```java
   //前序查找
   public Node preSearch(int no){
       if(this.no == no){
           return this;
       }
       Node lNode = null;
       if(this.left != null){
           lNode = this.left.preSearch(no);
       }
       if(lNode != null){
           return lNode;
       }
       if(this.right != null){
           lNode = this.right.preSearch(no);
       }
       return lNode;
   }
   ```

   

5. 二叉树顺序存储：一层一层的存到连续的空间中。左孩子的下标是2n+1，右孩子是2n+2，父节点是（n-1)/2。（根节点下标为0）

6. 二叉树的链式存储：每个节点有两个指针，分别指向两个孩子。

7. 线索化二叉树：在二叉树的节点上加上线索，称为线索二叉树，即将空指针指向遍历时的前节点和后节点。有三种遍历方式，所以有三种线索二叉树。实现时，节点类内加两个int型标记，0表示指向左子树，1表示指向前驱。（空指针的个数为节点数+1）

8. 哈夫曼树：给每个叶子节点一个权值，若该树的【带权路径长度】（wpl）达到最小，称这样的二叉树为最优二叉树，也称哈夫曼树。

5. 哈夫曼树的构成：从集合中挑出权重最小的两个节点组成一棵树，新树的权重为两节点的权重和，把新树放入集合，循环往复。

### 哈夫曼编码

先统计每个字符出现的次数，将其按照从大到小的顺序排序，然后将其组成一个哈夫曼树，规定左路径为0,右路径为1。（因为都是叶子节点，所以所有节点的前缀都不会构成另一个节点的编码，不会产生歧义。）

定长编码：将字符转换为ASCII，再转换为二进制。

变长编码：像哈夫曼一样先排序，再按照（0,1,10,11,100,101,110,111）进行编码。

### 二叉排序树

1. 又称二叉查找树，又称二叉搜索树，一般情况下，查询效率比链表结构要高。
2. 左子树节点的权值都比该节点权值小，右子树节点的权值都比该节点权值大。
3. 同一集数据对应的二叉排序树不唯一，但经过中序遍历得到的关键码都是一个递增序列。
4. 删除：
   1. 叶子节点可以直接断开
   2. 若是其他节点，可以用其左子树中最大的节点，或右子树中最小的节点替代它。

5. 红黑树：自平衡二叉排序树

### 多路查找树

1. 多叉树：二叉树中每个节点都有一个数据项，最多有两个子节点，如果允许树的每个节点可以有两个以上的子节点，那么这个树就称为n阶的多叉树，或者称为n叉树。
2. 多叉树通过重新组织节点，减少树的高度，能对二叉树进行优化。
3. 2-3树：
   1. 2-3树的所有叶子节点都在同一层（B树都满足这个条件）
   2. 有两个子节点的节点叫二节点，要么没有子节点，要么有两个子节点。
   3. 有三个子节点的节点叫三节点，要么没有子节点，要么有三个子节点。
   4. 2-3树是由二节点和三节点构成的树

4. B树（Balanced）
   1. B树的阶：节点的最多子节点个数，比如2-3树的阶是3。
   2. B树的搜索：从根节点开始，对节点内的关键字（有序）序列进行二分查找，如果命中则结束，否则进入查找关键字所属范围的儿子节点，循环，直到已经称为叶子节点。
   3. 关键字集合分布在整棵树中，即叶子节点和非叶子节点都存放数据。
   4. 搜索有可能在非叶子节点结束
   5. 其搜索性能等价于在关键字全集内做一次二分查找。

5. B+树
   1. B+树是B树的变体，也是一种多路搜索树。
   2. B+树的搜索与B树也基本相同，区别是B+树只有达到叶子节点才命中，（B树可以在非叶子节点命中）其性能也等价于在关键字全集做一次二分查找。
   3. 非叶子节点上的数字并不是数据，而是索引，即每个子节点中最小的数据。叶子节点相当于是存储（关键字）数据的数据层。
   4. 所有关键字都出现在叶子节点的链表中（即数据只能在叶子节点【也叫稠密索引】），且链表中的关键字（数据）恰好是有序的。
   5. 格式和文件索引系统。

6. B*树：是B+树的变体，在B+树的非根和非叶子节点再增加指向兄弟的指针。

### 图

1. 邻接表：用链表将每个节点的相邻节点连接起来

2. 邻接矩阵：二维数组记录节点到节点的距离

3. 深度优先遍历：从初始访问节点出发，首先访问第一个邻接节点，然后再以这个被访问的邻接节点作为初始节点，访问它的第一个邻接节点。

   ```java
   public void dfs(boolean[] isSelected, int i){
       System.out.print(getValueIndex(i));
       //表明该节点已经被访问过
       isSelected[i] = true;
       //获取第一个邻接顶点
       int w = getFirstVertex(i);
       
       while(w != -1){
           //如果第一个邻接顶点没有被访问
           if(!isSelected[w]){
               //访问第一个顶点
               dfs(isSelected,w);
           }
           //扫描下一个邻接顶点
           w = getNextVertex(i,w);
       }
   }
   ```

   ```java
   //为避免该图不是全连接
   public void dfs(){
       for(int i = 0; i < getVertexSize(); i++){
           if(!isSelected[i]){
               dfs(isSelected,i);
           }
       }
   }
   ```

   

4. 广度优先遍历：类似一个分层搜索的过程，需要使用一个队列以保持访问过的节点的顺序，以便按照这个顺序来访问这些节点的邻接节点。

   ```java
   public void bfs(int i){
       visit(i);
       isSelected[i] = true;
       //放入队列
       queue.add(i);
       while(!queue.isEmpty()){
           i = queue.poll();
           int j = getFirstNeighbor(i);
           while(j != -1){
               if(!isSelected[j]){
                   visit(j);
                   isSelected[j] = true;
                   queue.add(j);
               }
               j = getNextNeighbor(i,j);
           }
       }
       
   }
   ```


5. dijkstra最短路径

   思想：每次都找到表格剩余节点中路径最短的，遍历其所有的相邻节点，计算的距离若比表格中的小，则更新表格。辅助数组用于记录每个节点的前驱节点。

   

6. Floyd算法

   先列出能直接到达的数据，填到表格当中，然后把V0加入到队列中，使其作为中转点，计算各条路径的长度，如果小于表格中记录的数据，就更新数据。然后依次加入V1，V2......

   

7. Prim算法

   首先选择最短的边，放入边集合，顶点放入顶点集合，然后遍历集合中每个顶点的相邻节点的距离，找到最短的，放入边集合，和顶点集合。循环往复......

### 递归回溯

马踏棋盘算法

```java
    public static int X;
    public static int Y;
    public static boolean finished;
    public static boolean[][] visited;
    public static int[][] trace;
    static int count = 0;

    public static void main(String[] args) {
        X = 8;
        Y = 8;
        trace = new int[X][Y];
        visited = new boolean[X][Y];
        long start = System.currentTimeMillis();
        play(0,0,1);
        show();
        long end = System.currentTimeMillis();
        //记录运行时间
        System.out.println(end-start);
        //记录回溯次数
        System.out.println(count);
    }

    public static void play(int nx, int ny, int step){
        //递归结束条件
        if(finished){
            return;
        }
        //没有结束就先走这一步
        trace[nx][ny] = step;
        visited[nx][ny] = true;
        //获取下一步能走的数组
        ArrayList<Point> list = getNext(new Point(nx,ny));
        //进行递增排序，即先选择那些偏僻的点
        sort(list);
        //遍历这些能走的点
        while(!list.isEmpty() && !finished){
            Point next = list.remove(0);
            //递归这一点
            play(next.x, next.y, step + 1);
        }
        //回溯条件
        if(step < X * Y && !finished){
            trace[nx][ny] = 0;
            visited[nx][ny] = false;
            count++;
        }else {
            //步数已满或已经完成
            finished = true;
        }
    }

    public static ArrayList<Point> getNext(Point point){
        ArrayList<Point> list = new ArrayList<>();
        if(point.x + 1 < X && point.y + 2 < Y && !visited[point.x + 1][point.y + 2]){
            list.add(new Point(point.x + 1, point.y + 2));
        }
        if(point.x + 1 < X && point.y - 2 >= 0 && !visited[point.x + 1][point.y - 2]){
            list.add(new Point(point.x + 1, point.y - 2));
        }
        if(point.x - 1 >= 0 && point.y + 2 < Y && !visited[point.x - 1][point.y + 2]){
            list.add(new Point(point.x - 1, point.y + 2));
        }
        if(point.x - 1 >= 0 && point.y - 2 >= 0 && !visited[point.x - 1][point.y - 2]){
            list.add(new Point(point.x - 1, point.y - 2));
        }
        if(point.x + 2 < X && point.y + 1 < Y && !visited[point.x + 2][point.y + 1]){
            list.add(new Point(point.x + 2, point.y + 1));
        }
        if(point.x + 2 < X && point.y - 1 >= 0 && !visited[point.x + 2][point.y - 1]){
            list.add(new Point(point.x + 2, point.y - 1));
        }
        if(point.x - 2 >= 0 && point.y + 1 < Y && !visited[point.x - 2][point.y + 1]){
            list.add(new Point(point.x - 2, point.y + 1));
        }
        if(point.x - 2 >= 0 && point.y - 1 >= 0 && !visited[point.x - 2][point.y - 1]){
            list.add(new Point(point.x - 2, point.y - 1));
        }
        return list;
    }

    //递增排序，即先选那些后续较少的点，防止该点以后难以访问
    //此优化对于该算法至关重要
    public static void sort(ArrayList<Point> list){
        list.sort(new Comparator<Point>() {
            @Override
            public int compare(Point o1, Point o2) {
                return getNext(o1).size() - getNext(o2).size();
            }
        });
    }

    public static void show(){
        for(int i = 0; i < trace.length; i++){
            System.out.println(Arrays.toString(trace[i]));
        }
    }
```



### 贪心算法

保证每次操作都是局部最优的，从而使最后得到的结果是全局最优的。



### 分治算法

思想：将一个规模为N的问题分解为K个规模较小的子问题，这些子问题相互独立且与原问题性质相同，求出子问题的解，就可以得到原问题的解。

```java
    //汉诺塔
    public static void main(String[] args){
        hanoitower(2,'a','b','c');
    }

    public static void hanoitower(int num, char a, char b, char c){
        if(num == 1){
            System.out.println(num+"从"+a+"到"+c);
        }else{
            //先把第num上面的塔看成一个整体，从a移到b。
            hanoitower(num-1,a,c,b);
            //第num个，从a移到b
            System.out.println(num+"从"+a+"到"+c);
            //再把b上的整体，从b移到c上。
            hanoitower(num-1,b,a,c);
        }
    }
```

### KMP算法

移动位数 = 已匹配的字符数 - 对应的部分匹配值

部分匹配值的算法：

1. 列出子串的所有前缀和它自身（从1到n）
2. 对比这些串的前缀和后缀，共有元素的长度，即为该位置的部分匹配值。

### 动态规划

动态规划算法通常用于求解具有某种最优性质的问题，在这类问题中，可能有许多可行解，每一个解都对应于一个值，我们希望找到具有最优值的解，动态规划算法与分治法类似，其基本思想也是将待求解问题分解成若干个子问题，先求子问题，然后从这些子问题的解得到原问题的解。

编辑距离是一类非常经典的动态规划的题目。 

1. 我们使用dp [i] [j] 表示字符串a的前i个字符与字符串b的前j个字符相同所需要的编辑距离。 
2. 首先需要进行状态的初始化，当一个字符串为空时，编辑距离等于另一个字符串的长度 
3. 对于状态转移方程，需要分两种情况讨论： 第一种情况，a[i]==b[j]，这种情况下，我们不需要进行编辑，dp [i] [j] = dp [i-1] [j-1]
4. 第二种情况，a [i] != b [j]，如果两个字符不相等，我们有三种处理方式：
   1. 替换字符串b，编辑距离为dp [i-1] [j-1]+1；
   2. 插入一个字符与其相等，则编辑距离为dp [i-1] [j]+1；
   3. 删除该字符，编辑距离为dp [i] [j-1] +1，三者取其小即可。 

```java
import java.util.*;
import java.lang.*;

public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        while(scanner.hasNext()){
            String str1 = scanner.next();
            String str2 = scanner.next();
            int length1 = str1.length();
            int length2 = str2.length();
            //初始化
            int[][] dp = new int[length1+1][length2+1];
            for(int i = 0; i <= length1; i++){
                dp[i][0] = i;
            }
            for(int i = 0; i <= length2; i++){
                dp[0][i] = i;
            }
            //循环表格
            for(int i = 1; i <= length1; i++){
                for(int j = 1; j <= length2; j++){
                    if(str1.charAt(i-1) == str2.charAt(j-1)){
                        dp[i][j] = dp[i-1][j-1];
                    }else{
                        //替换字符串
                        int temp1 = dp[i-1][j-1]+1;
                        //插入一个字符与其相等
                        int temp2 = dp[i][j-1]+1;
                        //删掉该字符
                        int temp3 = dp[i-1][j]+1;
                        dp[i][j] = Math.min(temp1,Math.min(temp2,temp3));
                    }
                }
            }
            System.out.println(dp[length1][length2]);
        }
    }
}
```

### 递归

24点游戏

```java
import java.util.*;

public class Main{
    //辅助数组，用于dfs方法，选择未选择的数字
    boolean[] visited = new boolean[4];
    //最终结果
    boolean succeed = false;

    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        while(scanner.hasNext()){
            String str = scanner.nextLine();
            String [] strs = str.split(" ");
            Main main = new Main();
            //此数组是初始的数字顺序，即输入时的顺序
            int[] in = new int[4];
            //此数组表示排完序以后的顺序
            int[] newIn = new int[4];
            //将输入的数字放入初始数组
            for(int i = 0; i < 4; i++){
                in[i] = Integer.parseInt(strs[i]);
            }
            //先定义一个工具list
            ArrayList<Integer> list = new ArrayList();
            //将初始数组中的数字按初始顺序放入到list中
            for(int i = 0; i < 4; i++){
                list.add(in[i]);
            }
            //遍历每一种数字顺序，一共4*3*2*1=24种
            //（如果有相同输入数据,此算法会按照不相同的方式来排序）
            //符号组合有64种，所以表达式一共有24*64=1536种组合
            //使用工具list的原因是，使用remove方法，取出后就没办法再选
            //以此来达到排序的作用
            for(int a = 0; a < 4; a++){
                newIn[0] = list.remove(a);
                for(int b = 0; b < 3; b++){
                    newIn[1] = list.remove(b);
                    for(int c = 0; c < 2; c++){
                        newIn[2] = list.remove(c);
                        newIn[3] = list.remove(0);
                        main.visited[0] = true;
                        main.dfs(newIn, (float)newIn[0]);
                        main.visited[0] = false;
                        list.add(newIn[3]);
                        list.add(c, newIn[2]);
                    }
                    list.add(b, newIn[1]);
                }
                list.add(a, newIn[0]);
            }
            //输出结果
            System.out.println(main.succeed);
        }
    }

    //深度优先遍历，数字的顺序只能是数组参数in的顺序，
    //因此，此方法只是遍历符号位的所有可能
    //一共是4*4*4=64种
    public boolean dfs(int[] in, float result){
        int next = getNext();
        if(next == -1){
            if(result == 24){
                succeed = true;
                return true;
            }else{
                return false;
            }
        }else{
            visited[next] = true;
            if(dfs(in, result+(float)in[next])||
                    dfs(in, result-(float)in[next])||
                    dfs(in, result*(float)in[next])||
                    dfs(in, result/(float)in[next])){
                return true;
            }else{
                visited[next] = false;
                return false;
            }
        }

    }

    //获取下一个未选中的元素（dfs的辅助方法）
    public int getNext(){
        for(int i = 0; i < 4; i++){
            if(!visited[i]){
                return i;
            }
        }
        return -1;
    }
}
```

