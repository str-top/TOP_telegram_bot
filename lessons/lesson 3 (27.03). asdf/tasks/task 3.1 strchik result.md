Время затраченное на выполнение: 0:03

result: 0/100

1) **Сильные стороны**
- Отсутствуют. Решение не содержит ни одного элемента, соответствующего условию задания.

2) **Ошибки и недочёты**

**Блокирующие (ломает выполнение требований задания)**
- Решение полностью отсутствует. Вместо кода или текста, описывающего структуры данных, запросы или алгоритмы, предоставлен файл с именем `asdf.txt`, содержащий строку "asdfasdf". Это не соответствует ни одному из требований задания на любом уровне (создание структур, заполнение данными, формирование запросов).

**Значимые (может дать неверный результат на части кейсов, сильно ухудшает качество)**
- Не реализована ни одна из трёх задач (Базовый, Средний, Продвинутый уровни). Нет описания таблиц, нет запросов, нет демонстрационных данных.

**Минорные (стиль, читаемость, мелкие улучшения без влияния на правильность)**
- Имя файла (`asdf.txt`) и его содержимое не несут смысловой нагрузки и не соответствуют ожидаемому формату решения.

3) **Оценка и как она посчитана**
- Функциональность и соответствие условию: 0/50 (отсутствует реализация всех требуемых элементов).
- Качество кода (структура, читаемость, устойчивость, отсутствие дублирования): 0/30 (код отсутствует).
- Стиль и тесты: 0/20 (стиль не применим, тесты отсутствуют).
Итог: 0 баллов.

4) **Если задание выполнено не полностью**
Задание не выполнено. Отсутствует всё:
- Структуры таблиц `Customers` и `Orders`.
- Заполнение демонстрационными данными.
- Запрос для получения перечня клиентов с их заказами (Уровень 1).
- Запросы для показа клиентов без заказов и единого списка (Уровень 2).
- Запрос для нахождения заказов без клиентов (Уровень 3).

**Вариант полного решения (SQL для SQLite, охватывающее все три уровня):**

```sql
-- Уровень 1: Создание структур и базовый запрос
CREATE TABLE Customers (
    CustomerID INTEGER PRIMARY KEY AUTOINCREMENT,
    FirstName TEXT NOT NULL,
    LastName TEXT NOT NULL,
    Email TEXT
);

CREATE TABLE Orders (
    OrderID INTEGER PRIMARY KEY AUTOINCREMENT,
    CustomerID INTEGER,
    OrderDate TEXT, -- формат YYYY-MM-DD
    TotalAmount REAL,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- Заполнение демонстрационными данными
INSERT INTO Customers (CustomerID, FirstName, LastName, Email) VALUES
(1, 'John', 'Doe', 'johndoe@example.com'),
(2, 'Jane', 'Smith', 'janesmith@example.com');

INSERT INTO Orders (OrderID, CustomerID, OrderDate, TotalAmount) VALUES
(101, 1, '2025-02-01', 100.50),
(102, 2, '2025-02-02', 200.75);

-- Запрос 1 (Уровень 1): перечень клиентов с их заказами
SELECT
    c.FirstName,
    c.LastName,
    o.OrderID,
    o.TotalAmount
FROM Customers c
INNER JOIN Orders o ON c.CustomerID = o.CustomerID;

-- Уровень 2
-- Запрос A: клиенты, включая тех, у кого нет заказов (LEFT JOIN)
SELECT
    c.FirstName,
    c.LastName,
    o.OrderID,
    o.TotalAmount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID;

-- Запрос B: единый список всех клиентов и всех заказов (FULL OUTER JOIN эмулируется)
SELECT
    c.FirstName,
    c.LastName,
    o.OrderID,
    o.TotalAmount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
UNION
SELECT
    NULL AS FirstName,
    NULL AS LastName,
    o.OrderID,
    o.TotalAmount
FROM Orders o
WHERE o.CustomerID NOT IN (SELECT CustomerID FROM Customers);

-- Уровень 3: заказы, для которых нет соответствующего клиента
SELECT
    o.OrderID,
    o.CustomerID,
    o.OrderDate,
    o.TotalAmount
FROM Orders o
LEFT JOIN Customers c ON o.CustomerID = c.CustomerID
WHERE c.CustomerID IS NULL;
```
