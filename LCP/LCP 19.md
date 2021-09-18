> https://leetcode-cn.com/problems/UlBDOe/

``` rust
impl Solution {
    pub fn minimum_operations(leaves: String) -> i32 {
        let mut l_b = leaves.as_bytes();
        let mut dp = vec![vec![0x3f3f3f3f; 3]; leaves.len()];
        dp[0][0] = match l_b[0] {
            b'r' => 0,
            _ => 1,
        };
        
        (1..l_b.len()).for_each(|i| {
            dp[i][0] = match l_b[i] {
                b'r' => dp[i-1][0],
                _ => dp[i-1][0] + 1,
            };
            
            dp[i][1] = match l_b[i] {
                b'r' => (dp[i-1][0] + 1).min(dp[i-1][1] + 1),
                _ => dp[i-1][0].min(dp[i-1][1])
            };
            
            dp[i][2] = match l_b[i] {
                b'r' => dp[i-1][1].min(dp[i-1][2]),
                _ => (dp[i-1][1] + 1).min(dp[i-1][2] + 1),
            }
        });
        
        dp.last().unwrap()[2]
    }
}
```