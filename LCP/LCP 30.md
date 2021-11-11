> https://leetcode-cn.com/problems/p0NxJO/

``` rust
use std::collections::BinaryHeap;

impl Solution {
    // 模拟吗? 贪心
    pub fn magic_tower(nums: Vec<i32>) -> i32 {
        // 在扣血总量大于补血总量时, 我们无法走出这一层
        if nums.iter().sum::<i32>() < 0 {
            return -1;
        }
        
        let mut curr_hp = 0;
        let mut t_bh = BinaryHeap::new();
        let mut adjust_cnt = 0;
        nums.iter().for_each(|&n| {
            if n > 0 {
                curr_hp += n as i64;
            } else {
                t_bh.push(-n as i64);
                
                curr_hp += n as i64;
                if curr_hp < 0 {
                    curr_hp += t_bh.pop().unwrap();
                    adjust_cnt += 1;
                }
            }
        });
        
        adjust_cnt
    }
}
```