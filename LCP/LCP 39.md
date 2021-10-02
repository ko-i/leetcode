> https://leetcode-cn.com/problems/0jQkd0/

``` rust
impl Solution {
    pub fn minimum_switching_times(source: Vec<Vec<i32>>, target: Vec<Vec<i32>>) -> i32 {
        let mut src_cnt = vec![0i32; 16384];
        let mut tar_cnt = vec![0i32; 16384];
        source.iter().for_each(|r| r.iter().for_each(|c| src_cnt[*c as usize] += 1));
        target.iter().for_each(|r| r.iter().for_each(|c| tar_cnt[*c as usize] += 1));
        (0..16384).fold(0, |ans, i| ans + (src_cnt[i] - tar_cnt[i]).abs()) / 2
    }
}
```