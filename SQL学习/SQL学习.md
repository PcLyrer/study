SQL学习笔记

本笔记是基于书籍《SQL必知必会》和《MySQL必知必会》整理而成。

## 1、SQL基础知识

### 1.1、数据库基础

- 数据库的三层结构

1. 数据库管理系统（DBMS）：通常意义上的数据库是指DBMS，即数据库管理系统，通过这个管理系统可以管理多个数据库。
2. 数据库：保存有组织的数据容器（通常是一个文件或一组文件），数据库中存储的是表。
3. 表：某种特定类型数据的结构化清单，由不同的字段（列）组成，列中存储的是该字段下数据信息。

- 基础概念

1. 数据类型：每个表列都有对应的数据类型，它限制该列中存储的数据。
2. 行：表中的数据是按照行存储的，所保存的每个记录是存储在每一行内，所以一行也代表一条记录。
3. 主键：一列（或几列），其值能够唯一标识表中每一行。为了便于数据操作和管理，应尽量保证每一个表都具有至少一个主键。表中的任何列都可以作为主键，只要它满足以下条件：
   - 任意两行都不具有相同的主键值；
   -  每一行都必须具有一个主键值（主键列不允许空值 NULL）； 
   -  主键列中的值不允许修改或更新； 
   -  主键值不能重用（如果某行从表中删除，它的主键不能赋给以后的新行）

### 1.2、SQL基础知识

1. SQL：是 Structured Query Language（结构化查询语言）的缩写。是一种专门用来与数据库沟通的语言。
2. SQL的优势：
   - SQL 不是某个特定数据库厂商专有的语言。绝大多数重要的 DBMS 支持 SQL，所以学习此语言使你几乎能与所有数据库打交道。 
   - SQL 简单易学。它的语句全都是由有很强描述性的英语单词组成，而 且这些单词的数目不多。 
   - SQL 虽然看上去很简单，但实际上是一种强有力的语言，灵活使用其 语言元素，可以进行非常复杂和高级的数据库操作。
3. 在此使用Mysql数据库进行实践操作，使用图形化软件SQLyog来对Mysql数据库进行操作。

### 1.3、MySQL数据库

1. MySQL是一种DBMS，即它是一种数据库软件。

2. MySQL的优点：

   - 成本——MySQL是开放源代码的，一般可以免费使用（甚至可以 免费修改）。 
   - 性能——MySQL执行很快（非常快）。 
   - 可信赖——某些非常重要和声望很高的公司、站点使用MySQL， 这些公司和站点都用MySQL来处理自己的重要数据。 
   - 简单——MySQL很容易安装和使用。

3. MySQL是基于客户机-服务器的数据库。

   - 服务器部分是负责所有数据访问和处理的一个软件。这个软件运行在称为数据库服务器的计算机上。
   - 与数据文件打交道的只有服务器软件。关于数据、数据添加、删除 和数据更新的所有请求都由服务器软件完成。这些请求或更改来自运行客户机软件的计算机。
   - 客户机是与用户打交道的软件。例如，如果你请 求一个按字母顺序列出的产品表，则客户机软件通过网络提交该请求给 服务器软件。服务器软件处理这个请求，根据需要过滤、丢弃和排序数据；然后把结果送回到你的客户机软件。

4. MySQL命令行常见SHOW命令：

   ````mysql
   # 切换/使用数据库
   USE 数据库名
   # 查看当前所有数据库，返回一个数据库列表
   SHOW DATABASE
   # 查看一个数据库中的所有表，需要在 USE 数据库后进行
   SHOW TABLES
   # 显示数据库中表的列
   SHOW COLUMNS FROM customers
   
   ### 以下为不太常用的指令
   # 显示广泛的服务器状态信息
   SHOW STATUS
   # 分别用来显示创建特定数据库或表的MySQL语句
   SHOW CREATE DATABASE 
   SHOW CREATE TABLE
   # 显示授予用户（所有用户或特定用户）的安全权限
   SHOW GRANTS
   # 显示服务器错误或警告消息
   SHOW ERRORS
   SHOW WARNINGS
   ````

   



## 2、检索数据

1. 检索单列

   ````mysql
   SELECT 列名
   	FROM 表名;
   ````

2. 检索多列

   ````mysql
   SELECT 列名1,列名2,列名3
   	FROM 表名;
   ````

3. 检索所有列

   ````mysql
   # 使用通配符 * 来表示查询所有列内容
   SELECT *
   	FROM 表名;
   ````

4. 检索时去掉重复行的数据

   `````mysql
   # 使用 DISTINCT 关键字来去掉查询列中的重复数据，必须放在列名前
   SELECT DISTINCT 列名
   	FROM 表名;
   	
   # 测试
   SELECT DISTINCT prod_id, vend_id
   	FROM products;	# 查询成功，去重prod_id列
   	
   SELECT DISTINCT prod_id, DISTINCT vend_id
   	FROM products;	# 查询失败
   `````

5. 检索时只查看前几行

   `````mysql
   # 使用LIMIT 关键字 查看某一列前5行数据
   SELECT 列名
   	FROM 表名
   	LIMIT 5;
   # 指定从第3行开始,查看5行数据，第一行从第0条数据开始
    SELECT 列名
   	FROM 表名
   	LIMIT 5 OFFSET 3;   
   # 等价于：
    SELECT 列名
   	FROM 表名
   	LIMIT 3,5;   
   `````

6. 注意事项：

   1. 多条SQL语句必须以分号(；)分隔，单条SQL语句后不需要加，即使不一定需要，但加上分号肯定没有坏处。如果你使用的是mysql命令行，必须加上 分号来结束SQL语句。（因此推荐在每条SQL语句后加上分号）
   2. SQL语句不区分大小写，因此 SELECT与select是相同的。许多SQL开发人员喜欢对所有SQL关键字使用大写，而对所有列和表名使用小写，这样做使代码更易于阅读和调试。
   3. 不能部分使用DISTINCT，DISTINCT关键字应用于所有列而不仅是前置它的列。如果给出SELECT DISTINCT vend_id, prod_price，除非指定的两个列都不同，否则所有行都将被检索出来。
   4. 在LIMIT限制输出行时，行是从0开始计数，检索出来的第一行为行0而不是行1。因此，LIMIT 1, 1 将检索出第二行而不是第一行。
   5. 使用LIMIT限制输出行时，在行数不够时 LIMIT中指定要检索的行数为检索的最大行数。如果没有足够的行（例如，给出LIMIT 10, 5，但只有13 行），MySQL将只返回它能返回的那么多行。

7. 使用完全限定表名和完全限定列名：

   `````mysql
   # 使用完全限定表名和完全限定列名：
   SELECT prod_name
   	FROM products
   # 等价于
   SELECT products.prod_name
   	FROM book_test_mysql.products
   `````

   

## 3、排序检索数据

- 子句（clause）：SQL语句由子句构成，有些子句是必需的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。

- order by 子句

  位置：在指定一条 ORDER BY 子句时，应该保证它是 SELECT 语句中最后一 条子句。如果它不是最后的子句，将会出错。

1. 按照一个列排序

   `````mysql
   # 按照 prod_name 进行排序
   SELECT prod_name
   	FROM Products
   	ORDER BY prod_name;
   `````

2. 按照多个列排序，使用order by子句可以按多个列进行排序，即首先按照第一个列排序，当第一个列相同时，按照第二个列排序，依次类推。

   ````mysql
   # 首先按照prod_price 列进行排序，如果再按照prod_name列进行排序
   SELECT prod_id, prod_price, prod_name
   FROM Products
   ORDER BY prod_price, prod_name; 
   ````

3. 按照非选择列进行排序

   ````mysql
   SELECT prod_id, prod_price, prod_name
   	FROM Products
   	ORDER BY 2, 3; # 2,3表示SELECT清单的相对位置，从1开始计数，在此表示prod_price和prod_name这两列
   ````

4. 指定排序方向

   ````mysql
   # 按照prod_price 降序排序，如果相同，则按prod_name A-Z 排序（降序）
   # ASC 表示升序，默认就是升序，DESC表示降序，需要显式指定
   SELECT prod_id, prod_price, prod_name
       FROM Products
       ORDER BY prod_id, prod_price DESC, prod_name DESC;
   ````
   
   注意：DESC关键字只应用到直接位于其前面的列名，若没有显示指定，则默认升序。
   
5. 使用ORDER BY 和 LIMIT的组合，能够找出一个列中最高或者最低的值。

   `````mysql
   # 检索 products 表中的prod_price中最贵的商品
   SELECT prod_price
   	FROM products
   	ORDER BY prod_price DESC
   	LIMIT 1;
   `````

   

## 4、过滤数据

在 SELECT 语句中，数据根据 WHERE 子句中指定的搜索条件进行过滤。 WHERE 子句在表名（FROM 子句）之后给出，如下所示：

````mysql
SELECT 列名
	FROM 表名
	WHERE '过滤条件'
````

1. WHERE 子句的位置

   在同时使用 ORDER BY 和 WHERE 子句时，应该让 ORDER BY 位于 WHERE 之后，否则将会产生错误。

2. WHERE子句操作符

   ![image-20220624163810495](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220624163810495.png)
   
3. 检查单个值：

   ``````mysql
   # 检索文本类型时需要使用 单引号 来限定检索条件
   SELECT prod_name, prod_price
       FROM products
       WHERE prod_name = 'fuses'
   # 检索数值类型时不需要 单引号 来限定
   SELECT prod_name, prod_price
   	FROM products
   	WHERE prod_price <= 10;
   ``````

4. 不匹配检查

   `````mysql
   SELECT vend_id, prod_name
   FROM products
   WHERE vend_id <> /* != */ 1003;
   # <> 等价于 !=
   `````

5. 范围值检查

   `````mysql
   # 使用 BETWEEN AND 关键字
   SELECT prod_name,prod_price
   FROM products
   WHERE prod_price BETWEEN 5 AND 10;
   
   # 等价于
   SELECT prod_name,prod_price
   FROM products
   WHERE prod_price >= 5 AND prdo_price <= 10;
   `````

6. 空值检查

   ````mysql
   # 使用 IS NULL来判空
   SELECT prod_name
   FROM products
   WHERE prod_price IS NULL
   
   # 使用 IS NOT NULL 检索非NULL行数据
   SELECT prod_name
   FROM products
   WHERE prod_price IS NOT NULL
   ````

   

## 5、高级数据过滤

1. 使用AND操作符

   ````mysql
   # 此条件取的是vend_id 为 'DLL01'的并且 prod_price 小于等于 4 的数据，类似两个条件的交集
   SELECT prod_id, prod_price, prod_name
       FROM Products
       WHERE vend_id = 'DLL01' AND prod_price <= 4;
   ````

2. 使用OR操作符

   `````mysql
   # 此条件取得是 vend_id 为 1002 或者为 1003 的数据，类似两个条件的并集
   SELECT prod_name, prod_price
   FROM products
   WHERE vend_id = 1002 OR vend_id = 1003;
   `````
   
3. 同时使用AND和OR

   由于AND的优先级比OR高，所以在操作时往往使用圆括号来提高优先级，例如：

   ````mysql
   SELECT prod_name, prod_price
       FROM Products
       WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01')
        AND prod_price >= 10;
   ````

4. IN 操作符  

   IN 操作符用来指定条件范围，范围中的每个条件都可以进行匹配。IN 取一组由逗号分隔、括在圆括号中的合法值。如下所示：

   ```mysql
   SELECT prod_name, prod_price
       FROM Products
       WHERE vend_id IN ('DLL01','BRS01')
       ORDER BY prod_name;
   
   # 等价于下面，但是IN操作符执行效率更高
   SELECT prod_name, prod_price
       FROM Products
       WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'
       ORDER BY prod_name;
   ```

   IN操作符的优点：

   - 在使用长的合法选项清单时，IN操作符的语法更清楚且更直观。 
   - 在使用IN时，计算的次序更容易管理（因为使用的操作符更少）。 
   - IN操作符一般比OR操作符清单执行更快。 
   - IN的最大优点是可以包含其他SELECT语句，使得能够更动态地建立WHERE子句。

5. NOT操作符

   WHERE 子句中的 NOT 操作符有且只有一个功能，那就是否定其后所跟的任何条件。如：

   ```mysql
   # 查询vend_id 除 'DLL01'之外的其他产品
   SELECT prod_name
       FROM Products
       WHERE NOT vend_id = 'DLL01'
       ORDER BY prod_name;
   
   # 等价于
   SELECT prod_name
       FROM Products
       WHERE vend_id <> 'DLL01'
       ORDER BY prod_name; 
   ```

   NOT 操作符可以对IN、BETWEEN和EXISTE子句取反。



## 6、使用通配符进行过滤

1. 通配符：用来匹配值的一部分的特殊字符，通配符搜索只能用于文本字段（字符串），非文本数据类型字段使用通配符需要使用单引号来包裹条件。

2. 搜索模式：由字面值、通配符或两者组合构成的搜索条件。

3. 百分号（%）通配符：表示任何字符出现任意次数，可以匹配0、1或者多个字符。

   `````mysql
   # 使用LIKE 关键字
   # 找出所有以词 Fish 起头的产品
   SELECT prod_id, prod_name
   	FROM Products
   	WHERE prod_name LIKE 'Fish%'
   	
   # 找出包含bean bag 文本的产品
   SELECT prod_id, prod_name
   	FROM Products
   	WHERE prod_name LIKE '%bean bag%'; 
   `````

   - 注意： ‘%’通配符不能匹配为NULL的行

4. 下划线（_）通配符：用途与 ‘%’ 类似，但是只匹配单个字符，一个 ‘\_' 只能匹配一个字符。

   ````mysql
   SELECT prod_name, prod_price
   FROM products
   WHERE vend_id LIKE '10_2'	# 根据表中数据，查询id为 1002 的产品
   
   SELECT prod_id, prod_name
   	FROM Products
   	WHERE prod_name LIKE '__ inch teddy bear';
   ````
   
5. 使用通配符的注意事项

   - 不要过度使用通配符。如果其他操作符能达到相同的目的，应该 使用其他操作符。 
   - 在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起 来是最慢的。 
   - 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据。

   

## 7、使用正则表达式进行搜索

1. 随着业务条件变得复杂，对数据的查询也会变得复杂，这时可以使用正则表达式来匹配符合条件的数据。

2. 正则表达式是用来匹配文本的特殊的串（字符集合）。

3. 使用正则表达式进行基本字符匹配

   `````mysql
   # 使用正则表达式的关键字是 REGEXP ，返回 JetPack 1000
   SELECT *
   FROM products
   WHERE prod_name REGEXP '1000'		# 返回产品名字中 包含 '1000' 的产品信息
   ORDER BY prod_name;
   # 使用正则表达式中的 '.' 字符，返回 JetPack 1000 和 JetPack 2000
   SELECT *
   FROM products
   WHERE prod_name REGEXP '.000'
   ORDER BY prod_name;
   # 正则表达式中的'.'是一个特殊的字符，表示可以任意匹配一个字符
   `````

   注意：

   - 正则表达式的匹配是否区分大小写需要根据查询表的定义，如果表是区分大小写，则正则表达式的匹配条件也区分大小写。

   - 如果表是不区分大小写的，为了显式区分大小写，可以使用BINARY关键字，如：

     ````mysql
     # 显示区分大小写，使用BINARY关键字
     WHERE prod_name REGEXP BINARY 'JetPack .000'
     ````

4. 进行OR匹配

   为搜索两个串之一（或者为这个串，或者为另一个串），使用 ' | '符号，如下所示：

   `````mysql
   # 使用 | 进行 OR 匹配
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '1000|2000'
   ORDER BY prod_name;
   `````

   OR 匹配可以匹配两个或者两个以上的条件，例如：'1000 | 2000 | 3000'

5. 匹配几个字符之一

   如果想匹配特定的字符，可以使用 [ ] (方括号)来完成，例如：

   ````mysql
   # 使用 [] 匹配多个字符
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '[123] Ton'
   ORDER BY prod_name;
   
   # 使用 ^ 否定集合
   WHERE prod_name REGEXP '[^123] Ton'
   ````

   其中 [123] 定义一组字符，它的意思是匹配1或2或3，因此，1 ton和2 ton都匹配且返回（没有3 ton）。

   注意：

   - ' [123] Ton ' 是 ' [1 | 2 | 3] Ton '的缩写，但是与 ' 1 | 2 | 3 Ton ' 不同，' 1 | 2 | 3 Ton ' 表示的是匹配 ' 1 ' 或者 ' 2 ' 或者 '3 Ton'，使用该式子进行匹配会有额外的输出。
   - 字符集合也可以被否定，只需要在集合的开始防止一个 ^ 即可，例如[ ^ 123 ] 表示匹配除这些字符外的任何东西。

6. 匹配范围：

   集合可用来定义要匹配的一个或多个字符。例如，下面的集合将匹配数字0到9：[0123456789]，为了简化这种类型的集合，可以使用 '-' 来定义一个范围，例如上述集合可用 [0-9] 来表示。另外也可以使用 [a-z] 来匹配任意的字母和字符。

   实例：

   `````mysql
   # 匹配范围
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '[1-5] Ton'
   ORDER BY prod_name;
   `````

