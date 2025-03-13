---
title: '2025华北电力大学第一次GPLT淘汰赛题解'
description: '-'
publishDate: '2025-03-12'
heroImage: { src: './thumbnail.jpg', color: '#64574D' }
tags: ['题解']
language: '中文'
comment: true
---

## L1-1 你好筛选赛1！

### 分析

字符串直接输出题，一定要完全的复制粘贴。

### 代码

```cpp
#include <iostream>

int main() {
	std::cout << "He1Io Chapterl";
	return 0;
}
```

## L1-2 轮数计算

### 分析

可以发现就是 $⌈\frac{a}{b}⌉$。其中 $⌈x⌉$ 表示实数 $x$ 向上取整。

$a$ 和 $b$ 都是正整数，直接相除是向下取整。所以我们需要采取一定的方案。

- 第一种：判断是否能够整除，如果不可以整除则答案加一。代码中可以使用 `a/b+(a%b!=0)` 来实现。
- 第二种：分子加上 $b−1$，这样如果本来可以整除，答案也不会变化，否则一定可以加一。

其它方案同样可行，在此不作赘述。下面给出第二种方案的实现。

### 代码

```cpp
#include <iostream>

int main() {
	int a,b;
	std::cin >> a >> b;
	std::cout << (a + b - 1) / b;
	return 0;
}
```

## L1-3 课程冲突

### 分析

#### 读入一行

首先需要注意样例与题目描述，一个课程名是一行而不是以空格分割的。因此，我们必须使用一些方法读入整一行。

一个方案是使用 `getchar()` 一个一个字符读入，但是这太麻烦了。

这里推荐使用 `getline(cin,s)` 进行输入。它可以读入一行字符串存到一个名为 `s` 的 `string` 对象中。用法如下：

```cpp
string s;
getline(cin,s);
```

#### getline 的注意事项

本题没有遇到该问题，但是应当掌握。

由于它的原理是一直读入直到遇到回车字符。因此，有以下要点：

**当文件流接下来有空格时，读入会直接停止。**

我们可以观察以下代码：

```cpp
int n;
string s;

cin>>n;
getline(cin,s);

cout<<n<<endl<<s;
```

我们设置输入为如下：

```cpp
114514
What can I say?
```

预期结果应当与输入完全相同，但实际上第二行不会出现。因为 `cin` 读入整数时遇到回车直接停止，不会把它读入，导致此时文件流的下一个字符是回车，`getline` 读入回车后直接停止。

因此，这段代码应该补充一个 `getline(cin,s)`。

```cpp
int n;
string s;

cin>>n;
getline(cin,s);
getline(cin,s);

cout<<n<<endl<<s;
```

#### 判断冲突

首先注意本题的区间是左闭右开的。

实际上是判断区间是否相交。我们直接判是不是有相交有些困难，但是反过来想，判区间是不是不相交是很简单的。

对于 $[l_1,r_1)$ 和 $[l_2,r_2)$ 两个区间，两个集合不相交等价于 $r_1≤l_2$ 或 $r_2≤l_1$。

这道题只有 3 个课程，可以复制粘贴 3 份代码然后改变量名。

### 代码

```cpp
#include <iostream>
#include <string>

int main() {
	std::string s1,s2,s3;
	std::getline(std::cin, s1);
	std::getline(std::cin, s2);
	std::getline(std::cin, s3);
	int a1,a2,a3,b1,b2,b3;
	std::cin >> a1 >> b1 >> a2 >> b2 >> a3 >> b3;
	bool ok1and2 = b1 <= a2 || a1 >= b2;
	bool ok1and3 = b1 <= a3 || a1 >= b3;
	bool ok2and3 = b3 <= a2 || a3 >= b2;
	std::cout << s1 << ' ' << ((ok1and2 && ok1and3) ? "OK!\n" : "Conflicts!\n");
	std::cout << s2 << ' ' << ((ok2and3 && ok1and2) ? "OK!\n" : "Conflicts!\n");
	std::cout << s3 << ' ' << ((ok2and3 && ok1and3) ? "OK!" : "Conflicts!");
	return 0;
}
```

