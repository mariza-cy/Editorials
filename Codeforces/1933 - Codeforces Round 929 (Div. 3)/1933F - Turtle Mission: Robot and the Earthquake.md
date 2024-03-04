# 1933F - Turtle Mission: Robot and the Earthquake
You can view the problem statement and submit [here](https://codeforces.com/contest/1933/problem/F)

### Step 1
In order to find a path, we need to know what moves we can make from a certain cell at a certain time. However, we have $n⋅m$ cells and $m$ possible positions for the rocks (since every $n$ units of time they will return to their original position). That means we must find a solution where we only need to know either the position or the time.

We obviously can't solve the problem without knowing the position of RT, so we must find a way to do that without knowing the current time. The fact that the columns are cyclic allows us to do just that. Because of it, we don't actually need to know the coordinates of RT - just its position ralative to the rocks, so we can keep track of its position in the array $a$, moving it one cell downwards on every unit of time instead of moving the rocks upwards.

This means that the operations will change, too:
 - When moving upwards, it will stay on the same cell (as the rocks will also move one cell upwards).
 - When moving downwards, it will move two cells cyclically downwards, i.e. from $(i,j)$ to $((i+2) \bmod n,j)$.
 - When moving to the right, it will move one cell to the right and one cell cyclically downwards, i.e. from $(i,j)$ to $((i+1) \bmod n,j+1)$.

### Step 2
Now that the positions of the rocks are always the same we can use the grid as an unweighted graph, so we can use BFS to find the shortest path to the end. But first we need to find which moves are blocked by the rocks from each cell. Obviously, RT can't move to a cell when there is a rock there. It also can't move two cells downwards from $(i,j)$ when there is a rock at $((i+1) \bmod n,j)$, as it needs to pass from that cell too. However, as we can see in the problem statement, it can move to the right even if there is a rock in a cell he technically needs to visit to get to that cell.

### Step 3
There is still a problem: on each unit of time, the position of the goal (i.e. the cell $(n-1,m-1)$ of the planet) relative to the rocks will change, so getting to the cell $a$<sub>$i,j$</sub> is not enough. On each unit of time, the goal will move one cell cyclically downwards. So once it reaches the last column, RT must use the first operation (moving upwards) to stay in the same position until the goal reaches it. 

We can easily calculate that:
Let $dist$<sub>$i,j$</sub> be the least number of units of time needed to reach $(i,j)$. Since every $n$ units of time the goal will return to $(n-1,m-1)$, after using the shortest path to reach $(i,m-1)$ it will move $dist$<sub>$i,m-1$</sub> cells downwards, so it will be at $((n-1+dist$<sub>$i,m-1$</sub>$) \bmod n,m-1)$. 
Therefore, if RT is at $(i,m-1)$, it will need $(n+i-(n-1+dist$<sub>$i,m-1$</sub>$) \bmod n) \bmod n$ units of time to reach the goal (we add n to ensure the number stays positive).

Now we can simply find the minimum value of $dist$<sub>$i,j$</sub>$+(n+i-(n-1+dist$<sub>$i,m-1$</sub>$) \bmod n) \bmod n$ among all $i$, $0 \leq i < n$.

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main(){
    ll tc;
    cin>>tc;
    while(tc--){
        ll n, m;
        cin>>n>>m;
        ll a[n][m];
        for(ll i=0; i<n; i++){
            for(ll j=0; j<m; j++){
                cin>>a[i][j];
            }
        }

        ll dist[n][m];
        for(ll i=0; i<n; i++){
            for(ll j=0; j<m; j++){
                dist[i][j]=-1;
            }
        }
        dist[0][0]=0;
        queue<pair<ll,ll>> q; q.push({0,0});
        while(!q.empty()){
            ll i=q.front().first, j=q.front().second;
            q.pop();

            if(a[(i+1)%n][j]!=1 && a[(i+2)%n][j]==0){ 
                dist[(i+2)%n][j]=dist[i][j]+1;
                q.push({(i+2)%n,j});
                a[(i+2)%n][j]=2;
            }

            if(j<m-1 && a[(i+1)%n][j+1]==0){
                dist[(i+1)%n][j+1]=dist[i][j]+1;
                q.push({(i+1)%n,j+1});
                a[(i+1)%n][j+1]=2;
            }
        }

        ll ans=1e18;
        for(ll i=0; i<n; i++){
            if(a[i][m-1]==2) ans=min(ans,dist[i][m-1]+(n+i+1-dist[i][m-1]%n)%n);
        }
        cout<<((ans==1e18)?-1:ans)<<endl;
    }

    return 0;
}
```
