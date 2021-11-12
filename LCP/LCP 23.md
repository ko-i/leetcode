> https://leetcode-cn.com/problems/er94lq/

``` rust
impl Solution {
    pub fn is_magic(target: Vec<i32>) -> bool {
        let n = target.len();
        // 我们可以根据第一次洗牌的结果找到k
        let mut cards = Vec::with_capacity(n);
        (1..=n as i32).skip(1).step_by(2).for_each(|i| cards.push(i));
        (1..=n as i32).step_by(2).for_each(|i| cards.push(i));
        let mut idx = 0;
        while idx < n && cards[idx] == target[idx] {
            idx += 1;
        }
        
        if idx == 0 {
            // 第1个就不一样
            return false;
        }
        
        let k = idx;
        while idx < cards.len() {
            Solution::shuffle(&mut cards[idx..]);
            
            // 然后我们要向后数k个
            for _ in 0..k {
                if idx == cards.len() {
                    return true;
                }
                
                if cards[idx] != target[idx] {
                    return false;
                }
                
                idx += 1;
            }
        }
        
        true
    }
    
    fn shuffle(cards: &mut [i32]) {
        let mut t_arr = Vec::new();
        (0..cards.len()).skip(1).step_by(2).for_each(|i| t_arr.push(cards[i]));
        (0..cards.len()).step_by(2).for_each(|i| t_arr.push(cards[i]));
        (0..cards.len()).for_each(|i| cards[i] = t_arr[i]);
    }
}
```