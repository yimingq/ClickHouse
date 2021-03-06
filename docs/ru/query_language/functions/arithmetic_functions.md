# Арифметические функции

Для всех арифметических функций, тип результата вычисляется, как минимальный числовой тип, который может вместить результат, если такой тип есть. Минимум берётся одновременно по числу бит, знаковости и "плавучести". Если бит не хватает, то берётся тип максимальной битности.

Пример:

``` sql
SELECT toTypeName(0), toTypeName(0 + 0), toTypeName(0 + 0 + 0), toTypeName(0 + 0 + 0 + 0)
```

```
┌─toTypeName(0)─┬─toTypeName(plus(0, 0))─┬─toTypeName(plus(plus(0, 0), 0))─┬─toTypeName(plus(plus(plus(0, 0), 0), 0))─┐
│ UInt8         │ UInt16                 │ UInt32                          │ UInt64                                   │
└───────────────┴────────────────────────┴─────────────────────────────────┴──────────────────────────────────────────┘
```

Арифметические функции работают для любой пары типов из UInt8, UInt16, UInt32, UInt64, Int8, Int16, Int32, Int64, Float32, Float64.

Переполнение производится также, как в C++.

## plus(a, b), оператор a + b

Вычисляет сумму чисел.
Также можно складывать целые числа с датой и датой-с-временем. В случае даты, прибавление целого числа означает прибавление соответствующего количества дней. В случае даты-с-временем - прибавление соответствующего количества секунд.

## minus(a, b), оператор a - b

Вычисляет разность чисел. Результат всегда имеет знаковый тип.

Также можно вычитать целые числа из даты и даты-с-временем. Смысл аналогичен - смотрите выше для plus.

## multiply(a, b), оператор a \* b

Вычисляет произведение чисел.

## divide(a, b), оператор a / b

Вычисляет частное чисел. Тип результата всегда является типом с плавающей запятой.
То есть, деление не целочисленное. Для целочисленного деления, используйте функцию intDiv.
При делении на ноль получится inf, -inf или nan.

## intDiv(a, b)

Вычисляет частное чисел. Деление целочисленное, с округлением вниз (по абсолютному значению).
При делении на ноль или при делении минимального отрицательного числа на минус единицу, кидается исключение.

## intDivOrZero(a, b)

Отличается от intDiv тем, что при делении на ноль или при делении минимального отрицательного числа на минус единицу, возвращается ноль.

## modulo(a, b), оператор a % b

Вычисляет остаток от деления.
Если аргументы - числа с плавающей запятой, то они предварительно преобразуются в целые числа, путём отбрасывания дробной части.
Берётся остаток в том же смысле, как это делается в C++. По факту, для отрицательных чисел, используется truncated division.
При делении на ноль или при делении минимального отрицательного числа на минус единицу, кидается исключение.

## negate(a), оператор -a

Вычисляет число, обратное по знаку. Результат всегда имеет знаковый тип.

## abs(a) {#arithm_func-abs}

Вычисляет абсолютное значение для числа a. То есть, если a &lt; 0, то возвращает -a.
Для беззнаковых типов ничего не делает. Для чисел типа целых со знаком, возвращает число беззнакового типа.

## gcd(a, b)
Вычисляет наибольший общий делитель чисел.
При делении на ноль или при делении минимального отрицательного числа на минус единицу, кидается исключение.

## lcm(a, b)
Вычисляет наименьшее общее кратное чисел.
При делении на ноль или при делении минимального отрицательного числа на минус единицу, кидается исключение.

[Оригинальная статья](https://clickhouse.yandex/docs/ru/query_language/functions/arithmetic_functions/) <!--hide-->
