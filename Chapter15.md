# 第十五章：图

## 面试题105：最大的岛屿
### 题目
海洋岛屿地图可以用由0、1组成的二维数组表示，水平或者竖直方向相连的一组1表示一个岛屿。请计算最大的岛屿的面积（即岛屿中1的数目）。例如，在图15.5中有4个岛屿，其中最大的岛屿的面积为5。
 
![图15.5](./Figures/1505.png)

图15.5：用0、1矩阵表示的海洋岛屿地图。地图中有4个岛屿，最大的岛屿的面积为5。

### 参考代码
#### 解法一
``` python
# bfs
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        def getArea(i, j):
            que = collections.deque()
            que.append([i, j])
            visited[i][j] = 1
            area = 0
            while que:
                a, b = que.popleft()
                area += 1
                for dx, dy in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                    x = a + dx
                    y = b + dy
                    if x >= 0 and x < m and y >= 0 and y < n and grid[x][y] and not visited[x][y]:
                        que.append([x, y])
                        visited[x][y] = 1 ###
            return area

        if not grid or not grid[0]: return 0
        m, n = len(grid), len(grid[0])
        visited = [[0] * n for _ in range(m)]
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] and not visited[i][j]:
                    res = max(res, getArea(i, j))
        return res
```

#### 解法二
``` python
# dfs stack
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        def getArea(i, j):
            stack = []
            stack.append([i, j])
            visited[i][j] = 1
            area = 0
            while stack:
                a, b = stack.pop()
                area += 1
                for dx, dy in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                    x = a + dx
                    y = b + dy
                    if x >= 0 and x < m and y >= 0 and y < n and grid[x][y] and not visited[x][y]:
                        stack.append([x, y])
                        visited[x][y] = 1 ###
            return area

        if not grid or not grid[0]: return 0
        m, n = len(grid), len(grid[0])
        visited = [[0] * n for _ in range(m)]
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] and not visited[i][j]:
                    res = max(res, getArea(i, j))
        return res
```

#### 解法三
``` python
# dfs recursion
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        def getArea(i, j):
            area = 1
            visited[i][j] = 1
            for dx, dy in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                x = i + dx
                y = j + dy
                if x >= 0 and x < m and y >= 0 and y < n and grid[x][y] and not visited[x][y]:
                    area += getArea(x, y)
            return area

        if not grid or not grid[0]: return 0
        m, n = len(grid), len(grid[0])
        visited = [[0] * n for _ in range(m)]
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] and not visited[i][j]:
                    res = max(res, getArea(i, j))
        return res
```

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        def getArea(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or not grid[i][j] or visited[i][j]:
                return 0
            area = 1
            visited[i][j] = 1
            for dx, dy in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                x = i + dx
                y = j + dy
                area += getArea(x, y)
            return area

        if not grid or not grid[0]: return 0
        m, n = len(grid), len(grid[0])
        visited = [[0] * n for _ in range(m)]
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] and not visited[i][j]:
                    res = max(res, getArea(i, j))
        return res
```

## 面试题106：二分图
### 题目
如果能将一个图的结点分成A、B两部分，使得任意一条边的一个结点属于A另一个结点属于B，那么该图就是一个二分图。输入一个由数组graph表示的图，graph[i]里包含所有和结点i相邻的结点，请判断该图是否为二分图。例如，如果输入graph为[[1, 3], [0, 2], [1, 3], [0, 2]]，那么我们可以将结点分为{0, 2}、{1, 3}两部分，因此该图是一个二分图，如图15.7（a）所示。如果输入graph为[[1,2,3],[0,2],[0,1,3],[0,2]]，则不是一个二分图，如图15.7（b）所示。

 ![图15.7](./Figures/1507.png)

图15.7：二分图与非二分图。（a）二分图。（b）不是二分图。

### 参考代码
#### 解法一
``` python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def setColor(i, color):
            que = collections.deque()
            que.append(i)
            colors[i] = color
            while que:
                v = que.popleft()
                for neighbor in graph[v]:
                    if colors[neighbor] >= 0:
                        if colors[neighbor] == colors[v]:
                            return False
                    else:
                        que.append(neighbor)
                        colors[neighbor] = 1 - colors[v]
            return True

        n = len(graph)
        colors = [-1] * n
        for i in range(n):
            if colors[i] == -1:
                if not setColor(i, 0):
                    return False
        return True
