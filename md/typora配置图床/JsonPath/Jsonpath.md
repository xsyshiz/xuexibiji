全文来自：http://www.ibloger.net/article/2329.html
因公司内网不能访问此网站，可以访问GitHub。

```xml
<dependency>
    <groupId>com.jayway.jsonpath</groupId>
    <artifactId>json-path</artifactId>
    <version>2.2.0</version>
</dependency>
```



JsonPath中的“根成员对象”始终称为$，无论是对象还是数组。

JsonPath表达式可以使用点表示法

```
$.store.book [0].title
```

或括号表示法

```
$['store']['book'][0]['title']
```

### 操作符

| 操作                      | 说明                                      |
| ------------------------- | ----------------------------------------- |
| `$`                       | 查询根元素。这将启动所有路径表达式。      |
| `@`                       | 当前节点由过滤谓词处理。                  |
| `*`                       | 通配符，必要时可用任何地方的名称或数字。  |
| `..`                      | 深层扫描。 必要时在任何地方可以使用名称。 |
| `.<name>`                 | 点，表示子节点                            |
| `['<name>' (, '<name>')]` | 括号表示子项                              |
| `[<number> (, <number>)]` | 数组索引或索引                            |
| `[start:end]`             | 数组切片操作                              |
| `[?(<expression>)]`       | 过滤表达式。 表达式必须求值为一个布尔值。 |

### 函数

函数可以在路径的尾部调用，函数的输出是路径表达式的输出，该函数的输出是由函数本身所决定的。

| 函数       | 描述                     | 输出     |
| ---------- | ------------------------ | -------- |
| `min()`    | 提供数字数组的最小值     | `Double` |
| `max()`    | 提供数字数组的最大值     | `Double` |
| `avg()`    | 提供数字数组的平均值     | `Double` |
| `stddev()` | 提供数字数组的标准偏差值 | `Double` |
| `length()` | 提供数组的长度           | Integer  |

### 过滤器运算符

过滤器是用于筛选数组的逻辑表达式。一个典型的过滤器将是[?(@.age > 18)]，其中@表示正在处理的当前项目。 可以使用逻辑运算符&&和||创建更复杂的过滤器。 字符串文字必须用单引号或双引号括起来([?(@.color == 'blue')] 或者 [?(@.color == "blue")]).

| 操作符  | 描述                                     |
| ------- | ---------------------------------------- |
| `==`    | left等于right（注意1不等于'1'）          |
| `!=`    | 不等于                                   |
| `<`     | 小于                                     |
| `<=`    | 小于等于                                 |
| `>`     | 大于                                     |
| `>=`    | 大于等于                                 |
| `=~`    | 匹配正则表达式[?(@.name =~ /foo.*?/i)]   |
| `in`    | 左边存在于右边 [?(@.size in ['S', 'M'])] |
| `nin`   | 左边不存在于右边                         |
| `size`  | （数组或字符串）长度                     |
| `empty` | （数组或字符串）为空                     |

### Java操作示例

JSON

```json
{
    "store": {
        "book": [
            {
                "category": "reference",
                "author": "Nigel Rees",
                "title": "Sayings of the Century",
                "price": 8.95
            },
            {
                "category": "fiction",
                "author": "Evelyn Waugh",
                "title": "Sword of Honour",
                "price": 12.99
            },
            {
                "category": "fiction",
                "author": "Herman Melville",
                "title": "Moby Dick",
                "isbn": "0-553-21311-3",
                "price": 8.99
            },
            {
                "category": "fiction",
                "author": "J. R. R. Tolkien",
                "title": "The Lord of the Rings",
                "isbn": "0-395-19395-8",
                "price": 22.99
            }
        ],
        "bicycle": {
            "color": "red",
            "price": 19.95
        }
    },
    "expensive": 10
}
```

