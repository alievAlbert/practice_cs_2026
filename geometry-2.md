# Практика 3: Геометрия

## 1. Описание предметной области и сущностей

- **IVisitor (Интерфейс)**: контракт с методами Visit для вызова логики под каждую фигуру.
- **Body (Абстрактный класс)**: база для тел с координатами Position и методом Accept для запуска визитора.
- **Ball (Класс)**: одиночная фигура, хранит Radius и пускает к себе визитор.
- **RectangularCuboid (Класс)**: одиночная фигура, хранит размеры куба SizeX, SizeY, SizeZ.
- **Cylinder (Класс)**: одиночная фигура, хранит размеры цилиндра Radius и SizeZ.
- **CompoundBody (Класс)**: группа фигур, хранит список деталей Parts и рекурсивно обходит их.
- **BoundingBoxVisitor (Класс)**: алгоритм, собирает крайние точки фигур и считает для них общую коробку.
- **BoxifyVisitor (Класс)**: алгоритм, заменяет все простые фигуры в сцене на коробки, сохраняя структуру.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class IVisitor {
        <<interface>>
        +Visit(Ball ball) Body
        +Visit(RectangularCuboid cuboid) Body
        +Visit(Cylinder cylinder) Body
        +Visit(CompoundBody compoundBody) Body
    }

    class Body {
        <<abstract>>
        +Vector3 Position
        +Accept(IVisitor visitor)* Body
    }

    class Ball {
        +double Radius
        +Accept(IVisitor visitor) Body
    }

    class RectangularCuboid {
        +double SizeX
        +double SizeY
        +double SizeZ
        +Accept(IVisitor visitor) Body
    }

    class Cylinder {
        +double SizeZ
        +double Radius
        +Accept(IVisitor visitor) Body
    }

    class CompoundBody {
        +IReadOnlyList~Body~ Parts
        +Accept(IVisitor visitor) Body
    }

    class BoundingBoxVisitor {
        -double minX
        -double maxX
        -double minY
        -double maxY
        -double minZ
        -double maxZ
        -bool isFirstMinMax
        -CheckBoxOnMinMax(RectangularCuboid box) void
        +Visit(Ball ball) Body
        +Visit(RectangularCuboid cuboid) Body
        +Visit(Cylinder cylinder) Body
        +Visit(CompoundBody compoundBody) Body
    }

    class BoxifyVisitor {
        +Visit(Ball ball) Body
        +Visit(RectangularCuboid cuboid) Body
        +Visit(Cylinder cylinder) Body
        +Visit(CompoundBody compoundBody) Body
    }

    Body <|-- Ball
    Body <|-- RectangularCuboid
    Body <|-- Cylinder
    Body <|-- CompoundBody
    
    IVisitor <|.. BoundingBoxVisitor
    IVisitor <|.. BoxifyVisitor

    CompoundBody o-- Body

    Body ..> IVisitor
