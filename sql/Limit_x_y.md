## Limit_x_y
from idx = x and get y rows
PS: **do not caluate in this clause**

```sql
delimiter ;;
create 
    function get_Nth_max_total(N int) returns DECIMAL(8, 2)
begin
    Declare M int;
    SET M = N - 1;
    return (
        select DISTINCT quantity * item_quality as tot
        from orders order by tot desc limit M, 1
    );
end;;
delimiter ;
```

**the complex structure**
```sql
 create
        function myfun_getMax(num1 int, num2 int)
        returns int        
begin
    declare res int;
    if(num1 > num2) then
        set res = num1;
    elseif (num1 < num2) then
        set res = num2;
    else
        set res = num1;
    end if;
    return res;
end;

```