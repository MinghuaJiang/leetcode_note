# Backtracking

### 基本思想

* 遍历所有的可能性
  * Permutation
  * Combination
* 模板

```
public void backtrack(result, path, candidate){
    if (满足条件){
        result.add(path);
        return;
    }

    for (next in candidate){
       path.add next
       backtrack(path, candidate)
       path.remove next
    }
}
```