## L1-4 i18n自动机

### 分析

题目要求如果化简完了长度不变就不改动。因此，字符串长度小于等于 3 时原封不动地输出，否则依次输出第一个字符、字符串长度减 2 和最后一个字符。使用 `char` 数组和 `string` 都可以。这里给出使用 `string` 的实现。

### 代码

```cpp
#include <iostream>
#include <string>

int main() {
	std::string s1;
	std::cin >> s1;
	if(s1.size() <= 3) {
		std::cout << s1;
		return 0;
	}
	std::cout << s1[0] << s1.size() - 2 << s1[s1.size() - 1];
	return 0;
}
```

## L1-5 红绿灯问题

### 分析

从左到右枚举红绿灯，我们可以计算从前一个红绿灯到当前红绿灯用了多少时间。

对于第 $i$ 个红绿灯，绿灯持续 $g_i$，红灯持续 $r_i$，循环节是 $g_i+r_i$，初始时刻是 0，因此我们将当前时间对 $g_i+r_i$ 取模，如果时间不超过 $g_i$ 说明是绿灯，可以直接通行，否则还要加上等红灯的时间。

模拟即可。

### 代码

```cpp
#include <iostream>
#include <string>

int main() {
	int n;
	std::cin >> n;
	unsigned long long ans = 0;
	for(int i = 0;i<n;i++) {
		int t,g,r;
		std::cin >> t >> g >> r;
		ans += t;
		int aPhase = ans % (g + r);
		if (aPhase < g)
			continue;
		ans += g + r - aPhase;
	}
	std::cout << ans;
	return 0;
}
```

## L1-6 数值错误

### 分析

#### 类型合法

首先明确一个坑点：不保证数据类型名合法。

#### 数据存储

然后就是如果读取真实数值。首先在 gcc 中有一个非标准类型，名为 `__int128`，可以储存 $[2^{127},2^{127}−1]$ 之间的整数。

但是输入数据的位数依然可能超过它，不过，需要判别的最高也就是 `unsigned long long` 类型，如果输入位数超过了 20 那必然是不合法的。我们只需要用 `__int128` 储存 20 位以内的数即可，这样一定不会出错。

#### 数据读入

`___int128` 是不支持 `cin` 和 `cout` 的，虽然我们可以使用其他类型转换过来，但是在这道题中只有 `long double` 可以存储。如果你不知道它的话，需要手写输入输出。注意特判开头的符号。

使用 `sscanf` 可以把一个字符串当作输入流，然后实现类似 `scanf` 的功能。我们可以用它读取 `long double` 类型的数，避免使用 `__int128`。

这道题没有卡的很死，使用它是可以通过的。

#### 分割字符串

我们按字符串读入一整行，然后我们对字符串进行分割。我们只需要找到最右边的空格即可。

使用 `string` 类的成员函数 `rfind` 即可从右向左找到第一个空格，再使用 `substr` 来截取子串。

此外，你也可以先读入一个字符串，看看是不是 unsigned，如果是则再读入一个。

前半部分决定上下界，后半部分决定数值大小，根据上下界判定数值大小是否合法即可。

至此，你可以解决这个问题了。

### 代码