```
#### 解法二
``` python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def setColor(i, color):
            stack = []
            stack.append(i)
            colors[i] = color
            while stack:
                v = stack.pop()
                for neighbor in graph[v]:
                    if colors[neighbor] >= 0:
                        if colors[neighbor] == colors[v]:
                            return False
                    else:
                        stack.append(neighbor)
                        colors[neighbor] = 1 - colors[v]
            return True

        n = len(graph)
        colors = [-1] * n
        for i in range(n):
            if colors[i] == -1:
                if not setColor(i, 0):
                    return False
        return True
```

```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def setColor(i, color):
            if colors[i] >= 0:
                return colors[i] == color
            
            colors[i] = color ###
            for neighbor in graph[i]:
                if not setColor(neighbor, 1 - color):
                    return False
            return True

        n = len(graph)
        colors = [-1] * n
        for i in range(n):
            if colors[i] == -1:
                if not setColor(i, 0):
                    return False
        return True
```

## 面试题107：矩阵中的距离
### 题目
输入一个有0、1组成的矩阵M，请输出一个大小相同的矩阵D，矩阵D中的每个格子是M中对应格子离最近的0的距离。水平或者竖直方向相邻两个格子的距离为1。假设矩阵M中至少要有一个0。例如，图15.8（a）是一个只包含0、1的矩阵M，它每个格子离最近的0的距离如15.8（b）的矩阵D所示。矩阵M[0][0]等于0，因此它离最近的0的距离是0，所以D[0][0]等于0。M[2][1]等于1，离它最近的0的坐标是(0, 1)、(1, 0)、(1, 2)，它们离坐标(2, 1)的距离都是2，所以D[2][1]等于2。

![图15.8](./Figures/1508.png)

图15.8：矩阵中离0最近的距离。（a）一个只包含0、1的矩阵。（b）每个格子为（a）中矩阵相应位置离最近的0的距离。

### 参考代码
``` python
class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        if not mat or not mat[0]: return [[]]
        m, n = len(mat), len(mat[0])
        distance = [[sys.maxsize] * n for _ in range(m)]
        que = collections.deque()
        for i in range(m):
            for j in range(n):
                if mat[i][j] == 0:
                    distance[i][j] = 0
                    que.append([i, j])
        while que:
            a, b = que.popleft()
            dist = distance[a][b]
            for dx, dy in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                x, y = a + dx, b + dy
                if x >= 0 and x < m and y >= 0 and y < n:
                    if distance[x][y] > dist + 1:
                        distance[x][y] = dist + 1
                        que.append([x, y])
        return distance 
```

## 面试题108：单词演变
### 题目
输入两个长度相同但内容不同的单词（beginWord和endWord）和一个单词列表，请问从beginWord到endWord的演变序列的最短长度，要求每一步只能改变单词中的一个字母，并且演变过程中每一步得到的单词都必须在给定的单词列表中。如果不能从beginWord演变到endWord，则返回0。假设所有单词只包含英文的小写字母。例如，如果beginWord为"hit"，endWord为"cog"，单词列表为["hot", "dot", "dog", "lot", "log", "cog"]，则演变序列的最短长度为5，一个可行的演变序列为"hit"→"hot"→"dot"→"dog"→"cog"。

### 参考代码
#### 解法一
``` python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        def getNeighbors(word):
            neighbors = []
            word = list(word)
            for i in range(len(word)):
                old = word[i]
                for j in range(26):
                    if ord(word[i]) - ord('a') != j:
                        word[i] = chr(ord('a') + j)
                        neighbors.append(''.join(word))
                word[i] = old
            return neighbors

        que1 = collections.deque()
        que2 = collections.deque()
        notVisited = set(wordList)
        que1.append(beginWord)
        length = 1
        while que1:
            word = que1.popleft()
            if word == endWord:
                return length
            words = getNeighbors(word)
            for w in words:
                if w in notVisited:
                    que2.append(w)
                    notVisited.remove(w)

            if not que1:
                length += 1
                que1 = que2
                que2 = collections.deque()
        return 0
