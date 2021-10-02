> https://leetcode-cn.com/problems/sZ59z6/

``` rust
use std::rc::Rc;
use std::cell::RefCell;

impl Solution {
    pub fn num_color(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let mut seen = vec![false; 1024];
        Solution::dfs(&root, &mut seen);
        seen.iter().filter(|e| **e).count() as i32
    }
    
    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, seen: &mut Vec<bool>) {
        if let Some(node) = root {
            if !seen[node.borrow().val as usize] {
                seen[node.borrow().val as usize] = true;
            }
            
            Solution::dfs(&node.borrow().left, seen);
            Solution::dfs(&node.borrow().right, seen);
        }
    }
}
```