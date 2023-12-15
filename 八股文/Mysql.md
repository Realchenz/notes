1. MySQL有哪几种锁
（1）行锁：间隙锁，记录锁，临键锁
（2）表锁：表锁，元数据锁
（4）意向锁：意向共享锁，意向排他锁
2. 触发器
```sql
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    order_details TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE audit_log (
    log_id INT AUTO_INCREMENT PRIMARY KEY,
    operation_type VARCHAR(20),
    order_id INT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
```sql
DELIMITER //
CREATE TRIGGER after_order_insert
AFTER INSERT ON orders 
FOR EACH ROW
BEGIN
    INSERT INTO audit_log (operation_type, order_id, timestamp)
    VALUES ('INSERT', NEW.order_id, NOW());
END;
//
DELIMITER ;
```
3. char和varchar的区别
（1）char是定长的，varchar是变长的
（2）char的存储空间是固定的，varchar的存储空间是可变的
（3）char的查询速度比varchar快
（4）char的存储效率比varchar高
（5）char的排序和比较速度比varchar快
4. 主键和候选键的区别
（1）主键是非空的，候选键不一定非空
（2）主键是候选键的子集
5. BLOB和TEXT的区别
（1）BLOB是二进制数据，TEXT是文本数据
6. 什么是通用SQL函数
（1）通用SQL函数是指在所有数据库中都可以使用的SQL函数
（2）通用SQL函数包括：数学函数、字符串函数、日期函数、转换函数、条件函数、聚合函数
7. Mysql如何优化distinct
（1）使用索引
（2）使用临时表
（3）减少查询字段
```sql
CREATE TABLE employees (
    id INT AUTO_INCREMENT,
    name VARCHAR(100),
    department VARCHAR(100),
    PRIMARY KEY (id)
);
SELECT DISTINCT department FROM employees;

SELECT DISTINCT name, department FROM employees; 

SELECT DISTINCT department FROM employees ORDER BY department;

SELECT department, COUNT(DISTINCT name) AS unique_names_count
FROM employees
GROUP BY department;
```
8. 分库分表后，主键如何处理
（1）使用复合主键
（2）使用全局唯一标识符（GUID/UUID）
（3）使用分布式中心主键生成器
（4）分段式序列号
（5）客户端生成主键