```
#### 解法二
``` python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        def getNeighbors(word):
            neighbors = []
            word = list(word)
            for i in range(len(word)):
                old = word[i]
                for j in range(26):
                    if ord(word[i]) - ord('a') != j:
                        word[i] = chr(ord('a') + j)
                        neighbors.append(''.join(word))
                word[i] = old
            return neighbors

        notVisited = set(wordList)
        if endWord not in notVisited: return 0 ###
        if endWord == beginWord: return 1
        set1, set2 = set(), set()
        set1.add(beginWord)
        set2.add(endWord)
        length = 2
        while set1 and set2:
            if len(set1) > len(set2):
                set1, set2 = set2, set1
            set3 = set()
            for word in set1:
                neighbors = getNeighbors(word)
                for w in neighbors:
                    if w in set2:
                        return length
                    if w in notVisited:
                        set3.add(w)
                        notVisited.remove(w)
            
            set1 = set3
            length += 1
        
        return 0
```

## 面试题109：开密码锁
### 题目
一个密码锁由四个环形转轮组成，每个转轮由0到9十个数字组成。我们每次可以上下拨动一个转轮，比如可以将  一个转轮从0拨到9，也可以从9拨到0。密码锁由若干个死锁状态，一旦四个转轮被拨到某个死锁状态，这个锁就不可能打开了。密码锁的状态可以用一个长度为4的字符串表示，字符串中每个字符对应一个转轮上的数字。输入密码锁的密码和它的所有死锁状态，请问至少需要拨动转轮多少次才能从起始状态"0000"开始打开这个密码锁？

例如，如果某个密码锁的密码是"0202"，它的死锁状态列表是["0102", "0201"]，那么至少需要拨动转轮6次才能打开这个密码锁，一个可行的开锁状态序列是"0000"→"1000"→"1100"→"1200"→"1201"→"1202"→"0202"。虽然序列"0000"→"0001"→"0002"→"0102"→"0202"更短，只需要拨动4次转轮，但它包含死锁状态"0102"，因此这是一个无效的开锁序列。

### 参考代码
``` python
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        def getNeighbors(state):
            state = list(state)
            neighbors = []
            for i in range(len(state)):
                old = state[i]
                state[i] = '0' if old == '9' else chr(ord(old) + 1)
                neighbors.append(''.join(state))
                state[i] = '9' if old == '0' else chr(ord(old) - 1)
                neighbors.append(''.join(state))
                state[i] = old ###
            return neighbors

        init = '0000'
        deads = set(deadends)
        visited = set()
        if init in deads or target in deads:
            return -1
        if init == target: return 0
        steps = 0
        que1 = collections.deque()
        que2 = collections.deque()
        que1.append(init)
        visited.add(init) ###
        while que1:
            state = que1.popleft()
            if state == target: return steps

            nexts = getNeighbors(state)
            for next in nexts:
                if next not in deads and next not in visited:
                    que2.append(next)
                    visited.add(next)
            
            if not que1:
                que1 = que2
                que2 = collections.deque()
                steps += 1
        return -1
```

```python
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        def getNeighbors(state):
            state = list(state)
            neighbors = []
            for i in range(len(state)):
                old = state[i]
                state[i] = '0' if old == '9' else chr(ord(old) + 1)
                neighbors.append(''.join(state))
                state[i] = '9' if old == '0' else chr(ord(old) - 1)
                neighbors.append(''.join(state))
                state[i] = old ###
            return neighbors

        init = '0000'
        deads = set(deadends)
        visited = set()
        if init in deads or target in deads:
            return -1
        if init == target: return 0
        set1, set2 = set(), set()
        set1.add(init)
        set2.add(target)
        visited.add(init) ###
        visited.add(target) ###
        steps = 1
        while set1 and set2:
            if len(set1) > len(set2):
                set1, set2 = set2, set1

            set3 = set()
            for state in set1:
                nexts = getNeighbors(state)
                for next in nexts:
                    if next in set2:
                        return steps
                    if next not in deads and next not in visited:
                        set3.add(next)
                        visited.add(next)
            
            set1 = set3
            steps += 1
        
        return -1
