### bash 学习笔记

- 赋值符号 = 前后不能加空格

  > name="DevDojo"

- 变量名括在花括号中不是必需的,但是我们建议你这么做

  > ```
  > echo ${name}
  > ```

- 命令行参数

  ```
  $1 是第一个参数
  $2 是第二个参数
  $0 用于引用脚本本身
  $@ 是所有参数
  ```

- 用户输入命令read

  ```
  #!/bin/bash
  
  echo "What is your name?"
  read name
  
  echo "Hi there $name"
  echo "Welcome to DevDojo!"
  ```

- read -p 可以输出提示

  > ```bash
  > read -p "What is your name? " name
  > ```

- 数组

  > 用空格分隔的值并将其括在 中来初始化数组`()`

  ```
  my_array=("value 1" "value 2" "value 3" "value 4")
  ```

  > 通过它们的数字索引来引用

​	echo ${my_array[1]}

> 这将返回最后一个元素：
>
> ```
> echo ${my_array[-1]}
> ```

- 数字切片和字符串切片

- 函数 function 可加可不加, 建议加, 容易区分, 调用函数不用括号