> https://leetcode-cn.com/problems/er-cha-shu-ran-se-UGC/

``` rust
use std::rc::Rc;
use std::cell::RefCell;

impl Solution {
    // 这也太难了吧
    // 第一次接触树状dp
    pub fn max_value(root: Option<Rc<RefCell<TreeNode>>>, k: i32) -> i32 {
        // dp[i] (i=[0..=k]) 表示当前节点作为第i个被涂色的节点时, 所能得到的最大值
        // dp[0] 表示当前节点不涂色
        let dp = Solution::dfs(root, k as usize);
        dp[0].max(dp[1]) // 当前节点不涂色, 或者当前节点作为第一个节点涂色
    }
    
    fn dfs(root: Option<Rc<RefCell<TreeNode>>>, k: usize) -> Vec<i32> {
        if root.is_none() {
            // 如果无节点可以涂色
            return vec![0; k + 1];
        }
        
        let node = root.unwrap();
        // 我们用后续遍历, 首先得到左子节点和柚子节点的dp信息表
        let dp_left = Solution::dfs(node.borrow().left.clone(), k);
        let dp_right = Solution::dfs(node.borrow().right.clone(), k);
        
        // 然后我们考虑如何着色
        let mut dp = vec![0; k + 1];
        
        // 情况1: 我们不给本节点涂色 dp[0]
        // 那么对于左子节点和柚子节点来说, 分别有不涂色dp_left[0](dp_right[0]), 与涂色(只能为第1个点: dp_...[1]), 我们去较大的那个即可
        dp[0] = dp_left[0].max(dp_left[1]) + dp_right[0].max(dp_right[1]);
        
        // 情况2: 本节点作为第k个节点, 左右子节点就无法涂色
        dp[k] = node.borrow().val + dp_left[0] + dp_right[0];
        
        // 情况3: 我们给本节点涂色:
        // 我们可以将本节点作为第 [1..=k] 个节点
        (1..k).for_each(|i| {
            // 分别代表了
            // 左子树不染色:
            //    右子树不染色 或右子树作为第i+1个节点染色
            // 右子树不染色:
            //    左子树不染色 或左子树作为第i+1个节点染色
            dp[i] = (dp_left[0] + dp_right[i+1].max(dp_right[0])).max(dp_right[0] + dp_left[i+1].max(dp_left[0]));
            
            // 本节点作为第 i 个节点被涂色, 还剩 k - i 个节点
            (1..k-i).for_each(|j| {
                // 左边选 j 个节点涂色, 右边选 k-i-j 个节点涂色
                // 对应就是左子节点作为第 k+1-j 个节点涂色:dp_left[k+1-j]
                // 对应就是柚子节点作为第 k+1-(k-i-j) 个节点涂色: dp_right[k+1-(k-i-j)]
                dp[i] = dp[i].max(dp_left[k+1-j] + dp_right[k+1-(k-i-j)]);
            });
            
            dp[i] += node.borrow().val;
        });
        
        dp
    }
}
```