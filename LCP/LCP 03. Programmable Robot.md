> https://leetcode-cn.com/problems/programmable-robot/

``` c

bool check(char* command, int x, int y, int tx, int ty) {
    char* pc = command;
    int round = tx / x < ty / y ? tx / x : ty / y;
    x *= round;
    y *= round;
    
    if(x == tx && y == ty) return true;
    
    while(*pc) {
        if(*pc == 'U') y += 1; else x += 1;
        if(x > tx || y > ty) return false;
        
        if(x == tx && y == ty) return true;
        
        pc += 1;
    }
    
    return false;
}

bool robot(char * command, int** obstacles, int obstaclesSize, int* obstaclesColSize, int x, int y){
    int delta_x = 0;
    int delta_y = 0;
    
    char* pc = command;
    while(*pc) {
        if(*pc == 'U') delta_y++; else delta_x++;
        pc++;
    }
    
    if(!check(command, delta_x, delta_y, x, y)) return false;
    
    for(int i = 0; i < obstaclesSize; ++i) {
        if(obstacles[i][0] > x || obstacles[i][1] > y) continue;
        if(check(command, delta_x, delta_y, obstacles[i][0], obstacles[i][1])) return false;
    }
    
    return true;
}
```