7. 匹配特殊字符

   正则表达式语言由具有特定含义的特殊字符构成。目前已经了解到 '. [] | -'这四种特殊字符，如果要查找包含这些特殊字符的值，需要使用 ' \\\\ ' 作为转义符使用，例如：

   ````mysql
   # 匹配特殊字符 例如 '.'
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '\\.'
   ORDER BY prod_name;
   ````

   另外：'\\\\' 还用来引用元字符（具有特殊含义的字符），如下图所示：

   ![image-20220629085559097](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629085559097.png)

   另外如果需要匹配包含 '\\'的值，我们需要以 '\\\\\\' 的形式，如果需要匹配 '\\\\\'的值，则需要 '\\\\\\\\\\\\\' 这样的形式，正则表达式一般是使用单个 '\\' 作为转义字符使用的，但是在MySQL中要求两个 '\\\\\' ，因为MySQL自己解释一个，正则表达式库解释另一个。

8. 匹配字符类

   存在找出你自己经常使用的数字、所有字母字符或所有数字字母字 符等的匹配。为更方便工作，可以使用预定义的字符集，称为字符类 （character class）。下图列出字符类以及它们的含义：

   ![image-20220629090157421](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629090157421.png)

9. 匹配多个实例

   目前为止使用的所有正则表达式都试图匹配单次出现。但有时需要对匹配的数目进行更强的控制。下面的表列出的正则表达式重复元字符。

   ![image-20220629091115316](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629091115316.png)

   实例：

   `````mysql
   # 实例1
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '\\([0-9] sticks?\\)'
   ORDER BY prod_name;
   /* 匹配结果
   TNT (1 stick)
   TNT (5 sticks)
   */
   
   # 实例2
   SELECT prod_name
   FROM products
   WHERE prod_name REGEXP '[[:digit:]]{4}'
   ORDER BY prod_name;
   /* 匹配结果
   JetPack 1000
   JetPack 2000
   */
   `````

   [:digit:] 匹配任意数字，因而它为数字的一个集合。{4}确切地要求它前面的字符（任意数字）出现4次，所以[[:digit:]]{4}匹配连在一起的任意4位数字。
   并且这个表达式等价于 '[0-9]\[0-9][0-9]\[0-9]'

10. 定位符

    目前为止的所有例子都是匹配一个串中任意位置的文本。为了匹配特定位置的文本，需要使用如下的定位符：

    ![image-20220629095052074](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629095052074.png)

    实例：

    `````mysql
    # 找出以一个数（包括以小数点开始的数）开始的所有产品，需要以 ^ 作为文本开始，否则仅使用[0-9\\.]（或[[:digit:]\\.]）会在文本内任意位置查询匹配。
    SELECT prod_name
    FROM products
    WHERE prod_name REGEXP '^[0-9\\.]'
    ORDER BY prod_name;
    `````

    注意：

    - ' ^ ' 有两种用法，一是在集合内表示否定该集合，另外一个是表示文本开始的符号。

    - LIKE 和 REGEXP 的不同在于，LIKE匹配整个串而REGEXP匹配子串。利用定位符，通过用^开始每个表达式，用$结束每个表达式，可以使 REGEXP的作用与LIKE一样。

      `````mysql
      # LIKE 与 REGEXP ,正则使用^开始和$结束可以达到和LIKE相当的效果
      SELECT *
      FROM orders
      WHERE order_num LIKE '2%5'
      
      SELECT *
      FROM orders
      WHERE order_num REGEXP '^2[0-9][0-9][0-9]5$'
      
      SELECT *
      FROM orders
      WHERE order_num REGEXP '^2[[:digit:]]{3}5$'
      `````

    - 另外还可以在不使用数据库表的时候用 SELECT 语句来测试正则表达式，但是只会返回0和1两个值，其中0表示不匹配，1表示匹配：

      ````mysql
      SELECT 'hello' regexp '[0-9]';
      # 返回0，表示不匹配
      ````

      

## 8、创建计算字段

1. 为什么需要计算字段：

- 需要显示公司名，同时还需要显示公司的地址，但这两个信息存储在 不同的表列中。 
- 城市、州和邮政编码存储在不同的列中（应该这样），但邮件标签打 印程序需要把它们作为一个有恰当格式的字段检索出来。 
- 列数据是大小写混合的，但报表程序需要把所有数据按大写表示出来。 
- 物品订单表存储物品的价格和数量，不存储每个物品的总价格（用价 格乘以数量即可）。但为打印发票，需要物品的总价格。 
- 需要根据表数据进行诸如总数、平均数的计算。

2. 有时我们需要直接从数据库中检索出转换、计算或格式化过的数据，而不是检索出数据，然后再在客户端应用程序中重新格式化，可以使用以下函数：

- 字符连接（Concat()函数）
  
  实例：
  创建由 vend_name 和 vend_country两列组成的标题，并使用括号将vend_country括起来。
  
  ````mysql
  # MySQL使用 Concat() 函数来进行拼接,格式为：Concat(str1,str2,...str)
  SELECT CONCAT(vend_name, '(', vend_country, ')')
  	FROM Vendors
  	ORDER BY vend_name
  	
  # 一些DBMS使用 + 来进行连接
  SELECT vend_name + '(' + vend_country + ')'
  	FROM Vendors
  	ORDER BY vend_name;
  
  # 一些DBMS使用 || 来进行连接
  SELECT vend_name || '(' || vend_country || ')'
  	FROM Vendors
  	ORDER BY vend_name;
  ````
  
- 去掉空格函数（Trim()函数）

  MySQL中可以通过 Trim() 函数删除串两端多余的空格来整理数据，另外还有LTrim() 函数和 RTrim() 函数，LTrim() 函数是去掉串左边的空格，RTrim() 函数是去掉串右边的空格。

  实例：

  ````mysql
  # 使用RTrim()函数
  SELECT CONCAT(RTRIM(vend_name), ' (', RTRIM(vend_country), ')')
  FROM vendors
  ORDER BY vend_name;
  ````

- 使用别名（AS关键字）

  ```mysql
  # 使用 AS 关键字 来为列增加别名
  SELECT CONCAT(vend_name, ' (', vend_country, ')') AS vend_title
  	FROM Vendors
  	ORDER BY vend_name
  ```

- 执行算术计算

  实例：

  Orders 表包含收到的所有订单，OrderItems 表包含每个订单中的各项物品，现在查询订单号为20008中的所有物品的id，数量以及单价，如下所示：

  ````mysql
  SELECT prod_id, quantity, item_price
      FROM OrderItems
      WHERE order_num = 20008; 
  # 输出如下
  prod_id quantity item_price
  RGAN01	    5 	   4.9900
  BR03 	    5      11.9900
  BNBG01 	    10 	   3.4900
  BNBG02      10     3.4900
  BNBG03      10     3.4900
  ````

  现在我们希望汇总物品的价格（单价*数量）

  ```mysql
  SELECT prod_id, quantity, item_price,
  	item_price * quantity AS expanded_price
  	FROM OrderItems
  	WHERE order_num = 20008
  ```

- SQL算术操作符(可以使用圆括号来修改优先级)

  ![image-20220625100053910](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220625100053910.png)



## 9、使用函数处理数据

1. SQL支持的函数类型：

- 用于处理文本字符串（如删除或填充值，转换值为大写或小写）的文 本函数。 
- 用于在数值数据上进行算术操作（如返回绝对值，进行代数运算）的 数值函数。 
- 用于处理日期和时间值并从这些值中提取特定成分（如返回两个日期 之差，检查日期有效性）的日期和时间函数。 
- 用于生成美观好懂的输出内容的格式化函数（如用语言形式表达出日 期，用货币符号和千分位表示金额）。 
- 返回 DBMS 正使用的特殊信息（如返回用户登录信息）的系统函数。



2. 常用的文本处理函数：

`````mysql
LOCATE()	# 找出串的一个子串
LEFT()		# （或使用子字符串函数） 返回字符串左边的字符
RIGHT()		# （或使用子字符串函数） 返回字符串右边的字符

LENGTH()	# （也使用DATALENGTH()或LEN()） 返回字符串的长度

LOWER() 	# 将字符串转换为小写
UPPER() 	# 将字符串转换为大写

LTRIM() 	# 去掉字符串左边的空格
RTRIM() 	# 去掉字符串右边的空格

SUBSTRING() # 返回字符串的字符
SOUNDEX() 	# 返回字符串的SOUNDEX值

`````

- SOUNDEX()函数说明

  寻找两个发音相似的值

  ````mysql
  # 实例：寻找发音类似于Michael Green 的练习名
  SELECT cust_name, cust_contact	
  	FROM Customers
  	WHERE SOUNDEX(cust_contact) = SOUNDEX('Michael Green');
  ````
  
- SUBSTRING()函数说明：

  ````mysql
  # SUBSTRING(str, pos, len)
  SELECT SUBSTRING('hello', 2)	# 从第二个字符开始，取后面所有的字符，返回 ello
  SELECT SUBSTRING('hello', 1, 3)	# 从第一个字符开始，取三个字符，返回 hel
  ````

  

3. 日期和处理函数：

- 一般，应用程序不使用用来存储日期和时间的格式，因此日期和时间函数总是被用来读取、统计和处理这些值。由于这个原因，日期和时间函数在MySQL语言中具有重要的作用。

- 常见的日期和时间处理函数：

![image-20220629102707211](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629102707211.png)

- 实例1：

  例如：YEAR()函数

  检索2020年的所有订单：

  `````mysql
  # 查询Orders 表中，订单日期是2020年的订单号
  SELECT order_num
  	FROM Orders
  	WHERE YEAR(order_date) = 2020; 
  `````

- 注意：

  - 另外需要注意的是，在使用Where 语句过滤时，日期的格式必须为 yyyy-mm-dd，即类似' 2022-06-09 ' 格式。

  - 在对有日期和时间的数据进行过滤时，以列为关键字进行匹配时，条件必须与表格中数据一致，否则会失败，例如：

    ````mysql
    SELECT cust_id, order_num
    From orders
    WHERE order_date = '2005-09-01'
    # 上述语句将失败，order_date 保存的是类似 '2005-09-01 12:42:30' 格式的数据，没有书写完整，就会匹配失败，正确的应该使用以下的语句（Date()函数用来提取日期部分）
    SELECT cust_id, order_num
    From orders
    WHERE Date(order_date) = '2005-09-01'
    ````

- 实例2：

  在日期时间函数中使用 BETWEEN 和 AND 关键字，如下：

  `````mysql
  # 查询order_date列中日期在 2005-09-01 到 2005-09-30 这个范围内的数据
  SELECT cust_id, order_num
  From orders
  WHERE Date(order_date) BETWEEN '2005-09-01' AND '2005-09-30'
  # 查询order_date列中，日期为2005年9月的所有行
  SELECT cust_id, order_num
  From orders
  WHERE Year(order_date) = 2005 AND Month(order_date) = 9;
  `````



4. 数值处理函数

- 数值处理函数仅处理数值数据。这些函数一般主要用于代数、三角 或几何运算，因此没有串或日期—时间处理函数的使用那么频繁。

- 常见的数据处理函数有：

  ![image-20220629104853044](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629104853044.png)

- 课后题

  ````mysql
  /* 第八章 课后题
  1. 我们的商店已经上线了，正在创建顾客账户。所有用户都需要登录名，
  默认登录名是其名称和所在城市的组合。编写 SQL 语句，返回顾客 ID
  （cust_id）、顾客名称（customer_name）和登录名（user_login），
  其中登录名全部为大写字母，并由顾客联系人的前两个字符（cust_
  contact）和其所在城市的前三个字符（cust_city）组成。例如，
  我的登录名是 BEOAK（Ben Forta，居住在 Oak Park）。提示：需要使用
  函数、拼接和别名。
  2. 编写 SQL 语句，返回 2020 年 1 月的所有订单的订单号（order_num）
  和订单日期（order_date），并按订单日期排序。你应该能够根据目
  前已学的知识来解决此问题，但也可以开卷查阅 DBMS 文档。
  */
  -- (1)
  SELECT cust_id,
  	cust_name,
  	cust_contact,
  	cust_city,	
  	UPPER(CONCAT(LEFT(cust_contact, 2),LEFT(cust_city,3))) AS user_login
  	FROM customers
  	ORDER BY cust_id
  
  -- (2)
  SELECT order_num, order_date
  	FROM orders
  	WHERE YEAR(order_date) = 2020 AND MONTH(order_date) = 01
  ````

  

## 10、汇总数据

聚集函数：对某些行运行的函数，计算并返回一个值。

1. 聚集函数的常用例子
   -  确定表中行数（或者满足某个条件或包含某个特定值的行数）； 
   - 获得表中某些行的和； 
   -  找出表列（或所有行或某些特定的行）的最大值、最小值、平均值。

2. SQL的五个聚集函数：

   ![image-20220626074650366](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220626074650366.png)

3. AVG()函数

   AVG()通过对表中行数计数并计算其列值之和，求得该列的平均值。AVG() 可用来返回所有列的平均值，也可以用来返回特定列或行的平均值。

   ````mysql
   # 使用 AVG()返回 Products 表中所有产品的平均价格
   SELECT AVG(prod_price) AS avg_price
   	FROM products
   
   # 返回 vend_id = 'DLL01' 商品的平均价格
   SELECT AVG(prod_price) AS avg_price
   	FROM products
   	WHERE vend_id = 'DLL01'
   
   注意：AVG()函数会忽略列值为NULL的行
   ````

4. COUNT()函数

   COUNT()函数进行计数。可利用 COUNT()确定表中行的数目或符合特定条件的行的数目。

   COUNT()函数的两种使用方式：

   - 使用 COUNT(*)对表中行的数目进行计数，不管表列中包含的是空值（NULL）还是非空值。 
   - 使用 COUNT(column)对特定列中具有值的行进行计数，忽略 NULL 值。

   ````mysql
   # 查看 Customers 表中数据的总行数
   SELECT COUNT(*) AS num_cust
   	FROM customers 	#返回5
   
   # 查看有邮箱的客户，查询结果表示5个人中只有3个人有邮箱
   SELECT COUNT(cust_email) AS num_cust
   	FROM customers	#返回3
   ````

5. MAX()函数

   MAX()返回指定列中的最大值。要求指定列名。

   ````mysql
   # 查询products表中，prod_price列的最大值
   SELECT MAX(prod_price) AS max_price
   	FROM products
   ````

6. MIN()函数

   MIN()的功能正好与 MAX()功能相反，它返回指定列的最小值。

   ````mysql
   # 查询products表中，prod_price列的最小值
   SELECT MIN(prod_price) AS min_price
   	FROM products
   ````

7. SUM()函数

   SUM()用来返回指定列值的和（总计）。

   ````mysql
   # 查询订单号为 20005 中的所有物品的数量之和
   SELECT SUM(quantity) AS items_ordered
   	FROM orderitems
   	WHERE order_num = 20005
   ````

   SUM()也可以用来合计计算值。

   ````mysql
   # 查询订单 20005 的总金额
   SELECT SUM(item_price*quantity) AS total_price
       FROM OrderItems
       WHERE order_num = 20005;
   ````

   SUM()也是忽略值为NULL的行的。

8. 聚集不同值

   - 以上五个函数都可以使用ALL和DISTINCT参数，其中对所有行执行计算，则指定ALL参数或者不给参数（ALL是默认参数），只包含不同的值（去掉重复值），需要指定DISTINCT参数。

   - AVG()函数使用DISTINCT后，只会计算不同值的平均数，对于相同数值的只会计算一次。

   - MIN()和MAX()函数使用DISTINCT没有价值，因为返回的是最大/最小的那一个数。

   - DISTINCT不能用于COUNT(*)，只能用于COUNT()。

- 课后题

  ````mysql
  /* 第九章课后题
  1. 编写 SQL 语句，确定已售出产品的总数（使用 OrderItems 中的
  quantity 列）。
  2. 修改刚刚创建的语句，确定已售出产品项（prod_item）BR01 的
  总数。
  3. 编写 SQL 语句，确定 Products 表中价格不超过 10 美元的最贵产品
  的价格（prod_price）。将计算所得的字段命名为 max_price。
  */
  -- (1)
  SELECT SUM(quantity)
  	FROM orderitems
  -- (2)
  SELECT SUM(order_item)
  	FROM orderitems
  	WHERE prod_id = 'BR01'
  -- (3)
  SELECT MAX(prod_price)
  	FROM products
  	WHERE prod_price <= 10
  ````

  

## 11、分组数据

1. 使用Group By子句进行分组

   ````mysql
   # 按vend_id(供应商)分组来统计产品数量
   SELECT vend_id,COUNT(*) AS num_prods
   	FROM products
   	GROUP BY vend_id
   ````



1. Group by 子句使用的一些规定

   - GROUP BY 子句可以包含任意数目的列，因而可以对分组进行嵌套， 更细致地进行数据分组。 

   - 如果在 GROUP BY 子句中嵌套了分组，数据将在最后指定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。 

   - GROUP BY 子句中列出的每一列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在 SELECT 中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式。不能使用别名。 

   - 除聚集计算语句外，SELECT 语句中的每一列都必须在 GROUP BY 子句 中给出。 

   - 如果分组列中包含具有 NULL 值的行，则 NULL 将作为一个分组返回。 如果列中有多行 NULL 值，它们将分为一组。 

   - GROUP BY 子句必须出现在 WHERE 子句之后，ORDER BY 子句之前。

     

2. 使用 WITH ROLLUP 关键字得到分组的汇总值

   `````mysql
   # 使用 WITH ROLLUP关键字，可以查看到汇总数
   SELECT vend_id, COUNT(*) AS num_prods
   FROM products
   GROUP BY vend_id WITH ROLLUP
   
   /* 查询结果
   vend_id	num_prods
   1005	2
   1003	7
   1002	2
   1001	3
   \NULL	14
   `````

   

