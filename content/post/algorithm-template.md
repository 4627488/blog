---
title: "常用裸题模板"
description: "常用裸题模板"
date: 2020-11-06T23:16:26Z
image: 
math: 
license: 
hidden: false
comments: true
draft: false
tags: 
    - ACM
---

## 常用~~垃圾题~~黑题模板

>长度,是难度的一部分.

## 快读

```cpp
inline int getInt()
{
    ll x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9')
    {
        if (ch == '-')
            f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9')
    {
        x = (x << 1) + (x << 3) + (ch ^ 48);
        ch = getchar();
    }
    return x * f;
}
inline void out(int a)
{
    if (a >= 10)
        out(a / 10);
    putchar(a % 10 + '0');
}

```

## 快速排序

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 100000
#define read(x) scanf("%d",&x)

int n;
int a[maxn+5];

int main() {
 read(n);
 for(int i=1;i<=n;i++) read(a[i]);
 sort(a+1,a+n+1);
 for(int i=1;i<=n;i++) printf("%d ",a[i]);
 return 0;
}
```

## 并查集

注意：不能忘了路径压缩

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 10000
#define read(x) scanf("%d",&x)

int n,m;
int fa[maxn+5];

int find(int x) {
 if(!fa[x]) return x;
 return fa[x]=find(fa[x]);
}

int main() {
 read(n),read(m);
 while(m--) {
  int x,y,z;
  read(z),read(x),read(y);
  int fa1=find(x),fa2=find(y);
  if(z==1) {
   if(fa1==fa2) continue;
   else fa[fa1]=fa2;
  } else {
   if(fa1==fa2) printf("Y\n");
   else printf("N\n");
  }
 }
 
 return 0;
}
```

## 快速幂

注意：在最后输出ans时要再模一次p，以免被指数为0模数为1的情况卡掉

```cpp
#include<bits/stdc++.h>
using namespace std;

#define ll long long
#define read(x) scanf("%d",&x)

int a,b,p;

int main() {
 read(a),read(b),read(p);
 int aa=a,bb=b;
 
 int ans=1;
 while(b) {
  if(b&1) ans=(ll)ans*a%p;
  b>>=1,a=(ll)a*a%p;
 }
 
 printf("%d^%d mod %d=%d",aa,bb,p,ans%p);
 
 return 0;
}
```

## 矩阵快速幂

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 100
#define md (int)(1e9+7)
#define ll long long

int n;
ll K;

struct Node{
    ll a[maxn+5][maxn+5];
    Node(){memset(a,0,sizeof(a));}
    void build() {for(int i=1;i<=n;i++) a[i][i]=1;}
};

Node operator * (const Node& x,const Node& y) {
    Node z;
    for(int k=1;k<=n;k++) {
        for(int i=1;i<=n;i++) {
            for(int j=1;j<=n;j++) {
                z.a[i][j]=(z.a[i][j]+x.a[i][k]*y.a[k][j]%md)%md;
            }
        }
    }
    return z;
}

Node a,ans;

int main() {
    scanf("%d%lld",&n,&K);
    for(int i=1;i<=n;i++) for(int j=1;j<=n;j++) scanf("%lld",&a.a[i][j]);
    
    ans.build();
    do{
        if(K&1) ans=ans*a;
        a=a*a,K>>=1;
    } while(K);
    
    for(int i=1;i<=n;i++) {
        for(int j=1;j<=n;j++) printf("%lld ",ans.a[i][j]);
        printf("\n");
    }
    
    return 0;
}
```

## 线性筛判素数

注意：

1、一定要背熟模板

2、不要忘写 &&i*prm[j]<=n 导致数组越界

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 10000000
#define read(x) scanf("%d",&x)

int n,m;
int Phi[maxn+5];
vector<int> prm;

void makephi() {
 Phi[1]=1;
 for(int i=2;i<=n;i++) {
  Phi[i]=i;
 }
 for(int i=2;i<=n;i++) {
  if(Phi[i]==i) Phi[i]--,prm.push_back(i);
  for(int j=0;j<prm.size()&&i*prm[j]<=n;j++) {
   int x=prm[j];
   if(i%x) Phi[i*x]=Phi[i]*x;
   else Phi[i*x]=Phi[i]*Phi[x];
  }
 }
}

int main() {
 read(n),read(m);
 makephi();
 while(m--) {
  int x;
  read(x);
  if(Phi[x]==x-1) printf("Yes\n");
  else printf("No\n");
 }
 
 return 0;
}
```

