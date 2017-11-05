# Strongly-connected-branches-of-DFS

Strongly connected branches of DFS

#include <iostream>
  
#include <algorithm>
  
#include <vector>
  
using namespace std;

#define M 20005

vector<int> g[M],gt[M];
  
bool used[M];

int ft[M],sort_v[M],tim;

bool comp(const int &u,const int &v){

	return ft[u]>ft[v];
  
}

inline int findp(int set[],int n){

	int n2=n;
  
	while(set[n]!=0) n=set[n];
  
	if(n2!=n) set[n2]=n;
  
	return n;
  
}

inline bool uni(int set[],int a,int b){ 

	int ac=0,a2=a,b2=b,bc=0,t;
  
	while(set[a]!=0) {a=set[a];ac++;}
  
	while(a2!=a) {t=set[a2]; set[a2]=a; a2=t;};
  
	while(set[b]!=0) {b=set[b];bc++;}
  
	while(b2!=b) {t=set[b2]; set[b2]=b; b2=t;}; 
  
	if(a==b) return false;
  
	if(ac<bc) set[a]=b;
  
	else set[b]=a;
  
	return true;
  
}

void dfs(vector<int> g[M],int u){
  
	if(used[u]) return;
  
	tim++;
  
	used[u]=true;
  
	int i;
  
	for(i=0;i<g[u].size();i++){
  
		dfs(g,g[u][i]);
    
	}
  
	tim++;
  
	ft[u]=tim;
  
	return;
  
}

void dfs2(vector<int> g[],int u,int r,int set[]){
  
	if(used[u]) return;
  
	uni(set,u,r);
  

	used[u]=true;
  
	int i;
  
	for(i=0;i<g[u].size();i++){
  
		dfs2(g,g[u][i],u,set);
    
	}
  
	return;
  
}

void scc(int n,vector<int> g[M],int set[]){
  
	int i,j;
  
	tim=0;
  
	memset(used,0,sizeof(used));
  
	memset(set,0,sizeof(set));
  
	for(i=1;i<=n;i++) sort_v[i]=i;
  
	for(i=1;i<=n;i++) if(!used[i]) dfs(g,i); //compute finishing times
  
	sort(&sort_v[1],&sort_v[n+1],comp); //decreasing f[u] order
  
	memset(used,0,sizeof(used));  
  
	for(i=1;i<=n;i++) for(j=0;j<g[i].size();j++) gt[g[i][j]].push_back(i); //compute gt
  
	for(i=1;i<=n;i++) if(!used[sort_v[i]]) dfs2(gt,sort_v[i],sort_v[i],set);  //make the scc
  
}

int main(){

	int i,j,n,m,u,v,set[M];
  
	cin >> n >> m;
  
	for(i=0;i<m;i++){
  
		scanf("%d%d",&u,&v);
    
		g[u].push_back(v);
    
	}
  
	scc(n,g,set);
  
	int pi=1,ptosc[M];
  
	struct Scc{
  
		int p,n;
    
	}sc[M];
  
	memset(sc,0,sizeof(sc));
  
	for(i=1;i<=n;i++){
  
		int p=findp(set,i);
    
		if(sc[p].p==0) {sc[p].p=pi; ptosc[pi++]=p;}
    
		sc[p].n++;
    
	}
  
	int n2=pi-1,od[M];
  
	memset(od,0,sizeof(od));
  
	for(i=1;i<=n;i++){
  
		for(j=0;j<g[i].size();j++){
    
			u=findp(set,i); v=findp(set,g[i][j]);
      

			if(sc[u].p!=sc[v].p) od[sc[u].p]++;
      
		}
    
	}
  
	int sum=0,s1=0;
  
	for(i=1;i<=n2;i++) if(od[i]==0) {s1++; sum+=sc[ptosc[i]].n;}
  
	if(s1!=1) sum=0;
  
	cout << sum << endl;
  
}
