### 什么是流

流是Java API的新成员，它允许你以声明性方式处理数据集合(通过查询语句来表达，而不是临时编写一个实现)。

可以看成遍历数据集的高级迭代器。流还可以透明的并行处理数据集。

```
    //普通实现
    public static List<String> getLowCaloricDishesNamesInJava7 (List<Dish> dishList) {
        //通过新建一个list,用for循环的方式，判断筛选数据累加数据
        List<Dish> lowCaloricDishes = new ArrayList<>();
        for (Dish dis:dishList) {
            if (dis.getCalories() < 400) {
                lowCaloricDishes.add(dis);
            }
        }
        //用匿名类对list进行比较排序
        List<String> lowCaloricDishesName = new ArrayList<>();
        Collections.sort(lowCaloricDishes,new Comparator<Dish>() {
            @Override
            public int compare(Dish dish1, Dish dish2) {
                return Integer.compare(dish1.getCalories(), dish2.getCalories());
            }
        });
        //处理排名好的数据
        for (Dish dish:lowCaloricDishes) {
            lowCaloricDishesName.add(dish.getName());
        }
        return lowCaloricDishesName;
    }
    
    //流实现
    public static List<String> getLowCaloricDishesNamesInJava8 (List<Dish> dishList) {
        //通过stream流,筛选数据，对筛选的进行排序，对排序的数据提取，最后返回list
        return dishList.stream()
                .filter(dish -> dish.getCalories() < 400)
                .sorted(comparing(Dish::getCalories))
                .map(Dish::getName)
                .collect(Collectors.toList());
    }
```

##### Java8中的Stream的优点：

- 声明性——更简洁，更易读

- 可复合——更灵活

- 可并行——性能更好

##### Stream操作数据的特点

- 流水线——很多流操作本身会返回一个流，这样多个操作就可以链接起来，形成一个大

  的流水线

- 内部迭代——与使用迭代器显式迭代的集合不同，流的迭代操作是在背后进行的。

### 集合与流

集合与流之间的差异就在于什么时候进行计算。集合是一个内存中的数据结构， 它包含数据结构中目前所有的值——集合中的每个元素都得先算出来才能添加到集合中。

流则是在概念上固定的数据结构(你不能添加或删除元素)，其元素则是按需计算的。

**流只能遍历一次**

### 内部迭代与外部迭代

使用Collection接口需要用户去做迭代(比如用for-each)，这称为外部迭代。

Streams库使用内部迭代

![](http://image.wangxiaohuan.com/blog/image/202203291527085.png)

### 中间操作与终端操作

```
List<String> names = menu.stream()  //从菜单获得流 
            .filter(d -> d.getCalories() > 300) //中间操作
            .map(Dish::getName)  //中间操作
            .limit(3)   //中间操作
            .collect(toList());  // 终端操作 将Stream 转 换为List
```

filter、map和limit可以连成一条流水线; 

collect触发流水线执行并关闭它。

可以连接起来的流操作称为中间操作

​                                                                         中间操作

| 操 作    | 类 型 | 返回类型  | 操作参数               | 函数描述符     |
| -------- | ----- | --------- | ---------------------- | -------------- |
| filter   | 中间  | Stream<T> | Predicate<T>           | T -> boolean   |
| map      | 中间  | Stream<T> | Function<T, R>         | T -> R         |
| limit    | 中间  | Stream<T> |                        |                |
| sorted   | 中间  | Stream<T> | Comparator<T>          | (T, T) -> int  |
| distinct | 中间  | Stream<T> |                        |                |
| skip     | 中间  | Stream<T> | long                   |                |
| flatMap  | 中间  | Stream<T> | Function<T, Stream<R>> | T -> Stream<R> |

关闭流的操作称为终端操作。			

​                                                                 终端操作

| 操做      | 类型 | 目的                                                   |
| --------- | ---- | ------------------------------------------------------ |
| forEach   | 终端 | 消费流中的每个元素并对其应用 Lambda。这一操作返回 void |
| count     | 终端 | 返回流中元素的个数。这一操作返回 long                  |
| collect   | 终端 | 把流归约成一个集合，比如 List、Map 甚至是 Integer。    |
| reduce    | 终端 | reduce方法用于对stream中元素进行聚合求值               |
| findFirst | 终端 | 查找第一个元素                                         |
| findAny   | 终端 | findAny方法将返回当前流中的任意元素                    |
| anyMatch  | 终端 | 检查流中元素是否至少匹配一个元素                       |
| allMatch  | 终端 | 检查流中元素是否匹配所有元素                           |
| noneMatch | 终端 | 流中没有任何元素与给定的元素匹配                       |

流的使用一般包括三件事:

1.  一个数据源(如集合)来执行一个查询;
2.  一个中间操作链，形成一条流的流水线;
3.  一个终端操作，执行流水线，并能生成结果。 		 		 	