## 堆（优先队列）

注意：```priority_queue```是大根堆，若要使用小跟堆需要push相反数，或使用struct封装后定义小于运算符。

```cpp
#include<bits/stdc++.h>
using namespace std;

#define read(x) scanf("%d",&x)

priority_queue<int> que;

int main() {
 int n;
 read(n);
 while(n--) {
  int opr;
  read(opr);
  if(opr==1) {
   int x;
   read(x);
   que.push(-x);
  } else if(opr==2) printf("%d\n",-que.top());
  else que.pop();
 }
 
 return 0;
}
```

## 字符串哈希

注意：

1、可以用 unsigned long long （ull）的自然溢出处理取模问题

2、使用双哈希不容易产生冲突

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 10000
#define maxm 1500
#define ull unsigned long long
#define read(x) scanf("%d",&x)

struct Pair{
 ull x,y;
 Pair(){}
 Pair(ull xx,ull yy) {x=xx,y=yy;}
 bool operator < (const Pair& oth) const {
  return x<oth.x||(x==oth.x&&y<oth.y);
 } 
 bool operator == (const Pair& oth) const {
  return x==oth.x&&y==oth.y;
 }
};

int n;
Pair hsh[maxn+5];

const int md1=13,md2=131;

void make_hash(int x,char* s,int m) {
 ull hsh1=0,hsh2=0;
 for(int i=0;i<m;i++) {
  hsh1=hsh1*md1+s[i]+1;
  hsh2=hsh2*md2+s[i]+1;
 }
 hsh[x]=Pair(hsh1,hsh2);
}

int main() {
 read(n);
 for(int i=1;i<=n;i++) {
  char s[maxm+5];
  scanf("%s",s);
  make_hash(i,s,strlen(s));
 }
 
 sort(hsh+1,hsh+n+1);
 
 int ans=0;
 for(int i=1;i<=n;i++) {
  if(hsh[i]==hsh[i-1]) continue;
  ans++;
 }
 
 printf("%d",ans);
  
 return 0;
}
```

## 最小生成树

注意：

1、正向定义小于运算符

2、图不连通的判断方法是求出的最小生成树的边数不等于n-1

3、边集的数组大小要开maxm，开成了maxn就彻底凉了

4、接上，n==m时一定要注意，不要把n、m弄反

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 5000
#define maxm 200000
#define read(x) scanf("%d",&x)

struct Edge{
 int x,y,z;
 Edge(){}
 Edge(int xx,int yy,int zz) {
  x=xx,y=yy,z=zz;
 }
 bool operator < (const Edge& oth) const {
  return z<oth.z;
 }
};

int n,m;
Edge e[maxm+5];
int fa[maxn+5];

int find(int x) {
 if(fa[x]) return fa[x]=find(fa[x]);
 else return x;
}

int kruskal() {
 int cnt=0,s=0;
 for(int i=1;i<=m;i++) {
  int fa1=find(e[i].x),fa2=find(e[i].y);
  if(fa1==fa2) continue;
  fa[fa1]=fa2;
  cnt++;
  s+=e[i].z;
 }
 if(cnt==n-1) return s;
 else return -1;
}

int main() {
 read(n),read(m);
 for(int i=1;i<=m;i++) read(e[i].x),read(e[i].y),read(e[i].z);
 sort(e+1,e+m+1);
 int ans=kruskal();
 if(ans==-1) printf("orz");
 else printf("%d",ans); 
 return 0;
}
```

## 最短路 dijkstra

注意：反向定义小于运算符

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 100000
#define maxm 200000
#define inf ((int)1e9)
#define read(x) scanf("%d",&x)

struct Edge{
 int x,y,z;
 Edge(){}
 Edge(int xx,int yy,int zz) {z=zz,y=yy,z=zz;}
};