```

## 面试题110：所有路径
### 题目
一个有向无环图由n个结点（标号从0到n-1，n≥2）组成，请找出所有从结点0到结点n-1的所有路径。图用一个数组graph表示，数组的graph[i]包含所有从结点i能直接到达的结点。例如，输入数组graph为[[1,2], [3], [3], []]，则输出两条从结点0到结点3的路径，分别为0→1→3和0→2→3，如图15.12所示。

![图15.12](./Figures/1512.png)

图15.12：一个有4个结点的有向无环图。从结点0到结点3有两条不同的路径，分别为0→1→3和0→2→3。

### 参考代码
``` python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        def dfs(start, cur):
            if start == n - 1:
                ans.append(cur.copy())
                return
            for next in graph[start]:
                cur.append(next)
                dfs(next, cur)
                cur.pop()

        n = len(graph)
        ans = []
        dfs(0, [0])
        return ans
```

## 面试题111：计算除法
### 题目
输入两个数组equations和values，其中数组equations的每一个元素包含两个表示变量名的字符串，数组values的每个元素是一个浮点数值。如果equations[i]的两个变量名分别是Ai和Bi，那么Ai/Bi=values[i]。再给你一个数组queries，它的每个元素也包含两个变量名。假设任意values[i]大于0。对于queries[j]的两个变量名为Cj和Dj，请计算Cj/Dj的结果。如果不能计算，那么返回-1。

例如，输入数组equations为[["a", "b"], ["b", "c"]]，数组values为[2.0, 3.0]，如果queries为[["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"]]，那么对应的计算结果为[6.0, 0.5, -1.0, 1.0, -1.0]。由equations我们知道a/b=2.0、b/c=3.0，所以a/c=6.0、b/a=0.5、a/a=1.0。

### 参考代码
``` python
class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        def buildGraph():
            graph = {}
            for i in range(n):
                a, b = equations[i][0], equations[i][1]
                if a not in graph:
                    graph[a] = {}
                if b not in graph:
                    graph[b] = {}
                graph[a][b] = values[i]
                graph[b][a] = 1 / values[i]
            return graph

        def dfs(a, b):
            if a == b: return 1.0
            visited.add(a) ###
            for next in graph[a].keys():
                if next not in visited:
                    nextValue = dfs(next, b)
                    if nextValue > 0:
                        return graph[a][next] * nextValue
            
            visited.remove(a) ###
            return -1.0

        n = len(equations)
        graph = buildGraph()
        res = []
        for a, b in queries:
            if a not in graph or b not in graph:
                res.append(-1.0)
            else:
                visited = set()
                res.append(dfs(a, b))
        return res
```

## 面试题112：最长递增路径
### 题目
输入一个有整数组成的矩阵，请找出最长递增路径的长度。矩阵中的路径可以沿着上、下、左、右四个方向前行。例如，图15.10中矩阵的最长递增路径的长度为4，其中一条最长的递增路径为3→4→5→8，如阴影部分所示。 

![图15.15](./Figures/1515.png)

图15.15：矩阵中的路径可以沿着上、下、左、右四个方向前行，其中一条最长的递增路径为3→4→5→8（阴影部分），它的长度为4。

### 参考代码
``` python
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        def dfs(i, j):
            if lengths[i][j] != 0: return lengths[i][j]
            length = 1
            for dx, dy in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                x, y = i + dx, j + dy
                if x >= 0 and x < m and y >= 0 and y < n and matrix[x][y] > matrix[i][j]:
                    path = dfs(x, y)
                    length = max(length, path + 1)
            lengths[i][j] = length
            return length
            
        if not matrix or not matrix[0]: return 0
        m, n = len(matrix), len(matrix[0])
        lengths = [[0] * n for _ in range(m)]
        longest = 0
        for i in range(m):
            for j in range(n):
                length = dfs(i, j)
                longest = max(longest, length)
        return longest
