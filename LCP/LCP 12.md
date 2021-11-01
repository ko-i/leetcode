> https://leetcode-cn.com/problems/xiao-zhang-shua-ti-ji-hua/

``` rust
// 这咋还没debug就直接过了呢
impl Solution {
    pub fn min_time(time: Vec<i32>, m: i32) -> i32 {
        if time.len() as i32 <= m {
            // 这就变成了小杨刷题计划
            return 0;
        }
        
        // 首先比较朴素的想法是: 把time数组分成m块, 然后去掉每一块的最大值, 将其它值求和即可
        // 看数据规模是最多O(nlogn)的算法...
        // 能O(mn) 的 动规吗? 应该不能
        
        // O(nlogn) 可以二分? 那要怎么二分呢
        // 我们可以设置检查每天耗时最多为mid 的情况下能否在m天内完成, 找这个最小值即可
        // 那上下界如何确定呢?
        // 下界可以是0, 上界应该是多少呢...上界可以是 (time.sum() / m).max(time.max())
        // 注意每天我们可以用一次场外求助, 用在当前耗时最多的那道题身上
        let (mut l, mut r) = (0, time.iter().sum::<i32>().max(time.iter().max().unwrap().to_owned()));
        let mut ans = -1;
        while l <= r {
            let mid = l + r >> 1;
            if Solution::check(&time, m, mid) {
                ans = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        
        ans
    }
    
    // 辅助函数: 检查在m天内，每天最多耗时limit, 能否将题写完
    fn check(times: &Vec<i32>, m: i32, limit: i32) -> bool {
        let mut idx = 0;
        let mut day_cnt = 0;
        while idx < times.len() && day_cnt < m {
            let mut time_consume = 0;
            let mut max_time = 0;
            
            // 看看一天最多能做多少题
            while idx < times.len() && time_consume + times[idx] - max_time.max(times[idx]) <= limit {
                time_consume += times[idx];
                max_time = max_time.max(times[idx]);
                idx += 1;
            }
            
            if idx == times.len() {
                // 如果已经做完了
                return true;
            }
            
            day_cnt += 1;
        }
        
        false
    }
}
```