```cpp
#include <iostream>
#include <string>

int main() {
	std::string aMainString;
	std::getline(std::cin, aMainString);
	int aClipPos = aMainString.rfind(' ');
	std::string aTypeString = aMainString.substr(0, aClipPos);
	std::string aNumString = aMainString.substr(aClipPos + 1);
	
	if(aNumString.size() > 30) {
		std::cout << "ERROR!";
		return 0;
	}
	__int128 aLow;
	__int128 aHight;
	if (aTypeString == "char") {
		aLow = -128;
		aHight = 127;
	}
	else if (aTypeString == "unsigned char") {
		aLow = 0;
		aHight = 255;
	}
	else if (aTypeString == "short") {
		aLow = -32768;
		aHight = 32767;
	}
	else if (aTypeString == "unsigned short") {
		aLow = 0;
		aHight = 65535;
	}
	else if (aTypeString == "int") {
		aLow = -2147483648ll;
		aHight = 2147483647ll;
	}
	else if (aTypeString == "unsigned int") {
		aLow = 0;
		aHight = 4294967295ull;
	}
	else if (aTypeString == "long long") {
		aLow = -9223372036854775808ll;
		aHight = 9223372036854775807ll;
	}
	else if (aTypeString == "unsigned long long") {
		aLow = 0;
		aHight = 18446744073709551615ull;
	}
	else {
		std::cout << "ERROR!";
		return 0;
	}
	bool aSignFlag = false;
	__int128 aTotalNum = 0;
	int aIndex = 0;
	if(aNumString[aIndex] == '-'){
		aSignFlag = true;
		aIndex++;
	}
	for(;aIndex < aNumString.size(); aIndex++) {
		aTotalNum = aTotalNum * 10 + aNumString[aIndex] - '0';
	}
	if(aSignFlag){
		aTotalNum = -aTotalNum;
	}
	//std::cout << "Parser: " << (long long)aTotalNum << '\n';

	/*
	//下面这段是使用 sscanf 的，和上面同功能
	long double aTotalNum;
    	sscanf(aNumString.c_str(),"%llf",&aTotalNum);
	*/

	if(aTotalNum < aLow || aTotalNum > aHight){
		std::cout << "ERROR!";
		return 0;
	}
	std::cout << "OK!";
	return 0;
}
```

## L1-7 节点关系

### 分析

本题本质上是给你一个树上差分让你还原，其实就是树上前缀和。

不过数据范围开的很小，你可以对于每一个点递归或者迭代找到根然后求和，复杂度是 $O(n^2)$，可以通过。

注意，如果数据给你一条链。且每一个点的权值开到尽可能大，那么一个点的值会达到 $nV=10^{11}$，必须使用 `long long` 进行存储。

### 代码

```cpp
#include <iostream>
#include <string>
#include <map>

int n;
int father[1000005];
long long x[1000005];
long long y[1000005];
bool visited[1000005];
long long factx[1000005];
long long facty[1000005];

void UpdateNode(int note){
	visited[note] = true;
	if(!visited[father[note]]) {
		UpdateNode(father[note]);
	}
	factx[note] = x[note] + factx[father[note]];
	facty[note] = y[note] + facty[father[note]];
}

int main() {
	visited[0] = true;
	int fatherCount = 0;
	std::cin >> n;
	for(int i=1;i<=n;i++) {
		std::cin >> father[i] >> x[i] >> y[i];
	}
	for(int i=1;i<=n;i++) {
		UpdateNode(i);
		std::cout << factx[i] << ' ' << facty[i] << '\n';
	}
	return 0;
}
```

## L1-8 串珠子

### 分析

数据规模很大，肯定是结论题。

#### 猜想

根据样例人工 OEIS 出答案。

首先根据直觉，答案一定是多项式。

观察样例易知答案是 $n^2−n−1$。

#### 证明

##### 充分性

我们首先找到什么情况下可能可以形成合法的。

每个连续的 $n+1$ 段出现了所有的 $n$ 个颜色，等价于每个颜色在每个连续的 $n+1$ 段出现了至少一次。

因此，每种颜色的珠子都有 $⌈\frac{m}{n+1}⌉$ 个，一共 $n$ 种颜色的种子，所以必要条件是

$$
n⌈\frac{m}{n+1}⌉≤m
$$
因此，上式取反就是原问题的充分形式：

$$
m<n⌈\frac{m}{n+1}⌉
$$
式中存在向上取整，考虑消去。

令 $m=q(n+1)+r(q≥1,r∈[0,n])$。显然 $q$ 与 $r$ 是独立的。

当 $r=0$ 时，原式为

$$
q(n+1)<qn
$$
上式一定不成立，没有办法确定。