```

## 面试题113：课程顺序
### 题目
n门课程的编号从0到n-1。输入一个数组prerequisites，它的每个元素prerequisites[i]表示两门课程的先修顺序。如果prerequisites[i]=[ai, bi]，那么我们必须先修完可以bi才能修ai。请根据总课程数n和表示先修顺序的prerequisites得出一个可行的修课序列。如果有多个可行的修课序列，则输出任意序列。如果没有可行的修课序列，则输出空序列。例如，总共有4门课程，先修顺序prerequisites为[[1, 0], [2, 0], [3, 1], [3, 2]]，一个可行的修课序列是0→2→1→3。

### 参考代码
``` python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = [[] for _ in range(numCourses)]
        inDegress = [0] * numCourses
        for p, q in prerequisites:
            graph[q].append(p)
            inDegress[p] += 1
        
        que = collections.deque()
        for i in range(numCourses):
            if inDegress[i] == 0:
                que.append(i)

        order = []
        while que:
            course = que.popleft()
            order.append(course)
            for next in graph[course]:
                inDegress[next] -= 1
                if inDegress[next] == 0:
                    que.append(next)

        return order if len(order) == numCourses else []
```

## 面试题114：外星文字典
## 题目
一种外星文语言的字母都是英文字母，但字母的顺序未知。给你该语言排序的单词列表，请推测可能的字母顺序。如果有多个可能的顺序，返回任意一个。如果没有满足条件的字母顺序，返回空字符串。例如，如果输入排序的单词列表为["ac", "ab", "bc", "zc", "zb"]，那么一个可能的字母顺序是"acbz"。

### 参考代码
``` python
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        graph = {}
        inDegress = {}
        for word in words:
            for c in word:
                if c not in graph:
                    graph[c] = set()
                if c not in inDegress:
                    inDegress[c] = 0

        for i in range(1, len(words)):
            w1, w2 = words[i - 1], words[i]
            if w1.startswith(w2) and w1 != w2: ###
                return ''
            for j in range(min(len(w1), len(w2))):
                c1, c2 = w1[j], w2[j]
                if c1 != c2:
                    if c2 not in graph[c1]:
                        graph[c1].add(c2)
                        inDegress[c2] += 1
                    break ###
        
        que = collections.deque()
        for key, value in inDegress.items():
            if value == 0:
                que.append(key)
        order = []
        while que:
            c = que.popleft()
            order.append(c)
            for next in graph[c]:
                inDegress[next] -= 1
                if inDegress[next] == 0:
                    que.append(next)

        return ''.join(order) if len(order) == len(graph) else ''
```

## 面试题115：重建序列
### 题目
长度为n的数组org是数字1到n的一个排列，seqs是若干个序列，请判断org是否为可由seqs重建的唯一序列。重建是指在seqs 中构建最短的公共超序列，即seqs 中的任意序列都是该最短序列的子序列。

例如，如果数组org为[4, 1, 5, 2, 6, 3]，而seqs为[[5, 2, 6, 3], [4, 1, 5, 2]]，因为用[[5, 2, 6, 3], [4, 1, 5, 2]]可以重建出唯一的序列[4, 1, 5, 2, 6, 3]，因此返回true。如果数组org为[1, 2, 3]，而seqs为[[1, 2], [1, 3]]，因为用[[1, 2], [1, 3]]可以重建出两个序列，[1, 2, 3]或者[1, 3, 2]，因此返回false。

### 参考代码
``` python
class Solution:
    def sequenceReconstruction(self, org: List[int], seqs: List[List[int]]) -> bool:
        graph = {}
        inDegress = {}
        for seq in seqs:
            for num in seq:
                if num < 1 or num > len(org):
                    return False
                if num not in graph:
                    graph[num] = set()
                if num not in inDegress:
                    inDegress[num] = 0
            for i in range(1, len(seq)):
                a, b = seq[i - 1], seq[i]
                if b not in graph[a]:
                    graph[a].add(b)
                    inDegress[b] += 1
            
        que = collections.deque()
        for k, v in inDegress.items():
            if v == 0:
                que.append(k)

        order = []
        while len(que) == 1:
            cur = que.popleft()
            order.append(cur)
            for next in graph[cur]:
                inDegress[next] -= 1
                if inDegress[next] == 0:
                    que.append(next)
        
        return org == order
