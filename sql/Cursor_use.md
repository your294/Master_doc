## cursor

**DECLARE语句的次序**: DECLARE语句的发布存在特定的次序。
用DECLARE语句定义的局部变量必须在定义任意游标或句柄之前定义，
而句柄必须在游标之后定义。不遵守此顺序将产生错误消息。

```sql
CREATE PROCEDURE processorders(
                out order_num INT
)
BEGIN

   -- Declare local variables
   -- Do not use out as name of variable because OUT is key words 
   -- DECLARE order_num INT;

   -- Declare the cursor
   DECLARE ordernumbers CURSOR
   FOR
   SELECT order_id FROM orders;

   -- Open the cursor
   OPEN ordernumbers;

   -- Get order number
   FETCH ordernumbers INTO order_num;

   -- Close the cursor
   CLOSE ordernumbers;

END;;
```

```sql
delimiter ;;
create procedure processorders()
    begin
    -- Declare local variables
    Declare o INT;
    Declare done Boolean Default 0;
    Declare t Decimal(8, 2);
    -- Declare cursor
    Declare order_cur CURSOR
    For 
    select order_id from orders;
    -- Declare continue handler 
    Declare continue handler for sqlstate '02000' SET done = 1;
    -- create table to store res
    create table if not exists ordertotals
        (order_num INT, total DECIMAL(8, 2));
    -- open cursor
    Open order_cur;
    -- loop
    REPEAT 
        Fetch order_cur into o;
        -- get the total for this order
        Call ordertotal(o, 1, t);

        -- insert order and total into ordertotals
        insert into ordertotals (order_num, total)
        Values(o, t);
    -- end loop
    Until done END REPEAT;
    close order_cur;
END;;
delimiter ;
```

```sql
delimiter ;;
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
   SELECT Sum(item_quality*quantity)
   FROM orders
   WHERE order_id = onumber
   INTO total;

   -- Is this taxable?
   IF taxable THEN
      -- Yes, so add taxrate to the total
      SELECT total+(total/100*taxrate) INTO total;
   END IF;

   -- And finally, save to out variable
   SELECT total INTO ototal;

END;;
delimiter ;
```