> https://leetcode-cn.com/problems/guess-numbers/

``` c
int game(int* guess, int guessSize, int* answer, int answerSize){
    int ans = 0;
    
    for(int i = 0; i < guessSize; i++)
    {
        ans += guess[i] == answer[i];
    }
    
    return ans;
}
```