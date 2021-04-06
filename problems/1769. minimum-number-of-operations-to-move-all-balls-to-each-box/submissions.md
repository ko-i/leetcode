> https://leetcode-cn.com/problems/minimum-number-of-operations-to-move-all-balls-to-each-box/

``` c
int* minOperations(char * boxes, int* returnSize){
    int s_len = strlen(boxes);
    
    int* pre = calloc(s_len, sizeof(int));
    int* res = calloc(s_len, sizeof(int));
    
    int pre_ball_cnt = 0;
    int res_ball_cnt = 0;
    int pre_sum = 0;
    int res_sum = 0;
    
    for(int i = 0; i < s_len; i++)
    {
        pre[i] = pre_sum;
        res[s_len - i - 1] = res_sum;
        
        pre_ball_cnt += boxes[i] == '1';
        res_ball_cnt += boxes[s_len - i - 1] == '1';
        
        pre_sum += pre_ball_cnt;
        res_sum += res_ball_cnt;
    }
    
    for(int i = 0; i < s_len; i++) pre[i] += res[i];
    *returnSize = s_len;
    
    free(res);
    
    return pre;
}
```