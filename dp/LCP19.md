## LCP 19. 秋叶收藏集

### python动态规划数组
```python
"""
一共三种状态：
dp_end      红黄红 完整状态
dp_notend   红黄   未完成状态
dp_notstart 红     未开始状态

红黄红 = min(红黄红+红，红黄+红)
红黄 = min(红黄 + 黄， 红 + 黄)
红 = 红 + 红
"""

class Solution:
    def minimumOperations(self, leaves: str) -> int:
        dp_end = [0] * len(leaves)
        dp_notend = [0] * len(leaves)
        dp_notstart = [0] * len(leaves)

        if leaves[0] == "r":
            dp_end[0] = 0
            dp_notend[0] = 0
            dp_notstart[0] = 0
        else:
            dp_end[0] = 1
            dp_notend[0] = 1
            dp_notstart[0] = 1

        if leaves[1] == "r":
            dp_end[1] = dp_end[0] + 1
            dp_notend[1] = dp_notend[0] + 1
            dp_notstart[1] = dp_notstart[0]
        else:
            dp_end[1] = dp_end[0]
            dp_notend[1] = dp_notend[0]
            dp_notstart[1] = dp_notstart[0] + 1

        for i in range(2, len(leaves)):
            if leaves[i] == "r":
                dp_end[i] = min(dp_end[i - 1], dp_notend[i - 1])
                dp_notend[i] = min(dp_notend[i - 1] + 1, dp_notstart[i - 1] + 1)
                dp_notstart[i] = dp_notstart[i - 1]
            else:
                dp_end[i] = min(dp_end[i - 1] + 1, dp_notend[i - 1] + 1)
                dp_notend[i] = min(dp_notend[i - 1], dp_notstart[i - 1])
                dp_notstart[i] = dp_notstart[i - 1] + 1

        return dp_end[len(leaves) - 1]
```

### python动态规划将数组压缩成3个变量,并注释不必要的赋值步骤
```python
class Solution(object):
    def minimumOperations(self, leaves):
        """
        :type leaves: str
        :rtype: int
        """

        if leaves[0] == "r":
            dp_end = 0
            dp_notend = 0
            dp_notstart = 0
        else:
            dp_end = 1
            dp_notend = 1
            dp_notstart = 1

        if leaves[1] == "r":
            dp_end = dp_end + 1
            dp_notend = dp_notend + 1
            # dp_notstart = dp_notstart
        else:
            # dp_end = dp_end
            # dp_notend = dp_notend
            dp_notstart = dp_notstart + 1

        for i in range(2, len(leaves)):
            if leaves[i] == "r":
                dp_end = min(dp_end, dp_notend)
                dp_notend = min(dp_notend + 1, dp_notstart + 1)
                # dp_notstart = dp_notstart
            else:
                dp_end = min(dp_end + 1, dp_notend + 1)
                dp_notend = min(dp_notend, dp_notstart)
                dp_notstart = dp_notstart + 1

        return dp_end
```

### 对应C++代码
```c++
#include <algorithm>

class Solution {
public:
    int dp_end = 0;
    int dp_notend = 0;
    int dp_notstart = 0;

    int minimumOperations(string leaves) {
        if(leaves[0] == 'r'){
            dp_end = 0;
            dp_notend = 0;
            dp_notstart = 0;
        }else{
            dp_end = 1;
            dp_notend = 1;
            dp_notstart = 1;
        }

        if(leaves[1] == 'r'){
            dp_end = dp_end + 1;
            dp_notend = dp_notend + 1;
        }else{
            dp_notstart = dp_notstart + 1;
        }
            
        for(int i = 2; i < leaves.size(); i++){
            if(leaves[i] == 'r'){
                dp_end = min(dp_end, dp_notend);
                dp_notend = min(dp_notend + 1, dp_notstart + 1);
            }else{
                dp_end = min(dp_end + 1, dp_notend + 1);
                dp_notend = min(dp_notend, dp_notstart);
                dp_notstart = dp_notstart + 1;
            }
        }
        return dp_end;
    }
};
```