但是，在整除的情况下，我们可以很轻松的构造出合法的情况。

当 $r\not=0$ 时，原式为

$$
q(n+1)+r<n(q+1)
$$
化简得到

$$
q+r<n
$$
我们需要将边界尽可能变大。如果要让 $m$ 尽可能大，那么就需要让 $q$ 尽可能大。

令 $q=n−2,r=1$，那么 $m=n^2−n−1$。

这样，我们就找到了一个可能的最大的解。

#### 必要性

接下来，我们需要证明 $m≥n^2−n$ 时一定存在解。

不妨设 $n=3$。

- 当 $m=n^2−n$ 时，我们可以按照 123_123_ 去放，这里 _ 只是一个占位符，方便与下文比较。
- 当 $m=n^2−n+1$ 时，我们可以按照 1231123_ 去放。
- 当 $m=n^2−n+2$ 时，我们可以按照 12311231 去放。
- 当 $m=n^2−n+3$ 时，我们可以按照 123_123_123_ 去放。此时可以看作回到了第一种情况。

因此，我们通过这样的构造，证明了当 $m≥n^2−n$ 时一定有解。

综上，我们得出最大的不合法情况是 $m=n^2−n−1$ 时。

### 代码

```cpp
#include <iostream>
#include <string>

int main() {
	long long n;
	std::cin >> n;
	std::cout << (n*n-n-1);
	return 0;
}
```

## L2-1 Summit with Double Dash

### 分析

典型分层图。

在做这道题之前，首先要注意 $x$ 和 $y$ 是反着的，是左手系，你需要在读入时反过来。

如果只有第一种单格移动，这道题是很简单的，朴素的 bfs 即可。

但是现在加上了第二种移动，只能进行 2 次。因此，我们需要特殊处理这种操作。

对于这种某些步只移动很少的次数的，可以采用分层图。

其实就是把一份图复制三份，竖着叠起来，相当于每个点除了 *x* 和 *y* 以外又多了一个 $z$ 轴。

- 当进行第一种单格移动时，不改变 $z$ 轴。
- 当进行第二种移动时，将 $z$ 轴坐标 +1，这样我们可以保证第二种移动不会超过 2 次。

这样就解决了这个问题。

### 代码

```cpp
#include <iostream>
#include <queue>

int n,m;
int x,y,u,v;
char gMap[1005][1005];
bool visited[1005][1005][3];
struct Info {
	int x;
	int y;
	int reanim_dash;
	int step;
};

bool IsGridOK(int x, int y, int reanim_dash) {
	if (x < 0 || y < 0 || x >= m || y >= n)
		return false;
	if (gMap[y][x] == '#')
		return false;
	return !visited[x][y][reanim_dash];
}


int main() {
	std::cin >> n >> m;
	std::cin >> x >> y >> u >> v;
	x--;	y--;	u--;	v--;
	for(int i=0;i<n;i++) {
		std::cin >> gMap[i];
	}
	std::queue<Info> anInfoDash;
	
	visited[x][y][2] = true;
	anInfoDash.push({x,y,2,0});
	
	while(!anInfoDash.empty()) {
		Info aTopInfo = anInfoDash.front();
		anInfoDash.pop();
		if(aTopInfo.x == u && aTopInfo.y == v) {
			std::cout << aTopInfo.step;
			return 0;
		}
		// Update Normal Move
		constexpr int offsetX[4] = {0, 0, 1, -1};
		constexpr int offsetY[4] = {1, -1, 0, 0};
		for(int i=0;i<4;i++) {
			int tx = aTopInfo.x + offsetX[i];
			int ty = aTopInfo.y + offsetY[i];
			if(!IsGridOK(tx,ty,aTopInfo.reanim_dash))
				continue;
			visited[tx][ty][aTopInfo.reanim_dash] = true;
			anInfoDash.push({tx,ty,aTopInfo.reanim_dash,aTopInfo.step + 1});
		}
		if(aTopInfo.reanim_dash == 0)
			continue;
		constexpr int DashOffsetX[8] = {0, 0, 2, -2,1,1,-1,-1};
		constexpr int DashOffsetY[8] = {2, -2, 0, 0,1,-1,1,-1};
		for(int i=0;i<8;i++) {
			int tx = aTopInfo.x + DashOffsetX[i];
			int ty = aTopInfo.y + DashOffsetY[i];
			if(!IsGridOK(tx,ty,aTopInfo.reanim_dash - 1))
				continue;
			visited[tx][ty][aTopInfo.reanim_dash - 1] = true;
			anInfoDash.push({tx,ty,aTopInfo.reanim_dash - 1,aTopInfo.step + 1});
		}
	}
	std::cout << -1;
	return 0;
}
```