3. 过滤分组，使用group by子句进行分组后，需要进行过滤要使用 having关键字，having关键字支持所有的where操作符

   ````mysql
   # 按照cust_id进行分组，然后过滤数量大于2的
   SELECT cust_id, COUNT(*) AS orders
   	FROM orders
   	GROUP BY cust_id
   	HAVING COUNT(*) >= 2
   ````

   

4. 可以同时使用where和having语句

   Where语句是在数据分组前进行过滤，而Having语句是在分组后进行过滤，where语句过滤的行不包括在分组内，如下面的例子：

   ```mysql
   # 先过滤prod_price大于4的，然后按照vend_id进行分组，过滤留下分组数据不小于2的数据
   SELECT vend_id, COUNT(*) AS num_prods
   	FROM Products
   	WHERE prod_price >= 4
   	GROUP BY vend_id
   	HAVING COUNT(*) >= 2;
   ```

   

5. ORDER BY和GROUP BY分组

   ![image-20220626161524463](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220626161524463.png)

   ````mysql
   # ORDER BY 和GROUP BY子句通常结合使用
   SELECT order_num, COUNT(*) AS items
   	FROM OrderItems
   	GROUP BY order_num
   	HAVING COUNT(*) >= 3
   	ORDER BY items, order_num;
   ````

6. 查询语句中的使用顺序：

   ![image-20220629110800991](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629110800991.png)

- 课后题

  ````mysql
  /* 第十章 课后题
  1. OrderItems 表包含每个订单的每个产品。编写 SQL 语句，返回每个
  订单号（order_num）各有多少行数（order_lines），并按 order_lines
  对结果进行排序。
  
  2. 编写 SQL 语句，返回名为 cheapest_item 的字段，该字段包含每个
  供应商成本最低的产品（使用 Products 表中的 prod_price），然后
  从最低成本到最高成本对结果进行排序。
  
  3. 确定最佳顾客非常重要，请编写 SQL 语句，返回至少含 100 项的所有
  订单的订单号（OrderItems 表中的 order_num）。
  
  4. 确定最佳顾客的另一种方式是看他们花了多少钱。编写 SQL 语句，
  返回总价至少为 1000 的所有订单的订单号（OrderItems 表中的
  order_num）。提示：需要计算总和（item_price 乘以 quantity）。
  按订单号对结果进行排序。
  
  5. 下面的 SQL 语句有问题吗？（尝试在不运行的情况下指出。）
  SELECT order_num, COUNT(*) AS items
  	FROM OrderItems
  	GROUP BY items
  	HAVING COUNT(*) >= 3
  	ORDER BY items, order_num;
  */
  -- (1)
  SELECT order_num, COUNT(*) AS order_lines
  	FROM orderitems
  	GROUP BY order_num
  	ORDER BY order_lines
  -- (2)
  SELECT vend_id,MIN(prod_price) AS cheapest_item
  	FROM products
  	GROUP BY vend_id
  	ORDER BY cheapest_item 
  -- (3)
  SELECT order_num, quantity 
  	FROM orderitems 
  	WHERE quantity >= 100
  -- (4)
  SELECT order_num, SUM(item_price*quantity) AS total_price
  	FROM orderitems 
  	GROUP BY order_num
  	HAVING total_price >= 1000
  -- (5)
  # 错误，按照count(*)来分组会出现错误
  SELECT order_num, COUNT(*) AS items
  	FROM OrderItems
  	GROUP BY items
  	HAVING COUNT(*) >= 3
  	ORDER BY items, order_num;
  ````

  

## 12、子查询

1. 子查询：嵌套在其他查询中的查询。

2. 利用子查询进行过滤

   - 实例1：

     `````mysql
     # 查询订购物品 RGAN01 的所有顾客
     -- (1) 检索包含物品 RGAN01 的所有订单的编号。
     SELECT order_num
     	FROM orderitems
     	WHERE prod_id = 'RGAN01'
     # 20007 20008
     -- (2) 检索具有前一步骤列出的订单编号的所有顾客的 ID。
     SELECT cust_id
     	FROM orders
     	WHERE order_num IN (20007,20008)
     # 1000000004 1000000005
     -- (3) 检索前一步骤返回的所有顾客 ID 的顾客信息。
     SELECT *
     	FROM customers
     	WHERE cust_id IN (1000000004,1000000005)
     # 合并上述三个查询
     SELECT *
     FROM customers
     WHERE cust_id IN (SELECT cust_id
     			FROM orders
     			WHERE order_num IN (SELECT order_num
     						FROM orderitems
     						WHERE prod_id = 'RGAN01'))
     `````

3. 作为计算字段使用子查询

   - 实例2：

     `````mysql
     -- 显示 Customers 表中每个顾客的订单总数
     -- (1) 从 Customers 表中检索顾客列表；
     SELECT cust_id
     	FROM Customers
     -- (2) 对于检索出的每个顾客，统计其在 Orders 表中的订单数目。
     SELECT cust_name,
     	cust_state,
     	(SELECT COUNT(*) 
     	FROM orders
     	WHERE Orders.cust_id = customers.cust_id) AS order_num
     FROM Customers
     ORDER BY cust_name
     `````

4. 相关子查询：涉及到外部查询的子查询，查询 列名 具有多义性时，就该列名在多个查询的表中出现，那么，就必须使用正确的完全限定列名（表名.列名）

5. 子查询比较经常出现的位置是FROM 语句 和 WHERE语句后，在FROM语句后跟子查询必须使用别名。

- 课后题

  ````mysql
  /* 第十一章 课后题
  1. 使用子查询，返回购买价格为 10 美元或以上产品的顾客列表。你需
  要使用 OrderItems 表查找匹配的订单号（order_num），然后使用
  Order 表检索这些匹配订单的顾客 ID（cust_id）。
  
  2. 你想知道订购 BR01 产品的日期。编写 SQL 语句，使用子查询来确定
  哪些订单（在 OrderItems 中）购买了 prod_id 为 BR01 的产品，然
  后从 Orders 表中返回每个产品对应的顾客 ID（cust_id）和订单日
  期（order_date）。按订购日期对结果进行排序。
  
  3. 现在我们让它更具挑战性。在上一个挑战题，返回购买 prod_id 为
  BR01 的产品的所有顾客的电子邮件（Customers 表中的 cust_email）。
  提示：这涉及 SELECT 语句，最内层的从 OrderItems 表返回 order_num，
  中间的从 Customers 表返回 cust_id。
  
  4. 我们需要一个顾客 ID 列表，其中包含他们已订购的总金额。编写 SQL
  语句，返回顾客 ID（Orders 表中的 cust_id），并使用子查询返回
  total_ordered 以便返回每个顾客的订单总数。将结果按金额从大到
  小排序。提示：你之前已经使用 SUM()计算订单总数。
  
  5. 再来。编写 SQL 语句，从 Products 表中检索所有的产品名称（prod_
  name），以及名为 quant_sold 的计算列，其中包含所售产品的总数
  （在 OrderItems 表上使用子查询和 SUM(quantity)检索）。
  */
  
  -- (1)
  SELECT cust_id
  	FROM orders
  	WHERE order_num IN (SELECT order_num
  				FROM orderitems
  				WHERE item_price >= 10)
  -- (2)
  SELECT cust_id
  	FROM orders
  	WHERE order_num IN (SELECT order_num
  				FROM orderitems
  				WHERE prod_id = 'BR01')
  -- (3)
  SELECT cust_id, cust_email
  	FROM customers
  	WHERE cust_id IN (SELECT cust_id
  				FROM orders
  				WHERE order_num IN (SELECT order_num
  							FROM orderitems
  							WHERE prod_id = 'BR01'))
  -- (4)
  SELECT cust_id, 
  	SUM(order_price.total_price) AS total_ordered
  	FROM orders,
  	(SELECT order_num, SUM(quantity * item_price) AS total_price
  		FROM orderitems
  		GROUP BY order_num) AS order_price
  	WHERE orders.order_num = order_price.order_num
  	GROUP BY cust_id
  	ORDER BY total_ordered DESC
  
  -- (5)
  SELECT prod_name, quant_sold
  	FROM products,
  	(SELECT prod_id, SUM(quantity) AS quant_sold
  	FROM orderitems
  	GROUP BY prod_id) AS prod_quant
  	WHERE products.prod_id = prod_quant.prod_id
  ````

  

## 13、联结表

1. 关系表-外键（foreign key）：外键为某个表中的一列，它包含另一个表 的主键值，定义了两个表之间的关系。使用外键的好处有：
   - 供应商信息不重复，从而不浪费时间和空间； 
   - 如果供应商信息变动，可以只更新vendors表中的单个记录，相关表中的数据不用改动； 
   - 由于数据无重复，显然数据是一致的，这使得处理数据更简单。
2. 关系表-可伸缩性（scale）：能够适应不断增加的工作量而不失败。设计良好的数据库或应用程序称之为可伸缩性好（scale well）。
3. 联结：联结是一种机制，用来在一条 SELECT 语句 中关联表，因此称为联结。创建联结非常简单，指定要联结的所有表以及关联它们的方式即可。

- 内连接实例：

  ````mysql
  # 简单式等值联结
  SELECT vend_name, prod_name, prod_price
      FROM Vendors, Products
      WHERE Vendors.vend_id = Products.vend_id;
  # 规范式内联结
  SELECT vend_name, prod_name, prod_price
      FROM Vendors
      INNER JOIN Products ON Vendors.vend_id = Products.vend_id
  ````

- 注意：

  （1）内联结和等值连接是一回事

  （2）内联结因为有两个表，注意“ 完全限定名 ”的使用。

  （3）不要过多对三个及三个以上表使用内联结，会降低性能，而且连接表有最大数目限制，不同数据库不一样，可参考文档。

   （4）内联结一定不能忘了 where 子句或者是 on 子句，否则会产生出乎意料的结果（笛卡尔积）

  （5）一般来说，查询两个表有一个联结条件，查询三个表有两个联结条件，查询N个表有N-1个联结条件。

- 内联结连接多个表的情况：

  `````mysql
  # 比较复杂，使用等值联结更加简单
  SELECT cust_name, SUM(quantity * item_price) AS total_price
  	FROM customers
  	INNER JOIN orders ON customers.cust_id = orders.cust_id
  	INNER JOIN orderitems ON orders.order_num = orderitems.order_num
  	GROUP BY orderitems.order_num
  	HAVING total_price >= 1000
  `````

- 课后题

  `````mysql
  /* 第十二章 课后题
  1. 编写 SQL 语句，返回 Customers 表中的顾客名称（cust_name）和
  Orders 表中的相关订单号（order_num），并 按顾客名称再按订单号
  对结果进行排序。实际上是尝试两次，一次使用简单的等联结语法，
  一次使用 INNER JOIN。
  2. 我们来让上一题变得更有用些。除了返回顾客名称和订单号，添加第
  三列 OrderTotal，其中包含每个订单的总价。有两种方法可以执行
  此操作：使用 OrderItems 表的子查询来创建 OrderTotal 列，或者
  将 OrderItems 表与现有表联结并使用聚合函数。提示：请注意需要
  使用完全限定列名的地方。
  3. 我们重新看一下第 11 课的挑战题 2。编写 SQL 语句，检索订购产品
  BR01 的日期，这一次使用联结和简单的等联结语法。输出应该与第
  11 课的输出相同。
  
  4. 很有趣，我们再试一次。重新创建为第 11 课挑战题 3 编写的 SQL 语
  句，这次使用 ANSI 的 INNER JOIN 语法。在之前编写的代码中使用
  了两个嵌套的子查询。要重新创建它，需要两个 INNER JOIN 语句，
  每个语句的格式类似于本课讲到的 INNER JOIN 示例，而且不要忘记
  WHERE 子句可以通过 prod_id 进行过滤。
  
  5. 再让事情变得更加有趣些，我们将混合使用联结、聚合函数和分组。
  准备好了吗？回到第 10 课，当时的挑战是要求查找值等于或大于 1000
  的所有订单号。这些结果很有用，但更有用的是订单数量至少达到
  这个数的顾客名称。因此，编写 SQL 语句，使用联结从 Customers
  表返回顾客名称（cust_name），并从 OrderItems 表返回所有订单的
  总价。
  提示：要联结这些表，还需要包括 Orders 表（因为 Customers 表
  与 OrderItems 表不直接相关，Customers 表与 Orders 表相关，而
  Orders 表与 OrderItems 表相关）。不要忘记 GROUP BY 和 HAVING，
  并按顾客名称对结果进行排序。你可以使用简单的等联结或 ANSI 的
  INNER JOIN 语法。或者，如果你很勇敢，请尝试使用两种方式编写。
  */
  -- (1)
  # 方式1：简单式等联结
  SELECT cust_name, order_num
  	FROM customers, orders
  	WHERE customers.cust_id = orders.cust_id
  # 方式2：规范式內联结
  SELECT cust_name, order_num
  	FROM customers
  	INNER JOIN orders ON customers.cust_id=orders.cust_id
  -- (2)
  SELECT cust_name,
  	orderitems.order_num,
  	SUM(quantity * item_price) AS OrderTotal
  	FROM customers, orders, orderitems
  	WHERE customers.cust_id = orders.cust_id 
  	AND orders.order_num = orderitems.order_num
  	GROUP BY orderitems.order_num
  -- (3)
  SELECT cust_id, order_date
  	FROM orderitems,orders
  	WHERE orderitems.order_num = orders.order_num
  	AND prod_id = 'BR01'
  -- (4)
  SELECT orders.cust_id, cust_email
  	FROM orderitems,orders,customers
  	WHERE orderitems.order_num = orders.order_num
  	AND orders.cust_id = customers.cust_id
  	AND prod_id = 'BR01'
  -- (5)
  # 方式1：简单式等联结
  SELECT cust_name, SUM(quantity * item_price) AS total_price
  	FROM customers, orderitems, orders
  	WHERE customers.cust_id = orders.cust_id
  	AND orders.order_num = orderitems.order_num
  	GROUP BY orderitems.order_num
  	HAVING total_price >= 1000
  # 方式2：标准式內联结
  SELECT cust_name, SUM(quantity * item_price) AS total_price
  	FROM customers
  	INNER JOIN orders ON customers.cust_id = orders.cust_id
  	INNER JOIN orderitems ON orders.order_num = orderitems.order_num
  	GROUP BY orderitems.order_num
  	HAVING total_price >= 1000
  `````

  

## 14、创建高级联结

1. 使用表别名，SQL除了可以对列名和计算字段使用别名，还允许给表名起别名。这样做有两个主要理由：

   - 缩短 SQL 语句； 

   - 允许在一条 SELECT 语句中多次使用相同的表。

   - 实例：

     ````mysql
     # 案例一
     SELECT CONCAT(RTRIM(vend_name),' (',RTRIM(vend_country),')') AS vend_title
     	FROM Vendors
     	ORDER BY vend_name;
     # 案例二
     SELECT cust_name, cust_contact
     	FROM Customers AS C, Orders AS O, OrderItems AS OI
     	WHERE C.cust_id = O.cust_id
     	AND OI.order_num = O.order_num
     	AND prod_id = 'RGAN01';
     ````

   - 注意：表别名只在查询执行中使用。与列别名不一样，表别名不返 回到客户端。 

2. 使用不同类型的联结：

   - 自联结（self-join）
   - 自然联结（natural join）
   - 外联结（outer join）

3. 自联结

   例如：在customers表中查询与 "Jim Jones"同一公司的顾客

   ```mysql
   # 使用子查询的方式
   SELECT cust_id, cust_name, cust_contact
   	FROM Customers
   	WHERE cust_name = (SELECT cust_name
   				 FROM Customers
   				 WHERE cust_contact = 'Jim Jones');
   				 
   # 使用自联结的方式，注意 表别名 和 列完全限定名 配合使用
   SELECT c1.cust_id, c1.cust_name, c1.cust_contact
   	FROM Customers AS c1, Customers AS c2
   	WHERE c1.cust_name = c2.cust_name
   	AND c2.cust_contact = 'Jim Jones'; 
   ```

4. 自然联结

   - 标准的联结（例如内部联结）返回所有数据，甚至相同的列多次出现。自然联结排除多次出现，使每个列只返回一次。但是需要我们人为的过滤掉重复的列，选择唯一的列。例如：

     ````mysql
     # 这个查询是一个自然联结
     SELECT vend_name, prod_name, prod_price
         FROM Vendors, Products
         WHERE Vendors.vend_id = Products.vend_id;
         
     # 这个查询就不是自然联结，因为包含有两个重复的列 vend_id（没有人工去重列）
     SELECT *
         FROM Vendors, Products
         WHERE Vendors.vend_id = Products.vend_id;
     ````

     

5. 外联结

   许多联结将一个表中的行与另一个表中的行相关联，但有时候需要包含没有关联行的那些行，此时需要使用外联结。

   外联结包含左外联结、右外联结和全外联结。

   （1）左外联结：从OUTER JOIN左边的表选择所有行

   （2）右外联结：从OUTER JOIN右边的表选择所有行

   （3）全外联结：包含两个表的不关联的行（MySQL不支持）

   ````mysql
   # 左外联结
   SELECT Customers.cust_id, Orders.order_num
   	FROM Customers
   	LEFT OUTER JOIN Orders ON Customers.cust_id = Orders.cust_id; 
   # 右外联结
   SELECT Customers.cust_id, Orders.order_num
   	FROM Customers
   	RIGHT OUTER JOIN Orders ON Customers.cust_id = Orders.cust_id;
   ````

   

