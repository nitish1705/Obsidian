```java
private int power(int base, int exp){
        int res = 1;
        while(exp > 0){
            if((exp & 1) == 1){
                res *= exp;
            }
            base *= base;
            exp >>= 1;
        }
        return res;
    }
```