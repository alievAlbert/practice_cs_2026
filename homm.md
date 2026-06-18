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


    IMapObject <|-- IArmy

    IMapObject <|-- ITreasure

    IMapObject <|-- IOwner


    IOwner <|.. Dwelling : реализует

    IOwner <|.. Mine : реализует

    IArmy <|.. Mine : реализует

    ITreasure <|.. Mine : реализует

    IArmy <|.. Creeps : реализует

    ITreasure <|.. Creeps : реализует

    IArmy <|.. Wolves : реализует

    ITreasure <|.. ResourcePile : реализует

    IMapObject <.. Interaction : принимает в параметрах

    IArmy <.. Interaction : проверяет охрану

    IOwner <.. Interaction : перезаписывает владельца

    ITreasure <.. Interaction : передает золото


```
