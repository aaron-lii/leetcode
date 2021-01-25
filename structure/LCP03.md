# LCP 03. 机器人大冒险
力扣团队买了一个可编程机器人，机器人初始位置在原点(0, 0)。小伙伴事先给机器人输入一串指令command，机器人就会无限循环这条指令的步骤进行移动。指令有两种：
```shell
U: 向y轴正方向移动一格
R: 向x轴正方向移动一格。
```
不幸的是，在 xy 平面上还有一些障碍物，他们的坐标用obstacles表示。机器人一旦碰到障碍物就会被损毁。

给定终点坐标(x, y)，返回机器人能否完好地到达终点。如果能，返回true；否则返回false。

```shell
示例 1：
输入：command = "URR", obstacles = [], x = 3, y = 2
输出：true
解释：U(0, 1) -> R(1, 1) -> R(2, 1) -> U(2, 2) -> R(3, 2)。
```

### python
```python
# 执行用时：40 ms, 在所有 Python3 提交中击败了80.36% 的用户
# 内存消耗：13.6 MB, 在所有 Python3 提交中击败了54.22% 的用户

# 记录一轮路径，是否经过某一点可以由减去重复n轮路径后，是否在第一轮路径中判断
# 终点坐标非常大，一个个模拟路径会非常耗时，考虑用数学计算
# 题目设定障碍数量只有1000，可以迭代判断障碍点是否在路径上

class Solution:
    def robot(self, command: str, obstacles: List[List[int]], x: int, y: int) -> bool:
        x_start = 0
        y_start = 0
        path = set()
        # 此处记得先记录一下(0, 0)
        # list无法加入set，使用tuple
        path.add((x_start, y_start))

        # 构建第一轮路径
        for i in command:
            if i == "U":
                y_start += 1
            else:
                x_start += 1
            path.add((x_start, y_start))

        # 判断终点是否在路径内
        loop_time = x // x_start
        loop_time = min(loop_time, y // y_start)
        if (x - loop_time * x_start, y - loop_time * y_start) not in path:
            return False

        # 判断障碍是否在路径内
        for i in obstacles:
            # 排除在终点右侧或者上方的点
            if i[0] > x or i[1] > y:
                continue
            loop_time = i[0] // x_start
            loop_time = min(loop_time, i[1] // y_start)
            if (i[0] - loop_time * x_start, i[1] - loop_time * y_start) in path:
                return False

        return True
```

### c++版本
```c++
// 执行用时：4 ms, 在所有 C++ 提交中击败了95.19% 的用户
// 内存消耗：19.2 MB, 在所有 C++ 提交中击败了5.26% 的用户

class Solution {
public:
    bool robot(string command, vector<vector<int>>& obstacles, int x, int y) {
        int x_start = 0;
        int y_start = 0;
        int n = command.size();
        // 用二维vector存储路径，需要考虑越界问题
        vector<vector<int>> path(n, vector<int>(n, 0));
        path[0][0] = 1;

        for(char &i : command){
            if(i == 'U'){
                ++y_start;
            }else{
                ++x_start;
            }
            path[x_start][y_start] = 1;
        }

        int loop_time;
        int r_x;
        int r_y;
        loop_time = x / x_start;
        loop_time = min(loop_time, y / y_start);
        r_x = x - loop_time * x_start;
        r_y = y - loop_time * y_start;
        if(r_x >= n || r_y >= n || path[r_x][r_y] == 0){
            return false;
        }

        for(vector<int> &i : obstacles){
            if (i[0] > x || i[1] > y) continue;
            loop_time = i[0] / x_start;
            loop_time = min(loop_time, i[1] / y_start);
            r_x = i[0] - loop_time * x_start;
            r_y = i[1] - loop_time * y_start;
            if(r_x >= n || r_y >= n) continue;
            if(path[r_x][r_y] == 1) return false;
        }
        return true;
    }
};
```