struct Node{
 int y,z;
 Node(){}
 Node(int yy,int zz){y=yy,z=zz;}
 bool operator < (const Node& oth) const {
  return z>oth.z;
 }
};

int n,m,s;
vector<Edge> g[maxn+5];

int dist[maxn+5];
priority_queue<Node> que;
bool vis[maxn+5];

void readin() {
 read(n),read(m),read(s);
 for(int i=1;i<=m;i++) {
  int x,y,z;
  read(x),read(y),read(z);
  g[x].push_back(Edge(x,y,z));
 }
}

void dijkstra() {
 for(int i=1;i<=n;i++) if(i!=s) dist[i]=inf;
 que.push(Node(s,0));
 while(!que.empty()) {
  int h=que.top().y;
  que.pop();
  if(vis[h]) continue;
  vis[h]=true;
  for(int i=0;i<g[h].size();i++) {
   int y=g[h][i].y;
   if(dist[y]>dist[h]+g[h][i].z) {
    dist[y]=dist[h]+g[h][i].z;
    que.push(Node(y,dist[y]));
   }
  }
 }
 for(int i=1;i<=n;i++) printf("%d ",dist[i]);
}

int main() {
 readin();
 dijkstra(); 
 return 0;
}
```

## 最短路 floyd

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 100
#define maxm 10000
#define read(x) scanf("%d",&x)
#define inf (1<<30)

int n,m;
int a[maxm+5];
int g[maxn+5][maxn+5];
int dist[maxn+5][maxn+5];

void readin() {
 read(n),read(m);
 for(int i=1;i<=m;i++) read(a[i]);
 for(int i=1;i<=n;i++) for(int j=1;j<=n;j++) read(g[i][j]);
}

void floyd() {
 for(int i=1;i<=n;i++) for(int j=1;j<=n;j++) dist[i][j]=g[i][j];
 for(int k=1;k<=n;k++) {
  for(int i=1;i<=n;i++) {
   for(int j=1;j<=n;j++) {
    dist[i][j]=min(dist[i][j],dist[i][k]+dist[k][j]);
   }
  }
 }
}

void print() {
 int ans=0;
 for(int i=2;i<=m;i++) {
  ans+=dist[a[i-1]][a[i]];
 }
 printf("%d",ans);
}

int main() {
 readin();
 floyd();
 print();
 
 return 0;
}
```

## 树状数组

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 500000
#define read(x) scanf("%d",&x)
#define lowbit(x) (x&-x)

int n,m;
int sum[maxn+5];

void add(int x,int w) {
 while(x<=n) {
  sum[x]+=w;
  x+=lowbit(x);
 }
}

int query(int x) {
 int ans=0;
 while(x>0) {
  ans+=sum[x];
  x-=lowbit(x);
 }
 return ans;
}

int main() {
 read(n),read(m);
 for(int i=1;i<=n;i++) {
  int x;
  read(x);
  add(i,x);
 }
 while(m--) {
  int opr,x,y;
  read(opr),read(x),read(y);
  if(opr==1) add(x,y);
  else printf("%d\n",query(y)-query(x-1));
 } 
 return 0;
}
```

## kmp

注意：nxt数组的意义——最长公共前后缀长

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 1000000

char a[maxn+5],b[maxn+5];
int nxt[maxn+5];
int n,m;

int main(){
 scanf("%s%s",a+1,b+1);
 n=strlen(a+1),m=strlen(b+1);
 
 for(int i=2;i<=m;i++) {
  int j=nxt[i-1];
  while(j&&b[i]!=b[j+1]) j=nxt[j];
  if(b[j+1]!=b[i]) nxt[i]=0;
  else nxt[i]=j+1;
 }
 
 int j=0;
 for(int i=1;i<=n;i++) {
  while(a[i]!=b[j+1]&&j) j=nxt[j];
  if(a[i]==b[j+1]) j++;
  if(j==m) {
   printf("%d\n",i-m+1);
  }
 }
 
 for(int i=1;i<=m;i++) printf("%d ",nxt[i]);
 
 return 0;
}
```

## lca倍增

注意：处理到第2^20个祖先就足够过至少1e6的数据范围了，开大了会影响效率

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 500000
#define maxm 500000
#define read(x) scanf("%d",&x)

