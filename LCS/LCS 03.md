> https://leetcode-cn.com/problems/YesdPw/

``` rust
use std::collections::VecDeque;

impl Solution {
    pub fn largest_area(mut grid: Vec<String>) -> i32 {
        // 首先将grid改为数字矩阵, 后续方便一些
        let mut grid = grid.drain(..).map(|rows| rows.chars().map(|ch| ch as u8 - b'0').collect::<Vec<u8>>()).collect::<Vec<Vec<u8>>>();
        let (m, n) = (grid.len(), grid[0].len()); // m行 n列
        // 可以将方块划分为2种状态: 是走廊或与走廊相邻(不安全), 不与走廊相邻(安全)
        // true: 安全
        // false: 不安全
        let mut is_safe = vec![vec![true; n]; m];
        // 然后我们考虑两种:
        // 处在grid边界上的方块 和 与0 相邻的方块
        // 1. 我们先考虑出现grid边界上的方块
        (0..m).for_each(|r| {
            // grid[r][0] grid[r][n-1]
            if is_safe[r][0] && grid[r][0] != 0 {
                Solution::change_2_not_safe(r, 0, &grid, &mut is_safe);
            }
            
            if is_safe[r][n-1] && grid[r][n-1] != 0 {
                Solution::change_2_not_safe(r, n-1, &grid, &mut is_safe);
            }
        });
        
        (0..n).for_each(|c| {
            // grid[0][c] grid[m-1][c]
            if is_safe[0][c] && grid[0][c] != 0 {
                Solution::change_2_not_safe(0, c, &grid, &mut is_safe);
            }
            
            if is_safe[m-1][c] && grid[m-1][c] != 0 {
                Solution::change_2_not_safe(m-1, c, &grid, &mut is_safe);
            }
        });
        
        // 2. 考虑所有与0相邻的房间
        (0..m).for_each(|r| (0..n).for_each(|c| if grid[r][c] == 0 {
            Solution::change_2_not_safe(r-1, c, &grid, &mut is_safe);
            Solution::change_2_not_safe(r+1, c, &grid, &mut is_safe);
            Solution::change_2_not_safe(r, c-1, &grid, &mut is_safe);
            Solution::change_2_not_safe(r, c+1, &grid, &mut is_safe);
        }));
        
        // 现在都转换完了, 下面我们就可以用普通的bfs来统计相邻相同主题空间的个数
        let mut ans = 0;
        let mut not_visited = vec![vec![true; n]; m];
        (0..m).for_each(|r| (0..n).for_each(|c| if grid[r][c] != 0 && is_safe[r][c] {
            ans = ans.max(Solution::change_2_not_safe(r, c, &grid, &mut not_visited));
        }));
        
        ans
    }
    
    // 辅助函数: bfs将相邻相同主题的房间转换为不安全状态
    fn change_2_not_safe(row: usize, col: usize, grid: &Vec<Vec<u8>>, is_safe: &mut Vec<Vec<bool>>) -> i32 {
        if row >= grid.len() || col >= grid[row].len() || !is_safe[row][col] || grid[row][col] == 0 {
            return 0;
        }
        
        let space_theme = grid[row][col];
        let mut q = VecDeque::new();
        is_safe[row][col] = false;
        q.push_back((row, col));
        let mut cnt = 1;
        while let Some((r, c)) = q.pop_front() {
            if r-1 < grid.len() && grid[r-1][c] == space_theme && is_safe[r-1][c] {
                is_safe[r-1][c] = false;
                q.push_back((r-1, c));
                cnt += 1;
            }
            
            if r+1 < grid.len() && grid[r+1][c] == space_theme && is_safe[r+1][c] {
                is_safe[r+1][c] = false;
                q.push_back((r+1, c));
                cnt += 1;
            }
            
            if c-1 < grid[r].len() && grid[r][c-1] == space_theme && is_safe[r][c-1] {
                is_safe[r][c-1] = false;
                q.push_back((r, c-1));
                cnt += 1;
            }
            
            if c+1 < grid[r].len() && grid[r][c+1] == space_theme && is_safe[r][c+1] {
                is_safe[r][c+1] = false;
                q.push_back((r, c+1));
                cnt += 1;
            }
        }
        
        cnt
    }
}
```