## L2-2 Dashless+

### 分析

买一送一的题目，这道题就是典型的 01bfs。如果距离是 0 就把下一个点插到队首，否则插入到队尾。

当然了你也可以选择打 Dijkstra，一样的。

由于数据特别弱，你甚至可以打一个 Bellman-Ford。

### 代码

```cpp
#include <iostream>
#include <queue>

int n,m;
int x,y,u,v;
char gMap[1005][1005];
unsigned int LessDashCount[1005][1005];
struct Info {
	int x;
	int y;
	int dashCount;
};

bool IsGridOK(int x, int y, int dashCount) {
	if (x < 0 || y < 0 || x >= m || y >= n)
		return false;
	if (gMap[y][x] == '#')
		return false;
	return LessDashCount[x][y] > dashCount;
}


int main() {
	std::cin >> n >> m;
	std::cin >> x >> y >> u >> v;
	x--;	y--;	u--;	v--;
	
	memset(LessDashCount,-1,sizeof(LessDashCount));
	
	for(int i=0;i<n;i++) {
		std::cin >> gMap[i];
	}
	std::queue<Info> anInfoDash;
	
	LessDashCount[x][y] = 0;
	anInfoDash.push({x,y,0});
	
	while(!anInfoDash.empty()) {
		Info aTopInfo = anInfoDash.front();
		anInfoDash.pop();
		// Update Normal Move
		constexpr int offsetX[4] = {0, 0, 1, -1};
		constexpr int offsetY[4] = {1, -1, 0, 0};
		for(int i=0;i<4;i++) {
			int tx = aTopInfo.x + offsetX[i];
			int ty = aTopInfo.y + offsetY[i];
			if(!IsGridOK(tx,ty,aTopInfo.dashCount))
				continue;
			LessDashCount[tx][ty] = aTopInfo.dashCount;
			anInfoDash.push({tx,ty,aTopInfo.dashCount});
		}
		
		constexpr int DashOffsetX[8] = {0, 0, 2, -2,1,1,-1,-1};
		constexpr int DashOffsetY[8] = {2, -2, 0, 0,1,-1,1,-1};
		for(int i=0;i<8;i++) {
			int tx = aTopInfo.x + DashOffsetX[i];
			int ty = aTopInfo.y + DashOffsetY[i];
			if(!IsGridOK(tx,ty,aTopInfo.dashCount + 1))
				continue;
			LessDashCount[tx][ty] = aTopInfo.dashCount + 1;
			anInfoDash.push({tx,ty,aTopInfo.dashCount + 1});
		}
	}
	std::cout << (int)(LessDashCount[u][v]);
	return 0;
}
```

## L2-3 Bead Arrangement

### 分析

乍一看很难，因为只是比较相邻的两个，那么同余关系没有用，在剩余类环上的东西通通失效了。

然后你就只能手动跑点小数据。

首先想到，如果 $x$ 是 $\{b_i\}$ 中的一个，那么必然存在 $2x≤n$。

那么 $\{b_i\}$ 中元素最多的情况就是 $[1,⌊\frac{n}{2}⌋]$ 中所有的数都存在 $B$ 中。

我们考虑这样连边：对于 $[1,⌊\frac{n}{2}⌋]$ 中所有的数 $i$，都连向 $2i$。

