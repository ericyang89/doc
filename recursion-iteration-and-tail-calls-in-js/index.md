http://www.jstips.co/en/javascript/recursion-iteration-and-tail-calls-in-js/

# the origin recursion
```javascript
function factorial(n) {
    if (n === 0) {
        return 1;
    }
    return n * factorial(n - 1);
}
// factorial(6)
```

factorial(6)
6 * factorial(5)
5 * factorial (4)
4 * factorial(3)
3 * factorial(2)
2 * factorial(1)
1 * factorial(0)
1
(resuming previous execution) 1 * 1 = 1
(resuming…) 2 * 1 = 2
(…) 3 * 2 = 6
… 4 * 6 = 24
5 * 24 = 120
6 * 120 = 720
factorial(6) = 720

# better 
```javascript
function factorial(n) {
    function inner_factorial(n, res) {
        if (n === 0) {
            return res;
        }
        return inner_factorial(n - 1, res * n);
    }
    return inner_factorial(n, 1);
}
```


factorial(6)
inner anonymous function (iaf) gets called with (n = 6, res = 1)
iaf(5, 1 * 6)
iaf(4, 6 * 5)
iaf(3, 30 * 4)
iaf(2, 120 * 3)
iaf(1, 360 * 2)
iaf(0, 720)
720
720
720
720
720
720
720
iaf (6, 1) = 720
factorial(6) = 720

# this best 
```javascript
res = 1;
n = 6;

while(n > 1) {
    res = res * n;
    n--;
}
```
