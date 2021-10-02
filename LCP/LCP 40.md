> https://leetcode-cn.com/problems/uOAnQW/

``` rust
impl Solution {
    pub fn maxmium_score(cards: Vec<i32>, mut cnt: i32) -> i32 {
        let mut odd_nums = vec![0];
        let mut even_nums = vec![0];
        cards.iter().for_each(|&n| match n & 1 {
            0 => even_nums.push(n),
            _ => odd_nums.push(n),
        });
        
        odd_nums[1..].sort_unstable_by(|a, b| b.cmp(a));
        even_nums[1..].sort_unstable_by(|a, b| b.cmp(a));
        
        (1..odd_nums.len()).for_each(|i| odd_nums[i] += odd_nums[i-1]);
        (1..even_nums.len()).for_each(|i| even_nums[i] += even_nums[i-1]);
        
        // 首先看能有多少个偶数
        let mut even_cnt = (even_nums.len() - 1).min(cnt as usize);
        // 然后看能有多少奇数
        let mut odd_cnt = cnt as usize - even_cnt;
        
        // 我们需要奇数的个数为偶数
        if odd_cnt & 1 == 1 {
            // 如果奇数的个数为奇数, 我们需要偶数的个数-1, 奇数的个数+1
            if even_cnt == 0 ||  odd_cnt == odd_nums.len() - 1 {
                // 如果没有偶数了 或者 如果此时奇数的个数没办法多1了
                return 0;
            }
            
            odd_cnt += 1;
            even_cnt -= 1;
        }
        
        let mut ans = even_nums[even_cnt] + odd_nums[odd_cnt];
        // 然后我们减少两个even_num, 增加两个odd_num
        while even_cnt < even_nums.len() && odd_cnt < odd_nums.len() {
            even_cnt -= 2;
            odd_cnt += 2;
            if even_cnt < even_nums.len() && odd_cnt < odd_nums.len() {
                ans = ans.max(even_nums[even_cnt] + odd_nums[odd_cnt]);
            }
        }
        
        ans   
    }
}
```