int n,m,rt;
vector<int> tr[maxn+5];
int anc[maxn+5][30];
int d[maxn+5];

void dfs(int x,int fa) {
 anc[x][0]=fa;
 for(int i=1;i<=20;i++) {
  anc[x][i]=anc[anc[x][i-1]][i-1];
 }
 d[x]=d[fa]+1;
 for(int i=0;i<tr[x].size();i++) {
  int y=tr[x][i];
  if(y==fa) continue;
  dfs(y,x);
 }
}

int findLCA(int x,int y) {
 if(d[x]<d[y]) swap(x,y);
 for(int i=20;i>=0;i--) {
  if(d[anc[x][i]]>=d[y]) x=anc[x][i];
 }
 if(x==y) return x;
 for(int i=20;i>=0;i--) {
  if(anc[x][i]!=anc[y][i]) {
   x=anc[x][i],y=anc[y][i];
  }
 }
 return anc[x][0];
}

int main() {
 read(n),read(m),read(rt);
 for(int i=1;i<n;i++) {
  int x,y;
  read(x),read(y);
  tr[x].push_back(y);
  tr[y].push_back(x);
 }
 
 dfs(rt,0);
 
 while(m--) {
  int x,y;
  read(x),read(y);
  printf("%d\n",findLCA(x,y));
 }
 
 return 0;
}
```

## 二分图匹配

注意：要用use数组标记下访问过的点，不然会重复搜索

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 1000
#define read(x) scanf("%d",&x)

int n,m,e;
int g[maxn+5][maxn+5];
int mch[maxn+5];
bool use[maxn+5];

bool dfs(int x) {
 for(int i=1; i<=m; i++) {
  if(g[x][i]&&0==use[i]) {
   use[i]=true;
   if(0==mch[i]||dfs(mch[i])) {
    mch[i]=x;
    return true;
   }
  }
 }
 return false;
}

int main() {
 read(n),read(m),read(e);
 for(int i=1; i<=e; i++) {
  int x,y;
  read(x),read(y);
  if(x>n||y>m) continue;
  g[x][y]=true;
 }

 int cnt=0;
 for(int i=1; i<=n; i++) {
  memset(use,0,sizeof(use));
  if(dfs(i)) cnt++;
 }
 printf("%d",cnt);

 return 0;
}
```

## 割点

注意：第一个节点的特判

```cpp
#include<bits/stdc++.h>
using namespace std;

#define read(x) scanf("%d",&x)
#define maxn 100000

int n,m;
vector<int> g[maxn+5];

int pre[maxn+5],low[maxn+5],cnt;
bool iscut[maxn+5];

void tarjan(int x,int fa) {
 pre[x]=low[x]=++cnt;
 int chd=0;
 for(int i=0;i<g[x].size();i++) {
  int y=g[x][i];
  if(y==fa) continue;
  if(!pre[y]) {
   chd++;
   tarjan(y,x);
   if(pre[x]<=low[y]&&fa) iscut[x]=true;
   low[x]=min(low[x],low[y]);
  } else if(pre[y]<pre[x]) low[x]=min(low[x],pre[y]);
 }
 if(fa==0&&chd>1) iscut[x]=true;
}

int main() {
 read(n),read(m);
 for(int i=1;i<=m;i++) {
  int x,y;
  read(x),read(y);
  g[x].push_back(y),g[y].push_back(x);
 }
 
 for(int i=1;i<=n;i++) {
  if(!pre[i]) tarjan(i,0);
 }
 
 int ans=0;
 for(int i=1;i<=n;i++) if(iscut[i]) ans++;
 printf("%d\n",ans);
 for(int i=1;i<=n;i++) if(iscut[i]) printf("%d ",i);
 
 return 0;
}
```

## 缩点

注意：找反边时注意要在同一次dfs中遍历到的点才能算，即需要判断```col[y]==0```

