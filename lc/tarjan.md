## use tarjan to cal 割点
**lc上存在类似题目**
**https://leetcode.cn/problems/critical-connections-in-a-network/**
```python
from typing import List, Tuple
 
 
'''
Trajan算法求无向图的桥
'''
 
class Tarjan:
    # 求无向连通图的桥
    @staticmethod
    def getCuttingPointAndCuttingEdge(edges: List[Tuple]):
        link, dfn, low = {}, {}, {}# link为字典邻接表
        global_time = [0]
        for a, b in edges:
            if a not in link:
                link[a] = []
            if b not in link:
                link[b] = []
            link[a].append(b)#无向图
            link[b].append(a)#无向图
            dfn[a], dfn[b] = 0x7fffffff, 0x7fffffff
            low[a], low[b] = 0x7fffffff, 0x7fffffff
 
 
        cutting_points, cutting_edges = [], []
 
        def dfs(cur, prev, root):
            global_time[0] += 1
            # dfn 只有当前时间戳可以更新
            # low 记录的是可以到达的祖先节点的更小的时间戳(前提是可以通过非搜索树的路径)
            dfn[cur], low[cur] = global_time[0], global_time[0]
 
            children_cnt = 0
            flag = False
            for next in link[cur]:
                if next != prev:
                    # 未访问过先访问他
                    if dfn[next] == 0x7fffffff:
                        # 记录儿子节点数量
                        children_cnt += 1
                        # 先把dfn 数组填满
                        dfs(next, cur, root)
                        # 不是根且无法跨越他回到祖先
                        if cur != root and low[next] >= dfn[cur]:
                            flag = True
                        # 儿子可以到达的, 父亲一定可以到达
                        low[cur] = min(low[cur], low[next])
                        # 是否是割边
                        if low[next] > dfn[cur]:
                            cutting_edges.append([cur, next] if cur < next else [next, cur])
                    else:
                        low[cur] = min(low[cur], dfn[next])
            # 如果是根节点, 只需要有2棵子树就说明去掉他图不连通
            # 否则: flag == True
            if flag or (cur == root and children_cnt >= 2):
                cutting_points.append(cur)
            print("points: ", cutting_points)
            print("edges: ", cutting_edges)
 
        dfs(edges[0][0], None, edges[0][0])
        return cutting_points, cutting_edges
 
 
class Solution:
    def criticalConnections(self, n: int, connections: List[List[int]]) -> List[List[int]]:
        edges = [(a, b) for a, b in connections]
        cutting_dots, cutting_edges = Tarjan.getCuttingPointAndCuttingEdge(edges)
        return [[a, b] for a, b in cutting_edges]
 
 
connections = [[0,1],[1,2],[2,0],[1,3]]
n = 4
s = Solution()
result = s.criticalConnections(n,connections)
print(result)
```