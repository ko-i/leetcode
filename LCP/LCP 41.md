> https://leetcode-cn.com/problems/fHi6rV/

``` rust
use std::collections::VecDeque;

impl Solution {
    // 需要细心设计的dfs
    pub fn flip_chess(mut chessboard: Vec<String>) -> i32 {
        // 第1步: 把字符串表示的棋盘转换成数字表示，方便一些
        // -1 代表空 '.'
        // 0 代表白棋 'O'
        // 1 代表嘿旗 'X'
        let chessboard = chessboard.into_iter().map(|row| row.chars().map(|c| match c {
            '.' => -1,
            'O' => 0,
            _ => 1,
        }).collect::<Vec<i32>>()).collect::<Vec<Vec<i32>>>();

        // 第2步: 我们尝试棋盘上的每一个空位, 将其摆上黑旗, 然后看最终能转化多少白旗, 取最多的那个即可
        let mut ans = 0;
        (0..chessboard.len()).for_each(|r| (0..chessboard[r].len()).for_each(|c| if chessboard[r][c] == -1 {
            ans = ans.max(Solution::change_white(r, c, chessboard.to_vec()));
        }));
        ans
    }
    
    // 辅助函数: 给定落子为黑旗的地方 和 棋盘信息， 返回最终能转换多少白棋
    fn change_white(row: usize, col: usize, mut chessboard: Vec<Vec<i32>>) -> i32 {
        // 我们可以构建一个队列, 用于存储填将要黑旗的位置
        // 我们在每放一个黑旗时, 检查这可黑旗能感染多少白棋, 计数的同时并把这些白棋的位置放到队列末尾
        let mut q = VecDeque::new();
        q.push_back((row, col));
        chessboard[row][col] = 1;
        
        let towards = [(-1, -1), (-1, 0), (-1, 1), (0, 1), (1, 1), (1, 0), (1, -1), (0, -1)];
        let mut changed_cnt = 0;
        while let Some((r, c)) = q.pop_front() {
            // 我们要检查以 (r, c) 为起点八个方向上 能否翻转白棋, 那要如何检查呢？
            (0..towards.len()).for_each(|i| {
                // 我们需要找到尽头的黑旗
                let (mut tail_r, mut tail_c) = ((r as i32 + towards[i].0) as usize, (c as i32 + towards[i].1) as usize);
                while tail_r < chessboard.len() && tail_c < chessboard[tail_r].len() && chessboard[tail_r][tail_c] == 0 {
                    tail_r = (tail_r as i32 + towards[i].0) as usize;
                    tail_c = (tail_c as i32 + towards[i].1) as usize;
                }
                
                if tail_r < chessboard.len() && tail_c < chessboard[tail_r].len() && chessboard[tail_r][tail_c] == 1 {
                    // 只有在末尾遇到了黑旗的情况下, 我们才能将中间的这些白棋转换为黑旗
                    let (mut cur_r, mut cur_c) = ((r as i32 + towards[i].0) as usize, (c as i32 + towards[i].1) as usize);
                    while cur_r != tail_r || cur_c != tail_c {
                        chessboard[cur_r][cur_c] = 1;
                        changed_cnt += 1;
                        q.push_back((cur_r, cur_c));
                        cur_r = (cur_r as i32 + towards[i].0) as usize;
                        cur_c = (cur_c as i32 + towards[i].1) as usize;
                    }
                }
            });
        }
        
        changed_cnt
    }
}
```