```

## 面试题116：朋友圈
### 题目
假设一个班上有n个同学。同学之间有些是朋友，有些不是。朋友关系是可以传递的。比如A是B的直接朋友，B是C的直接朋友，那么A是C的间接朋友。我们定义朋友圈就是一组直接或间接朋友的同学。输入一个n×n的矩阵M表示班上的朋友关系，如果M[i][j]=1,那么同学i和同学j是直接朋友。请计算该班朋友圈的数目？
例如输入数组[[1, 1, 0], [1, 1, 0], [0, 0, 1]]，则表明同学0和同学1是朋友，他们组成一个朋友圈。同学2一个人组成一个朋友圈。因此朋友圈的数目是2。

### 参考代码
#### 解法一
``` python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def bfs(i):
            que = collections.deque()
            que.append(i)
            visited[i] = 1
            while que:
                cur = que.popleft()
                for j in range(n):
                    if isConnected[cur][j] and not visited[j]:
                        que.append(j)
                        visited[j] = 1
        
        n = len(isConnected)
        visited = [0] * n
        cnt = 0
        for i in range(n):
            if not visited[i]:
                bfs(i)
                cnt += 1
        return cnt
```
#### 解法二
``` python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def findFather(i):
            if fathers[i] != i:
                fathers[i] = findFather(fathers[i])
            return fathers[i]
        
        def findFather1(i):
            r = i
            while r != fathers[r]:
                r = fathers[r]
            return r
        
        def union(i, j):
            father_i = findFather(i)
            father_j = findFather(j)
            if father_i != father_j:
                fathers[father_i] = father_j
                return True
            return False

        n = len(isConnected)
        fathers = [i for i in range(n)]
        cnt = n
        for i in range(n):
            for j in range(i + 1, n):
                if isConnected[i][j] and union(i, j):
                    cnt -= 1
        return cnt
```

## 面试题117：相似的字符串
### 题目
如果交换字符串X中的两个字符能得到字符串Y，那么两个字符串X和Y相似。例如字符串"tars"和"rats"相似（交换下标0和2的两个字符）、字符串"rats"和"arts"相似（交换下标0和1的字符），但字符串"star"和"tars"不相似。

输入一个字符串数组，根据字符串的相似性分组，请问能把输入数组分成几组？如果一个字符串至少和一组字符串中的一个相似，那么它就可以放到该组里去。假设输入数组中的所有字符串长度相同并且两两互为变位词。例如输入数组["tars","rats","arts","star"]，可以分成2组，一组为{"tars", "rats", "arts"}，另一组为{"star"}。

### 参考代码
``` python
class Solution:
    def numSimilarGroups(self, strs: List[str]) -> int:
        def findFather(i):
            if fathers[i] != i:
                fathers[i] = findFather(fathers[i])
            return fathers[i]

        def union(i, j):
            father_i = findFather(i)
            father_j = findFather(j)
            if father_i != father_j:
                fathers[father_i] = father_j
                return True
            return False

        def isSimilar(s1, s2):
            cnt = 0
            for i in range(len(s1)):
                if s1[i] != s2[i]:
                    cnt += 1
            return cnt <= 2

        n = len(strs)
        fathers = [i for i in range(n)]
        cnt = n
        for i in range(n):
            for j in range(i + 1, n):
                if isSimilar(strs[i], strs[j]) and union(i, j):
                    cnt -= 1
        return cnt
