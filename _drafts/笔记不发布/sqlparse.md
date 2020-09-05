
```bash
!pip install sqlparse
```

## 分割sql
```
sqlparse.split(sql,encoding = None)
```

返回一个list，里面的内容是单个的sql语句

## 美化sql

```Python
options = dict(
    keyword_case='upper'  # 保留字转大写，也可转小写 'lower'
    , identifier_case='upper'  # 标识符转大写，也可转小写 'lower'
    , strip_comments=True  # 删除注释
    , reindent=True  # 美化锁进
)

sqlparse.format(sql.lower(), encoding=None, **options)
```

## statement

```python
tokens_list = sqlparse.parse(sql) # 返回list，每个元素对应一个sql语句(statement)
```

然后可以对每一个statement做遍历：
```Python
for token in file_parse[0]:
    print('type_token={}|ttype={}|value={}'.format(type(token),token.ttype,token.value))

# ps，statement也可以索引，其实下面这些可迭代的都可以索引 file_parse[0][1]
```

遍历后，有可能会遇到以下对象：

### token


### IdentifierList
遇到 IdentifierList 可以继续遍历

```python
for token in file_parse[0]:
    if isinstance(token,sqlparse.sql.IdentifierList):
        print('-'*30)
        print('type_token={}|ttype={}|value={}'.format(type(token),token.ttype,token.value))
        for token_sub in token.get_identifiers():
            print('type_token={}|ttype={}|value={}'.format(type(token),token.ttype,token.value))
```
*IdentifierList 也可以直接遍历，会把token和Identifier都遍历出来*  

### Identifier
*Identifier 可以用 get_real_name() 获取真名*  

```python
for token in file_parse[0]:
    if isinstance(token,sqlparse.sql.Identifier):
        print('type_token={}|ttype={}|value={}｜real_name={}'.format(type(token),token.ttype,token.value,token.get_real_name()))
```

看了一眼都是啥
- 子查询是Identifier
- select 后面的字段是 `IdentifierList`，但如果select 1个字段，就变成了 `Identifier`
- 会把 `row_number()` 语句拆成好几个Identifier


### 有用的语句

```python
sqlparse.sql.Case
sqlparse.sql.Where
sqlparse.sql.If # if-else
sqlparse.sql.Parenthesis # 括号
sqlparse.sql.For # for
sqlparse.sql.Assignment # 像var：= val;这样的赋值
```



```python
tokens_list = sqlparse.parse(sql) # 返回list，每个元素对应一个sql语句
sqlparse.sql.Statement(tokens=tokens_list[1]).get_type()
```
返回这个sql语句的类型，`select`, `updata`, `delete` , `create`  
