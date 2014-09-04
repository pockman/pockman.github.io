<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. 来源</a></li>
<li><a href="#sec-2">2. 描述</a>
<ul>
<li><a href="#sec-2-1">2.1. 输入和输出</a></li>
</ul>
</li>
<li><a href="#sec-3">3. 解决问题</a>
<ul>
<li><a href="#sec-3-1">3.1. 分析</a>
<ul>
<li><a href="#sec-3-1-1">3.1.1. 方法一</a></li>
<li><a href="#sec-3-1-2">3.1.2. 方法二</a></li>
<li><a href="#sec-3-1-3">3.1.3. 方法三</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#sec-4">4. 评价</a></li>
</ul>
</div>
</div>

# 来源

NOIP2010第二题

# 描述

学校里有一个水房，水房里一共装有m 个龙头可供同学们打开水，每个龙头每秒
钟的供水量相等，均为1。 现在有n 名同学准备接水，他们的初始接水顺序已经
确定。将这些同学按接水顺序从1到n 编号，i 号同学的接水量为wi。接水开始时，
1 到m 号同学各占一个水龙头，并同时打开水龙头接水。当其中某名同学j 完成
其接水量要求wj 后，下一名排队等候接水的同学k马上接替j 同学的位置开始接
水。这个换人的过程是瞬间完成的，且没有任何水的浪费。即j 同学第x 秒结束
时完成接水，则k 同学第x+1 秒立刻开始接水。若当前接水人数n’不足m，则只有
n’个龙头供水，其它m−n’个龙头关闭。 现在给出n 名同学的接水量，按照上述接
水规则，问所有同学都接完水需要多少秒。


## 输入和输出

输入格式：第1行2个整数 \(n\) 和 \(m(1 \le n \le 10000, 1 \le m \le 100, m \le n)\) ，用一个空
格隔开，分别表示接水人数和龙头个数。

第2行 \(n\) 个整数\(w_1,w_2,w_3, \ldots,w_n(1 \le w_i \le 100)\) ，每两个整数之间用一个空格隔
开， \(w_i\)  表示 \(i\) 号同学的接水量。

输出格式：只有一行，1 个整数，表示接水所需的总时间。

# 解决问题

## 分析

看懂题意后，发现这个题用简单模拟就可以搞定。于是，我们就有了方法一：模
拟。

### 方法一

从数组a中找出M个元素，找出其中最小的，将它值为0,并将其他m-1个元素的值减
去这个最小值。就这样不停地往后找m个元素，注意0要排除掉，不需要尾处理。

代码（废话注释可以忽视）

    program water;
    
    type Tarr = array [1..10001] of longint;
    
    function empty(s : Tarr; n :longint) : boolean;
    var i : longint;
    
    begin
       empty := true;
       for i := 1 to n do if s[i] <> 0 then 
       begin empty := false;
          break;
       end;
    end;
    
    
    function Go(a : Tarr ; m :longint; n :longint) : longint;
    var 
       total : longint;
       min : longint;
       i, cal : longint;
    
    begin total := 0;
       while not empty(a, n) do //如果数组不空（不全为0）循环
       begin 
          min := 9;
          cal := 1;
    
          for i := 1 to n do 
          begin 
             if (cal=m+1)then break;
             if (a[i] <= min) and(a[i]<>0) then //找出m个中最小的数，用cal滤过0 
             begin
                min := a[i];
                inc(cal);
             end;
          end;
    
          cal := 1;
          for i := 1 to n do 
          begin
             if (cal=m+1) then break;
             if a[i] <> 0 then 
             begin 
                if a[i] = min then //对这m个数分别进行处理： 如果最小赋值0，否则减去最小，同样用cal滤过0 
                   a[i] := 0 
                else 
                begin
                   a[i] := a[i]-min;
                end;
                inc(cal);
             end;
    
          end;
    
          total := total + min;
          //总时间加上这个最小值end;
          go := total;
    
       end;
    
    
       var 
          n, m, i :longint;
          s : string;
          a :Tarr;
    
       begin readln(n, m);
    
    
          for i := 1 to n do 
          begin 
             read(a[i]);
             //读入数据
    
          end;
          writeln(go(a, m, n));
          //操作开始
    
       end.