```cpp
#include<bits/stdc++.h>
using namespace std;

#define read(x) scanf("%d",&x)
#define maxn 100000

int n,m;
vector<int> g[maxn+5];
int v[maxn+5];

int pre[maxn+5],low[maxn+5],cnt=0;
stack<int> stk;

int col[maxn+5],sum,w[maxn+5];
vector<int> a[maxn+5];

int dist[maxn+5];

int dfs(int x) {
 if(dist[x]) return dist[x];
 for(int i=0;i<a[x].size();i++) {
  dist[x]=max(dist[x],dfs(a[x][i]));
 }
 dist[x]+=w[x];
 return dist[x];
}

void make_a() {
 for(int i=1;i<=n;i++) {
  for(int j=0;j<g[i].size();j++) {
   int y=g[i][j];
   if(col[i]!=col[y]) a[col[i]].push_back(col[y]);
  }
 }
}

void tarjan(int x) {
 pre[x]=low[x]=++cnt;
 stk.push(x);
 
 for(int i=0;i<g[x].size();i++) {
  int y=g[x][i];
  if(!pre[y]) {
   tarjan(y);
   low[x]=min(low[x],low[y]);
  } else if(pre[x]>pre[y]&&!col[y]) low[x]=min(low[x],pre[y]);
 }
 
 if(low[x]>=pre[x]) {
  int u;sum++;
  do{
   u=stk.top();stk.pop();
   col[u]=sum;
   w[sum]+=v[u];
  } while(u!=x);
 }
}

int main() {
 read(n),read(m);
 for(int i=1;i<=n;i++) read(v[i]);
 for(int i=1;i<=m;i++) {
  int x,y;
  read(x),read(y);
  g[x].push_back(y);
 }
 
 for(int i=1;i<=n;i++) {
  if(!pre[i]) tarjan(i);
 }
 
 make_a();
 
 for(int i=1;i<=sum;i++) if(!dist[i]) dfs(i);
 
 int ans=0;
 for(int i=1;i<=sum;i++) ans=max(ans,dist[i]);

 printf("%d",ans);
 
 return 0;
```

## 最大流 dinic

```cpp
#include<bits/stdc++.h>
using namespace std;

#define maxn 200000
#define read(x) scanf("%d",&x)
#define inf (1<<30)

struct Edge{
 int u,v,w;
 Edge(){}
 Edge(int uu,int vv,int ww) {u=uu,v=vv,w=ww;}
};

int n,m,S,T;

Edge e[maxn+5];
int h[maxn+5],nxt[maxn+5],cnt=-1;

void add_edge(int u,int v,int w) {
 e[++cnt]=Edge(u,v,w);
 nxt[cnt]=h[u];
 h[u]=cnt;
}

int c[maxn+5],d[maxn+5];

queue<int> que;

bool bfs() {
 memset(d,0,sizeof(d)),d[S]=1;
 que.push(S);
 while(!que.empty()) {
  int x=que.front();que.pop();
  for(int i=h[x];~i;i=nxt[i]) {
   Edge y=e[i];
   if(d[y.v]||y.w==0) continue;
   d[y.v]=d[x]+1;
   que.push(y.v);
  }
 }
 
 return d[T];
}

int dfs(int x,int w) {
 if(x==T) return w;
 for(int& i=c[x];~i;i=nxt[i]) {
  Edge y=e[i];
  if(d[y.v]!=d[x]+1||y.w==0) continue;
  int z=dfs(y.v,min(w,y.w));
  if(z) {
   e[i].w-=z;
   e[i^1].w+=z;
   return z;
  }
 }
 return 0;
}

int dinic() {
 int ans=0;
 while(bfs()) {
  for(int i=1;i<=n;i++) c[i]=h[i];
  while(int x=dfs(S,inf)) ans+=x;
 }
 return ans;
}

int main() {
 memset(nxt,-1,sizeof(nxt));
 memset(h,-1,sizeof(h));
 read(n),read(m),read(S),read(T);
 for(int i=1;i<=m;i++) {
  int x,y,z;
  read(x),read(y),read(z);
  add_edge(x,y,z);add_edge(y,x,0);
 }
 int ans=dinic();
 printf("%d",ans);
 
 return 0;
}
```

## 加法乘法求和线段树

