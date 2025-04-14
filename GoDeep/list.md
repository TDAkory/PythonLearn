# list

#python #list

## 列表推导式

Python 的列表推导式（List Comprehension）是一种简洁且强大的语法，用于从一个可迭代对象（如列表、元组、集合等）生成新的列表。列表推导式可以包含条件语句，从而实现更复杂的逻辑。

### 基本语法

列表推导式的基本语法如下：

```python
[expression for item in iterable if condition]
```

- **expression**：对每个元素执行的表达式。
- **item**：可迭代对象中的每个元素。
- **iterable**：可迭代对象，如列表、元组、集合等。
- **condition**：可选的条件语句，用于过滤元素。

### 示例

```python
squares = [x**2 for x in range(10)]
print(squares)  # 输出: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

even_squares = [x**2 for x in range(10) if x % 2 == 0]
print(even_squares)  # 输出: [0, 4, 16, 36, 64]

list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']
combinations = [(x, y) for x in list1 for y in list2]
print(combinations)  # 输出: [(1, 'a'), (1, 'b'), (1, 'c'), (2, 'a'), (2, 'b'), (2, 'c'), (3, 'a'), (3, 'b'), (3, 'c')]

nested_list = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened_list = [item for sublist in nested_list for item in sublist]
print(flattened_list)  # 输出: [1, 2, 3, 4, 5, 6, 7, 8, 9]

mixed_operations = [x**2 if x % 2 == 0 else x**3 for x in range(10)]
print(mixed_operations)  # 输出: [0, 1, 4, 27, 16, 125, 36, 343, 64, 729]
```

### 注意事项

1. **性能**：列表推导式通常比等效的 `for` 循环更快，因为它们在内部进行了优化。
2. **可读性**：虽然列表推导式非常强大，但过度使用或嵌套过深可能会降低代码的可读性。在复杂的情况下，建议使用普通的 `for` 循环。
3. **内存使用**：列表推导式会立即生成整个列表，这可能会消耗大量内存。如果处理大量数据，可以考虑使用生成器表达式（Generator Expression）。

### 生成器表达式

生成器表达式与列表推导式类似，但返回一个生成器对象，而不是一个列表。生成器对象可以逐个生成元素，从而节省内存。

```python
squares_gen = (x**2 for x in range(10))
for square in squares_gen:
    print(square)
```
