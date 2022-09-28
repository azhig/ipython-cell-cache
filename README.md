# Create cache, that active in running cell only
In some cases you need to create cache from some functions, but it should be updated each time cell of ipython notebook is started to run. For example you have functions, that made some SQL query to database, but you don't want to make same queries for many time while running some code in one cell, but you know, that data in database can be updated, when you'll run cell for next time. In this case you create cachable function, which is calculated each time the cell is running. 


## Examples of usage:

### For standalone function

```python
import timeit
from time import sleep

import ipython_cell_cache

ipython_cell_cache.In = In


@ipython_cell_cache.cache_function_for_cell(maxsize=10)
def my_test(a: int):
    sleep(5)
    return a
```
Let's see measures of timeit
```python
import timeit

for _ in range(5):
    action_time=timeit.timeit("my_test(1)", number =1 ,globals=globals())
    print(f"action time is {action_time:.5f} second")
```
Output:
```
action time is 5.00095 second
action time is 0.00002 second
action time is 0.00001 second
action time is 0.00001 second
action time is 0.00001 second
```
If you'll run the cell again, function will be calculated once again but only one time
the output will be the same.
Output:
```
action time is 5.00095 second
action time is 0.00002 second
action time is 0.00001 second
action time is 0.00001 second
action time is 0.00001 second
```

### For classes method
For class method use `cache_method_for_cell` like in above example.
```python
import ipython_cell_cache
from time import sleep

ipython_cell_cache.In = In

class myclass:
    @ipython_cell_cache.cache_method_for_cell()
    def main(self, a):
        sleep(5)
        return a
```

---
**NOTE**

This code mostly took from functools utils.
This module make decorator for cachable methods in class and standalone functions, running in current
Jupyter Notebook cell or python application.
For standalone method you can use 'cache_function_for_cell', for class method - 'cache_method_for_cell'


---