```

## 面试题118：多余的边
### 题目
树可以看成是无环的无向图。在一个含有n个结点（结点标号从1到n）的树中添加一条边，就变成一个有环的图。给你一个往树里添加了一条边的图，请找出这条多余的边（用该边连接的两个结点表示）。输入的图由一个二维数组edges表示，数组的每个元素是由一条边连接的两个结点[u, v]（u<v）。如果有多个答案，请输出在数组edges中最后出现的边。

例如，如果输入edges为[[1, 2], [1, 3], [2, 4], [3, 4], [2, 5]]，则它对应的无向图如图15.22所示。输出为边[3, 4]。

![图15.22](./Figures/1522.png)
 
图15.22：由边列表[[1, 2], [1, 3], [2, 4], [3, 4], [2, 5]]构成的图。

### 参考代码
``` python
# dfs
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        def hasPath(start, end):
            if start == end: return True
            visited.add(start)
            for next in graph[start]:
                if next not in visited and hasPath(next, end):
                    return True
            return False

        n = len(edges)
        graph = [[] for _ in range(n + 1)]
        for u, v in edges:
            visited = set()
            if hasPath(u, v):
                return [u, v]
            graph[u].append(v)
            graph[v].append(u)
        return []
```

```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        def findFather(i):
            if fathers[i] != i:
                fathers[i] = findFather(fathers[i])
            return fathers[i]

        def union(i, j):
            father_i = findFather(i)
            father_j = findFather(j)
            if father_i != father_j:
                fathers[father_i] = father_j
                return True
            return False
            
        n = 0
        for u, v in edges:
            n = max(n, u, v)
        fathers = [i for i in range(n + 1)]
        for u, v in edges:
            if not union(u, v):
                return [u, v]
        return []
```

## 面试题119：最长连续序列

### 题目
输入一个无序的整数数组，请计算最长的连续数值序列的长度。例如，输入数组[10, 5, 9, 2, 4, 3]，则最长的连续数值序列是[2, 3, 4, 5]，因此输出4。

### 参考代码
#### 解法一
``` python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        def bfs(num):
            que = collections.deque()
            que.append(num)
            num_set.remove(num)
            length = 1
            while que:
                i = que.popleft()
                for neighbor in [i - 1, i + 1]:
                    if neighbor in num_set:
                        que.append(neighbor)
                        num_set.remove(neighbor)
                        length += 1
            return length
        
        
        num_set = set(nums)
        longest = 0
        while num_set:
            it = iter(num_set)
            longest = max(longest, bfs(next(it))) ###
        return longest
```

#### 解法二
``` python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        def findFather(i):
            if fathers[i] != i:
                fathers[i] = findFather(fathers[i])
            return fathers[i]

        def union(i, j):
            father_i = findFather(i)
            father_j = findFather(j)
            if father_i != father_j:
                fathers[father_i] = father_j
                cnt[father_j] += cnt[father_i]

        num_set = set(nums)
        fathers = {x : x for x in num_set}
        cnt = {x: 1 for x in num_set}
        for num in num_set:
            if num - 1 in num_set:
                union(num, num - 1)
            if num + 1 in num_set:
                union(num, num + 1)
        longest = 0
        for length in cnt.values():
            longest = max(longest, length)
        return longest
```
#### 解法三
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        num_set = set(nums)
        longest = 0
        for num in num_set:
            if num - 1 not in num_set:
                cur = num
                cnt = 1
                while cur + 1 in num_set:
                    cnt += 1
                    cur += 1
                longest = max(longest, cnt)
        return longest

# 增加了判断跳过的逻辑之后，时间复杂度是多少呢？外层循环需要 
# O(n) 的时间复杂度，只有当一个数是连续序列的第一个数的情况下才会进入内层循环，然后在内层循环中匹配连续序列中的数，因此数组中的每个数只会进入内层循环一次。根据上述分析可知，总时间复杂度为 O(n)
```