6. 带聚集函数的连接

   案例：

   ````mysql
   # 使用內联结只能查看到cust_id 为 1、3、4、5和他们对应的订单数，
   # 因为id为2的在orders表中没有订单
   SELECT Customers.cust_id,
   	COUNT(Orders.order_num) AS num_ord
   	FROM Customers
   	INNER JOIN Orders ON Customers.cust_id = Orders.cust_id
   	GROUP BY Customers.cust_id;
   # 使用左外联结，可以查看Customers所有的cust_id及其对应的订单数，包含id为2的顾客
   SELECT Customers.cust_id,
   	COUNT(Orders.order_num) AS num_ord
   	FROM Customers
   	LEFT OUTER JOIN Orders ON Customers.cust_id = Orders.cust_id
   	GROUP BY Customers.cust_id;
   ````

7. 使用联结的要点：

   - 注意所使用的联结类型。一般我们使用内联结，但使用外联结也有效。  
   - 保证使用正确的联结条件（不管采用哪种语法），否则会返回不正确的数据。 
   - 应该总是提供联结条件，否则会得出笛卡儿积。 
   - 在一个联结中可以包含多个表，甚至可以对每个联结采用不同的联结类型。虽然这样做是合法的，一般也很有用，但应该在一起测试它们前分别测试每个联结。这会使故障排除更为简单。

- 课后题

  ````mysql
  /* 第十三章 课后题
  1. 使用 INNER JOIN 编写 SQL语句，以检索每个顾客的名称（Customers
  表中的 cust_name）和所有的订单号（Orders 表中的 order_num）。
  2. 修改刚刚创建的 SQL 语句，仅列出所有顾客，即使他们没有下过订单。
  3. 使用 OUTER JOIN 联结 Products 表和 OrderItems 表，返回产品名
  称（prod_name）和与之相关的订单号（order_num）的列表，并按
  商品名称排序。
  4. 修改上一题中创建的 SQL 语句，使其返回每一项产品的总订单数
  （不是订单号）。
  5. 编写 SQL语句，列出供应商（Vendors 表中的 vend_id）及其可供产品
  的数量，包括没有产品的供应商。你需要使用 OUTER JOIN 和 COUNT()
  聚合函数来计算 Products 表中每种产品的数量。注意：vend_id 列
  会显示在多个表中，因此在每次引用它时都需要完全限定它。
  */
  -- (1)
  SELECT cust_name, orders.cust_id, orders.order_num
  	FROM customers
  	INNER JOIN orders ON customers.cust_id = orders.cust_id
  -- (2)
  SELECT cust_name, orders.cust_id, orders.order_num
  	FROM customers
  	LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id
  -- (3)
  SELECT prod_name, order_num
  	FROM products AS P
  	LEFT OUTER JOIN orderitems AS OI ON P.prod_id = OI.prod_id
  	ORDER BY prod_name
  -- (4)
  SELECT prod_name, COUNT(order_num) AS num_ord
  	FROM products AS P
  	LEFT OUTER JOIN orderitems AS OI ON P.prod_id = OI.prod_id
  	GROUP BY prod_name
  	ORDER BY prod_name
  -- (5)
  SELECT V.vend_id, COUNT(prod_name)
  	FROM vendors AS V
  	LEFT OUTER JOIN	products AS P ON V.vend_id = P.vend_id
  	GROUP BY V.vend_id
  ````



## 15、组合查询

多数 SQL 查询只包含从一个或多个表中返回数据的单条 SELECT 语句。 但是，SQL 也允许执行多个查询（多条 SELECT 语句），并将结果作为一 个查询结果集返回。这些组合查询通常称为并（union）或复合查询。

1. 会使用到组合查询的两种情况：
   - 在一个查询中从不同的表返回结构数据； 
   - 对一个表执行多个查询，按一个查询返回数据。

- 实例：

  ````mysql
  # 单独查询
  SELECT cust_name, cust_contact, cust_email
  	FROM Customers
  	WHERE cust_state IN ('IL','IN','MI')
  SELECT cust_name, cust_contact, cust_email
  	FROM Customers
  	WHERE cust_name = 'Fun4All';
  	
  # 合并两条查询语句（合并查询）
  SELECT cust_name, cust_contact, cust_email
  	FROM Customers
  	WHERE cust_state IN ('IL','IN','MI')
  UNION
  SELECT cust_name, cust_contact, cust_email
  	FROM Customers
  	WHERE cust_name = 'Fun4All';
  ````

2. UNION规则

   - UNION 必须由两条或两条以上的 SELECT 语句组成，语句之间用关键字 UNION 分隔（因此，如果组合四条 SELECT 语句，将要使用三个 UNION 关键字）。 
   - UNION 中的每个查询必须包含相同的列、表达式或聚集函数（不过， 各个列不需要以相同的次序列出）。 
   - 列数据类型必须兼容：类型不必完全相同，但必须是 DBMS 可以隐含转换的类型（例如，不同的数值类型或不同的日期类型）。
   - 使用UNION使用查询语句遇到不同的列名会返回第一条语句的列名，这也代表我们只能使用第一个列名来进行排序，或者分组。

3. 取消重复行（UNION ALL）

   使用UNION关键字后，会自动的去掉两个查询结果中的重复数据，最终只保留一条，如果不希望去掉重复行，可以使用 UNION ALL关键字。

4. UNION、UNION ALL 和 WHERE

   UNION 几乎总是完成与多个 WHERE 条件相同的工作，但是使用UNION可以极大的简化复杂的WHERE子句。UNION ALL 为 UNION 的一种形式，它完成 WHERE 子句完成不了的工作。如果确实需要每个条件的匹配行全部出现（包括重复行），就必须使用 UNION ALL，而不是 WHERE。

5. 对组合查询结果排序

   在用 UNION 组合查询时，只 能使用一条 ORDER BY 子句，它必须位于最后一条 SELECT 语句之后。例如：

   ````mysql
   SELECT cust_name, cust_contact, cust_email
   	FROM Customers
   	WHERE cust_state IN ('IL','IN','MI')
   UNION
   SELECT cust_name, cust_contact, cust_email
   	FROM Customers
   	WHERE cust_name = 'Fun4All'
   	ORDER BY cust_name, cust_contact
   ````

   虽然ORDER BY 子句似乎只是最后一条 SELECT 语句的组成部分，但实际上 DBMS 将 用它来排序所有 SELECT 语句返回的所有结果。

- 课后题

  ````mysql
  /* 第十四章课后题
  1. 编写 SQL 语句，将两个 SELECT 语句结合起来，以便从 OrderItems
  表中检索产品 ID（prod_id）和 quantity。其中，一个 SELECT 语
  句过滤数量为 100 的行，另一个 SELECT 语句过滤 ID 以 BNBG 开头的
  产品。按产品 ID 对结果进行排序。
  2. 重写刚刚创建的 SQL 语句，仅使用单个 SELECT 语句。
  3. 我知道这有点荒谬，但这节课中的一个注释提到过。编写 SQL 语句，
  组合 Products 表中的产品名称（prod_name）和 Customers 表中的
  顾客名称（cust_name）并返回，然后按产品名称对结果进行排序。
  4. 下面的 SQL 语句有问题吗？（尝试在不运行的情况下指出。）
  SELECT cust_name, cust_contact, cust_email
  	FROM Customers
  	WHERE cust_state = 'MI'
  	ORDER BY cust_name;
  UNION
  SELECT cust_name, cust_contact, cust_email
  	FROM Customers
  	WHERE cust_state = 'IL'ORDER BY cust_name;
  */
  -- (1)
  SELECT prod_id, quantity
  	FROM orderitems
  	WHERE quantity >= 100
  UNION 
  SELECT prod_id, quantity
  	FROM orderitems
  	WHERE prod_id LIKE 'BNBG%'
  	ORDER BY prod_id
  -- (2)
  SELECT prod_id, quantity
  	FROM orderitems
  	WHERE quantity >= 100 OR prod_id LIKE 'BNBG%'
  	ORDER BY prod_id
  -- (3)
  SELECT prod_name
  	FROM products
  UNION ALL
  SELECT cust_name
  	FROM customers
  	ORDER BY prod_name
  -- (4)
  # 有问题，Order By 子句应该放在最后一个SELECT子句中的末尾位置，也是整个SQL语句的末尾位置
  ````



## 16、全文本搜索

1. 全文本搜索：在使用全文本搜索时，MySQL不需要分别查看每个行，不需要分别分析和处理每个词。

2. 对于MySQL两个最常用的引擎：MyISAM（my-eizem） 和InnoDB（in-no-db），MyISAM引擎支持全文本搜索，而InnoDB引擎不支持全文本搜索。

3. 启用（定义）全文本搜索：一般在创建表时启用全文本搜索。在CREATE TABLE语句接受FULLTEXT子句。如：

   `````mysql
   # 该语句创建了 productnotes 表，并且指定 note_text 这一列可以对它进行索引
   CREATE TABLE productnotes
   (
   note_id 	INT 		NOT NULL 	AUTO_INCREMENT,
   prod_id 	CHAR(10) 	NOT NULL,
   note_date 	DATETIME 	NOT NULL,
   note_text 	TEXT		NULL,
   PRIMARY KEY(note_id)
   FULLTEXT(note_text)
   )ENGINE=MYISAM;
   `````

   注意：FULLTEXT可以索引单个列，也可以索引多个列，在定义之后，MySQL会自动维护该索引，在增加、更新或删除行时， 索引随之自动更新。

4. 进行全文本搜索：

   - 在定义索引之后，使用函数 Match()和 Against()来执行全文本搜索，其中Match()指定被搜索的列，Against()指定要使用的搜索表达式。例如：

     `````mysql
     SELECT note_text
     FROM productnotes
     WHERE Match(note_text) Against('rabbit');
     
     # 查询数据结果等价于：
     SELECT note_text
     FROM productnotes
     WHERE note_text LIKE '%rabbit%'
     `````

     上述两条SELECT语句都不包含ORDER BY子句。后者（使用LIKE）以不特别有用的顺序返回数据。前者（使用全文本搜索）返回以文本匹配的良好程度排序的数据。两个行都包含词rabbit，但包含词rabbit作为第3个词的行的等级比作为第20个词的行高。这很重要。全文本搜索的一个重要部分就是对结果排序。具有较高等级的行先返回（因为这些行很可能是你真正想要的行）。另外由于数据是索引的，所以全文本搜索很很快。

   - 查看全文本搜索的排序工作：

     ````mysql
     # 查看全文本搜索的排序工作
     SELECT note_text,
     	MATCH(note_text) AGAINST('rabbit') AS rank
     FROM productnotes
     # 输出每一行及其等级，等级由MySQL根据行中词的数目、唯一词的数目、整个索引中词的总数以及包含该词的行的数目计算出来。正如所见，不包含词rabbit的行等级为0（因此不被前一例子中的WHERE子句选择）。确实包含词rabbit的两个行每行都有一个等级值，文本中词靠前的行的等级值比词靠后的行的等级值高。这个例子有助于说明全文本搜索如何排除行（排除那些等级为0的行），如何排序结果（按等级以降序排序）。
     ````

5. 全文本搜索总结：

   - 进行全文搜索之前需要定义索引，使用FULLTEXT关键字。
   - 使用全文本搜索用Match()函数和Against()函数，Mathch()函数中，是索引的列，Against()函数中是搜索的关键词。
   - 全文本搜索由于数据是索引的，因此查询速度更快，效率更高。
   - 全文本搜索的结果是按照等级降序来进行输出的。

6. 使用查询扩展：查询扩展用来设法放宽所返回的全文本搜索结果的范围。

   实例：

   ````mysql
   # 找出所有提到anvils的注释。只有一个注释包含词anvils，但你还想找出可能与你的搜索有关的所有其他行，即使它们不包含词anvils
   # 查询扩展
   SELECT note_text
   FROM productnotes
   WHERE MATCH(note_text) AGAINST('anvils' WITH QUERY EXPANSION)
   
   /* 返回结果：
   返回结果一共包含7行，第一行包含词anvils，因此等级最高。第二行与anvils无关，但因为它包含第一行中的两个词（customer和recommend），所以也被检索出来。第3行也包含这两个相同的词，但它们在文本中的位置更靠后且分开得更远，因此也包含这一行，但等级为第三。第三行确实也没有涉及anvils（按它们的产品名）。
   ````

   - 一般来说，表中的行越多（这些行中的文本就越多），使用 查询扩展返回的结果越好。

   - 在使用查询扩展时，MySQL对数据和索引进行两遍扫描来完成搜索，其流程去下：

     - 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行；
     - 其次，MySQL检查这些匹配行并选择所有有用的词（我们将会简 要地解释MySQL如何断定什么有用，什么无用）。 
     - 再其次，MySQL再次进行全文本搜索，这次不仅使用原来的条件， 而且还使用所有有用的词。

     这样，利用查询扩展，能找出可能相关的结果，即使它们并不精确包含所查找的词，但是也达到了扩展的效果。

7. 布尔文本搜索：MySQL支持全文本搜索的另外一种形式，称为布尔方式（boolean mode）。

   - 布尔搜索可提供关于如下内容的细节：

     - 要匹配的词； 
     - 要排斥的词（如果某行包含这个词，则不返回该行，即使它包含其他指定的词也是如此）； 
     - 排列提示（指定某些词比其他词更重要，更重要的词等级更高）；
     - 表达式分组；
     - 另外一些内容。

   - 布尔搜索没有FULLTEXT索引也可以使用：布尔方式不同于迄今为止使用的全文本搜索语法的地方在于，即使没有定义FULLTEXT索引，也可以使用它。但这是一种非常缓慢的操作（其性能将随着数据量的增加而降低）。

   - 实例：

     ````mysql
     # 使用布尔搜索 检索包含词heavy的所有行
     SELECT note_text
     FROM productnotes
     WHERE MATCH(note_text) AGAINST('heavy' IN BOOLEAN MODE)
     
     /* 返回结果
     此全文本搜索检索包含词heavy的所有行（有两行）。
     */
     
     # 使用布尔搜索 检索包含heavy但不包含任意以rope开始的词的行
     SELECT note_text
     FROM productnotes
     WHERE MATCH(note_text) AGAINST('heavy -rope*' IN BOOLEAN MODE)
     /* 返回结果
     只有一行，排除了包含rope*（任何以rope开始的词，包括ropes）的行，
     ````

   - MySQL支持的所有布尔搜索操作符：

     ![image-20220629143918374](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220629143918374.png)

   - 举例说明其中操作符的使用：

     ````mysql
     # 布尔搜索 操作符
     # 这个搜索匹配包含词rabbit和bait的行。
     SELECT note_text
     FROM productnotes
     WHERE MATCH(note_text) AGAINST('+rabbit +bait' IN BOOLEAN MODE);
     
     # 没有指定操作符，这个搜索匹配包含rabbit和bait中的至少一个词的行。
     SELECT note_text
     FROM productnotes
     WHERE MATCH(note_text) AGAINST('rabbit bait' IN BOOLEAN MODE);
     
     # 这个搜索匹配短语rabbit bait而不是匹配两个词rabbit和bait。
     SELECT note_text
     FROM productnotes
     WHERE MATCH(note_text) AGAINST('"rabbit bait"' IN BOOLEAN MODE);
     
     # 匹配rabbit和carrot，增加前者的等级，降低后者的等级。
     SELECT note_text
     FROM productnotes
     WHERE MATCH(note_text) AGAINST('>rabbit <carrot' IN BOOLEAN MODE);
     
     # 这个搜索匹配词safe和combination，降低后者的等级。
     SELECT note_text
     FROM productnotes
     WHERE MATCH(note_text) AGAINST('+safe +(<combination)' IN BOOLEAN MODE);
     ````

     排列而不排序：在布尔方式中，不按等级值降序排序返回的行。

8. 全文本搜索的重要使用说明：

   - 在索引全文本数据时，短词被忽略且从索引中排除。短词定义为那些具有3个或3个以下字符的词（如果需要，这个数目可以更改）。 
   - MySQL带有一个内建的非用词（stopword）列表，这些词在索引全文本数据时总是被忽略。如果需要，可以覆盖这个列表（请参阅MySQL文档以了解如何完成此工作）。
   - 许多词出现的频率很高，搜索它们没有用处（返回太多的结果）。因此，MySQL规定了一条50%规则，如果一个词出现在50%以的行中，则将它作为一个非用词忽略。50%规则不用于IN BOOLEAN MODE。
   - 如果表中的行数少于3行，则全文本搜索不返回结果（因为每个词或者不出现，或者至少出现在50%的行中）。 
   - 忽略词中的单引号。例如，don't索引为dont。 
   - 不具有词分隔符（包括日语和汉语）的语言不能恰当地返回全文本搜索结果。 
   - 如前所述，仅在MyISAM数据库引擎中支持全文本搜索。



## 17、数据插入

1. INSERT关键字是用于将行插入（或添加）到数据库表，常见的插入分为以下几种方式：

   - 插入完整的行
   - 插入行的一部分
   - 插入多行
   - 插入某些查询的结果

   

2. 插入完整行

   ````mysql
   INSERT INTO Customers
   	VALUES(1000000006,
   	 'Toy Land',
   	 '123 Any Street',
   	 'New York',
   	 'NY',
   	 '11111',
   	 'USA',
   	 NULL,
   	 NULL);
   # 但是上述方式并不安全，因为其高度依赖表中列的定义次序，使用下面的方式更加安全（虽然繁琐）：指定列名和插入数据值
   INSERT INTO Customers(cust_id,
   		      cust_name,
   		      cust_address,
   		      cust_city,
   		      cust_state,
   		      cust_zip,
   		      cust_country,
   		      cust_contact,
   		      cust_email)
   VALUES(1000000006,
   	 'Toy Land',
   	 '123 Any Street',
   	 'New York',
   	 'NY',
   	 '11111',
   	 'USA',
   	 NULL,
   	 NULL);
   ````

   

3. 插入部分行

   `````mysql
   INSERT INTO Customers(cust_id,
                         cust_name,
                         cust_address,
                         cust_city,
                         cust_state,
                         cust_zip,
                         cust_country)
   VALUES(1000000006,
            'Toy Land',
            '123 Any Street',
            'New York',
            'NY',
            '11111',
            'USA');
   `````

   - 如上述SQL语句，没有给 cust_contact 和 cust_email 这两列提供值。这表示没必要在 INSERT 语句中包含它们。因此，这里的 INSERT 语句省略了这两列及其对应的值。

   - 进行部分插入，省略列时必须满足下列条件（不满足则会插入失败）：

     - 该列定义为允许 NULL 值（无值或空值）。 
     - 在表定义中给出默认值。这表示如果不给出值，将使用默认值。

     

4. 插入多个行

   `````mysql
   # 插入多个行数据
   INSERT INTO customers(cust_name,
   	cust_address,
   	cust_city,
   	cust_state,
   	cust_zip,
   	cust_country)
   VALUES('Pep E. LaPew',
   	'100 Main Street',
   	'Los Angeles',
   	'CA',
   	'90046',
   	'USA');
   INSERT INTO customers(cust_name,
   	cust_address,
   	cust_city,
   	cust_state,
   	cust_zip,
   	cust_country)
   VALUES('M. Martian',
   	'42 Galaxy Way',
   	'New York',
   	'NY',
   	'11213',
   	'USA');
   	
   # 在每条INSERT语句中的列名（和次序）相同时，可简化为以下语句：
   INSERT INTO customers(cust_name,
   	cust_address,
   	cust_city,
   	cust_state,
   	cust_zip,
   	cust_country)
   VALUES('Pep E. LaPew',
   	'100 Main Street',
   	'Los Angeles',
   	'CA',
   	'90046',
   	'USA')，
   (
   	'M. Martian',
   	'42 Galaxy Way',
   	'New York',
   	'NY',
   	'11213',
   	'USA'
   );
   `````

5. 插入检索出的数据（INSERT SELECT）

   所谓的 INSERT SELECT，它是由一条 INSERT 语句和一条 SELECT 语句组成的。

   ````mysql
   INSERT INTO Customers(cust_id,
                         cust_contact,
                         cust_email,
                         cust_name,
                         cust_address,
                         cust_city,
                         cust_state,
                         cust_zip,
                         cust_country)
   SELECT cust_id,
   	   cust_contact,
   	   cust_email,
   	   cust_name,
   	   cust_address,
   	   cust_city,
   	   cust_state,
   	   cust_zip,
   	   cust_country
   FROM CustNew;
   ````

   - 上面的例子使用 INSERT SELECT 从 CustNew 中将所有数据导入Customers。SELECT 语句从 CustNew 检索出要插入的值，而不是列出它们。

   - 在实际使用中不需要列名一致，只要列对应的数据类型一致就可以成功插入。
   - INSERT SELECT中SELECT语句可包含WHERE子句以过滤插入的数据。

6. 复制表

   使用 CREATE SELECT 语句，可以要将一个表的内容复制到一个全新的表（未存在的表），实例：

   `````mysql
   # 将Customers 复制到一个新表（custCopy）
   CREATE TABLE CustCopy AS SELECT * FROM Customers;
   `````

   

- 课后题

  ````mysql
  /* 第十五章 课后题
  1. 使用 INSERT 和指定的列，将你自己添加到 Customers 表中。明确列出要添加哪几列，且仅需列出你需要的列。
  2. 备份 Orders 表和 OrderItems 表。
  */
  -- (1)
  INSERT INTO customers(cust_id,
  			cust_name,
  			cust_city,
  			cust_state,
  			cust_country,
  			cust_contact)
  VALUES(1000000006,
  	'SXD',
  	'南京市',
  	'江苏省',
  	'中国',
  	'lx')
   DELETE FROM customers
  	WHERE cust_id LIKE '1%6'
  -- (2)
  CREATE TABLE Orders_copy AS SELECT * FROM orders
  CREATE TABLE Ordersitems_copy AS SELECT * FROM OrderItems
  DROP TABLE Orders_copy
  DROP TABLE Ordersitems_copy
  ````
  