| JsonPath (点击链接测试)                                      | 结果                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`$.store.book[*].author`](http://jsonpath.herokuapp.com/?path=$.store.book[*].author) | 获取json中store下book下的所有author值                        |
| [`$..author`](http://jsonpath.herokuapp.com/?path=$..author) | 获取所有json中所有author的值                                 |
| [`$.store.*`](http://jsonpath.herokuapp.com/?path=$.store.*) | 所有的东西，书籍和自行车                                     |
| [`$.store..price`](http://jsonpath.herokuapp.com/?path=$.store..price) | 获取json中store下所有price的值                               |
| [`$..book[2\]`](http://jsonpath.herokuapp.com/?path=$..book[2]) | 获取json中book数组的第3个值                                  |
| [`$..book[-2\]`](http://jsonpath.herokuapp.com/?path=$..book[2]) | 倒数的第二本书                                               |
| [`$..book[0,1\]`](http://jsonpath.herokuapp.com/?path=$..book[0,1]) | 前两本书                                                     |
| [`$..book[:2\]`](http://jsonpath.herokuapp.com/?path=$..book[:2]) | 从索引0（包括）到索引2（排除）的所有图书                     |
| [`$..book[1:2\]`](http://jsonpath.herokuapp.com/?path=$..book[1:2]) | 从索引1（包括）到索引2（排除）的所有图书                     |
| [`$..book[-2:\]`](http://jsonpath.herokuapp.com/?path=$..book[-2:]) | 获取json中book数组的最后两个值                               |
| [`$..book[2:\]`](http://jsonpath.herokuapp.com/?path=$..book[2:]) | 获取json中book数组的第3个到最后一个的区间值                  |
| [`$..book[?(@.isbn)\]`](http://jsonpath.herokuapp.com/?path=$..book[?(@.isbn)]) | 获取json中book数组中包含isbn的所有值                         |
| [`$.store.book[?(@.price < 10)\]`](http://jsonpath.herokuapp.com/?path=$.store.book[?(@.price < 10)]) | 获取json中book数组中price<10的所有值                         |
| [`$..book[?(@.price <= $['expensive'\])]`](http://jsonpath.herokuapp.com/?path=$..book[?(@.price <= $['expensive'])]) | 获取json中book数组中price<=expensive的所有值                 |
| [`$..book[?(@.author =~ /.*REES/i)\]`](http://jsonpath.herokuapp.com/?path=$..book[?(@.author =~ /.*REES/i)]) | 获取json中book数组中的作者以REES结尾的所有值（REES不区分大小写） |
| [`$..*`](http://jsonpath.herokuapp.com/?path=$..*)           | 逐层列出json中的所有值，层级由外到内                         |
| [`$..book.length()`](http://jsonpath.herokuapp.com/?path=$..book.length()) | 获取json中book数组的长度                                     |

### 阅读文档

使用JsonPath的最简单的最直接的方法是通过静态读取API。

```java
String json = "...";
List<String> authors = JsonPath.read(json, "$.store.book[*].author");
```

如果你只想读取一次，那么上面的代码就可以了

如果你还想读取其他路径，现在上面不是很好的方法，因为他每次获取都需要再解析整个文档。所以，我们可以先解析整个文档，再选择调用路径。

 

```java
String json = "...";
Object document = Configuration.defaultConfiguration().jsonProvider().parse(json);

String author0 = JsonPath.read(document, "$.store.book[0].author");
String author1 = JsonPath.read(document, "$.store.book[1].author");
```

JsonPath还提供流畅的API。 这也是最灵活的一个。

```java
String json = "...";
ReadContext ctx = JsonPath.parse(json);

List<String> authorsOfBooksWithISBN = ctx.read("$.store.book[?(@.isbn)].author");

List<Map<String, Object>> expensiveBooks = JsonPath
                            .using(configuration)
                            .parse(json)
                            .read("$.store.book[?(@.price > 10)]", List.class);
```

### 何时返回

当在java中使用JsonPath时，重要的是要知道你在结果中期望什么类型。 JsonPath将自动尝试将结果转换为调用者预期的类型。

```java
// 抛出 java.lang.ClassCastException 异常
List<String> list = JsonPath.parse(json).read("$.store.book[0].author")

// 正常
String author = JsonPath.parse(json).read("$.store.book[0].author")
```

 

当评估路径时，你需要理解路径确定的概念。路径是不确定的，它包含

- `..` ：深层扫描操作
- `?(<expression>)` ：表达式
- `[<number>, <number> (, <number>)]` ：多个数组索引

不确定的路径总是返回一个列表（由当前的JsonProvider表示）。

默认情况下，MappingProvider SPI提供了一个简单的对象映射器。 这允许您指定所需的返回类型，MappingProvider将尝试执行映射。 在下面的示例中，演示了Long和Date之间的映射。

```java
String json = "{\"date_as_long\" : 1411455611975}";
Date date = JsonPath.parse(json).read("$['date_as_long']", Date.class);
```

如果您将JsonPath配置为使用JacksonMappingProvider或GsonMappingProvider，您甚至可以将JsonPath输出直接映射到POJO中。

```java
Book book = JsonPath.parse(json).read("$.store.book[0]", Book.class);
```

要获取完整的泛型类型信息，请使用TypeRef。

```java
TypeRef<List<String>> typeRef = new TypeRef<List<String>>(){};
List<String> titles = JsonPath.parse(JSON_DOCUMENT).read("$.store.book[*].title", typeRef);
```

### 谓词

在JsonPath中创建过滤器谓词有三种不同的方法。

#### 内联谓词

内联谓词是路径中定义的谓词。

```java
List<Map<String, Object>> books =  JsonPath.parse(json).read("$.store.book[?(@.price < 10)]");
```

你可以使用 && 和 || 结合多个谓词 [?(@.price < 10 && @.category == 'fiction')] , [?(@.category == 'reference' || @.price > 10)].

你也可以使用！否定一个谓词 [?(!(@.price < 10 && @.category == 'fiction'))].

#### 过滤谓词

谓词可以使用Filter API构建，如下所示：


```java
import static com.jayway.jsonpath.JsonPath.parse;
import static com.jayway.jsonpath.Criteria.where;
import static com.jayway.jsonpath.Filter.filter;
...
...

Filter cheapFictionFilter = filter(
   where("category").is("fiction").and("price").lte(10D)
);

List<Map<String, Object>> books = parse(json).read("$.store.book[?]", cheapFictionFilter);
```


注意占位符？ 为路径中的过滤器。 当提供多个过滤器时，它们按照占位符数量与提供的过滤器数量相匹配的顺序应用。 您可以在一个过滤器操作[？，？]中指定多个谓词占位符，这两个谓词都必须匹配。

过滤器也可以与“OR”和“AND”


```java
Filter fooOrBar = filter(
   where("foo").exists(true)).or(where("bar").exists(true)
);
   
Filter fooAndBar = filter(
   where("foo").exists(true)).and(where("bar").exists(true)
);
```


#### 自定义

第三个选择是实现你自己的谓词


```java
Predicate booksWithISBN = new Predicate() {
    @Override
    public boolean apply(PredicateContext ctx) {
        return ctx.item(Map.class).containsKey("isbn");
    }
};

List<Map<String, Object>> books = reader.read("$.store.book[?].isbn", List.class, booksWithISBN);
```


#### Path vs Value

在Goessner实现中，JsonPath可以返回Path或Value。 值是默认值，上面所有示例返回。 如果你想让我们查询的元素的路径可以通过选项来实现。


```java
Configuration conf = Configuration.builder().options(Option.AS_PATH_LIST).build();

List<String> pathList = using(conf).parse(json).read("$..author");

assertThat(pathList).containsExactly(
        "$['store']['book'][0]['author']", "$['store']['book'][1]['author']",
        "$['store']['book'][2]['author']", "$['store']['book'][3]['author']");
```

### 调整配置

选项创建配置时，有几个可以改变默认行为的选项标志。

#### DEFAULT_PATH_LEAF_TO_NULL

此选项使JsonPath对于缺少的叶子返回null。 考虑下面的json

```json
[
   {
      "name" : "john",
      "gender" : "male"
   },
   {
      "name" : "ben"
   }
]
```

```java
Configuration conf = Configuration.defaultConfiguration();

// 正常
String gender0 = JsonPath.using(conf).parse(json).read("$[0]['gender']");
// 异常 PathNotFoundException thrown
String gender1 = JsonPath.using(conf).parse(json).read("$[1]['gender']");

Configuration conf2 = conf.addOptions(Option.DEFAULT_PATH_LEAF_TO_NULL);

// 正常
String gender0 = JsonPath.using(conf2).parse(json).read("$[0]['gender']");
// 正常 (返回 null)
String gender1 = JsonPath.using(conf2).parse(json).read("$[1]['gender']");
```

#### ALWAYS_RETURN_LIST

此选项配置JsonPath返回列表

```java
Configuration conf = Configuration.defaultConfiguration();

// 正常
List<String> genders0 = JsonPath.using(conf).parse(json).read("$[0]['gender']");
// 异常 PathNotFoundException thrown
List<String> genders1 = JsonPath.using(conf).parse(json).read("$[1]['gender']");
```

#### SUPPRESS_EXCEPTIONS

该选项确保不会从路径评估传播异常。 遵循这些简单的规则：

- 如果选项ALWAYS_RETURN_LIST存在，将返回一个空列表
- 如果选项ALWAYS_RETURN_LIST不存在返回null

#### JsonProvider SPI

JsonPath is shipped with three different JsonProviders:

- [`JsonSmartJsonProvider`](https://code.google.com/p/json-smart/) (default)
- [`JacksonJsonProvider`](https://github.com/FasterXML/jackson)
- [`JacksonJsonNodeJsonProvider`](https://github.com/FasterXML/jackson)
- [`GsonJsonProvider`](https://code.google.com/p/google-gson/)
- [`JsonOrgJsonProvider`](http://www.json.org/java/index.html)

更改配置默认值只能在应用程序初始化时完成。 强烈不建议在运行时进行更改，尤其是在多线程应用程序中。

```java
Configuration.setDefaults(new Configuration.Defaults() {

    private final JsonProvider jsonProvider = new JacksonJsonProvider();
    private final MappingProvider mappingProvider = new JacksonMappingProvider();
      
    @Override
    public JsonProvider jsonProvider() {
        return jsonProvider;
    }

    @Override
    public MappingProvider mappingProvider() {
        return mappingProvider;
    }
    
    @Override
    public Set<Option> options() {
        return EnumSet.noneOf(Option.class);
    }
});
```

 

请注意，JacksonJsonProvider需要com.fasterxml.jackson.core：jackson-databind：2.4.5，GsonJsonProvider需要在您的类路径上使用com.google.code.gson：gson：2.3.1。

#### Cache SPI

在JsonPath 2.1.0中，引入了一种新的Cache SPI。 这允许API消费者以适合其需求的方式配置路径缓存。 缓存必须在首次访问之前配置，或者抛出`JsonPathException`。 JsonPath附带两个缓存实现

- com.jayway.jsonpath.spi.cache.LRUCache（默认，线程安全）
- com.jayway.jsonpath.spi.cache.NOOPCache（无缓存）

如果要实现自己的缓存，API很简单。

```java
CacheProvider.setCache(new Cache() {
    //Not thread safe simple cache
    private Map<String, JsonPath> map = new HashMap<String, JsonPath>();

    @Override
    public JsonPath get(String key) {
        return map.get(key);
    }

    @Override
    public void put(String key, JsonPath jsonPath) {
        map.put(key, jsonPath);
    }
}); 
```

### Java操作示例源码

json

```json
{
    "store":{
        "book":[
            {
                "category":"reference",
                "author":"Nigel Rees",
                "title":"Sayings of the Century",
                "price":8.95
            },
            {
                "category":"fiction",
                "author":"Evelyn Waugh",
                "title":"Sword of Honour",
                "price":12.99
            },
            {
                "category":"fiction",
                "author":"Herman Melville",
                "title":"Moby Dick",
                "isbn":"0-553-21311-3",
                "price":8.99
            },
            {
                "category":"JavaWeb",
                "author":"X-rapido",
                "title":"Top-link",
                "isbn":"0-553-211231-3",
                "price":32.68
            },
            {
                "category":"fiction",
                "author":"J. R. R. Tolkien",
                "title":"The Lord of the Rings",
                "isbn":"0-395-19395-8",
                "price":22.99
            }
        ],
        "bicycle":{
            "color":"red",
            "price":19.95
        }
    },
    "expensive":10
}
```

 

Java

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.util.Iterator;
import java.util.List;
import com.jayway.jsonpath.JsonPath;

public class TestJsonPath {
    public static void main(String[] args) {
        String sjson = readtxt();
        print("--------------------------------------getJsonValue--------------------------------------");
        getJsonValue(sjson);
    }

    private static String readtxt() {
        StringBuilder sb = new StringBuilder();
        try {
            FileReader fr = new FileReader("D:/books.txt");
            BufferedReader bfd = new BufferedReader(fr);
            String s = "";
            while((s=bfd.readLine())!=null) {
                sb.append(s);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(sb.toString());
        return sb.toString();
    }
    
    private static void getJsonValue(String json) {
        //The authors of all books：获取json中store下book下的所有author值
        List<String> authors1 = JsonPath.read(json, "$.store.book[*].author");
        
        //All authors：获取所有json中所有author的值
        List<String> authors2 = JsonPath.read(json, "$..author");
        
        //All things, both books and bicycles 
        //authors3返回的是net.minidev.json.JSONArray：获取json中store下的所有value值，不包含key，如key有两个，book和bicycle
        List<Object> authors3 = JsonPath.read(json, "$.store.*");
        
        //The price of everything：获取json中store下所有price的值
        List<Object> authors4 = JsonPath.read(json, "$.store..price");
        
        //The third book：获取json中book数组的第3个值
        List<Object> authors5 = JsonPath.read(json, "$..book[2]");
        
        //The first two books：获取json中book数组的第1和第2两个个值
        List<Object> authors6 = JsonPath.read(json, "$..book[0,1]");
        
        //All books from index 0 (inclusive) until index 2 (exclusive)：获取json中book数组的前两个区间值
        List<Object> authors7 = JsonPath.read(json, "$..book[:2]");
        
        //All books from index 1 (inclusive) until index 2 (exclusive)：获取json中book数组的第2个值
        List<Object> authors8 = JsonPath.read(json, "$..book[1:2]");
        
        //Last two books：获取json中book数组的最后两个值
        List<Object> authors9 = JsonPath.read(json, "$..book[-2:]");
        
        //Book number two from tail：获取json中book数组的第3个到最后一个的区间值
        List<Object> authors10 = JsonPath.read(json, "$..book[2:]");
        
        //All books with an ISBN number：获取json中book数组中包含isbn的所有值
        List<Object> authors11 = JsonPath.read(json, "$..book[?(@.isbn)]");
        
        //All books in store cheaper than 10：获取json中book数组中price<10的所有值
        List<Object> authors12 = JsonPath.read(json, "$.store.book[?(@.price < 10)]");
        
        //All books in store that are not "expensive"：获取json中book数组中price<=expensive的所有值
        List<Object> authors13 = JsonPath.read(json, "$..book[?(@.price <= $['expensive'])]");
        
        //All books matching regex (ignore case)：获取json中book数组中的作者以REES结尾的所有值（REES不区分大小写）
        List<Object> authors14 = JsonPath.read(json, "$..book[?(@.author =~ /.*REES/i)]");
        
        //Give me every thing：逐层列出json中的所有值，层级由外到内
        List<Object> authors15 = JsonPath.read(json, "$..*");
        
        //The number of books：获取json中book数组的长度
        List<Object> authors16 = JsonPath.read(json, "$..book.length()");
        print("**********authors1**********");
        print(authors1);
        print("**********authors2**********");
        print(authors2);
        print("**********authors3**********");
        printOb(authors3);
        print("**********authors4**********");
        printOb(authors4);
        print("**********authors5**********");
        printOb(authors5);
        print("**********authors6**********");
        printOb(authors6);
        print("**********authors7**********");
        printOb(authors7);
        print("**********authors8**********");
        printOb(authors8);
        print("**********authors9**********");
        printOb(authors9);
        print("**********authors10**********");
        printOb(authors10);
        print("**********authors11**********");
        printOb(authors11);
        print("**********authors12**********");
        printOb(authors12);
        print("**********authors13**********");
        printOb(authors13);
        print("**********authors14**********");
        printOb(authors14);
        print("**********authors15**********");
        printOb(authors15);
        print("**********authors16**********");
        printOb(authors16);
        
    }
    
    private static void print(List<String> list) {
        for(Iterator<String> it = list.iterator();it.hasNext();) {
            System.out.println(it.next());
        }
    }
    
    private static void printOb(List<Object> list) {
        for(Iterator<Object> it = list.iterator();it.hasNext();) {
            System.out.println(it.next());
        }
    }
    
    private static void print(String s) {
        System.out.println("\n"+s);
    }

}

输出
{"store":{"book":[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"JavaWeb","author":"X-rapido","title":"Top-link","isbn":"0-553-211231-3","price":32.68},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}],"bicycle":{"color":"red","price":19.95}},"expensive":10}

--------------------------------------getJsonValue--------------------------------------
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/D:/workSpaces/SupportPackge/MavenRepository/org/apache/logging/log4j/log4j-slf4j-impl/2.0.2/log4j-slf4j-impl-2.0.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/D:/workSpaces/SupportPackge/MavenRepository/org/slf4j/slf4j-log4j12/1.7.10/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
ERROR StatusLogger No log4j2 configuration file found. Using default configuration: logging only errors to the console.

**********authors1**********
Nigel Rees
Evelyn Waugh
Herman Melville
X-rapido
J. R. R. Tolkien

**********authors2**********
Nigel Rees
Evelyn Waugh
Herman Melville
X-rapido
J. R. R. Tolkien

**********authors3**********
[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"JavaWeb","author":"X-rapido","title":"Top-link","isbn":"0-553-211231-3","price":32.68},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}]
{color=red, price=19.95}

**********authors4**********
8.95
12.99
8.99
32.68
22.99
19.95

**********authors5**********
{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}

**********authors6**********
{category=reference, author=Nigel Rees, title=Sayings of the Century, price=8.95}
{category=fiction, author=Evelyn Waugh, title=Sword of Honour, price=12.99}

**********authors7**********
{category=reference, author=Nigel Rees, title=Sayings of the Century, price=8.95}
{category=fiction, author=Evelyn Waugh, title=Sword of Honour, price=12.99}

**********authors8**********
{category=fiction, author=Evelyn Waugh, title=Sword of Honour, price=12.99}

**********authors9**********
{category=JavaWeb, author=X-rapido, title=Top-link, isbn=0-553-211231-3, price=32.68}
{category=fiction, author=J. R. R. Tolkien, title=The Lord of the Rings, isbn=0-395-19395-8, price=22.99}

**********authors10**********
{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}
{category=JavaWeb, author=X-rapido, title=Top-link, isbn=0-553-211231-3, price=32.68}
{category=fiction, author=J. R. R. Tolkien, title=The Lord of the Rings, isbn=0-395-19395-8, price=22.99}

**********authors11**********
{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}
{category=JavaWeb, author=X-rapido, title=Top-link, isbn=0-553-211231-3, price=32.68}
{category=fiction, author=J. R. R. Tolkien, title=The Lord of the Rings, isbn=0-395-19395-8, price=22.99}

**********authors12**********
{category=reference, author=Nigel Rees, title=Sayings of the Century, price=8.95}
{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}

**********authors13**********
{category=reference, author=Nigel Rees, title=Sayings of the Century, price=8.95}
{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}

**********authors14**********
{category=reference, author=Nigel Rees, title=Sayings of the Century, price=8.95}

**********authors15**********
{book=[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"JavaWeb","author":"X-rapido","title":"Top-link","isbn":"0-553-211231-3","price":32.68},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}], bicycle={color=red, price=19.95}}
10
[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"JavaWeb","author":"X-rapido","title":"Top-link","isbn":"0-553-211231-3","price":32.68},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}]
{color=red, price=19.95}
{category=reference, author=Nigel Rees, title=Sayings of the Century, price=8.95}
{category=fiction, author=Evelyn Waugh, title=Sword of Honour, price=12.99}
{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}
{category=JavaWeb, author=X-rapido, title=Top-link, isbn=0-553-211231-3, price=32.68}
{category=fiction, author=J. R. R. Tolkien, title=The Lord of the Rings, isbn=0-395-19395-8, price=22.99}
reference
Nigel Rees
Sayings of the Century
8.95
fiction
Evelyn Waugh
Sword of Honour
12.99
fiction
Herman Melville
Moby Dick
0-553-21311-3
8.99
JavaWeb
X-rapido
Top-link
0-553-211231-3
32.68
fiction
J. R. R. Tolkien
The Lord of the Rings
0-395-19395-8
22.99
red
19.95

**********authors16**********
5
```

 

示例2

Java

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

import com.jayway.jsonpath.Configuration;
import com.jayway.jsonpath.JsonPath;
import com.jayway.jsonpath.ReadContext;

public class TestJsonPath3 {
    public static void main(String[] args) {
        String sjson = readtxt();
        print("-----------------------getJsonValue0-----------------------");
        getJsonValue0(sjson);
        print("-----------------------getJsonValue1-----------------------");
        getJsonValue1(sjson);
        print("-----------------------getJsonValue2-----------------------");
        getJsonValue2(sjson);
        print("-----------------------getJsonValue3-----------------------");
        getJsonValue3(sjson);
        print("-----------------------getJsonValue4-----------------------");
        getJsonValue4(sjson);
    }

    private static String readtxt() {
        StringBuilder sb = new StringBuilder();
        try {
            FileReader fr = new FileReader("D:/books.txt");
            BufferedReader bfd = new BufferedReader(fr);
            String s = "";
            while((s=bfd.readLine())!=null) {
                sb.append(s);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(sb.toString());
        return sb.toString();
    }
    

    /**
     * 读取json的一种写法，得到匹配表达式的所有值
     * */
    private static void getJsonValue0(String json) {
        List<String> authors = JsonPath.read(json, "$.store.book[*].author");
        print(authors);
        
    }
    
    /**
     * 读取JSON得到某个具体值（推荐使用这种方法，一次解析多次调用）
     * */
    private static void getJsonValue1(String json) {
        Object document = Configuration.defaultConfiguration().jsonProvider().parse(json);
        String author0 = JsonPath.read(document, "$.store.book[0].author");
        String author1 = JsonPath.read(document, "$.store.book[1].author");
        print(author0);
        print(author1);
        
    }
    
    /**
     * 读取json的一种写法
     * */
    private static void getJsonValue2(String json) {
        ReadContext ctx = JsonPath.parse(json);
        // 获取json中book数组中包含isbn的作者
        List<String> authorsOfBooksWithISBN = ctx.read("$.store.book[?(@.isbn)].author");
        
        // 获取json中book数组中价格大于10的对象
        List<Map<String, Object>> expensiveBooks = JsonPath
                                    .using(Configuration.defaultConfiguration())
                                    .parse(json)
                                    .read("$.store.book[?(@.price > 10)]", List.class);
        print(authorsOfBooksWithISBN);
        print("********Map********");
        printListMap(expensiveBooks);
    }
    
    /**
     * 读取JSON得到的值是一个String，所以不能用List存储 
     * */
    private static void getJsonValue3(String json) {
        //Will throw an java.lang.ClassCastException    
        //List<String> list = JsonPath.parse(json).read("$.store.book[0].author");
        //由于会抛异常，暂时注释上面一行，要用的话，应使用下面的格式
        
        String author = JsonPath.parse(json).read("$.store.book[0].author");
        print(author);
    }
    
    /**
     * 读取json的一种写法，支持逻辑表达式，&&和||
     */
    private static void getJsonValue4(String json) {
        List<Map<String, Object>> books1 =  JsonPath.parse(json).read("$.store.book[?(@.price < 10 && @.category == 'fiction')]");
        List<Map<String, Object>> books2 =  JsonPath.parse(json).read("$.store.book[?(@.category == 'reference' || @.price > 10)]");
        print("********books1********");
        printListMap(books1);
        print("********books2********");
        printListMap(books2);
    }
    
    private static void print(List<String> list) {
        for(Iterator<String> it = list.iterator();it.hasNext();) {
            System.out.println(it.next());
        }
    }
    
    private static void printListMap(List<Map<String, Object>> list) {
        for(Iterator<Map<String, Object>> it = list.iterator();it.hasNext();) {
            Map<String, Object> map = it.next();
            for(Iterator iterator =map.entrySet().iterator();iterator.hasNext();) {
                System.out.println(iterator.next());
            }
        }
    }
    
    private static void print(String s) {
        System.out.println("\n"+s);
    }

}

输出
{"store":{"book":[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"JavaWeb","author":"X-rapido","title":"Top-link","isbn":"0-553-211231-3","price":32.68},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}],"bicycle":{"color":"red","price":19.95}},"expensive":10}

-----------------------getJsonValue0-----------------------
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/D:/workSpaces/SupportPackge/MavenRepository/org/apache/logging/log4j/log4j-slf4j-impl/2.0.2/log4j-slf4j-impl-2.0.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/D:/workSpaces/SupportPackge/MavenRepository/org/slf4j/slf4j-log4j12/1.7.10/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
ERROR StatusLogger No log4j2 configuration file found. Using default configuration: logging only errors to the console.
Nigel Rees
Evelyn Waugh
Herman Melville
X-rapido
J. R. R. Tolkien

-----------------------getJsonValue1-----------------------

Nigel Rees

Evelyn Waugh

-----------------------getJsonValue2-----------------------
Herman Melville
X-rapido
J. R. R. Tolkien

********Map********
category=fiction
author=Evelyn Waugh
title=Sword of Honour
price=12.99
category=JavaWeb
author=X-rapido
title=Top-link
isbn=0-553-211231-3
price=32.68
category=fiction
author=J. R. R. Tolkien
title=The Lord of the Rings
isbn=0-395-19395-8
price=22.99

-----------------------getJsonValue3-----------------------

Nigel Rees

-----------------------getJsonValue4-----------------------

********books1********
category=fiction
author=Herman Melville
title=Moby Dick
isbn=0-553-21311-3
price=8.99

********books2********
category=reference
author=Nigel Rees
title=Sayings of the Century
price=8.95
category=fiction
author=Evelyn Waugh
title=Sword of Honour
price=12.99
category=JavaWeb
author=X-rapido
title=Top-link
isbn=0-553-211231-3
price=32.68
category=fiction
author=J. R. R. Tolkien
title=The Lord of the Rings
isbn=0-395-19395-8
price=22.99
```

 

### 过滤器示例

Java

```java
public static void main(String[] args) {
    String json = readtxt();
    Object document = Configuration.defaultConfiguration().jsonProvider().parse(json);
    
    // 过滤器链（查找包含isbn并category中有fiction或JavaWeb的值）
    Filter filter = Filter.filter(Criteria.where("isbn").exists(true).and("category").in("fiction", "JavaWeb"));
    List<Object> books = JsonPath.read(document, "$.store.book[?]", filter);
    printOb(books);
    
    System.out.println("\n----------------------------------------------\n");
    
    // 自定义过滤器
    Filter mapFilter = new Filter() {
        @Override
        public boolean apply(PredicateContext context) {
            Map<String, Object> map = context.item(Map.class);    
            if (map.containsKey("isbn")) {
                return true;
            }
            return false;
        }
    };
    List<Object> books2 = JsonPath.read(document, "$.store.book[?]", mapFilter);
    
    printOb(books2);
    
}

输出
{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}
{category=JavaWeb, author=X-rapido, title=Top-link, isbn=0-553-211231-3, price=32.68}
{category=fiction, author=J. R. R. Tolkien, title=The Lord of the Rings, isbn=0-395-19395-8, price=22.99}

----------------------------------------------

{category=fiction, author=Herman Melville, title=Moby Dick, isbn=0-553-21311-3, price=8.99}
{category=JavaWeb, author=X-rapido, title=Top-link, isbn=0-553-211231-3, price=32.68}
{category=fiction, author=J. R. R. Tolkien, title=The Lord of the Rings, isbn=0-395-19395-8, price=22.99} 
```

由此可见，使用过滤器，或者使用上述的表达式，都是可以达到理想的效果

示例中的books2中，注意到context.item(Map.class)； 这句话

这句中的Map.class是根据预定的结果类型定义的，如果返回的是String类型值，那就改为String.class

我的示例只简单判断了一下isbn属性，可以根据实际情况进行变更条件





```json
{
   "completed_in" : 0.072,
   "max_id" : 336448826225328100,
   "max_id_str" : "336448826225328128",
   "next_page" : "?page=2&max_id=336448826225328128&q=gatling",
   "page" : 1,
   "query" : "gatling",
   "refresh_url" : "?since_id=336448826225328128&q=gatling",
   "results" : [
      {
         "created_at" : "Mon, 20 May 2013 11:50:25 +0000",
         "from_user" : "anna_gatling",
         "from_user_id" : 1237619628,
         "from_user_id_str" : "1237619628",
         "from_user_name" : "Anna Gatling",
         "geo" : null,
         "id" : 336448826225328100,
         "id_str" : "336448826225328128",
         "iso_language_code" : "en",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3402698946/bebb3fcb7a7a3977a69c30797cfce5db_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3402698946/bebb3fcb7a7a3977a69c30797cfce5db_normal.jpeg",
         "source" : "&lt;a href=&quot;http://twitter.com/download/iphone&quot;&gt;Twitter for iPhone&lt;/a&gt;",
         "text" : "Last Monday of 6th grade yayyyyy"
      },
      {
         "created_at" : "Mon, 20 May 2013 11:49:40 +0000",
         "from_user" : "ddnn_",
         "from_user_id" : 482530993,
         "from_user_id_str" : "482530993",
         "from_user_name" : "D'の純情",
         "geo" : null,
         "id" : 336448635355144200,
         "id_str" : "336448635355144192",
         "iso_language_code" : "tl",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3676715268/74fa7666ffce02296cc7dfa25f6a8f9c_normal.png",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3676715268/74fa7666ffce02296cc7dfa25f6a8f9c_normal.png",
         "source" : "&lt;a href=&quot;http://m.ubersocial.com&quot;&gt;UberSocial Mobile&lt;/a&gt;",
         "text" : "@Arc46_ gatling gun... as in minigun or what?",
         "to_user" : "Arc46_",
         "to_user_id" : 336528266,
         "to_user_id_str" : "336528266",
         "to_user_name" : "SamuelJunioPradipta",
         "in_reply_to_status_id" : 336448069233172500,
         "in_reply_to_status_id_str" : "336448069233172480"
      },
      {
         "created_at" : "Mon, 20 May 2013 11:48:56 +0000",
         "from_user" : "FonshudelSur",
         "from_user_id" : 300933633,
         "from_user_id_str" : "300933633",
         "from_user_name" : "Fonshu",
         "geo" : null,
         "id" : 336448452076634100,
         "id_str" : "336448452076634113",
         "iso_language_code" : "es",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3654066775/8da1e0e3f2d9ef89f34ac66993bb8836_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3654066775/8da1e0e3f2d9ef89f34ac66993bb8836_normal.jpeg",
         "source" : "&lt;a href=&quot;http://www.tweetdeck.com&quot;&gt;TweetDeck&lt;/a&gt;",
         "text" : "@IsaraMiau Y tengo que currarme un cosplay para ir del patriota mecanizado con la gatling steampunk, que es DE LO MEJOR &lt;3",
         "to_user" : "FonshudelSur",
         "to_user_id" : 300933633,
         "to_user_id_str" : "300933633",
         "to_user_name" : "Fonshu",
         "in_reply_to_status_id" : 336448304835608600,
         "in_reply_to_status_id_str" : "336448304835608576"
      },
      {
         "created_at" : "Mon, 20 May 2013 11:47:25 +0000",
         "from_user" : "Arc46_",
         "from_user_id" : 336528266,
         "from_user_id_str" : "336528266",
         "from_user_name" : "SamuelJunioPradipta",
         "geo" : null,
         "id" : 336448069233172500,
         "id_str" : "336448069233172480",
         "iso_language_code" : "en",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3684084503/2e880c5f9b9d7d6030d6d972feb9f5c4_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3684084503/2e880c5f9b9d7d6030d6d972feb9f5c4_normal.jpeg",
         "source" : "&lt;a href=&quot;http://www.tweetdeck.com&quot;&gt;TweetDeck&lt;/a&gt;",
         "text" : "Something like Gatling gun, Bazooka RT @ddnn_: what are you exactly trying to make?"
      },
      {
         "created_at" : "Mon, 20 May 2013 11:37:49 +0000",
         "from_user" : "anna_gatling",
         "from_user_id" : 1237619628,
         "from_user_id_str" : "1237619628",
         "from_user_name" : "Anna Gatling",
         "geo" : null,
         "id" : 336445653662191600,
         "id_str" : "336445653662191616",
         "iso_language_code" : "en",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3402698946/bebb3fcb7a7a3977a69c30797cfce5db_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3402698946/bebb3fcb7a7a3977a69c30797cfce5db_normal.jpeg",
         "source" : "&lt;a href=&quot;http://twitter.com/download/iphone&quot;&gt;Twitter for iPhone&lt;/a&gt;",
         "text" : "Mondays&lt;"
      },
      {
         "created_at" : "Mon, 20 May 2013 11:29:57 +0000",
         "from_user" : "GATLING_FIGHTER",
         "from_user_id" : 1184500015,
         "from_user_id_str" : "1184500015",
         "from_user_name" : "M134 みに-がん",
         "geo" : null,
         "id" : 336443676270157800,
         "id_str" : "336443676270157826",
         "iso_language_code" : "ja",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3607829622/68059740f01d6532bc876e2e7ffb84d0_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3607829622/68059740f01d6532bc876e2e7ffb84d0_normal.jpeg",
         "source" : "&lt;a href=&quot;http://theworld09.com/&quot;&gt;TheWorld⠀&lt;/a&gt;",
         "text" : "@Orz619 何ですって！？",
         "to_user" : "Orz619",
         "to_user_id" : 1393895851,
         "to_user_id_str" : "1393895851",
         "to_user_name" : "ぐっさん@黒猫は号哭していますよ。",
         "in_reply_to_status_id" : 336443439870779400,
         "in_reply_to_status_id_str" : "336443439870779394"
      },
      {
         "created_at" : "Mon, 20 May 2013 11:29:48 +0000",
         "from_user" : "GATLING_FIGHTER",
         "from_user_id" : 1184500015,
         "from_user_id_str" : "1184500015",
         "from_user_name" : "M134 みに-がん",
         "geo" : null,
         "id" : 336443634872360960,
         "id_str" : "336443634872360960",
         "iso_language_code" : "ja",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3607829622/68059740f01d6532bc876e2e7ffb84d0_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3607829622/68059740f01d6532bc876e2e7ffb84d0_normal.jpeg",
         "source" : "&lt;a href=&quot;http://theworld09.com/&quot;&gt;TheWorld⠀&lt;/a&gt;",
         "text" : "RT @Orz619: ほんきでスパブロしようかと検討中w"
      },
      {
         "created_at" : "Mon, 20 May 2013 10:46:41 +0000",
         "from_user" : "Gatling_gun_k",
         "from_user_id" : 1126180920,
         "from_user_id_str" : "1126180920",
         "from_user_name" : "개틀링 기관총",
         "geo" : null,
         "id" : 336432787248787460,
         "id_str" : "336432787248787456",
         "iso_language_code" : "ko",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3171674022/d318e014542845396c11662986b4285d_normal.png",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3171674022/d318e014542845396c11662986b4285d_normal.png",
         "source" : "&lt;a href=&quot;http://twittbot.net/&quot;&gt;twittbot.net&lt;/a&gt;",
         "text" : "타타타타타타타타타타타타타타!!"
      },
      {
         "created_at" : "Mon, 20 May 2013 10:22:40 +0000",
         "from_user" : "slam_gatling",
         "from_user_id" : 1316243516,
         "from_user_id_str" : "1316243516",
         "from_user_name" : "SxOxE",
         "geo" : null,
         "id" : 336426741893578750,
         "id_str" : "336426741893578754",
         "iso_language_code" : "ja",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3606289395/106dee12ccd2fb0bca99ba4b85304d54_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3606289395/106dee12ccd2fb0bca99ba4b85304d54_normal.jpeg",
         "source" : "&lt;a href=&quot;http://twitter.com/&quot;&gt;web&lt;/a&gt;",
         "text" : "妹がHUNTER×HUNTERを読んでるせいか、俺も中学以来久々にハマった。暗黒大陸編がどうなるか気になるけど、大暮維人みたいに話をデカくしすぎて収集つかなくなる可能性も微レ存。あとクラピカが今後どう関わってくるのかも気になるところ。"
      },
      {
         "created_at" : "Mon, 20 May 2013 09:46:45 +0000",
         "from_user" : "Gatling_gun_k",
         "from_user_id" : 1126180920,
         "from_user_id_str" : "1126180920",
         "from_user_name" : "개틀링 기관총",
         "geo" : null,
         "id" : 336417701356507140,
         "id_str" : "336417701356507136",
         "iso_language_code" : "ko",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3171674022/d318e014542845396c11662986b4285d_normal.png",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3171674022/d318e014542845396c11662986b4285d_normal.png",
         "source" : "&lt;a href=&quot;http://twittbot.net/&quot;&gt;twittbot.net&lt;/a&gt;",
         "text" : "투투투투투투투투투투투투투투투!!"
      },
      {
         "created_at" : "Mon, 20 May 2013 09:17:47 +0000",
         "from_user" : "Gatling_gun_k",
         "from_user_id" : 1126180920,
         "from_user_id_str" : "1126180920",
         "from_user_name" : "개틀링 기관총",
         "geo" : null,
         "id" : 336410413707169800,
         "id_str" : "336410413707169792",
         "iso_language_code" : "ko",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3171674022/d318e014542845396c11662986b4285d_normal.png",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3171674022/d318e014542845396c11662986b4285d_normal.png",
         "source" : "&lt;a href=&quot;http://twittbot.net/&quot;&gt;twittbot.net&lt;/a&gt;",
         "text" : "뚜두두두둗두두두두두두두두두두!!"
      },
      {
         "created_at" : "Mon, 20 May 2013 09:03:00 +0000",
         "from_user" : "Rebelzonderrede",
         "from_user_id" : 983435376,
         "from_user_id_str" : "983435376",
         "from_user_name" : "Rebel South",
         "geo" : null,
         "id" : 336406693384708100,
         "id_str" : "336406693384708096",
         "iso_language_code" : "nl",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/2920503711/6db32e22de8ee8efa92ba1e16ef341b6_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/2920503711/6db32e22de8ee8efa92ba1e16ef341b6_normal.jpeg",
         "source" : "&lt;a href=&quot;http://www.tweetdeck.com&quot;&gt;TweetDeck&lt;/a&gt;",
         "text" : "20-5-1874: Sterfdag Alexander B. Dyer, Gen vd Unie. 1e Generaal die aanschaf Gatling gun nastreefde #amerikaanseburgeroorlog"
      },
      {
         "created_at" : "Mon, 20 May 2013 08:45:00 +0000",
         "from_user" : "MahdayMayday",
         "from_user_id" : 896598301,
         "from_user_id_str" : "896598301",
         "from_user_name" : "C H E ' N G A P !",
         "geo" : null,
         "id" : 336402162995310600,
         "id_str" : "336402162995310592",
         "iso_language_code" : "in",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/3089084467/b5406e2a17e115108d798220ef872e59_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/3089084467/b5406e2a17e115108d798220ef872e59_normal.jpeg",
         "source" : "&lt;a href=&quot;http://twitter.com/&quot;&gt;web&lt;/a&gt;",
         "text" : "RT @mnhfz92: @MahdayMayday ah ye..bg Gomu Gomu no Elephant Gatling kang",
         "in_reply_to_status_id" : 336401088506900500,
         "in_reply_to_status_id_str" : "336401088506900480"
      },
      {
         "created_at" : "Mon, 20 May 2013 08:43:55 +0000",
         "from_user" : "mnhfz92",
         "from_user_id" : 282901250,
         "from_user_id_str" : "282901250",
         "from_user_name" : "Nor Hafiz",
         "geo" : null,
         "id" : 336401891959402500,
         "id_str" : "336401891959402496",
         "iso_language_code" : "in",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/2661900827/f41fc3f51c58daac67f1aa481de43819_normal.jpeg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/2661900827/f41fc3f51c58daac67f1aa481de43819_normal.jpeg",
         "source" : "&lt;a href=&quot;http://twitter.com/&quot;&gt;web&lt;/a&gt;",
         "text" : "@MahdayMayday ah ye..bg Gomu Gomu no Elephant Gatling kang",
         "to_user" : "MahdayMayday",
         "to_user_id" : 896598301,
         "to_user_id_str" : "896598301",
         "to_user_name" : "C H E ' N G A P !",
         "in_reply_to_status_id" : 336401088506900500,
         "in_reply_to_status_id_str" : "336401088506900480"
      },
      {
         "created_at" : "Mon, 20 May 2013 08:00:00 +0000",
         "from_user" : "origichara_bot",
         "from_user_id" : 410573195,
         "from_user_id_str" : "410573195",
         "from_user_name" : "みんなのオリキャラ紹介bot",
         "geo" : null,
         "id" : 336390837846020100,
         "id_str" : "336390837846020096",
         "iso_language_code" : "ja",
         "metadata" : {
            "result_type" : "recent"
         },
         "profile_image_url" : "http://a0.twimg.com/profile_images/1634960810/_____bot_normal.jpg",
         "profile_image_url_https" : "https://si0.twimg.com/profile_images/1634960810/_____bot_normal.jpg",
         "source" : "&lt;a href=&quot;http://twittbot.net/&quot;&gt;twittbot.net&lt;/a&gt;",
         "text" : "「救世主(メシア)とよべ、いいな？」「黙れ変態変人ケミカルブレイン女が。私だって元は人間だ」／八ヶ峰臨太郎（男･15歳・自称救世主の中二病ハイカラ少年）http://t.co/FS7av2d077"
      }
   ],
   "results_per_page" : 15,
   "since_id" : 0,
   "since_id_str" : "0"
}
```

```json
{
   "web-app" : {
      "servlet" : [
         {
            "servlet-name" : "cofaxCDS",
            "servlet-class" : "org.cofax.cds.CDSServlet",
            "init-param" : {
               "configGlossary:installationAt" : "Philadelphia, PA",
               "configGlossary:adminEmail" : "ksm@pobox.com",
               "configGlossary:poweredBy" : "Cofax",
               "configGlossary:poweredByIcon" : "/images/cofax.gif",
               "configGlossary:staticPath" : "/content/static",
               "templateProcessorClass" : "org.cofax.WysiwygTemplate",
               "templateLoaderClass" : "org.cofax.FilesTemplateLoader",
               "templatePath" : "templates",
               "templateOverridePath" : "",
               "defaultListTemplate" : "listTemplate.htm",
               "defaultFileTemplate" : "articleTemplate.htm",
               "useJSP" : false,
               "jspListTemplate" : "listTemplate.jsp",
               "jspFileTemplate" : "articleTemplate.jsp",
               "cachePackageTagsTrack" : 200,
               "cachePackageTagsStore" : 200,
               "cachePackageTagsRefresh" : 60,
               "cacheTemplatesTrack" : 100,
               "cacheTemplatesStore" : 50,
               "cacheTemplatesRefresh" : 15,
               "cachePagesTrack" : 200,
               "cachePagesStore" : 100,
               "cachePagesRefresh" : 10,
               "cachePagesDirtyRead" : 10,
               "searchEngineListTemplate" : "forSearchEnginesList.htm",
               "searchEngineFileTemplate" : "forSearchEngines.htm",
               "searchEngineRobotsDb" : "WEB-INF/robots.db",
               "useDataStore" : true,
               "dataStoreClass" : "org.cofax.SqlDataStore",
               "redirectionClass" : "org.cofax.SqlRedirection",
               "dataStoreName" : "cofax",
               "dataStoreDriver" : "com.microsoft.jdbc.sqlserver.SQLServerDriver",
               "dataStoreUrl" : "jdbc:microsoft:sqlserver://LOCALHOST:1433;DatabaseName=goon",
               "dataStoreUser" : "sa",
               "dataStorePassword" : "dataStoreTestQuery",
               "dataStoreTestQuery" : "SET NOCOUNT ON;select test='test';",
               "dataStoreLogFile" : "/usr/local/tomcat/logs/datastore.log",
               "dataStoreInitConns" : 10,
               "dataStoreMaxConns" : 100,
               "dataStoreConnUsageLimit" : 100,
               "dataStoreLogLevel" : "debug",
               "maxUrlLength" : 500
            }
         },
         {
            "servlet-name" : "cofaxEmail",
            "servlet-class" : "org.cofax.cds.EmailServlet",
            "init-param" : {
               "mailHost" : "mail1",
               "mailHostOverride" : "mail2"
            }
         },
         {
            "servlet-name" : "cofaxAdmin",
            "servlet-class" : "org.cofax.cds.AdminServlet"
         },
         {
            "servlet-name" : "fileServlet",
            "servlet-class" : "org.cofax.cds.FileServlet"
         },
         {
            "servlet-name" : "cofaxTools",
            "servlet-class" : "org.cofax.cms.CofaxToolsServlet",
            "init-param" : {
               "templatePath" : "toolstemplates/",
               "log" : 1,
               "logLocation" : "/usr/local/tomcat/logs/CofaxTools.log",
               "logMaxSize" : "",
               "dataLog" : 1,
               "dataLogLocation" : "/usr/local/tomcat/logs/dataLog.log",
               "dataLogMaxSize" : "",
               "removePageCache" : "/content/admin/remove?cache=pages&id=",
               "removeTemplateCache" : "/content/admin/remove?cache=templates&id=",
               "fileTransferFolder" : "/usr/local/tomcat/webapps/content/fileTransferFolder",
               "lookInContext" : 1,
               "adminGroupID" : 4,
               "betaServer" : true
            }
         }
      ],
      "servlet-mapping" : {
         "cofaxCDS" : "/",
         "cofaxEmail" : "/cofaxutil/aemail/*",
         "cofaxAdmin" : "/admin/*",
         "fileServlet" : "/static/*",
         "cofaxTools" : "/tools/*"
      },
      "taglib" : {
         "taglib-uri" : "cofax.tld",
         "taglib-location" : "/WEB-INF/tlds/cofax.tld"
      }
   }
}
```

```json
[
   {
      "id" : 0,
      "guid" : "d7c8d5e2-cfdc-43b9-9bd4-15e8fd344157",
      "isActive" : true,
      "balance" : "$3,772.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 35,
      "name" : "Snow Rodriguez",
      "gender" : "male",
      "company" : "Applideck",
      "email" : "snowrodriguez@applideck.com",
      "phone" : "+1 (867) 551-3944",
      "address" : "548 Maple Avenue, Russellville, Pennsylvania, 2467",
      "about" : "Ex aute adipisicing incididunt labore est magna aliquip non consequat fugiat nostrud ex. Occaecat commodo irure ut mollit veniam fugiat esse esse laboris velit officia. Duis cillum aute ipsum sunt elit irure laborum nostrud. Dolor aliquip reprehenderit voluptate enim cupidatat nisi cillum enim quis aute fugiat id consequat.\r\n",
      "registered" : "1992-04-24T19:03:08 -02:00",
      "latitude" : -69.347636,
      "longitude" : 92.197293,
      "tags" : [
         "enim",
         "culpa",
         "proident",
         "duis",
         "aute",
         "tempor",
         "anim"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Garrison Tillman"
         },
         {
            "id" : 1,
            "name" : "Shawn Holman"
         },
         {
            "id" : 2,
            "name" : "Amy Guerra"
         }
      ],
      "randomArrayItem" : "lemon"
   },
   {
      "id" : 1,
      "guid" : "2b7e2b0a-288f-404e-9fd0-ad2064cc49f4",
      "isActive" : true,
      "balance" : "$3,046.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 24,
      "name" : "Phyllis Calhoun",
      "gender" : "female",
      "company" : "Olucore",
      "email" : "phylliscalhoun@olucore.com",
      "phone" : "+1 (890) 443-2093",
      "address" : "355 Brighton Avenue, Shrewsbury, Washington, 8719",
      "about" : "Excepteur magna culpa do exercitation anim cillum ad proident nostrud ex. Voluptate sint pariatur nisi magna et cillum Lorem Lorem in deserunt eu. Proident dolor laborum nisi anim. Veniam aliqua nulla minim adipisicing magna exercitation veniam sint ea minim nulla.\r\n",
      "registered" : "1996-12-05T15:20:59 -01:00",
      "latitude" : -19.65876,
      "longitude" : 168.158924,
      "tags" : [
         "ad",
         "exercitation",
         "laboris",
         "id",
         "incididunt",
         "nisi",
         "non"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Serena Collins"
         },
         {
            "id" : 1,
            "name" : "Carolina Anthony"
         },
         {
            "id" : 2,
            "name" : "Kendra Avery"
         }
      ],
      "randomArrayItem" : "lemon"
   },
   {
      "id" : 2,
      "guid" : "523d89ac-63aa-4fac-9352-07cbd8f21a6a",
      "isActive" : true,
      "balance" : "$1,242.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 38,
      "name" : "Lindsay Stuart",
      "gender" : "female",
      "company" : "Papricut",
      "email" : "lindsaystuart@papricut.com",
      "phone" : "+1 (859) 589-3940",
      "address" : "688 Wyckoff Street, Carrizo, California, 6999",
      "about" : "Do exercitation labore in excepteur laborum eiusmod enim officia proident nulla veniam. Do Lorem dolor eiusmod proident qui in consequat aliqua. Lorem nulla id consequat ea enim ad id exercitation dolor ad dolore.\r\n",
      "registered" : "1997-01-09T23:08:18 -01:00",
      "latitude" : 87.910568,
      "longitude" : 163.037238,
      "tags" : [
         "qui",
         "mollit",
         "aute",
         "et",
         "veniam",
         "elit",
         "labore"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Bullock Hutchinson"
         },
         {
            "id" : 1,
            "name" : "Guthrie Clark"
         },
         {
            "id" : 2,
            "name" : "Elena Delaney"
         }
      ],
      "randomArrayItem" : "cherry"
   },
   {
      "id" : 3,
      "guid" : "bc7a2767-94e5-4ebc-8f9d-ead42c3e0107",
      "isActive" : true,
      "balance" : "$2,533.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 33,
      "name" : "Mcmahon Adams",
      "gender" : "male",
      "company" : "Balooba",
      "email" : "mcmahonadams@balooba.com",
      "phone" : "+1 (984) 475-3671",
      "address" : "880 Pilling Street, Omar, Minnesota, 8563",
      "about" : "Excepteur incididunt commodo veniam enim esse magna. Exercitation dolor cupidatat consequat irure. Ex duis dolor non nostrud duis aliqua enim labore aliquip. Ad qui reprehenderit aute aliquip duis magna aliquip nisi nulla. Cillum ad sint reprehenderit officia id nostrud cillum excepteur cillum ea labore ullamco esse. Laboris sint ut id incididunt Lorem non reprehenderit adipisicing. Id sint laboris deserunt ea.\r\n",
      "registered" : "2005-03-01T02:26:24 -01:00",
      "latitude" : 6.359315,
      "longitude" : 33.053339,
      "tags" : [
         "Lorem",
         "ex",
         "dolore",
         "pariatur",
         "officia",
         "ipsum",
         "dolore"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Mollie Wilcox"
         },
         {
            "id" : 1,
            "name" : "Alta Hall"
         },
         {
            "id" : 2,
            "name" : "Paulette Pollard"
         }
      ],
      "randomArrayItem" : "lemon"
   },
   {
      "id" : 4,
      "guid" : "9bbe7981-9e4c-4140-a7f4-be8fcae8ae18",
      "isActive" : true,
      "balance" : "$2,731.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 29,
      "name" : "Nanette Butler",
      "gender" : "female",
      "company" : "Nikuda",
      "email" : "nanettebutler@nikuda.com",
      "phone" : "+1 (989) 498-2116",
      "address" : "637 Colby Court, Whitewater, Vermont, 1992",
      "about" : "Magna Lorem sit sit duis. Aliqua eiusmod dolor enim est duis nulla in eu deserunt commodo. Sit laboris deserunt sint duis reprehenderit in sunt excepteur proident. Elit ad aliquip deserunt in ad occaecat id elit non commodo adipisicing ullamco magna.\r\n",
      "registered" : "1995-04-25T05:22:25 -02:00",
      "latitude" : -45.069546,
      "longitude" : 83.01566,
      "tags" : [
         "est",
         "in",
         "nulla",
         "ea",
         "dolore",
         "aliqua",
         "deserunt"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Noble Rodgers"
         },
         {
            "id" : 1,
            "name" : "Haley Mayer"
         },
         {
            "id" : 2,
            "name" : "Hudson Gentry"
         }
      ],
      "randomArrayItem" : "lemon"
   },
   {
      "id" : 5,
      "guid" : "f0449f33-5c39-4a94-a114-e96d466cc9a8",
      "isActive" : true,
      "balance" : "$1,769.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 26,
      "name" : "Miranda Thomas",
      "gender" : "male",
      "company" : "Softmicro",
      "email" : "mirandathomas@softmicro.com",
      "phone" : "+1 (870) 591-2118",
      "address" : "503 Linden Street, Jackpot, Rhode Island, 419",
      "about" : "Consequat tempor esse pariatur minim magna ut ut dolor officia anim. Mollit mollit tempor do dolor ad aute proident commodo sit ea minim. Eu et laborum est in. Non exercitation est deserunt do mollit tempor duis. Nisi eu nulla ea cillum minim officia et duis quis sint. Consequat velit cupidatat Lorem sunt do do quis et ullamco. Elit eiusmod consectetur ullamco ea amet sint aute quis ipsum.\r\n",
      "registered" : "2010-10-07T07:31:46 -02:00",
      "latitude" : -28.329413,
      "longitude" : -17.80299,
      "tags" : [
         "fugiat",
         "reprehenderit",
         "adipisicing",
         "culpa",
         "quis",
         "pariatur",
         "deserunt"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Brewer Norton"
         },
         {
            "id" : 1,
            "name" : "Ramona Jacobs"
         },
         {
            "id" : 2,
            "name" : "Russo Hewitt"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 6,
      "guid" : "18fb0572-e2b8-4f47-bd71-dc9349396227",
      "isActive" : true,
      "balance" : "$3,081.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 30,
      "name" : "Ada Potter",
      "gender" : "female",
      "company" : "Melbacor",
      "email" : "adapotter@melbacor.com",
      "phone" : "+1 (913) 474-2861",
      "address" : "947 Sumner Place, Haena, North Dakota, 685",
      "about" : "Nostrud amet id id qui. Reprehenderit officia duis ad aute ipsum eiusmod excepteur Lorem voluptate. Cillum laborum cillum aliquip duis reprehenderit amet sint non nulla in culpa irure veniam aliqua.\r\n",
      "registered" : "2008-09-26T03:40:28 -02:00",
      "latitude" : -11.153539,
      "longitude" : 115.382908,
      "tags" : [
         "laboris",
         "proident",
         "dolore",
         "sunt",
         "ut",
         "irure",
         "dolor"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Bethany Carroll"
         },
         {
            "id" : 1,
            "name" : "Moody Andrews"
         },
         {
            "id" : 2,
            "name" : "Trujillo Hester"
         }
      ],
      "randomArrayItem" : "cherry"
   },
   {
      "id" : 7,
      "guid" : "69cc1cdc-2953-4a00-84df-bed00120ab12",
      "isActive" : true,
      "balance" : "$2,520.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 36,
      "name" : "Levine Deleon",
      "gender" : "male",
      "company" : "Dragbot",
      "email" : "levinedeleon@dragbot.com",
      "phone" : "+1 (979) 575-3162",
      "address" : "902 Elmwood Avenue, Caberfae, Delaware, 4651",
      "about" : "Laboris nulla velit dolor ex duis pariatur aute est. Incididunt incididunt laborum non culpa. Consequat elit ipsum et sint nisi dolore dolore nisi ut. Quis nostrud ex ad duis. Enim aliquip pariatur deserunt amet est ullamco in. Dolor sunt deserunt id deserunt labore reprehenderit enim ut in consectetur laboris incididunt in nisi. Ipsum sint in ipsum minim ipsum proident commodo excepteur cillum.\r\n",
      "registered" : "2000-08-15T05:06:23 -02:00",
      "latitude" : -48.334821,
      "longitude" : 66.328798,
      "tags" : [
         "nostrud",
         "labore",
         "ipsum",
         "reprehenderit",
         "mollit",
         "consequat",
         "adipisicing"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Jolene Vang"
         },
         {
            "id" : 1,
            "name" : "Diana Frost"
         },
         {
            "id" : 2,
            "name" : "Bobbi Franklin"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 8,
      "guid" : "3a6bf80f-273e-40e9-b5b4-d6d7e07e76c6",
      "isActive" : false,
      "balance" : "$1,694.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 32,
      "name" : "Sophia Duran",
      "gender" : "female",
      "company" : "Magmina",
      "email" : "sophiaduran@magmina.com",
      "phone" : "+1 (843) 415-3460",
      "address" : "133 Norfolk Street, Worcester, Virginia, 3631",
      "about" : "Lorem occaecat officia proident consectetur sint anim id sit anim. Deserunt veniam aute deserunt proident. Ut cillum aliquip deserunt ipsum Lorem est qui cupidatat. Ipsum irure ullamco do cillum duis est in officia et officia elit nisi dolore cillum. Occaecat eu ad pariatur ad Lorem incididunt exercitation nulla. Adipisicing id mollit labore incididunt eiusmod. Esse voluptate ad cupidatat est commodo velit velit laboris ad fugiat mollit.\r\n",
      "registered" : "2003-01-12T07:36:55 -01:00",
      "latitude" : 49.58874,
      "longitude" : 140.795001,
      "tags" : [
         "nulla",
         "officia",
         "amet",
         "irure",
         "voluptate",
         "exercitation",
         "ad"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Twila Holcomb"
         },
         {
            "id" : 1,
            "name" : "Katy Cruz"
         },
         {
            "id" : 2,
            "name" : "Winnie Savage"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 9,
      "guid" : "bf01372f-e27c-4f56-9831-9599cc11a73a",
      "isActive" : true,
      "balance" : "$1,087.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 37,
      "name" : "Boyd Tyson",
      "gender" : "male",
      "company" : "Skyplex",
      "email" : "boydtyson@skyplex.com",
      "phone" : "+1 (913) 438-2918",
      "address" : "966 Winthrop Street, Weogufka, Massachusetts, 2510",
      "about" : "Pariatur occaecat eiusmod incididunt elit irure sit enim proident et aute aliquip quis ea exercitation. Cillum et labore est anim sunt. Sint consectetur ullamco exercitation sit consequat laborum exercitation ea dolor deserunt. Proident fugiat ipsum voluptate commodo mollit pariatur commodo veniam eu ex duis nulla. Ex officia qui occaecat duis consectetur. Ullamco fugiat adipisicing quis Lorem velit laborum officia ex veniam id excepteur in.\r\n",
      "registered" : "2000-12-30T18:33:42 -01:00",
      "latitude" : 17.222712,
      "longitude" : 110.698092,
      "tags" : [
         "esse",
         "consequat",
         "dolor",
         "proident",
         "Lorem",
         "ex",
         "ex"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Stout Herring"
         },
         {
            "id" : 1,
            "name" : "Noemi Vincent"
         },
         {
            "id" : 2,
            "name" : "Johnnie Yates"
         }
      ],
      "randomArrayItem" : "cherry"
   },
   {
      "id" : 10,
      "guid" : "ff535706-ea8c-44fe-b1aa-ae85c3e88931",
      "isActive" : false,
      "balance" : "$2,573.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 34,
      "name" : "Lucille Juarez",
      "gender" : "female",
      "company" : "Essensia",
      "email" : "lucillejuarez@essensia.com",
      "phone" : "+1 (842) 518-3032",
      "address" : "976 Lexington Avenue, Jacumba, Tennessee, 2110",
      "about" : "Minim esse in nostrud et dolor ut dolore dolor. Eiusmod sint incididunt id dolore elit eu quis dolor. Reprehenderit occaecat reprehenderit minim aute tempor sit. Voluptate sunt eiusmod sit id cillum magna aliquip aliquip. Magna cillum qui id adipisicing irure quis laboris.\r\n",
      "registered" : "2002-08-18T00:54:13 -02:00",
      "latitude" : 23.945548,
      "longitude" : 34.181728,
      "tags" : [
         "veniam",
         "ex",
         "aliquip",
         "ex",
         "ad",
         "reprehenderit",
         "et"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Norma Weiss"
         },
         {
            "id" : 1,
            "name" : "Elnora Odom"
         },
         {
            "id" : 2,
            "name" : "Jennifer Puckett"
         }
      ],
      "randomArrayItem" : "lemon"
   },
   {
      "id" : 11,
      "guid" : "30eb71bc-e76e-41eb-90db-b6607b2471b5",
      "isActive" : true,
      "balance" : "$3,019.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 27,
      "name" : "Antonia Byers",
      "gender" : "female",
      "company" : "Pearlessa",
      "email" : "antoniabyers@pearlessa.com",
      "phone" : "+1 (986) 456-2695",
      "address" : "112 Montieth Street, Hegins, Arkansas, 614",
      "about" : "Consequat id Lorem pariatur sunt ea veniam anim aliquip amet duis. Minim id nulla est est cupidatat laboris commodo. Et eu magna commodo commodo esse ut deserunt cupidatat ea velit. Sint duis voluptate non amet ea laborum. Dolore ullamco ut Lorem aliquip pariatur laboris. Laborum deserunt Lorem sit eu elit duis exercitation proident sit proident.\r\n",
      "registered" : "2004-01-17T16:00:50 -01:00",
      "latitude" : -69.440764,
      "longitude" : -135.627868,
      "tags" : [
         "adipisicing",
         "voluptate",
         "incididunt",
         "laboris",
         "veniam",
         "commodo",
         "velit"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Melba Fields"
         },
         {
            "id" : 1,
            "name" : "Miles Holden"
         },
         {
            "id" : 2,
            "name" : "Lauren Goff"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 12,
      "guid" : "5d1f9fd9-b3a6-4320-ba8a-cb9273bca3e9",
      "isActive" : false,
      "balance" : "$1,723.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 28,
      "name" : "Wyatt Douglas",
      "gender" : "male",
      "company" : "Paragonia",
      "email" : "wyattdouglas@paragonia.com",
      "phone" : "+1 (885) 580-2488",
      "address" : "674 Gerritsen Avenue, Needmore, Michigan, 608",
      "about" : "Enim adipisicing ut proident occaecat cupidatat excepteur sint do id aute ad amet laboris est. Sit culpa consequat nisi sunt exercitation ullamco est commodo exercitation fugiat velit. Laboris et sit deserunt commodo exercitation nisi aliqua non incididunt id id. Tempor exercitation aute Lorem anim ad nulla dolor aliqua do dolore.\r\n",
      "registered" : "1991-09-02T02:08:08 -02:00",
      "latitude" : 66.105776,
      "longitude" : 91.843229,
      "tags" : [
         "consectetur",
         "sunt",
         "ex",
         "enim",
         "labore",
         "esse",
         "ut"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Mcpherson Velez"
         },
         {
            "id" : 1,
            "name" : "Cochran Bennett"
         },
         {
            "id" : 2,
            "name" : "Welch Glover"
         }
      ],
      "randomArrayItem" : "cherry"
   },
   {
      "id" : 13,
      "guid" : "139ebd33-41d1-4a12-aaf0-e2957587ea9f",
      "isActive" : true,
      "balance" : "$1,838.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 26,
      "name" : "Leah Downs",
      "gender" : "female",
      "company" : "Idealis",
      "email" : "leahdowns@idealis.com",
      "phone" : "+1 (878) 537-3328",
      "address" : "174 Herkimer Court, Hamilton, Indiana, 8465",
      "about" : "Elit deserunt ipsum exercitation dolor cillum excepteur culpa culpa ut sunt veniam. Elit non ut magna ea nostrud excepteur nisi aliquip pariatur. Veniam culpa officia ea nostrud minim et eiusmod. Sunt aliquip in non pariatur eu dolore aliquip magna. Velit exercitation et nisi deserunt exercitation voluptate Lorem enim elit fugiat adipisicing excepteur aliquip ut. Duis id reprehenderit ullamco commodo nulla nisi officia commodo occaecat sunt tempor veniam. Cillum minim do adipisicing cupidatat anim officia velit elit ea magna nisi do enim reprehenderit.\r\n",
      "registered" : "1988-08-25T19:07:19 -02:00",
      "latitude" : -73.977289,
      "longitude" : -116.135149,
      "tags" : [
         "ex",
         "et",
         "non",
         "commodo",
         "cupidatat",
         "cupidatat",
         "nisi"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Soto Brown"
         },
         {
            "id" : 1,
            "name" : "Henry Conrad"
         },
         {
            "id" : 2,
            "name" : "Marshall Wolf"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 14,
      "guid" : "1082a62a-c042-4826-a50a-b5be72a07037",
      "isActive" : false,
      "balance" : "$3,349.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 29,
      "name" : "Schneider Durham",
      "gender" : "male",
      "company" : "Memora",
      "email" : "schneiderdurham@memora.com",
      "phone" : "+1 (882) 421-2055",
      "address" : "166 Chauncey Street, Coultervillle, Idaho, 8121",
      "about" : "Aute laboris nisi elit do dolor tempor reprehenderit cillum esse non consectetur nostrud voluptate. Tempor mollit do dolor irure non in deserunt ut proident nisi cupidatat. Anim mollit labore quis nulla nostrud reprehenderit sit aliqua exercitation nisi.\r\n",
      "registered" : "2003-09-21T21:15:56 -02:00",
      "latitude" : -56.159995,
      "longitude" : 4.310579,
      "tags" : [
         "adipisicing",
         "ex",
         "exercitation",
         "aliquip",
         "commodo",
         "non",
         "excepteur"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Jo Lara"
         },
         {
            "id" : 1,
            "name" : "Merrill Gray"
         },
         {
            "id" : 2,
            "name" : "Jacobson Burris"
         }
      ],
      "randomArrayItem" : "lemon"
   },
   {
      "id" : 15,
      "guid" : "b58c304c-cca9-482c-b2b5-810e8fa15ef6",
      "isActive" : true,
      "balance" : "$2,923.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 30,
      "name" : "Shelton Burnett",
      "gender" : "male",
      "company" : "Geekola",
      "email" : "sheltonburnett@geekola.com",
      "phone" : "+1 (821) 501-3726",
      "address" : "117 Chase Court, Joes, Alabama, 774",
      "about" : "Non sit cupidatat laboris deserunt adipisicing nisi ipsum commodo. Labore duis sint eu dolor esse consectetur esse ullamco Lorem id nisi culpa sint. Cillum commodo incididunt occaecat reprehenderit enim aliquip velit. Sit voluptate dolore deserunt magna voluptate laborum fugiat ad. Voluptate ad aliqua ullamco eiusmod. Labore ut non ut reprehenderit enim non.\r\n",
      "registered" : "1992-10-02T12:42:44 -01:00",
      "latitude" : -58.765178,
      "longitude" : -155.109015,
      "tags" : [
         "culpa",
         "do",
         "do",
         "enim",
         "minim",
         "enim",
         "nulla"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Lillie Todd"
         },
         {
            "id" : 1,
            "name" : "Morales Crosby"
         },
         {
            "id" : 2,
            "name" : "Tracie Hamilton"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 16,
      "guid" : "1a33e0ff-909e-4e31-b4f7-a6d0d0d72320",
      "isActive" : false,
      "balance" : "$1,693.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 27,
      "name" : "Crane Livingston",
      "gender" : "male",
      "company" : "Enomen",
      "email" : "cranelivingston@enomen.com",
      "phone" : "+1 (990) 563-2727",
      "address" : "248 Elliott Place, Walland, New Jersey, 8005",
      "about" : "Culpa anim sunt esse commodo deserunt reprehenderit laboris commodo ullamco Lorem. Tempor veniam officia eiusmod excepteur pariatur quis excepteur qui dolore consequat. Dolore tempor duis occaecat deserunt aute elit ex quis do occaecat et.\r\n",
      "registered" : "2007-03-09T12:39:06 -01:00",
      "latitude" : 45.176807,
      "longitude" : -2.485176,
      "tags" : [
         "ad",
         "ea",
         "in",
         "consequat",
         "aute",
         "laborum",
         "commodo"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Katrina Hoover"
         },
         {
            "id" : 1,
            "name" : "Angeline Farmer"
         },
         {
            "id" : 2,
            "name" : "Perkins Haney"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 17,
      "guid" : "e6e7cbf1-e991-4e7b-96b7-bef35f249ffd",
      "isActive" : true,
      "balance" : "$2,167.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 21,
      "name" : "Bean Moody",
      "gender" : "male",
      "company" : "Terascape",
      "email" : "beanmoody@terascape.com",
      "phone" : "+1 (898) 582-2002",
      "address" : "773 Brighton Court, Kenwood, Missouri, 6928",
      "about" : "Id consequat cupidatat ex cupidatat eiusmod dolore ea incididunt sunt deserunt. Consequat dolor veniam amet incididunt Lorem exercitation dolore do velit tempor id culpa eiusmod. Occaecat esse est laboris nisi consectetur minim. Exercitation ea nostrud laboris aliqua consectetur aliquip nostrud dolor commodo. Elit officia quis in elit enim cupidatat sit sint deserunt dolore duis.\r\n",
      "registered" : "1997-10-19T10:30:44 -02:00",
      "latitude" : 48.493501,
      "longitude" : -53.665346,
      "tags" : [
         "duis",
         "proident",
         "eu",
         "dolor",
         "occaecat",
         "sit",
         "eu"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Beasley Mcmillan"
         },
         {
            "id" : 1,
            "name" : "Saundra Morales"
         },
         {
            "id" : 2,
            "name" : "Morrow Mcclure"
         }
      ],
      "randomArrayItem" : "lemon"
   },
   {
      "id" : 18,
      "guid" : "53d4712a-aaad-4cf7-8de9-a0e2422a070c",
      "isActive" : true,
      "balance" : "$1,193.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 20,
      "name" : "Hodges Lawson",
      "gender" : "male",
      "company" : "Medesign",
      "email" : "hodgeslawson@medesign.com",
      "phone" : "+1 (843) 453-3635",
      "address" : "216 Fairview Place, Balm, Louisiana, 5648",
      "about" : "Proident aliqua exercitation minim est. Esse veniam sunt dolore laborum dolor minim. Dolor id eiusmod fugiat qui anim incididunt occaecat pariatur non. Ea exercitation incididunt elit non eu.\r\n",
      "registered" : "2008-01-14T06:12:50 -01:00",
      "latitude" : 77.014486,
      "longitude" : 171.670465,
      "tags" : [
         "dolor",
         "ullamco",
         "minim",
         "sunt",
         "laborum",
         "dolor",
         "ex"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Mcconnell Hahn"
         },
         {
            "id" : 1,
            "name" : "King Baker"
         },
         {
            "id" : 2,
            "name" : "Virgie Holt"
         }
      ],
      "randomArrayItem" : "cherry"
   },
   {
      "id" : 19,
      "guid" : "99db9ee5-bd4c-41f6-9f73-92f5644b7b97",
      "isActive" : true,
      "balance" : "$3,744.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 40,
      "name" : "Lancaster Walton",
      "gender" : "male",
      "company" : "Codax",
      "email" : "lancasterwalton@codax.com",
      "phone" : "+1 (965) 409-3192",
      "address" : "646 Harwood Place, Bodega, New Hampshire, 606",
      "about" : "Proident nostrud enim consequat irure. Duis amet incididunt ex do quis consectetur nisi laboris est voluptate esse. Laboris consectetur esse ullamco nostrud aute fugiat ad dolore eiusmod. Sit commodo elit aute Lorem ad deserunt sit est Lorem reprehenderit dolor minim ex.\r\n",
      "registered" : "2011-03-05T16:51:53 -01:00",
      "latitude" : -67.193873,
      "longitude" : 45.464244,
      "tags" : [
         "qui",
         "ea",
         "laborum",
         "voluptate",
         "occaecat",
         "nisi",
         "in"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Zamora Trujillo"
         },
         {
            "id" : 1,
            "name" : "Latisha Fernandez"
         },
         {
            "id" : 2,
            "name" : "Mayo Harris"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 20,
      "guid" : "d54147b7-09fe-4dfb-8828-a73a5d2654b9",
      "isActive" : true,
      "balance" : "$1,534.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 36,
      "name" : "Lindsay Hawkins",
      "gender" : "male",
      "company" : "Isosphere",
      "email" : "lindsayhawkins@isosphere.com",
      "phone" : "+1 (957) 416-3291",
      "address" : "461 Conover Street, Epworth, Wyoming, 2303",
      "about" : "Enim amet sint minim pariatur aliquip proident fugiat consequat deserunt proident. Do excepteur Lorem velit sint proident ea aute eiusmod ipsum. Laborum id eiusmod do nostrud proident consequat eu fugiat elit nostrud nisi. Id aliqua est aliqua dolore ad id commodo est cillum in ipsum nostrud. Enim anim velit id adipisicing non exercitation dolore proident. Dolor sit amet commodo duis.\r\n",
      "registered" : "1991-07-10T19:55:25 -02:00",
      "latitude" : -71.393384,
      "longitude" : 129.718457,
      "tags" : [
         "nostrud",
         "reprehenderit",
         "ea",
         "elit",
         "ipsum",
         "cillum",
         "nulla"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Bartlett Shaw"
         },
         {
            "id" : 1,
            "name" : "Black Barron"
         },
         {
            "id" : 2,
            "name" : "Nona Mcgee"
         }
      ],
      "randomArrayItem" : "apple"
   },
   {
      "id" : 21,
      "guid" : "d648f647-adaa-4bbe-9285-344a916f2241",
      "isActive" : true,
      "balance" : "$1,387.00",
      "picture" : "http://placehold.it/32x32",
      "age" : 22,
      "name" : "Jewel Mccoy",
      "gender" : "female",
      "company" : "Tetak",
      "email" : "jewelmccoy@tetak.com",
      "phone" : "+1 (931) 568-2889",
      "address" : "691 Putnam Avenue, Nettie, Arizona, 7175",
      "about" : "Enim proident occaecat culpa cupidatat exercitation aute ullamco aliquip id in et ea sit enim. Id Lorem veniam minim laborum labore minim fugiat eu anim. Eiusmod esse magna sunt ea. Proident deserunt dolore consequat mollit culpa laboris. Tempor veniam fugiat eu aliqua. Sint velit consequat in tempor velit non ullamco consequat duis dolore mollit cupidatat in consectetur. Culpa consequat et non nostrud amet fugiat ea do sit est reprehenderit incididunt aute.\r\n",
      "registered" : "2013-09-21T12:52:13 -02:00",
      "latitude" : 12.704629,
      "longitude" : 96.065025,
      "tags" : [
         "irure",
         "aliqua",
         "excepteur",
         "elit",
         "occaecat",
         "nostrud",
         "dolore"
      ],
      "friends" : [
         {
            "id" : 0,
            "name" : "Ida Harvey"
         },
         {
            "id" : 1,
            "name" : "Ora Santana"
         },
         {
            "id" : 2,
            "name" : "Lynnette Bullock"
         }
      ],
      "randomArrayItem" : "lemon"
   }
]
```

