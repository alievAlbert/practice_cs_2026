# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
- Algebra: принимает лямбда-выражение, обходит его дерево рекурсивно через метод Visit и строит новое дерево.
- BinaryExpression: абстрактный класс Expression, представляет бинарные операции (сложение, умножение) и хранит левый и правый дочерние узлы.
- MethodCallExpression: абстрактный класс Expression, представляет вызовы функций (Sin, Cos) и хранит информацию о методе и его аргументах.
- ParameterExpression: абстрактный класс Expression, это переменная z.
- ConstantExpression: абстрактный класс Expression, числовая константа.
- BinaryRules и FunctionRules: словари, которые отображают тип узла на соответствующий метод дифференцирования 

## 2. Диаграмма классов (Mermaid)

``` mermaid
classDiagram
    class Algebra {
        -Dictionary~ExpressionType,Func~ BinaryRules$
        -Dictionary~string,Func~ FunctionRules$
        +Differentiate(func Expression) Expression$
        -Visit(exp Expression, param ParameterExpression) Expression$
        -DifferentiateSum(bin BinaryExpression, param ParameterExpression) Expression$
        -DifferentiateProduct(bin BinaryExpression, param ParameterExpression) Expression$
        -DifferentiateSin(call MethodCallExpression, param ParameterExpression) Expression$
        -DifferentiateCos(call MethodCallExpression, param ParameterExpression) Expression$
    }

    class Expression {
        <<abstract>>
    }

    class BinaryExpression {
        +Left Expression
        +Right Expression
        +NodeType ExpressionType
    }

    class MethodCallExpression {
        +Method MethodInfo
        +Arguments IList~Expression~
    }

    class ParameterExpression {
        +Name string
        +Type Type
    }

    class ConstantExpression {
        +Value object
        +Type Type
    }

    Expression <|-- BinaryExpression
    Expression <|-- MethodCallExpression
    Expression <|-- ParameterExpression
    Expression <|-- ConstantExpression

    Algebra ..> Expression
    Algebra ..> BinaryExpression
    Algebra ..> MethodCallExpression
    Algebra ..> ParameterExpression
    Algebra ..> ConstantExpression
```