## 18、更新和删除数据

### 18.1、更新数据

1. 使用UPDATE更新表中的数据

   - 更新表中的特定行
   - 更新表中的所有行

   实例：

   ````mysql
   # 更新一个列
   UPDATE Customers
   SET cust_email = 'kim@thetoystore.com'
   WHERE cust_id = 1000000005;
   # 更新多个列
   UPDATE Customers
   SET cust_contact = 'Sam Roberts',
    	cust_email = 'sam@toyland.com'
   WHERE cust_id = 1000000006;
   ````

2. UPDATE语句由以下三部分组成：

   - 要更新的表； 
   - 列名和它们的新值；
   - 确定要更新行的过滤条件
   
   实例：
   
   `````mysql
   # 将客户 10005 的邮箱进行更新
   UPDATE customers
   SET cust_email = 'elmer@fudd.com'
   WHERE cust_id = 10005;
   
   # 更新多个列
   UPDATE customers
   	SET cust_name = 'The Fudds',
   	cust_email = 'elmer@fudd.com'
   	WHERE cust_id = 10005;
   `````
   
   
   
3. 使用UPDATE的要点：

   - 不要省略 WHERE 子句，如果没有 WHERE子句，则是将该列中所有的值都修改为新值。
   - 可以以更新值为NULL的方式来删除某个列的值（在表定义允许为Null值时）。
   - UPDATE子句可以使用子查询
   - 在使用UPDATE时可能需要特殊的安全权限，因此需要保证有足够的安全权限。

   

4. UPDATE中的 Ignore 关键字

   如果用UPDATE语句更新多行，并且在更新这些行中的一行或多行时出一个现错误，则整个UPDATE操作被取消（错误发生前更新的所有行被恢复到它们原来的值）。为即使是发生错误，也继续进行更新，可使用IGNORE关键字，如下所示：

   `````mysql
   UPDATE IGNORE customers 
   	SET cust_name = 'The Fudds',
   	cust_email = 'elmer@fudd.com'
   	WHERE cust_id = 10005;
   `````

   

### 18.2、删除数据

1. 使用DELETE子句，删除表中的数据：

   - 从表中删除特定的行
   - 从表中删除所有行

   实例：

   `````mysql
   # 删除一行数据
   DELETE FROM Customers
   WHERE cust_id = 1000000006;
   # 删除所有行（慎用）
   DELETE FROM Customers
   `````

2. DELETE删除的是表的内容，而不是删除表。

3. DELETE删除的是整行，而不是列，如果要删除列，可以使用UPDATE语句。

4. 在使用DELETE关键字时，一定不要省略WHERE子句，否则可能导致不可挽回的结果。

5. 更快删除所有行：如果真的想要删除所有行，可以使用 TRUNCATE TABLE 进行更快的删除，因为这个操作不记录数据的变动。（底层是是删除原来的表并重新创建一个表，而不是逐行删除表中的数据）。



### 18.3、更新和删除数据的原则

1. 除非确实打算更新和删除每一行，否则绝对不要使用不带 WHERE 子句的 UPDATE 或 DELETE 语句。 
2. 保证每个表都有主键，尽可能像 WHERE 子句那样使用它（可以指定各主键、多个值或值的范围）。 
3. 在 UPDATE 或 DELETE 语句使用 WHERE 子句前，应该先用 SELECT 进 行测试，保证它过滤的是正确的记录，以防编写的 WHERE 子句不正确。 
4. 使用强制实施引用完整性的数据库， 这样 DBMS 将不允许删除其数据与其他表相关联的行。 
6. MySQL没有撤销（undo）按钮，应该非常小心地使用 UPDATE 和 DELETE，否则无法恢复错误更新和删除了的数据。

- 课后题

  ````mysql
  -- 第十六章 更新、删除数据
  /*
  1. 美国各州的缩写应始终用大写。编写 SQL语句来更新所有美国地址，包
  括供应商状态（Vendors 表中的 vend_state）和顾客状态（Customers
  表中的 cust_state），使它们均为大写。
  2. 第 15 课的挑战题 1 要求你将自己添加到 Customers 表中。现在请删除
  自己。确保使用 WHERE 子句（在 DELETE 中使用它之前，先用 SELECT
  对其进行测试），否则你会删除所有顾客！
  */
  -- (1)
  UPDATE vendors,customers
  SET vend_state = UPPER(vend_state),
  	cust_state = UPPER(cust_state)
  -- (2)
  DELETE FROM customers
  	WHERE cust_id LIKE '%6'
  # 查看删除情况
  SELECT *
  	FROM customers
  	WHERE cust_id LIKE '%6'
  ````

  

## 19、创建和操纵表

### 19.1、创建表

1. 常见的两种创建表的方式
   - 使用具有交互式创建和管理数据库表的工具； 
   - 直接用 MySQL 语句操纵。

2. 使用CREATE TABLE创建表，必须给出的信息：

   - 新表的名字，在关键字CREATE TABLE之后给出； 
   - 表列的名字和定义，用逗号分隔。
   
3. 使用SQL语句创建表

   ````mysql
   CREATE TABLE Products
   (
    prod_id 		CHAR(10)		 NOT NULL,
    vend_id 		CHAR(10) 		 NOT NULL,
    prod_name 		CHAR(254) 	   	  NOT NULL,
    prod_price 	DECIMAL(8,2) 	  NOT NULL,
    prod_desc 		VARCHAR(1000)	  NULL
   ) ENGIN = InnoDB;
   # 或者使用以下方式
   CREATE TABLE ‘user‘ (
   	id INT,
   	‘name‘ VARCHAR(255),
   	‘password‘ VARCHAR(255),
   	‘birthday‘ DATE)
   	CHARACTER SET utf8 COLLATE utf8_bin ENGINE INNODB;
   ````

   ![image-20220627165327735](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220627165327735.png)

4. 在创建表时，要求指定的表名必须不存在，否则会出错，这样是为了防止意外的覆盖已有的表。

5. 在创建表时，如果没有指定列为NOT NULL，则列默认为NULL（主键不允许为NULL），如下：

   ````mysql
   CREATE TABLE Vendors
   (
    vend_id 		CHAR(10) 		NOT NULL,
    vend_name 		CHAR(50) 		NOT NULL,
    vend_address 	CHAR(50) 		,
    vend_city 		CHAR(50) 		,
    vend_state		CHAR(5) 		,
    vend_zip 		CHAR(10) 		,
    vend_country 	CHAR(50)
   ) ENGIN = InnoDB;
   ````

6. 指定主键，使用的CREATE TABLE例子都是用单个列作为主键。其中主键用以下的类似的语句定义：

   `````mysql
   # 指定但列作为主键
   PRIMARY KEY (vend_id)
   # 创建多个列组成的主键
   CREATE TABLE Vendors
   (
    vend_id 		CHAR(10) 		NOT NULL,
    vend_name 		CHAR(50) 		NOT NULL,
    vend_address 	CHAR(50) 		,
    vend_city 		CHAR(50) 		,
    vend_state		CHAR(5) 		,
    vend_zip 		CHAR(10) 		,
    vend_country 	CHAR(50)		,
    PRIMARY KEY (vend_id,vend_name)
   ) ENGINE = InnoDB;
   `````

   - 另外，主键可以在创建好表之后再定义
   - 主键作为唯一标识表中每个行的列，不允许为NULL。

7. 使用自动增量（AUTO_INCREMENT关键字），用于指定ID或者编号自增长。

   ````mysql
   # 创建customers表的CREATE TABLE语句的组成部分
   cust_id 	INT 	NOT NULL 	AUTO_INCREMENT
   ````

   - AUTO_INCREMENT告诉MySQL，本列每当增加一行时自动增量。每次执行一个INSERT操作时，MySQL自动对该列增量（从而才有这个关键字AUTO_INCREMENT），给该列赋予下一个可用的值。这样给每个行分配一个唯一的cust_id，从而可以用作主键值。
   - 每个表只允许一个AUTO_INCREMENT列，而且它必须被索引（如，通 过使它成为主键）。
   - 确定 AUTO_INCREMENT 值，使用 last_insert_id() 函数返回最后一个 AUTO_INCREMENT 值，然后可以将它用于后续的MySQL语句。

   

8. 指定默认值，对于一些列，还可以指定默认主键值，使用DEFAULT关键字来完成。

   ````mysql
   CREATE TABLE OrderItems
   (
    order_num 			INTEGER 		NOT NULL,
    order_item 		INTEGER 		NOT NULL,
    prod_id 			CHAR(10) 		NOT NULL,
    quantity 			INTEGER 		NOT NULL 	DEFAULT 1,
    item_price 		DECIMAL(8,2) 	 NOT NULL
   ) ENGIN = InnoDB;
   ````

   - MySQL不允许使用函数作为默认值，只支持常量。
   - 使用默认值而不是NULL值：许多数据库开发人员使用默认值而不是NULL列，特别是对用于计算或数据分组的列更是如此。
   - 默认值常用于日期或者时间戳列，例如，通过指定引用系统日期的函数或变量，将系统日期用作默认日期。MySQL 用户指定 DEFAULT CURRENT_ DATE()。

   - 常见数据库获取系统日期函数如下：

   ![image-20220627170600086](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220627170600086.png)

   

