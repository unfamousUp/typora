# MyBatis-Plus

## 通用CRUD

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@TableName("book")
public class Book {

    @TableId(value = "id",type = IdType.AUTO)
    private Integer id;

    @TableField("book_name")
    private String bookName;

    @TableField("writer")
    private String writer;

    @TableField("price")
    private String price;

    @TableField("loc")
    private String loc;

    @TableField("state")
    private String state;
}

```

### 1、插入操作

- `insert()`

```java
@Test
// 插入一条记录
void testInsert(){
    Book book = new Book();
    book.setBookName("刀剑神域2");
    book.setPrice("100");
    // 如果插入部分数据，其它没设置值的数据库字段需要有默认值
    int result = bookMapper.insert(book);
    System.out.println(result);
}
```

- 自定义insert方法`insertSelective()`

```java
// 注解配置mapper接口
@Mapper
// 指定表名
@TableName("book")
public interface BookMapper extends BaseMapper<Book> {

    @Insert("INSERT INTO book (book_name, price) VALUES (#{bookName}, #{price})")
    int insertSelective(Book book);

}
```

### 2、更新操作

- `updateById()`

```java

```

- `update()`

```java
// 更新数据
@Test
void testUpdate(){
    Book book = new Book();
    book.setBookName("未闻花名"); // 更新的字段
    book.setPrice("1001");
    QueryWrapper<Book> wrapper = new QueryWrapper<>();
    // 查询条件：column数据库字段名,val条件的值。
    wrapper.eq("id", 441); // 相当于where column = #{val}
    int result = bookMapper.update(book,wrapper);
    System.out.println("result = "+result);
}
```

```java
// 更新数据
@Test
void testUpdate2(){
    UpdateWrapper<Book> wrapper = new UpdateWrapper<>();
    wrapper.set("book_name","咒术回战"). // set:更新的字段
            eq("id",441); // 更新的条件
    int result = bookMapper.update(null,wrapper);
    System.out.println("result = "+result);
}
```

### 3、删除操作

- `deleteById()`

```java
/**
 * 根据id删除数据
 */
@Test
void testDeleteById(){
    int result = bookMapper.deleteById(443);
}
```

- `deleteByMap`

```java
/**
 * 根据map删除数据，多条件之间是and关系
 */
@Test
void testDeleteByMap(){
    Map<String,Object> map = new HashMap<>();
    map.put("book_name","刀剑神域2");
    map.put("price","100");
    int result = bookMapper.deleteByMap(map);
    System.out.println("result = "+result);
}
```

- `delete()`

用法一：

```java
/**
 * 根据包装条件查询然后删除
 */
@Test
void testDelete(){
    QueryWrapper<Book> wrapper = new QueryWrapper<>();
    wrapper.eq("id",444);
    int result = bookMapper.delete(mapper);
    System.out.println("result = "+result);
}
```

用法二：

```java
/**
 * 根据包装条件查询然后删除
 */
@Test
void testDelete(){
	Book book = new Book();
    book.setId(441); 
    // 根据set的属性和属性值为条件 where id = 441
    QueryWrapper<Book> wrapper = new QueryWrapper<>(book);
    int result = bookMapper.delete(wrapper); 
    System.out.println("result = "+result);
}
```

- `deleteBatchIds()`

```java
/**
 * 根据id批量删除
 */
@Test
void testDeleteBatchIds(){
    // 相当于 WHERE id IN ( ? , ? )
    int result = bookMapper.deleteBatchIds(Arrays.asList(440, 441));
    System.out.println("result = "+result);
}
```

### 4、查询操作

MP提供了多种查询方式：根据id查询、批量查询、查询单条数据、查询列表、分页查询等操作

#### 1. selectById

```java
/**
 * 根据id查询
 */
@Test
void testSelectById(){
    Users user = usersMapper.selectById(1);
    System.out.println(user);
}
```

#### 2. selectBatchIds

```java
/**
 * 根据id批量查询
 */
@Test
void testSelectBatchIds(){
    // 相当于WHERE user_id IN ( ? , ? )
    List<Users> users = usersMapper.selectBatchIds(Arrays.asList(1, 2));
    for (Users user :
            users) {
        System.out.println(user);
    }
}
```

#### 3. selectOne

```java
/**
 * 根据QueryWrapper条件查询一条数据
 */
@Test
void testSelectOne(){
    QueryWrapper<Book> wrapper = new QueryWrapper<>();
    // 查询条件
    wrapper.eq("id",439);
    // 查询出多条则会抛出异常
    Book book = bookMapper.selectOne(wrapper);
    System.out.println(book);
}
```

#### 4. selectCount

```java
/**
 * 查询符合条件的记录条数
 */
@Test
void testSelectCount(){
    QueryWrapper<Book> wrapper = new QueryWrapper<>();
    // 查询条件：grant than
    wrapper.gt("id",300);
    Integer count = bookMapper.selectCount(wrapper);
    System.out.println("count = "+count);
}
```

#### 5. selectList

```java
/**
 * 查询为list集合
 */
@Test
void testSelectList(){
    QueryWrapper<Book> wrapper = new QueryWrapper<>();
    // 模糊查询: WHERE (book_name LIKE %Spring%)
    wrapper.like("book_name","Spring");
    List<Book> bookList = bookMapper.selectList(wrapper);
    bookList.forEach(System.out::println);
}
```

#### 6. selectPage

分页查询

```java
/**
 * 测试分页查询
 */
@Test
void testSelectPage(){
    /**
     * current:当前页码
     * size:设置每页数据个数
     */
    Page<Book> page = new Page<>(2,5);
    QueryWrapper<Book> wrapper = new QueryWrapper<>();
    wrapper.lt("id",200);
    Page<Book> iPage = bookMapper.selectPage(page, wrapper);
    System.out.println("数据总条数：" + iPage.getTotal());
    System.out.println("数据总页数：" + iPage.getPages());
    System.out.println("当前页数：" + iPage.getCurrent());
    // 获取分页数据
    List<Book> records = iPage.getRecords();
    records.forEach(System.out::println);
}
```

## 条件构造器

### 1、allEq

```java
/**
 * allEq
 */
@Test
void testAllEq(){
    Map<String,Object> map = new HashMap<>();
    map.put("writer","某某");
    map.put("price","19.9");
    QueryWrapper<Book> wrapper = new QueryWrapper<>();
    // WHERE (price = ? AND writer = ?)
    wrapper.allEq(map);
    List<Book> list = bookMapper.selectList(wrapper);
    list.forEach(System.out::println);
}
```

### 2、基本比较操作