这样的方法时间复杂度大概是 \(O（N^2)\) 的上限.结果有一个超大数据没有过。。。。Time
limit exceeded。。。。。 >\_< 只能换一种方法了。

1.  类似于方法一的优化算法

    在讲下一种算法之前，我先给大家介绍一种思路巧妙的优化算法，这对以后的算
    法有启发作用。。。。。思路是这样的：先读入m个数据，存入数组，继续读数据，
    并在前M个数据中找到最小的并加上这个数，直到数据读完为止，输出数组中最大
    的数字。注意：加数之后可能就不是最小的了。For example, n=5, m=3, 打水时
    间分别为4 2 1 2 1. 先读入前3个4 2 1，继续读数，找到最小的1，加上2变成4
    2 3，继续读入1，找到最小的2，加1变成4 3 3，没有数可读，找到最大的4就是
    答案。
    
    思路就是这么简单，程序不给了，自己想去吧= =

### 方法二

由于第一种方法的失败，我们开始考虑另一种算法。是使用一个优先队列。思路：
先读入m个数据，将它们从小到大排列。先令总时间加上最小的数，令其他数减去
这个最小值，这就是初始化。再一个一个读入剩下的n-m个数，同时进行操作。将
读入的数字存在数组首部，如果它大于下一个元素，就令它减去下一个元素
（ \(a[2]\) )的值,不断地往后比较，每次比较先减去 \(a[2]\) 的值，这样可以保证队列从
小到大排列。如果 \(a[1] < a[2]\) 那么所有元素减去 \(a[1]\) ， \(a[1]\) 变为0，
继续读数。最后还需要一个尾处理，在总时间里加上数组中剩下的最大值。这样
的时间复杂度为 \(O（n^2)\) 的 \(\frac{1}{2}\) .

注意事项：此种算法需要避免M=1的情况，因为当m等于1， \(a[2]\) 不存在，而算
法无法避免，必须另外考虑。。。一开始没想到竟然WA中枪。。。我的AC率啊 =
=附程序：

    program water;
    
    
    type Tarr = array [1..10001] of longint;
    
    
    
      procedure Exchange(var a, b : longint);
      var 
         c : longint;
      begin 
         c := a;
         a := b;
         b := c;
      end;
    
    
    //冒泡排序用来初始化
      procedure Sort(var a : Tarr; size :longint);
      var
         j, i: longint;
    
      begin 
         for i := size-1 downto 1 do 
            for j := 1 to i do
            begin 
               if a[j] > a[j+1] then Exchange(a[j], a[j+1]);
            end;
      end;
    
    
      procedure insertarr(var a : Tarr; size, x : longint);
      var 
         i : longint;
    
      begin 
         for i := 1 to size-1 do 
         begin 
            a[i+1] := a[i+1]-x; //此处即是将 a[1]不断往后处理直到找到它合适的位置。
            if a[i] > a[i+1] then
               Exchange(a[i], a[i+1]);
         end;
      end;
    
    
    var 
       max, n, m, i,j,total,tmp,x :longint;
       a : Tarr;
    
    
    begin total := 0;
       readln(n, m);
       if m = 1 then //对m=1的特殊处理
       begin
          for i := 1 to n do 
          begin 
             read(x);
             total := total + x;
          end;
    
          writeln(total);
          halt;
       end 
       else 
       begin //初始化
          for i := 1 to m do
             read(a[i]);
    
          sort(a, m);
          total := total+a[1];
    
          tmp := a[1];
          for i := 1 to m do a[i] := a[i]-tmp;
    
    
          for i := 1 to n-m do 
          begin 
             read(a[1]);
             if a[1] > a[2] then //分情况讨论
             begin 
                total := total + a[2];
                a[1] := a[1]-a[2];
                insertarr(a, m, a[2]);
             end else begin 
                total := total + a[1];
                for j := 2 to m do 
                   a[j] := a[j] - a[1];
                a[1] := 0;
             end;
          end;
          max := -maxlongint; //尾处理
          for i := 1 to n do 
             if a[i] > max then
                max := a[i];
          total := total + max;
          writeln(total);
       end;
    end.