```cpp
#include <bits/stdc++.h>
#define int long long
#define maxn 400010

using namespace std;

int n, m, p;

struct a
{
    int v, mul, add;
} xds[maxn];

int num[maxn];

inline void build(int root, int l, int r)
{
    xds[root].add = 0;
    xds[root].mul = 1;
    if (l == r)
    {
        xds[root].v = num[l] % p;
        return;
    }
    int m = (l + r) / 2;
    build(root * 2, l, m);
    build(root * 2 + 1, m + 1, r);
    xds[root].v = (xds[root * 2].v + xds[root * 2 + 1].v) % p;
}

inline void pushdown(int root, int l, int r)
{
    int m = (l + r) / 2;
    //value
    xds[root * 2].v *= xds[root].mul;
    xds[root * 2].v %= p;
    xds[root * 2].v += xds[root].add * (m - l + 1);
    xds[root * 2].v %= p;
    xds[root * 2 + 1].v *= xds[root].mul;
    xds[root * 2 + 1].v %= p;
    xds[root * 2 + 1].v += xds[root].add * (r - m);
    xds[root * 2 + 1].v %= p;

    //lazytag
    xds[root * 2].mul *= xds[root].mul;
    xds[root * 2].mul %= p;
    xds[root * 2].add *= xds[root].mul;
    xds[root * 2].add %= p;
    xds[root * 2].add += xds[root].add;
    xds[root * 2].add %= p;

    xds[root * 2 + 1].mul *= xds[root].mul;
    xds[root * 2 + 1].mul %= p;
    xds[root * 2 + 1].add *= xds[root].mul;
    xds[root * 2 + 1].add %= p;
    xds[root * 2 + 1].add += xds[root].add;
    xds[root * 2 + 1].add %= p;

    //init
    xds[root].mul = 1;
    xds[root].add = 0;
}

inline void mul(int root, int l, int r, int f, int t, int k)
{
    if (r < f || t < l)
    {
        return;
    }
    if (f <= l && r <= t)
    {
        xds[root].mul *= k;
        xds[root].mul %= p;
        xds[root].add *= k;
        xds[root].add %= p;
        xds[root].v *= k;
        xds[root].v %= p;
        return;
    }
    int m = (l + r) / 2;
    pushdown(root, l, r);
    mul(root * 2, l, m, f, t, k);
    mul(root * 2 + 1, m + 1, r, f, t, k);
    xds[root].v = (xds[root * 2].v + xds[root * 2 + 1].v) % p;
}

inline void add(int root, int l, int r, int f, int t, int k)
{
    if (r < f || t < l)
    {
        return;
    }
    if (f <= l && r <= t)
    {
        xds[root].add += k;
        xds[root].add %= p;
        xds[root].v += k * (r - l + 1);
        xds[root].v %= p;
        return;
    }
    int m = (l + r) / 2;
    pushdown(root, l, r);
    add(root * 2, l, m, f, t, k);
    add(root * 2 + 1, m + 1, r, f, t, k);
    xds[root].v = (xds[root * 2].v + xds[root * 2 + 1].v) % p;
}

inline int query(int root, int l, int r, int f, int t)
{
    if (r < f || t < l)
    {
        return 0;
    }
    if (f <= l && r <= t)
    {
        return xds[root].v % p;
    }
    pushdown(root, l, r);
    int m = (l + r) / 2;
    return (query(root * 2, l, m, f, t) + query(root * 2 + 1, m + 1, r, f, t)) % p;
}

signed main()
{
    int tp1, tp2, tp3, tp4;
    scanf("%lld%lld%lld", &n, &m, &p);
    for (int i = 1; i <= n; i++)
    {
        scanf("%lld", &num[i]);
    }
    build(1, 1, n);
    while (m--)
    {
        scanf("%lld", &tp1);
        switch (tp1)
        {
        case 1:
            scanf("%lld%lld%lld", &tp2, &tp3, &tp4);
            mul(1, 1, n, tp2, tp3, tp4);
            break;
        case 2:
            scanf("%lld%lld%lld", &tp2, &tp3, &tp4);
            add(1, 1, n, tp2, tp3, tp4);
            break;
        case 3:
            scanf("%lld%lld", &tp2, &tp3);
            cout << query(1, 1, n, tp2, tp3) << endl;
            break;
        }
    }
    return 0;
}
```