显然，每一个数最多只会指向一个数，也最多只会被一个数指。这样就是很多条链。

我们把这些链依次抄下来到一个序列中，显然一个链的链尾和另一条链的链首之间的最大公因数是没有贡献的，因为所有可能出现的最大公因数必定存在于这些链内部。这样我们就可以构造出数量最多的公因数了。

如果要求的比这个少，只需要在上述图中减边即可。我们只连接 $[1,k]$ 向 2 倍于自身的位置的边，然后将他们拼接。由于我们保证了 $[1,k]$ 所有的数都是最大公因数，所以链与链之间不会有额外的贡献。

### 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll t,n,k;
inline ll read(){
	ll s = 0,w = 1;
	char ch = getchar();
	while (ch > '9' || ch < '0'){ if (ch == '-') w = -1; ch = getchar();}
	while (ch <= '9' && ch >= '0') s = (s << 1) + (s << 3) + (ch ^ 48),ch = getchar();
	return s * w;
}
int main(){
	t = read();
	while (t --){
		n = read(),k = read();
		if (k > n / 2){puts("No"); continue;}
		puts("Yes"),printf("1 ");
		for (ll i = n;i >= k * 2 + 1;i -= 1) printf("%lld ",i);
		for (ll i = 1;i <= k * 2;i += 2) for (ll j = max(2ll,i);j <= k * 2;j *= 2) printf("%lld ",j);
		puts("");
	}
	return 0;
}
```

## L2-4 Continuous Monotone Split

### 分析

题目叙述有问题，分割的意思不是连续的字段，而是可以随便选取的子序列，间断也可以，所以这题和相对顺序无关。不然这道题是非常难的。

这道题的题目来源是 [P447 [AHOI2018初中组\] 分组](https://www.luogu.com.cn/problem/P4447)。

做法有很多，首先我们可看到，最少人数是满足单调性的。因为一定存在一个最大值的情况，然后如果要取的更小，只需要把块切一下就行。而比最大值更大直接违背了假设。所以满足单调性。

然后考虑验证答案。从小到大排序，然后从左往右扫，每遇到一个新的值就把他加入到能加入的最短的那个连续段里。这样就可以以 $O(nlogn)$ 的时间复杂度解决问题。

### 代码

```cpp
#include<cstdio>
#include<map>
#include<queue>
#include<algorithm>
using namespace std;
map<int,int> p;//记录的是以a[i]结尾的组是哪一个小根堆。
priority_queue<int,vector<int>,greater<int> > q[100005];
int n,a[100005],k,s,ans=1<<30;
int mn(int x,int y){return x>y?y:x;}//min
int read(){
	int x=0,flag=1;char c=getchar();
	while(c<'0'||c>'9'){flag*=(c=='-'?-1:1);c=getchar();}
	while(c>='0'&&c<='9'){x=(x<<3)+(x<<1)+c-'0';c=getchar();}
	return flag*x;
}
int main(){
	n=read();
	for(int i=1;i<=n;++i)
		a[i]=read();
	sort(a+1,a+n+1);
	for(int i=1;i<=n;++i){
		if(p[a[i]]==0)p[a[i]]=++k;//如果a[i]之前从未出现，就新开一个小根堆。
		if(p[a[i]-1]==0||q[p[a[i]-1]].size()==0)//不能接上任何一组
			q[p[a[i]]].push(1);
		else {//能接上一组
			s=q[p[a[i]-1]].top()+1;//加给以a[i]-1结尾最小的那个组
			q[p[a[i]-1]].pop();//删除该组
			q[p[a[i]]].push(s);//组大小+1，再放进以a[i]结尾的小根堆。
		}
	}
	for(int i=1;i<=n;++i){
		if(q[p[a[i]]].size()>0)//注意，如果小根堆为空不能取top
			ans=mn(ans,q[p[a[i]]].top());//用以a[i]结尾的最小组更新答案
	}
	printf("%d\n",ans);
	return 0;
}
```

