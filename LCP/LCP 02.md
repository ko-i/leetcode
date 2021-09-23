> https://leetcode-cn.com/problems/deep-dark-fraction/

``` rust
int gcd(int a, int b)
{
    if(a < b) 
    {
        int t = a; a = b; b = t;
    }
    
    if(a % b == 0) return b;
    
    return gcd(b, a % b);
}

int* fraction(int* cont, int contSize, int* returnSize){
    int up = cont[contSize - 1];
    int down = 1;
    
    for(int i = contSize - 2; i >= 0; i--)
    {
        int t = up; up = down; down = t;
        
        up += cont[i] * down;
    }
    
    int gcd_num = gcd(up, down);
    
    int* ans = malloc(2 * sizeof(int));
    ans[0] = up / gcd_num;
    ans[1] = down / gcd_num;
    *returnSize = 2;
    
    return ans;
}
```