9. MySQL常见的引擎类型：

   - InnoDB是一个可靠的事务处理引擎，它不支持全文本搜索； 
   - MEMORY在功能等同于MyISAM，但由于数据存储在内存（不是磁盘）中，速度很快（特别适合于临时表）； 
   - MyISAM是一个性能极高的引擎，它支持全文本搜索，但不支持事务处理。

   [MySQL支持的所有引擎完整列表文档](http://dev.mysql.com/doc/refman/5.0/en/storage_engines.html)

   MyISAM 由于其性能和特性可能是最受欢迎的引擎。但如果你需要可靠的事务处理，可以使用其他引擎。



### 19.2、更新表

1. 更新表使用ALTER TABLE语句

2. 使用ALTER TABLE注意事项：
   - 理想情况下，不要在表中包含数据时对其进行更新。应该在表的设计过程中充分考虑未来可能的需求，避免今后对表的结构做大改动。 
   - 所有的 DBMS 都允许给现有的表增加列，不过对所增加列的数据类型 （以及 NULL 和 DEFAULT 的使用）有所限制。 
   - 许多 DBMS 不允许删除或更改表中的列。 
   - 多数 DBMS 允许重新命名表中的列。 
   - 许多 DBMS 限制对已经填有数据的列进行更改，对未填有数据的列几乎没有限制。
   
3. 使用ALTER TABLE 更改表结构，必须给出以下信息：
   - 在 ALTER TABLE 之后给出要更改的表名（该表必须存在，否则将出错）； 
   - 所做更改的列表。

4. 使用ALTER TABLE增加列，删除列操作

   ````mysql
   # 增加列操作
   ALTER TABLE Vendors
   ADD vend_phone CHAR(20);
   # 删除列操作
   ALTER TABLE vendors
   DROP COLUMN vend_phone
   ````

5. 对于复杂的表结构，更改一般需要手动删除，涉及到以下步骤：

   - 用新的列布局创建一个新表； 
   - 使用INSERT SELECT语句从旧表复制数据到新表。如果有必要，可使用转换函数和计算字段；
   - 检验包含所需数据的新表；
   - 重命名旧表（如果确定，可以删除它）；
   - 用旧表原来的名字重命名新表； 
   - 根据需要，重新创建触发器、存储过程、索引和外键。

6. 注意使用ALTER TABLE关键字时，需要特别小心，在改动之前需要做完整的备份（表结构和数据的备份），因为数据库标的更改不能撤销，如果增加了不需要的列，可能就无法删除了，同样，删除了不应该删除的列，也可能会丢失该列中的所有数据。



### 19.3、删除表

1. 删除表的操作特变简单，使用DROP TABLE 表名，即可删除对应的表。

   ```mysql
   DROP TABLE CustCopy
   ```

2. 需要注意的是，删除表没有确认步骤，运行后直接删除，也不能撤销，执行语句后将永久删除指定的表。



### 19.4、重命名表

1. 重命名表使用 RENAME 关键字，使用格式如下：

   ````
   RENAME table 表名 to 新表名
   ````

2. 重命名表实例：

   `````mysql
   # 重命名单个表
   RENAME TABLE customers2 TO customers
   
   # 重命名多个表
   RENAME TABLE backup_customers TO customers,
   			backup_vendors TO vendors,
   			backup_products TO products;
   `````

   

- 课后题

  ````mysql
  /*
  1. 在 Vendors 表中添加一个网站列（vend_web）。你需要一个足以容纳
  URL 的大文本字段。
  2. 使用 UPDATE 语句更新 Vendor 记录，以便加入网站（你可以编造任
  何地址）。
  */
  -- (1)
  ALTER TABLE Vendors
  ADD vend_web CHAR(64)
  
  -- (2)
  # 将vendors 表的vend_web这一列都更新为 www.baidu.com
  UPDATE vendors
  SET vend_web = 'www.baidu.com'
  
  SELECT *
  	FROM vendors
  ````

  

## 20、使用视图

### 20.1、视图

1. 视图是虚拟的表。与包含数据的表不一样，视图只包含使用时动态检索数据的查询。

1. 视图的作用：将整个查询包装成一个虚拟表，这样可以轻松检索数据，需要查询其他条件的产品时，只需要修改最后的WHERE子句就可以了。

   例如：

   ````mysql
   SELECT cust_name, cust_contact
       FROM Customers, Orders, OrderItems
       WHERE Customers.cust_id = Orders.cust_id
       AND OrderItems.order_num = Orders.order_num
       AND prod_id = 'RGAN01'; 
       
   # 将中间的等值连接包装成一个名为 ProductCustomers 的虚拟表
   SELECT cust_name, cust_contact
       FROM ProductCustomers
       WHERE prod_id = 'RGAN01';	
   ````

2. 视图的常见应用

   - 重用 SQL 语句。 
   - 简化复杂的 SQL 操作。在编写查询后，可以方便地重用它而不必知道其基本查询细节。 
   - 使用表的一部分而不是整个表。 
   - 保护数据。可以授予用户访问表的特定部分的权限，而不是整个表的访问权限。 
   - 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据。

3. 视图仅仅是用来查看存储在别处数据的一种设施。视图本身不包含数据，因此返回的数据是从其他表中检索出来的。在添加或更改这些表中的数据时，视图将返回改变过的数据。

3. 嵌套视图，可能会影响查询性能，因此，在部署使用了大量视图的应用前，应该进行测试。

4. 视图创建和使用的常见规则和限制

   - 与表一样，视图必须唯一命名（不能给视图取与别的视图或表相同的 名字）。 
   - 对于可以创建的视图数目没有限制。 
   - 为了创建视图，必须具有足够的访问权限。这些权限通常由数据库管理人员授予。 
   - 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造视图。 所允许的嵌套层数在不同的 DBMS中有所不同（嵌套视图可能会严重降低查询的性能，因此在产品环境中使用之前，应该对其进行全面测试）。
   - ORDER BY 可以用在视图中，但如果从该视图检索数据SELECT中也含有ORDER BY，那么该视图中的ORDER BY将被覆盖。
   - 视图不能索引，也不能有关联的触发器或默认值。 
   - 视图可以和表一起使用。例如，编写一条联结表和视图的SELECT 语句。
   
   

### 20.2、创建视图

1. 在理解什么是视图（以及管理它们的规则及约束）后，我们来看一下视图的创建。

   - 视图用 CREATE VIEW 语句来创建。 
   - 使用 SHOW CREATE VIEW 视图名；来查看创建视图的语句。 
   - 用DROP删除视图，其语法为 DROP VIEW 视图名 。 
   - 更新视图时，可以先用 DROP 再用 CREATE，也可以直接用 CREATE OR REPLACE VIEW。如果要更新的视图不存在，则第2条更新语句会创建一个视图；如果要更新的视图存在，则第2条更新语句会替换原有视图。

2. 创建视图使用 CREATE VIEW 语句来创建，与CREATE TABLE一样，CREATE VIEW只能用于创建不存在的视图。

   ````mysql
   CREATE VIEW 视图名 AS
   	SELECT 语句
   ````

3. 重命名视图，重命名视图（覆盖、更新），可以先删除视图，然后重新创建，删除视图使用DROP语句，语法为DROP VIEW 视图名，或者使用CREATE OR REPLACE VIEW 语句来更新视图。

   ```mysql
   # 删除视图，再重新创建的方式
   DROP VIEW 旧视图名
   CREATE VIEW 新视图名
   
   # 使用 CREATE OR REPLACE VIEW 的方式来更新视图
   CREATE OR REPLACE VIEW 视图名
   # 如果要更新的视图不存在，则会创建一个视图；如果要更新的视图存在，则会替换原有视图。
   ```

4. 利用视图简化复杂的联结（隐藏复杂的SQL）

   ````mysql
   # 创建视图隐藏复杂的联结
   CREATE VIEW ProductCustomers AS
   	SELECT cust_name, cust_contact, prod_id
   	FROM Customers, Orders, OrderItems
   	WHERE Customers.cust_id = Orders.cust_id
   	AND OrderItems.order_num = Orders.order_num;
   # 利用创建的视图进行查询
   SELECT cust_name, cust_contact
   	FROM ProductCustomers
   	WHERE prod_id = 'RGAN01';
   ````

5. 利用视图重新格式化检索出的数据

   ````mysql
   # 重新格式化检索的数据
   SELECT CONCAT(RTRIM(vend_name),' (',RTRIM(vend_country),')')
    AS vend_title
   FROM Vendors
   ORDER BY vend_name;
   # 创建视图来重新格式化检索的数据
   CREATE VIEW VendorLocations AS
   	SELECT CONCAT(RTRIM(vend_name),' (',RTRIM(vend_country),')')
   	 AS vend_title
   	FROM Vendors
   # 查询视图
   SELECT *
   	FROM VendorLocations
   ````

6. 利用视图过滤不想要的数据

   ```mysql
   # 创建视图过滤邮箱为NULL的的顾客
   CREATE VIEW CustomerEMailList AS
   	SELECT cust_id, cust_name, cust_email
   	FROM Customers
   	WHERE cust_email IS NOT NULL;
   # 查询视图
   SELECT *
   	FROM CustomerEMailList
   ```

7. 利用视图保留计算字段

   ````mysql
   # 计算字段
   SELECT prod_id,
   	 quantity,
   	 item_price,
   	 quantity*item_price AS expanded_price
   	FROM OrderItems
   	WHERE order_num = 20008;
   # 利用视图保存计算字段
   CREATE VIEW OrderItemsExpanded AS
   	SELECT order_num,
   		 prod_id,
   		 quantity,
   		 item_price,
   		 quantity*item_price AS expanded_price
   	FROM OrderItems
   # 利用视图查询
   SELECT *
   	FROM orderitemsExpanded
   	WHERE order_num = 20008
   ````

### 20.3、更新视图

1. 通常，视图是可更新的（即，可以对它们使用INSERT、UPDATE 和 DELETE）。更新一个视图将更新其基表（可以回忆一下，视图本身没有数据）。如果你对视图增加或删除行，实际上是对其基表增加或删除行。
2. 但是，并非所有视图都是可更新的。基本上可以说，如果MySQL不能正确地确定被更新的基数据，则不允许更新（包括插入和删除）。这实际上意味着，如果视图定义中有以下操作，则不能进行视图的更新：
   - 分组（使用 GROUP BY 和 HAVING）
   - 联结
   - 子查询
   - 并查询
   - 聚集函数（MIN()、COUNT()、SUM()等）
   - DISTINCT
   - 导出（计算）列
3. 对于视图的使用，我们一般用于检索（SELECT语句） 而不用于更新（INSERT、UPDATE 和 DELETE）。

- 课后题

  ````mysql
  /* 第十八章课后题
  1. 创建一个名为 CustomersWithOrders 的视图，其中包含 Customers
  表中的所有列，但仅仅是那些已下订单的列。提示：可以在 Orders
  表上使用 JOIN 来仅仅过滤所需的顾客，然后使用 SELECT 来确保拥
  有正确的数据。
  2. 下面的 SQL 语句有问题吗？（尝试在不运行的情况下指出。）
  CREATE VIEW OrderItemsExpanded AS
  	SELECT order_num,
  	 prod_id,
  	 quantity,
  	 item_price,
  	 quantity*item_price AS expanded_price
  	FROM OrderItems
  	ORDER BY order_num;
  */
  -- (1)
  CREATE VIEW CustomersWithOrders AS
  	SELECT C.*
  	FROM customers AS C
  	RIGHT OUTER JOIN orders AS O ON C.cust_id = O.cust_id
  # 删除视图	
  DROP VIEW CustomersWithOrders
  SELECT *
  	FROM CustomersWithOrders
  -- (2)
  # 错误，因为视图中不允许使用 ORDER BY 子句
  ````

  

## 21、使用存储过程

1. 存储过程：存储过程就是为以后使用而保存的一条或多条 SQL 语句。

2. 使用存储过程：

   - 通过把处理封装在一个易用的单元中，可以简化复杂的操作。

   - 由于不要求反复建立一系列处理步骤，因而保证了数据的一致性。如果所有开发人员和应用程序都使用同一存储过程，则所使用的代码都是相同的。 

     这一点的延伸就是防止错误。需要执行的步骤越多，出错的可能性就越大。防止错误保证了数据的一致性。

   - 简化对变动的管理。如果表名、列名或业务逻辑（或别的内容）有变化，那么只需要更改存储过程的代码。使用它的人员甚至不需要知道这些变化。 

     这一点的延伸就是安全性。通过存储过程限制对基础数据的访问，减少了数据讹误（无意识的或别的原因所导致的数据讹误）的机会。 

   - 提高性能。因为使用存储过程比使用单独的SQL语句要快。

   - 存在一些只能用在单个请求中的 SQL 元素和特性，存储过程可以使用它们来编写功能更强更灵活的代码。

3. 换句话说，使用存储过程有3个主要的好处，即简单、安全、高性能。 显然，它们都很重要。不过，在将SQL代码转换为存储过程前，也必须知道它的一些缺陷。

   - 一般来说，存储过程的编写比基本SQL语句复杂，编写存储过程需要更高的技能，更丰富的经验。
   - 你可能没有创建存储过程的安全访问权限。许多数据库管理员限制存储过程的创建权限，允许用户使用存储过程，但不允许他们 创建存储过程。

4. 创建存储过程（使用 CREATE PROCEDURE 关键字）

   ````mysql
   # ()中接收参数，即使不接收参数也需要显式声明(),BEGIN和END语句用来限定存储过程体
   CREATE PROCEDURE 存储过程名称()	
   BEGIN
       SQL 语句;
   END;
   ````

   实例：

   `````mysql
   # 实例（在SQLyog中创建不成功）：
   CREATE PROCEDURE productpricing()	
   BEGIN
       SELECT Avg(prod_price) AS priceaverage
       FROM products;
   END;
   
   # 在 mysql 命令行需要使用以下的格式（该格式在SQLyog中创建成功）：
   DELIMITER //
   CREATE PROCEDURE productpricing()	
   BEGIN
       SELECT Avg(prod_price) AS priceaverage
       FROM products;
   END //
   DELIMITER ;
   `````

5. 使用存储过程（使用CALL 关键字）

   `````mysql
   CALL productpricing();
   
   /* 输出结果
   priceaverage
   16.133571
   `````

6. 删除存储过程（使用DROP PROCEDURE 关键字）

   `````mysql
   DROP PROCEDURE productpricing;
   # 需要注意的是，使用 DROP PROCEDURE 关键字，必须是存储过程存在的情况下，否则会产生错误。当过程存在想删除它时（如果过程不存在也不产生错误）可使用
   DROP PROCEDURE IF EXISTS productpricing;
   `````

7. 存储过程使用参数

   - 一般，存储过程并不显示结果，而是把结果返回给你指定的变量。
   - 变量：内存中一个特定的位置，用来临时存储数据。

   

   - 实例1：

   `````mysql
   # 以下为 productpricing 的修改版本，（如果不先删除此存储过程，则不能再次创建它）
   DELIMITER //
   CREATE PROCEDURE productpricing(
   	OUT pricelow DECIMAL(8,2),
   	OUT pricehigh DECIMAL(8,2),
   	OUT priceavg DECIMAL(8,2)
   )
   BEGIN
   	SELECT MIN(prod_price)
   	INTO pricelow
   	FROM products;
   	SELECT MAX(prod_price)
   	INTO pricehigh
   	FROM products;
   	SELECT AVG(prod_price)
   	INTO priceavg
   	FROM products;
   END //
   DELIMITER ;
   
   /* 分析：
   此存储过程接受3个参数：pl存储产品最低价格，ph存储产品最高价格，pa存储产品平均价格。每个参数必须具有指定的类型，这里使用十进制值。关键字OUT指出相应的参数用来从存储过程传出一个值（返回给调用者）。MySQL支持IN（传递给存储过程）、OUT（从存储过程传出，如这里所用）和INOUT（对存储过程传入和传出）类型的参数。存储过程的代码位于BEGIN和END语句内，如前所见，它们是一系列SELECT语句，用来检索值，然后保存到相应的变量（通过指定INTO关键字）。
   */
   
   # 使用带变量的存储过程
   CALL productpricing(@pricelow,
   		@pricehigh,
   		@priceavg);
   # 检索产品平均价格
   SELECT @priceavg;
   # 获取存储过程的三个变量
   SELECT @pricelow,@pricehigh,@priceavg
   `````

   

   - 实例2：

   ````mysql
   # 存储过程案例2 使用IN 和OUT 参数
   DELIMITER //
   CREATE PROCEDURE ordertotal(
   	IN onumber  INT,
   	OUT ototal DECIMAL(8,2)
   )
   BEGIN	
   	SELECT SUM(item_price*quantity)
   	FROM orderitems
   	WHERE order_num = onumber
   	INTO ototal;
   END //
   DELIMITER ;
   
   /* 分析：
   onumber定义为IN，因为订单号被传入存储过程。ototal定义为OUT，因为要从存储过程返回合计。SELECT语句使用这两个参数，WHERE子句使用onumber选择正确的行，INTO使用ototal存储计算出来的合计。
   */
   
   # 调用该存储过程
   CALL ordertotal(20005, @total)
   /* 分析：
   必须给ordertotal传递两个参数；第一个参数为订单号，第二个参数为包含计算出来的合计的变量名。
   */
   
   # 显示统计
   SELECT @total
   
   
   # 查看另外的订单合计显示
   CALL ordertotal(20009, @total);
   SELECT @total;
   ````

8. 建立智能存储过程

   例如：你需要获得与以前一样的订单合计，但需要对合计增加营业税，不过只针对某些顾客（或许是你所在州中那些顾客）。那么，你需要做下面几件事情：

   - 获得合计
   - 把营业税有条件的添加到合计
   - 返回合计（带或不带税）

   ````mysql
   ## 定义智能的存储过程
   -- Name: ordertotal
   -- Parameters: onumber = order_number
   -- 		taxable = 0 if not taxable, 1 if taxable
   -- 		ototal = order total variable
   DELIMITER //
   CREATE PROCEDURE ordertotal(
   	IN onumber INT,
   	IN taxable BOOLEAN,
   	OUT ototal DECIMAL(8,2)
   	) COMMENT 'Obtain order total, optionally adding tax'
   BEGIN
   	-- Declare variable for total
   	DECLARE total DECIMAL(8,2);
   	-- Declare tax percentage
   	DECLARE taxrate INT DEFAULT 6;
   	
   	-- Get the order total
   	SELECT SUM(item_price*quantity)
   	FROM orderitems
   	WHERE order_num = onumber
   	INTO total;
   	
   	-- Is this taxable?
   	IF taxable THEN
   		-- Yes, so add taxrate to the total
   		SELECT total+(total/100*taxrate) INTO total;
   	END IF;
   		-- And finally, save to out variable
   		SELECT total INTO ototal;
   END //
   DELIMITER ;
   
   # 不增加营业税
   CALL ordertotal(20005, 0, @total);
   SELECT @total
   # 增加营业税
   CALL ordertotal(20005, 1, @total);
   SELECT @total
   
   /* 分析：
   BOOLEAN值指定为1表示真，指定为0表示假（实际上，非零值都考虑为真，只有0被视为假）。通过给中间的参数指定0或1，可以有条件地将营业税加到订单合计上。
   ````

   - IF 语句：这个例子给出了MySQL的IF语句的基本用法。IF语句还支持ELSEIF和ELSE子句（前者还使用THEN子句，后者不使用）。在以后章节中我们将会看到IF的其他用法（以及其他流控制语句）。

9. 检查存储过程

   - 为了显示用来创建一个存储过程的CREATE语句，可以使用 SHOW CREATE PROCEDURE 语句。
   - 为了获得包括何时、由谁创建等详细信息的存储过程列表，使用SHOW PROCEDURE STATUS。

   ``````mysql
   # 显示创建存储过程的CREATE语句
   SHOW CREATE PROCEDURE ordertotal;
   
   # 查看所有存储过程的信息
   SHOW PROCEDURE STATUS
   
   # 使用 Like 关键字 只查询'ordertotal'这个存储过程的信息
   SHOW PROCEDURE STATUS LIKE 'ordertotal';
   ``````

   



## 22、使用游标

1. 游标（cursor）是一个存储在MySQL服务器上的数据库查询，它不是一条SELECT语句，而是被该语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。游标主要用于交互式应用，其中用户需要滚动屏幕上的数据，并对数据进行浏览或做出更改。

2. MySQL的游标只能用于存储过程和函数。

3. 使用游标的步骤：

   - 在能够使用游标前，必须声明（定义）它。这个过程实际上没有检索数据，它只是定义要使用的SELECT语句。 
   - 一旦声明后，必须打开游标以供使用。这个过程用前面定义的 SELECT语句把数据实际检索出来。 
   - 对于填有数据的游标，根据需要取出（检索）各行。 
   - 在结束游标使用时，必须关闭游标。
   
4. 创建游标

   游标用DECLARE语句创建，DECLARE命名游标，并定义相应的SELECT语句，根据需要带WHERE和其他子句。

   实例：

   ````mysql
   # 定义ordernumbers 的游标，可以检索所有订单的 SELECT 语句
   DELIMITER //
   CREATE PROCEDURE processorders()
   BEGIN
   	DECLARE ordernumbers CURSOR
   	FOR 
   	SELECT order_num FROM orders;
   END //
   DELIMITER ;
   ````

5. 打开和关闭游标

   ````mysql
   # 打开游标
   OPEN ordernumbers;
   # 关闭游标
   CLOSE ordernumbers;
   
   /* 分析
   1. CLOSE释放游标使用的所有内部内存和资源，因此在每个游标不再需要时都应该关闭。
   2. 在一个游标关闭后，如果没有重新打开，则不能使用它。但是，使用声明过的游标不需要再次声明，用OPEN语句打开它就可以了。
   3. 如果你不明确关闭游标，MySQL将会在到达END语句时自动关闭它。
   */
   
   # 前面例子的修改版本，将游标的打开和关闭声明在存储过程中，对于检索的数据，没有做任何操作
   DELIMITER //
   CREATE PROCEDURE processorders()
   BEGIN
   	-- Declare the cursor
   	DECLARE ordernumbers CURSOR
   	FOR 
   	SELECT order_num FROM orders;
   	
   	-- Open the cursor
   	OPEN ordernumbers;
   	
   	-- Close the cursor
   	CLOSE ordernumbers;
   	
   END //
   DELIMITER ;
   ````

6. 使用游标数据

   在一个游标被打开后，可以使用FETCH语句分别访问它的每一行。FETCH指定检索什么数据（所需的列），检索出来的数据存储在什么地方。它还向前移动游标中的内部行指针，使下一条FETCH语句检索下一行（不重复读取同一行）。

   实例：

   `````mysql
   # 实例1：使用 FETCH 检索单行（第一行）
   DELIMITER //
   CREATE PROCEDURE processorders()
   BEGIN
   	-- Declare local variables
   	DECLARE o INT;
   	
   	-- Declare the cursor
   	DECLARE ordernumbers CURSOR
   	FOR 
   	SELECT order_num FROM orders;
   	
   	-- Open the cursor
   	OPEN ordernumbers;
   	
   	-- Get order number
   	FETCH ordernumbers INTO o;
   	
   	-- Close the cursor
   	CLOSE ordernumbers;
   	
   END //
   DELIMITER ;
   /* 分析：
   其中FETCH用来检索当前行的order_num列（将自动从第一行开始）到一个名为o的局部声明的变量中。对检索出的数据不做任何处理。
   */
   
   # 实例2
   DELIMITER //
   CREATE PROCEDURE processorders()
   BEGIN
   	-- Declare local variables
   	DECLARE done BOOLEAN DEFAULT 0;
   	DECLARE o INT;
   	
   	-- Declare the cursor
   	DECLARE ordernumbers CURSOR
   	FOR 
   	SELECT order_num FROM orders;
   	-- Declare continue handler
   	DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;
   	
   	-- Open the cursor
   	OPEN ordernumbers;
   	
   	-- Loop through all rows
   	REPEAT
   		-- Get order number
   		FETCH ordernumbers INTO o;
   		
   	-- End of loop
   	UNTIL done END REPEAT;
   	
   	-- Close the cursor
   	CLOSE ordernumbers;
   	
   END //
   DELIMITER ;
   
   /* 分析：
   与前一个例子一样，这个例子使用FETCH检索当前order_num到声明的名为o的变量中。但与前一个例子不一样的是，这个例子中的FETCH是在REPEAT内，因此它反复执行直到done为真（由UNTILdone END REPEAT;规定）。为使它起作用，用一个DEFAULT 0（假，不结束）定义变量done。那么，done怎样才能在结束时被设置为真呢？答案
   是用以下语句：
   	DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;
   这条语句定义了一个CONTINUE HANDLER，它是在条件出现时被执行的代码。这里，它指出当SQLSTATE '02000'出现时，SET done=1。SQLSTATE'02000'是一个未找到条件，当REPEAT由于没有更多的行供循环而不能继续时，出现这个条件。
   
   如果调用这个存储过程，它将定义几个变量和一个CONTINUE HANDLER，定义并打开一个游标，重复读取所有行，然后关闭游标。
   `````

   - DECLARE 语句的次序：DECLARE语句的发布存在特定的次序。用DECLARE语句定义的局部变量必须在定义任意游标或句柄之前定义，而句柄必须在游标之后定义。不遵守此顺序将产生错误消息。
   - [MySQL的错误代码列表](http://dev.mysql.com/doc/mysql/en/error-handling.html)

   实例3：

   `````mysql
   DELIMITER //
   CREATE PROCEDURE processorders()
   BEGIN
   	-- Declare local variables
   	DECLARE done BOOLEAN DEFAULT 0;
   	DECLARE o INT;
   	DECLARE t DECIMAL(8,2);
   	
   	-- Declare the cursor
   	DECLARE ordernumbers CURSOR
   	FOR 
   	SELECT order_num FROM orders;
   	-- Declare continue handler
   	DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;
   	
   	-- Create a table to store the results
   	CREATE TABLE IF NOT EXISTS ordertotals
   	(order_num INT, total DECIMAL(8,2));
   	
   	-- Open the cursor
   	OPEN ordernumbers;
   	
   	-- Loop through all rows
   	REPEAT
   		-- Get order number
   		FETCH ordernumbers INTO o;
   		
   		-- Get the total for this order
   		CALL ordertotal(o,1,t);
   		
   		-- Insert order and total into ordertotals
   		INSERT INTO ordertotals(order_num, total)
   		VALUES(o, t);
   		
   	-- End of loop
   	UNTIL done END REPEAT;
   	
   	-- Close the cursor
   	CLOSE ordernumbers;
   	
   END //
   DELIMITER ;
   
   /* 分析：
   在这个例子中，我们增加了另一个名为t的变量（存储每个订单的合计）。此存储过程还在运行中创建了一个新表（如果它不存在的话），名为ordertotals。这个表将保存存储过程生成的结果。FETCH像以前一样取每个order_num，然后用CALL执行另一个存储过程（我们在前一章中创建）来计算每个订单的带税的合计（结果存储到t）。最后，
   用INSERT保存每个订单的订单号和合计。
   此存储过程不返回数据，但它能够创建和填充另一个表，可以用一条简单的SELECT语句查看该表：
   */
   SELECT *
   FROM ordertotals;
   /* 输出
   order_num 	total
   20005		158.86
   20006		58.30
   20007		1060.00
   20008		132.50
   20009		40.78
   */
   `````

   

## 23、触发器

1. 触发器：触发器是特殊的存储过程，它在特定的数据库活动发生时自动执行。

2. 触发器是MySQL响应以下任意语句而 自动执行的一条MySQL语句（或位于BEGIN和END语句之间的一组语 句）：

   - DELETE
   - INSERT
   - UPDATE
   - 其他MySQL语句不支持触发器

3. 与存储过程不一样（存储过程只是简单的存储 SQL 语句），触发器与单个的表相关联。与 Orders 表上的 INSERT 操作相关联的触发器只在 Orders 表中插入行时执行。类似地，Customers 表上的 INSERT 和 UPDATE 操作的触发器只在表上出现这些操作时执行。

4. 触发器的常见用途：

   - 保证数据一致。例如，在 INSERT 或 UPDATE 操作中将所有州名转换 为大写。 
   - 基于某个表的变动在其他表上执行活动。例如，每当更新或删除一行时将审计跟踪记录写入某个日志表。
   - 进行额外的验证并根据需要回退数据。例如，保证某个顾客的可用资 金不超限定，如果已经超出，则阻塞插入。 
   - 计算计算列的值或更新时间戳。

5. 创建触发器

   在创建触发器时，需要给出4条信息：

   - 唯一的触发器名； 
   - 触发器关联的表； 
   - 触发器应该响应的活动（DELETE、INSERT或UPDATE）； 
   - 触发器何时执行（处理之前或之后）。

   实例：

   `````mysql
   # 创建触发器
   CREATE TRIGGER newproduct AFTER INSERT ON products
   FOR EACH ROW SELECT 'Product added';
   
   /* 
   为了测试这个触发器，使用INSERT语句添加一行或多行到products中，你将看到对每个成功的插入，显示Product added消息。
   */
   `````

   - 触发器仅支持表：只有表才支持触发器，视图不支持（临时表也不支持）。
   - 触发器按每个表每个事件每次地定义，每个表每个事件每次只允许一个触发器。因此，每个表最多支持6个触发器（每条INSERT、UPDATE 和DELETE的之前和之后）。
   - 单一触发器不能与多个事件或多个表关联，所以，如果你需要一个对INSERT和UPDATE操作执行的触发器，则应该定义两个触发器。
   - 如果BEFORE触发器失败，则MySQL将不执行请求的操作。此外，如果BEFORE触发器或语句本身失败，MySQL 将不执行AFTER触发器（如果有的话）。

6. 删除触发器：

   ````mysql
   DROP TRIGGER newproduct;
   ````

   - 触发器不能更新或覆盖。为了修改一个触发器，必须先删除它， 然后再重新创建。

7. INSERT 触发器：

   INSERT触发器在INSERT语句执行之前或之后执行。需要知道以下几点：

   - 在INSERT触发器代码内，可引用一个名为NEW的虚拟表，访问被插入的行； 
   - 在BEFORE INSERT触发器中，NEW中的值也可以被更新（允许更改被插入的值）； 
   - 对于AUTO_INCREMENT列，NEW在INSERT执行之前包含0，在INSERT 执行之后包含新的自动生成值。

   案例：

   ````mysql
   # INSERT 触发器
   CREATE TRIGGER neworder AFTER INSERT ON orders
   FOR EACH ROW SELECT New.order_num INTO @ee;
   
   # 测试INSERT 触发器
   INSERT INTO orders(order_date, cust_id)
   VALUES(NOW(), 10001);
   # 查看结果
   SELECT @ee AS order_num
   ````

8. DELETE触发器：

   DELETE触发器在DELETE语句执行之前或之后执行。需要知道以下两点：

   - 在DELETE触发器代码内，你可以引用一个名为OLD的虚拟表，访问被删除的行；
   - OLD中的值全都是只读的，不能更新。

   案例：

   `````mysql
   # 创建 DELETE 触发器
   DELIMITER //
   CREATE TRIGGER deleteorder BEFORE DELETE ON orders
   FOR EACH ROW
   BEGIN
   	INSERT INTO archive_orders(order_num,order_date,cust_id)
   	VALUES(OLD.order_num,OLD.order_date,OLD.cust_id);
   END //
   DELIMITER ;
   
   /* 分析：
   在任意订单被删除前将执行此触发器。它使用一条INSERT语句将OLD中的值（要被删除的订单）保存到一个名为archive_orders的存档表中（为实际使用这个例子，你需要用与orders相同的列创建一个名为archive_orders的表）。
   `````

9. UPDATE触发器：

   UPDATE触发器在UPDATE语句执行之前或之后执行。需要知道以下几点：

   - 在UPDATE触发器代码中，你可以引用一个名为OLD的虚拟表访问以前（UPDATE语句前）的值，引用一个名为NEW的虚拟表访问新 更新的值； 
   - 在BEFORE UPDATE触发器中，NEW中的值可能也被更新（允许更改将要用于UPDATE语句中的值）； 
   - OLD中的值全都是只读的，不能更新。

   实例：

   `````mysql
   # UPDATE 触发器
   CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors
   FOR EACH ROW SET NEW.vend_state = UPPER(NEW.vend_state);
   `````

   

10. BEFORE 或 AFTER：对于 Insert 和 Update 触发器，将BEFORE用于数据验证和净化（目的是保证插入表中的数据确实是需要的数据）。

11. 需要注意的是：约束的处理比触发器更快，因此在可能的时候，应该尽量使用约束。

12. 些使用触发器时需要记住的重点：

    - 与其他DBMS相比，MySQL 5中支持的触发器相当初级。未来的 MySQL版本中有一些改进和增强触发器支持的计划。 
    - 创建触发器可能需要特殊的安全访问权限，但是，触发器的执行是自动的。如果INSERT、UPDATE或DELETE语句能够执行，则相关的触发器也能执行。 
    - 应该用触发器来保证数据的一致性（大小写、格式等）。在触发器中执行这种类型的处理的优点是它总是进行这种处理，而且是透 明地进行，与客户机应用无关。 
    - 触发器的一种非常有意义的使用是创建审计跟踪。使用触发器， 把更改（如果需要，甚至还有之前和之后的状态）记录到另一个 表非常容易。 
    - 遗憾的是，MySQL触发器中不支持CALL语句。这表示不能从触发器内调用存储过程。所需的存储过程代码需要复制到触发器内。



## 24、事务处理

### 24.1、事务处理

1. MyISAM引擎不支持明确的事务管理，而InnoDB引擎支持事务管理。

2. 使用事务管理：事务管理是一种机制，其目的是通过确保成批的 SQL 操作要么完全执行，要么完全不执行，来维护数据库的完整性。

3. 例如给系统添加订单包含以下几个过程：

   (1) 检查数据库中是否存在相应的顾客，如果不存在，添加他；

   (2) 检索顾客的 ID；

   (3) 在 Orders 表添加一行，它与顾客 ID 相关联；

   (4) 检索 Orders 表中赋予的新订单 ID； 

   (5) 为订购的每个物品在 OrderItems 表中添加一行，通过检索出来的 ID 把它与 Orders 表关联（并且通过产品 ID 与 Products 表关联）。

   在使用事务管理时，如果以上的某个过程出现故障，则回退所有添加的数据。

4. 事务处理的术语：

   - 事务（transaction）指一组 SQL 语句； 
   - 回退（rollback）指撤销指定 SQL 语句的过程； 
   - 提交（commit）指将未存储的 SQL 语句结果写入数据库表； 
   - 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），可以对它发布回退（与回退整个事务处理不同）。

5. 事务管理可以管理INSERT、UPDATE和DELETE语句，不能回退SELECT语句（无意义）、CREATE 和DROP操作。

### 24.2、控制事务处理

1. MySQL中的事务处理语句：

   `````mysql
   START TRANSACTION
   ..... # 表示具体SQL语句
   `````

2. 使用ROLLBACK（回退）来撤销SQL语句

   ````mysql
   # 使用ROLLBACK 撤销MySQL语句
   SELECT * FROM ordertotals;
   INSERT INTO ordertotals(
   	order_num,total)
   	VALUES(20010, 145);
   START TRANSACTION;
   DELETE FROM ordertotals;
   SELECT * FROM ordertotals;
   ROLLBACK;
   SELECT * FROM ordertotals;
   ````

3. 使用Commit语句将事务写入数据库：

   一般的MySQL语句都是直接针对数据库表执行和编写的。这就是所谓的隐含提交（implicit commit），即提交（写或保存）操作是自动 进行的。

   但是，在事务处理块中，提交不会隐含地进行。为进行明确的提交， 使用COMMIT语句，如下所示：

   ````mysql
   # 删除涉及到更新两个数据库表 Orders 和 OrderItems，所以使用事务处理块来保证订单不被部分删除。最后的 COMMIT 语句仅在不出错时写出更改。如果第一条 DELETE 起作用，但第二条失败，则 DELETE 不会提交。
   START TRANSACTION
   DELETE OrderItems WHERE order_num = 12345;
   DELETE Orders WHERE order_num = 12345;
   COMMIT; 
   ````

4. 隐含事务关闭：当COMMIT或ROLLBACK语句执行后，事务会自动关闭（将来的更改会隐含提交）。

5. 使用保留点

   `````mysql
   # 保留点
   SAVEPOINT delete1
   # 回退到保留点
   ROLLBCK TO delete1
   `````

   - 保留点越多越好：可以在MySQL代码中设置任意多的保留点，越多越好。为什么呢？因为保留点越多，你就越能按自己的意愿灵活地进行回退。
   - 释放保留点保留点在事务处理完成（执行一条ROLLBACK或 COMMIT）后自动释放。自MySQL 5以来，也可以用RELEASE SAVEPOINT明确地释放保留点。

6. 更改默认的提交行为：

   通常情况下：执行一条MySQL语句，该语句实际上都是针对表执行的，而且所做的更改立即生效。为指示MySQL不自动提交更改，需要使用以下语句：

   `````mysql
   SET autocommit = 0;
   /*
   autocommit标志决定是否自动提交更改，不管有没有COMMIT语句。设置autocommit为0（假）指示MySQL不自动提交更改（直到autocommit被设置为真为止）
   `````

   - 标志为连接专用：autocommit标志是针对每个连接而不是服务器的。

## 25、字符集和校对顺序

1. 字符集：为字母和符号的集合

2. 编码：为某个字符集成员的内部表示

3. 校对：为规定字符如何比较的指令

4. MySQL支持众多的字符集。为查看所支持的字符集完整列表，使用以下语句：

   `````mysql
   # 查看字符集
   SHOW CHARACTER SET;
   /* 结果输出为 41 行的字符集*/
   `````

5. 为了查看所支持校对的完整列表，使用以下语句：

   ````mysql
   # 查看校对规则
   SHOW COLLATION;
   /* 结果输出为 222 行的 校对规则*/
   ````

6. 通常系统管理在安装时定义一个默认的字符集和校对。此外，也可 以在创建数据库时，指定默认的字符集和校对。为了确定所用的字符集和校对，可以使用以下语句：

   `````mysql
   SHOW VARIABLES LIKE 'character%';
   SHOW VARIABLES LIKE 'collation%';
   `````

7. 给表指定字符集和校对，一般是在使用CREATE TABLE创建表的时候指定。

   ![image-20220630142401971](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220630142401971.png)

   ````mysql
   # 给表指定字符集和校对规则
   CREATE TABLE mytalbe
   (
   	column1	INT,
   	column2	VARCHAR(10)
   ) DEFAULT CHARACTER SET hebrew
   COLLATE hebre_qeneral_ci;
   ````

8. 除了能指定字符集和校对的表范围外，MySQL还允许对每个列设置它们，如下所示：

   `````mysql
   CREATE TABLE mytalbe
   (
   	column1	INT,
   	column2	VARCHAR(10)
   	column3 VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_general_ci
   ) DEFAULT CHARACTER SET hebrew
   COLLATE hebre_qeneral_ci;
   `````

9. 在ORDER BY 子句中使用校对规则

   通常是为了在不区分大小写的表上进行临时区分大小写的搜索的（反过来也一样），作用范围只在该条 检索语句中。

   ````mysql
   SELECT * FROM customers
   ORDER BY lastname, firstname COLLATE latin1_general_cs;
   ````

10. 校对规则除了能在 ORDER BY子 句中使用以外，还可以用于GROUP BY、HAVING、聚集 函数、别名等。

11. 如果需要串可以在字符集之间进行转换。可以使用Cast()或Convert()函数。

12. 字符集和校对规则说明：

    - 如果不指定字符集，默认为utf8
    - 常用的校验规则有：utf8_bin[区分大小写] 和 utf8_general_ci[不区分大小写]，如果不指定，默认utf8_general_ci，不区分大小写

    

## 26、安全管理和访问控制

1. MySQL服务器的安全基础是：用户应该对他们需要的数据具有适当的访问权，既不能多也不能少。换句话说，用户不能对过多的数据具有过多的访问权。

   - 多数用户只需要对表进行读和写，但少数用户甚至需要能创建和 删除表； 
   - 某些用户需要读表，但可能不需要更新表； 
   - 你可能想允许用户添加数据，但不允许他们删除数据；
   - 某些用户（管理员）可能需要处理用户账号的权限，但多数用户不需要；
   - 你可能想让用户通过存储过程访问数据，但不允许他们直接访问数据； 
   - 你可能想根据用户登录的地点限制对某些功能的访问。 

2. MySQL用户账号和信息存储在名为mysql的MySQL数据库中。mysql数据库有一个名为user的表，它包含所有用户账号。user 表有一个名为user的列，它存储用户登录名。新安装的服务器可能只有一个用户（如这里所示），过去建立的服务器可能具有很多用户。如果需要访问用户列表可以使用以下代码：

   `````mysql
   USE mysql;
   SELECT user FROM user;
   `````

3. 创建用户账号：

   使用CREATE USER 语句：

   `````mysql
   CREATE USER ben IDENTIFIED BY 'p@$$wOrd';
   # 该用户名 为： ben，密码为：'p@$$wOrd'
   /*
   IDENTIFIED BY指定的口令为纯文本，MySQL将在保存到user表之前对其进行加密。为了作为散列值指定口令，使用IDENTIFIED BY PASSWORD
   */
   CREATE USER ben IDENTIFIED BY PASSWORD 'p@$$wOrd';
   `````

4. 重命名用户账号：

   使用RENAME USER语句：

   ````mysql
   RENAME USER ben TO bforta;
   ````

5. 删除用户账号：

   使用DROP USER 语句：

   `````mysql
   DROP USER bforta;
   `````

6. 查看用户账号权限：

   使用SHOW GRANTS FOR语句

   `````mysql
   SHOW GRANTS FOR bforta;
   `````

7. 使用GRANT 语句设置权限：

   - 要授予的权限； 
   - 被授予访问权限的数据库或表；
   - 用户名。

   实例：

   `````mysql
   GRANT SELECT ON crashcourse.* TO bforta;
   
   /* 分析：
   此GRANT允许用户在crashcourse.*（crashcourse数据库的所有表）上使用SELECT。通过只授予SELECT访问权限，用户bforta对crashcourse数据库中的所有数据具有只读访问权限。
   */
   # 可以使用 SHOW GRANTS 反应这个更改
   SHOW GRANTS FOR bforta;
   `````

8. 使用REVOKE来撤销特定的权限：

   实例：

   `````mysql
   REVOKE SELECT ON crashcourse.* FROM bforta;
   /* 分析：
   这条REVOKE语句取消刚赋予用户bforta的SELECT访问权限。被撤销的访问权限必须存在，否则会出错。
   */
   `````

9. GRANT和REVOKE可在几个层次上控制访问权限：

   - 整个服务器，使用GRANT ALL和REVOKE ALL； 
   - 整个数据库，使用ON database.*；
   - 特定的表，使用ON database.table； 
   - 特定的列； 
   - 特定的存储过程。

10. 可以授予和撤销的权限：

    ![QQ图片20220630162704](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\QQ图片20220630162704.jpg)

    

11. 同时授权多个权限，中间使用逗号分隔。

    实例：

    `````mysql
    GRANT SELECT, INSERT ON crashcourse.* TO bforta;
    `````

12. 更改用户密码：

    使用SET PASSWORD语句：

    `````mysql
    SET PASSWORD FOR bforta = Password('n3w p@$$wOrd');
    # SET PASSWORD更新用户口令。新口令必须传递到Password()函数进行加密。
    # 修改当前用户登录密码
    SET PASSWORD = Password('n3w p@$$wOrd');
    `````

    

## 27、数据库维护

### 27.1、备份数据

1. 几种备份数据的方式：
   - 使用命令行实用程序mysqldump转储所有数据库内容到某个外部文件。在进行常规备份前这个实用程序应该正常运行，以便能正确地备份转储文件。 
   - 可用命令行实用程序mysqlhotcopy从一个数据库复制所有数据（并非所有数据库引擎都支持这个实用程序）。 
   - 可以使用MySQL的BACKUP TABLE或SELECT INTO OUTFILE转储所有数据到某个外部文件。这两条语句都接受将要创建的系统文件名，此系统文件必须不存在，否则会出错。数据可以用RESTORE TABLE来复原。
2. 为了保证所有数据被写到磁盘（包括索引数据），可能需要在进行备份前使用FLUSH TABLES语句，来刷新未写的数据。

### 27.2、进行数据库维护

1. MySQL提供了一系列的语句，可以（应该）用来保证数据库正确和正常运行。

   - ANALYZE TABLE，用来检查表键是否正确。ANALYZE TABLE返回如下所示的状态信息：

     `````mysql
     # 查看表的键是否正确
     ANALYZE TABLE orders;
     /* 输出：
     Table					Op		Msg_type	Msg_text
     book_test_mysql.orders	analyze		status		OK
     `````

   - CHECK TABLE用来针对许多问题对表进行检查。在MyISAM表上还对索引进行检查。CHECK TABLE支持一系列的用于MyISAM表的方式。 CHANGED检查自最后一次检查以来改动过的表。EXTENDED执行最彻底的检查，FAST只检查未正常关闭的表，MEDIUM检查所有被删除的链接并进行键检验，QUICK只进行快速扫描。

     `````mysql
     # 对表进行检查
     CHECK TABLE oreders, orderitems;
     `````

   - 如果MyISAM表访问产生不正确和不一致的结果，可能需要用 REPAIR TABLE来修复相应的表。这条语句不应该经常使用，如果需要经常使用，可能会有更大的问题要解决。

   - 如果从一个表中删除大量数据，应该使用OPTIMIZE TABLE来收回所用的空间，从而优化表的性能。

### 27.3、诊断启动问题

1. 在排除系统启动问题时，首先应该尽量用手动启动服务器。MySQL 服务器自身通过在命令行上执行mysqld启动。下面是几个重要的mysqld 命令行选项：

   `````mysql
   --help				# 显示帮助——一个选项列表
   --safe-mode			# 装载减去某些最佳配置的服务器
   --verbose			# 显示全文本消息（为获得更详细的帮助消息与--help联合使用）
   --version			# 显示版本信息然后退出。
   `````

### 27.4、查看日志文件

1. MySQL维护管理员依赖的一系列日志文件。主要的日志文件有以下几种：
   - 错误日志。它包含启动和关闭问题以及任意关键错误的细节。此日志通常名为hostname.err，位于data目录中。此日志名可用 `--log-error` 命令行选项更改。 
   - 查询日志。它记录所有MySQL活动，在诊断问题时非常有用。此日志文件可能会很快地变得非常大，因此不应该长期使用它。此日志通常名为hostname.log，位于data目录中。此名字可以用 `--log` 命令行选项更改。 
   - 二进制日志。它记录更新过数据（或者可能更新过数据）的所有语句。此日志通常名为hostname-bin，位于data目录内。此名字 可以用`--log-bin`命令行选项更改。注意，这个日志文件是MySQL5中添加的，以前的MySQL版本中使用的是更新日志。 
   - 缓慢查询日志。顾名思义，此日志记录执行缓慢的任何查询。这个日志在确定数据库何处需要优化很有用。此日志通常名为 hostname-slow.log ，位于 data 目录中。此名字可以用 `--log-slow-queries`命令行选项更改。

## 28、改善性能

1. 关于性能探讨的一些点：
   - 首先，MySQL（与所有DBMS一样）具有特定的硬件建议。在学习和研究MySQL时，使用任何旧的计算机作为服务器都可以。但对用于生产的服务器来说，应该坚持遵循这些硬件建议。
   - 一般来说，关键的生产DBMS应该运行在自己的专用服务器上。
   - MySQL是用一系列的默认设置预先配置的，从这些设置开始通常是很好的。但过一段时间后你可能需要调整内存分配、缓冲区大小等。（为查看当前设置，可使用`SHOW VARIABLES;`和`SHOW STATUS;`）
   - MySQL是一个多用户多线程的DBMS，换言之，它经常同时执行多个任务。如果这些任务中的某一个执行缓慢，则所有请求都会执行缓慢。如果你遇到显著的性能不良，可使用`SHOW PROCESSLIST` 显示所有活动进程（以及它们的线程ID和执行时间）。你还可以用`KILL`命令终结某个特定的进程（使用这个命令需要作为管理员登录）。
   - 总是有不止一种方法编写同一条SELECT语句。应该试验联结、并、子查询等，找出最佳的方法。
   - 使用`EXPLAIN`语句让MySQL解释它将如何执行一条SELECT语句。
   - 一般来说，存储过程执行得比一条一条地执行其中的各条MySQL 语句快。
   - 应该总是使用正确的数据类型。
   - 决不要检索比需求还要多的数据。换言之，不要用SELECT *（除 非你真正需要每个列）。
   - 有的操作（包括INSERT）支持一个可选的DELAYED关键字，如果使用它，将把控制立即返回给调用程序，并且一旦有可能就实际执行该操作。
   - 在导入数据时，应该关闭自动提交。你可能还想删除索引（包括 FULLTEXT索引），然后在导入完成后再重建它们。
   - 必须索引数据库表以改善数据检索的性能。确定索引什么不是一件微不足道的任务，需要分析使用的SELECT语句以找出重复的 WHERE和ORDER BY子句。如果一个简单的WHERE子句返回结果所花的时间太长，则可以断定其中使用的列（或几个列）就是需要索引的对象。
   - 你的SELECT语句中有一系列复杂的OR条件吗？通过使用多条 SELECT语句和连接它们的UNION语句，你能看到极大的性能改进。
   - 索引改善数据检索的性能，但损害数据插入、删除和更新的性能。 如果你有一些表，它们收集数据且不经常被搜索，则在有必要之前不要索引它们。（索引可根据需要添加和删除。）
   - LIKE很慢。一般来说，最好是使用FULLTEXT而不是LIKE。
   - 数据库是不断变化的实体。一组优化良好的表一会儿后可能就面目全非了。由于表的使用和内容的更改，理想的优化和配置也会改变。
   - 最重要的规则就是，每条规则在某些条件下都会被打破。
2. [MySQL浏览文档](http://dev.mysql.com/doc/)



## 29、高级SQL特性

### 29.1、约束

1. **约束**（constraint）：管理如何插入或处理数据库数据的规则。

2. **主键**：主键是一种特殊的约束，用来保证一列（或 一组列）中的值是唯一的，而且永不改动。换句话说，表中的一列（或多个列）的值唯一标识表中的每一行。

3. 满足主键的条件：

   - 任意两行的主键值都不相同。
   - 每行都具有一个主键值（即列中不允许 NULL 值）。 
   - 包含主键值的列从不修改或更新。（大多数 DBMS 不允许这么做，但 如果你使用的 DBMS 允许这样做，好吧，千万别！）
   - 主键值不能重用。如果从表中删除某一行，其主键值不分配给新行。

4. 指定主键（定义主键）：

   `````mysql
   # 1. 在定义表时指定
   CREATE TABLE Vendors
   (
        vend_id CHAR(10) NOT NULL PRIMARY KEY,
        vend_name CHAR(50) NOT NULL,
        vend_address CHAR(50) NULL,
        vend_city CHAR(50) NULL,
        vend_state CHAR(5) NULL,
        vend_zip CHAR(10) NULL,
        vend_country CHAR(50) NULL
   );
   # 2. 对给定表指定列为主键
   ALTER TABLE Vendors
   ADD CONSTRAINT PRIMARY KEY (vend_id);
   `````

5. **外键**：外键是表中的一列，其值必须列在另一表的主键中。外键是保证引用完 整性的极其重要部分。

6. 指定外键（定义外键）：

   `````mysql
   # 1. 创建表时指定外键
   CREATE TABLE Orders
   (
        order_num 	INTEGER	NOT NULL	PRIMARY KEY,
        order_date	DATETIME NOT NULL,
        cust_id	CHAR(10) NOT NULL REFERENCES Customers(cust_id)
   );
   # 2. 对存在的表使用CONSTRAINT 关键字指定
   ALTER TABLE Orders
   ADD CONSTRAINT
   FOREIGN KEY (cust_id) REFERENCES Customers (cust_id);
   `````

7. 使用外键除了能够保证引用完整性外，还可以防止意外删除。因为在定义外键后，DBMS不允许删除在另一个表中具有关联行的行。

8. **唯一约束**：唯一约束用来保证一列（或一组列）中的数据是唯一的。它们类似于主键，但存在以下重要区别：

   - 表可包含多个唯一约束，但每个表只允许一个主键。 
   - 唯一约束列可包含 NULL 值。 
   - 唯一约束列可修改或更新。
   - 唯一约束列的值可重复使用。
   - 与主键不一样，唯一约束不能用来定义外键。

9. 定义唯一约束：可以使用UNIQUE关键字在表定义中定义，也可以使用单独的CONSTRAINT定义

10. **检查约束**：检查约束用来保证一列（或一组列）中的数据满足一组指定的条件。检 查约束的常见用途有以下几点：

    - 检查最小或最大值。例如，防止 0 个物品的订单（即使 0 是合法的数）。 
    - 指定范围。例如，保证发货日期大于等于今天的日期，但不超过今天起一年后的日期。 
    - 只允许特定的值。例如，在性别字段中只允许 M 或 F。

11. 指定检查约束：

    ````mysql
    # 1. 在表定义时指定检查约束
    CREATE TABLE OrderItems
    (
         order_num INTEGER NOT NULL,
         order_item INTEGER NOT NULL,
         prod_id CHAR(10) NOT NULL,
         quantity INTEGER NOT NULL CHECK (quantity > 0),
         item_price MONEY NOT NULL
    );
    # 2. 使用 CONSTRAINT 关键字指定 检查约束
    ADD CONSTRAINT CHECK (gender LIKE '[MF]'); 
    ````

    

### 29.2、索引

1. 索引用来排序数据以加快搜索和排序操作的速度。

2. 创建索引时需要注意的事项：

   - 索引改善检索操作的性能，但降低了数据插入、修改和删除的性能。 在执行这些操作时，DBMS 必须动态地更新索引。 
   - 索引数据可能要占用大量的存储空间。
   - 并非所有数据都适合做索引。取值不多的数据（如州）不如具有更多可能值的数据（如姓或名），能通过索引得到那么多的好处。
   - 索引用于数据过滤和数据排序。如果你经常以某种特定的顺序排序数据，则该数据可能适合做索引。
   - 可以在索引中定义多个列（例如，州加上城市）。这样的索引仅在以州加城市的顺序排序时有用。如果想按城市排序，则这种索引没有用处。

3. 创建索引：

   ````mysql
   CREATE INDEX prod_name_ind
   ON Products (prod_name);
   # 索引必须唯一命名。这里的索引名 prod_name_ind 在关键字 CREATE INDEX 之后定义。ON 用来指定被索引的表，而索引中包含的列（此例中仅有一列）在表名后的圆括号中给出。
   ````

### 29.3、数据库安全

1. 常见的需要保护的操作：
   - 对数据库管理功能（创建表、更改或删除已存在的表等）的访问； 
   - 对特定数据库或表的访问； 
   - 访问的类型（只读、对特定列的访问等）； 
   - 仅通过视图或存储过程对表进行访问； 
   - 创建多层次的安全措施，从而允许多种基于登录的访问和控制；
   - 限制管理用户账号的能力。



## 30、MySQL数据类型

1. 串数据类型：

   ![image-20220630142054974](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220630142054974.png)

2. 数值数据类型

   ![image-20220630142134460](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220630142134460.png)

3. 日期和时间数据类型

   ![image-20220630142210405](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220630142210405.png)

4. 二进制数据类型

   ![image-20220630142230662](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220630142230662.png)



