

MySQL предоставляет три цикла: `while`, `repeat` и `loop`. Их можно использовать в теле хранимой процедуры или функции, т.е., между ключевыми словами `BEGIN` и `END`.

##### Цикл WHILE

```sql
DROP PROCEDURE IF EXISTS my_now;
DELIMITER //

CREATE PROCEDURE my_now ()
BEGIN
  DECLARE i INT DEFAULT 3;
  WHILE i > 0 DO
	SELECT NOW();
	SET i = i - 1;
  END WHILE;
END//

CALL my_now()//
DELIMITER ; -- вернем прежний разделитель
```

`DO` - начало тела цикла  
`END WHILE` - завершение цикла  
Все команды, которые располагаются между этими ключевыми словами, выполняются на каждой итерации цикла.

Количество повторов не обязательно задавать внутри хранимой процедуры. Можно задать его в качестве входящего параметра.

```sql
DROP PROCEDURE IF EXISTS my_now_in;
DELIMITER //

CREATE PROCEDURE my_now_in (IN num INT)
BEGIN
  DECLARE i INT DEFAULT 0;
  IF (num > 0) THEN
	WHILE i < num DO
  	SELECT NOW();
  	SET i = i + 1;
	END WHILE;
  ELSE
	SELECT 'Ошибочное значение параметра';
  END IF;
END//

CALL my_now_in(4) //
DELIMITER ; -- вернем прежний разделитель
```

Локальная переменная `i` пробегает значение от 0 до заданного в параметре `num`. Как только оно достигает заданного пользователем значения, условие становится ложным и цикл прекращает работу.

Для досрочного выхода из цикла предназначен оператор `LEAVE`.

```sql
DROP PROCEDURE IF EXISTS my_now_in;
DELIMITER //

CREATE PROCEDURE my_now_in (IN num INT)
BEGIN
  DECLARE i INT DEFAULT 0;
  IF (num > 0) THEN
	cycle: WHILE i < num DO
  	IF i >= 3 THEN LEAVE cycle;
  	END IF;
  	SELECT NOW();
  	SET i = i + 1;
	END WHILE cycle;
  ELSE
	SELECT 'Ошибочное значение параметра';
  END IF;
END//

CALL my_now_in(10)//
DELIMITER ; -- вернем прежний разделитель
```

Не зависимо от переменной `num`, максимальное количество итераций будет не больше 3.

Циклы можно вкладывать друг в друга, поэтому, чтобы команда `**LEAVE**` понимала, какой из циклов следует останавливать, ей всегда передается метка цикла, в данном случае `**cycle**`. Эту метку мы должны поместить перед ключевым словом `**WHILE**` и после ключевого слова `**END WHILE**`.

От оператора `LEAVE`, ITERATE отличается тем, что не прекращает выполнение цикла, а лишь досрочно прекращает текущую итерацию.

```sql
DROP PROCEDURE IF EXISTS num_str;
DELIMITER //

CREATE PROCEDURE num_str (IN num INT)
BEGIN
  DECLARE i INT DEFAULT 0;
  DECLARE var TINYTEXT DEFAULT '';
  IF (num > 0) THEN
	cycle : WHILE i < num DO
  	SET i = i + 1;
  	SET var = CONCAT(var, i);
  	IF i > CEILING(num / 2) THEN ITERATE cycle;
  	END IF;
  	SET var = CONCAT(var, i);
	END WHILE cycle;
	SELECT var;
  ELSE
	SELECT 'Ошибка выполнения if-условия';
  END IF;
END//

CALL num_str(10)//
DELIMITER ; -- вернем прежний разделитель
```

Счетчик `i` проходит по значениям от 1 до 10, на каждой итерации значение счетчика добавляется к строке var. Если if-условие ложное, то значение добавляется два раза, если истинное, срабатывает оператор `ITERATE` и текущая итерация завершается досрочно. Поэтому в результатах происходит удвоенные цифры до 5, а после 5 - одиночные.

##### Цикл REPEAT

В отличии от цикла `WHILE`, в `REPEAT` условие для выхода из цикла располагается не в начале тела цикла, а в конце после ключевого слова `UNTIL`.  
Если условие `TRUE` - происходит еще одна итерация, если `FALSE` - работа цикла прекращается.

```sql
DROP PROCEDURE IF EXISTS my_now;
DELIMITER //

CREATE PROCEDURE my_now ()
BEGIN
  DECLARE i INT DEFAULT 4;
  WHILE i > 0 DO
	SELECT NOW();
	SET i = i - 1;
  END WHILE;
END//

CALL my_now()//
DELIMITER ; -- вернем прежний разделитель
```

##### Цикл LOOP

`LOOP`, в отличие от `WHILE` и `REPEAT` не имеет условий выхода из цикла. Выход из цикла осуществляется с помощью оперетора `LEAVE`, который досрочно прекращает текущую итерацию.

```sql
DROP PROCEDURE IF EXISTS my_now_in;
DELIMITER //

CREATE PROCEDURE my_now_in ()
BEGIN
  DECLARE i INT DEFAULT 4;
  cycle: LOOP
	SELECT NOW();
	SET i = i - 1;
	IF i <= 0 THEN LEAVE cycle;
	END IF;
  END LOOP cycle;
END//
CALL my_now_in()//
DELIMITER ; -- вернем прежний разделитель
```

#sql 