### 方法三

好的，只要有一个优先队列就已经可以AC了。而且比朴素算法快了100倍。那么我
们能不能对优先队列进行改进呢？答案是可以的，我们可以用一个二叉堆来实现，
堆的操作简单而且效率高，不错。

二叉堆思路：同样先读入m个数据建一个小根堆，不断地取出堆顶元素并令总时间
加上它，再读数放置在堆顶并向下调整，直到完成合法的小根堆。重复以上步骤，
最后仍是在数组中找到最大元素加入总时间。容易吧哈哈哈哈！代码也是这么的
简洁：

    
        program water_heap;
    
    type Theap = record data : array [1..10001] of longint; 
                    len : longint;
                 end;
    
        procedure Exchange(var a, b : longint); 
        var 
           c : longint; 
    
        begin 
           c := a;
           a := b; 
           b := c; 
        end;
    
        procedure put(x : longint; var heap : Theap); //将一个数据x放入堆中
        var
           tmp :longint; 
        begin 
           inc(heap.len); 
           heap.data[heap.len] := x; 
           tmp := heap.len; 
           while (tmp<>1) and (heap.data[tmp div 2] > heap.data[tmp]) do 
           begin 
              Exchange(heap.data[tmp div 2], heap.data[tmp]); 
              tmp := tmp div 2; 
           end; 
        end;
    
        function get(var heap : Theap) : longint; //取得堆顶元素
        begin 
           get := heap.data[1];
        end;
    
        procedure adjust(var heap : Theap); //调整堆
        var father, son : longint;
        begin
    
           father := 1; 
           while (father*2<=heap.len) do 
           begin if (father*2+1 > heap.len) or (heap.data[father*2] < heap.data[father*2+1]) then
              son := father*2 
           else 
              son := father*2+1; 
    
              if heap.data[father] > heap.data[son] then 
              begin 
                 Exchange(heap.data[father], heap.data[son]); 
                 father := son; 
              end 
              else 
                 break; 
           end; 
        end;
    
    var 
       n, m, total, x, ok, max,i,j : longint;
       heap : Theap;
    
    begin readln(n, m); 
       fillchar(heap, sizeof(heap), 0);
       for i := 1 to m do //初始化在堆中放入m个元素
       begin 
          read(x); 
          put(x, heap); 
       end;
    
       total := 0; 
       for i := 1 to n-m do 
       begin 
          ok := get(heap); 
          total := total + ok; //取出堆顶元素
    
          for j := 1 to heap.len do 
             heap.data[j] := heap.data[j]-ok; 
    
          read(x); 
    
          heap.data[1] := x; //读数调整
          adjust(heap); 
       end; 
    
       max := -maxlongint; 
       for i := 1 to heap.len do 
          if  heap.data[i] > max then //尾处理
             max := heap.data[i]; 
       total := total + max; writeln(total); 
    end.

提交证明：二叉堆又比优先队列快了3倍。

1.  对方法三的优化

    我说过，上述的简单模拟会对方法三进行进一步优化。优化策略是这样的：先读
    入m个数，构建小根堆。继续读数 \(x\) ，在堆顶 \(a[1]\) 中加上这个数并向下调
    整堆。重复操作，最后只需在堆中找出最大元素即可。

# 评价

-   第一种算法好比是小刀砍老怪，当老怪还有半滴血的时候，你被他的大招搞死了。

-   第二种方法的优化好比是长剑砍老怪，当你砍死老怪时，你的血也所剩不多。

-   第三种方法好比是你学了一个小魔法，可以只需“杀敌一万，自损三千”了。

-   第三种方法好比是你学了一个大魔法，无伤击杀BOSS。最后的优化策略简直是老怪见到你吓得直接逃跑了。。。。这比喻真形象！
