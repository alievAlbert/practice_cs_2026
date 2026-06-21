## Практика 2. HoMM
P.S поменял тип связей на диаграмме, и добавил базовый интерфейс IMapObject от которого наследуются все остальные интерфейсы и соответственно он передается в метод Interaction.Make

### 1. Описание предметной области:

- IMapObject (Объект карты): базовый интерфейс-маркер, являющийся корневой абстракцией для всех сущностей на карте.
- Боевой объект (IArmy): Содержит вражескую армию (Army). Обязывает принять бой. При проигрыше наступает смерть персонажа.
- Захватываемый объект (IOwner): Имеет владельца (Owner). После посещения переходит под контроль игрока.
- Источник сокровищ (ITreasure): Хранит ценности (Treasure), которые передаются игроку.
- Модуль взаимодействия (Interaction): Принимает игрока и абстрактный IMapObject и поочередно проверяет объект на три роли (Бой -> Захват -> Награда).
- Классы: Dwelling, Mine, Creeps, Wolves, ResourcePile реализуют интерфейсы IArmy, IOwner и ITreasure согласно заданию, подробнее на диаграмме.

### 2. Диаграмма классов:
```mermaid
classDiagram
    direction BT

    class Interaction {
        +Make(Player player, IMapObject mapObject) static void
    }

    class IMapObject {
        <<interface>>
    }

    class IArmy {
        <<interface>>
        +Army Army
    }

    class ITreasure {
        <<interface>>
        +Treasure Treasure
    }

    class IOwner {
        <<interface>>
        +int Owner
    }

    class Dwelling {
        +int Owner
    }

    class Mine {
        +int Owner
        +Army Army
        +Treasure Treasure
    }

    class Creeps {
        +Army Army
        +Treasure Treasure
    }

    class Wolves {
        +Army Army
    }

    class ResourcePile {
        +Treasure Treasure
    }

    class Player {
        +int Id
        +CanBeat(Army army) bool
        +Consume(Treasure treasure) void
        +Die() void
    }

    IMapObject <|.. IArmy
    IMapObject <|.. ITreasure
    IMapObject <|.. IOwner

    Dwelling ..|> IOwner
    Mine ..|> IOwner
    Mine ..|> IArmy
    Mine ..|> ITreasure
    Creeps ..|> IArmy
    Creeps ..|> ITreasure
    Wolves ..|> IArmy
    ResourcePile ..|> ITreasure

    Interaction ..> IMapObject : принимает в параметрах
    Interaction ..> Player : принимает в параметрах
    Interaction ..> IArmy : использует при наличии охраны
    Interaction ..> IOwner : использует для смены владельца
    Interaction ..> ITreasure : использует для сбора золота
```
