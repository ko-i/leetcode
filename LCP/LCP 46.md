> https://leetcode-cn.com/problems/05ZEDJ/

``` rust
impl Solution {
    pub fn volunteer_deployment(final_cnt: Vec<i32>, total_num: i64, edges: Vec<Vec<i32>>, plans: Vec<Vec<i32>>) -> Vec<i32> {
        // 首先能做些什么呢？
        // 先把领接表做出来
        let mut adj_table = vec![Vec::new(); final_cnt.len() + 1];
        edges.iter().for_each(|e| {
            adj_table[e[0] as usize].push(e[1] as usize);
            adj_table[e[1] as usize].push(e[0] as usize);
        });
        
        // room_info[i].0 表示已知的人数
        // room_info[i].1 表示位置人数的系数, 由第0个房间扩散而来
        let mut room_info = vec![(0, 0); final_cnt.len() + 1];
        room_info[0].1 = 1i64;  // 设最终时 第0号房间 有 1 * x(1 乘 x) 人
        (0..final_cnt.len()).for_each(|i| room_info[i+1].0 = final_cnt[i] as i64);
        
        // 然后我们倒退
        plans.iter().rev().for_each(|plan| {
            match plan[0] {
                1 => {
                    // 给plan[1]号房减半, 因为我们是倒退, 也就是翻倍
                    room_info[plan[1] as usize].0 <<= 1;
                    room_info[plan[1] as usize].1 <<= 1;
                },
                2 => {
                    // 给plan[1]号房所有相邻的房间人数加上 plan[1]号房的当前人数
                    // 逆推回去就是减掉人数
                    adj_table[plan[1] as usize].iter().for_each(|&adj_room| {
                        room_info[adj_room].0 -= room_info[plan[1] as usize].0;
                        room_info[adj_room].1 -= room_info[plan[1] as usize].1;
                    });
                },
                _ => {
                    // 这边就是增加了
                    adj_table[plan[1] as usize].iter().for_each(|&adj_room| {
                        room_info[adj_room].0 += room_info[plan[1] as usize].0;
                        room_info[adj_room].1 += room_info[plan[1] as usize].1;
                    });
                }
            }
        });
        
        // 现在room_info里就存储了初始情况下的人员信息
        let x = (total_num - room_info.iter().map(|r| r.0).sum::<i64>()) / room_info.iter().map(|r| r.1).sum::<i64>();
        (0..room_info.len()).map(|i| (room_info[i].0 + room_info[i].1 * x) as i32).collect::<Vec<i32>>()
    }
}
```