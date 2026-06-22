# Практика: TaxiOrder

## 1. Описание предметной области и сущностей
Ключевой сущностью и корнем агрегата выступает TaxiOrder, отвечающий за жизненный цикл поездки и инкапсулирующий всю бизнес-логику валидных переходов между статусами.
Для хранения структурированных данных заказ использует неизменяемые объекты-значения: PersonName (имена участников),
Address (точки маршрута) и Car (характеристики автомобиля), которые не имеют собственной идентичности и существуют только в контексте своих владельцев.
Взаимодействие с внешним контекстом происходит через независимую сущность Driver,
получаемую из DriversRepository, а TaxiApi выступает в роли прикладного сервиса, который лишь делегирует команды объекту TaxiOrder, не нарушая его инкапсуляцию.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    Entity <|-- TaxiOrder
    Entity <|-- Driver
    ValueType <|-- PersonName
    ValueType <|-- Address
    ValueType <|-- Car
    
    ITaxiApi <|.. TaxiApi
    
    TaxiOrder *-- PersonName : ClientName
    TaxiOrder *-- Address : Start
    TaxiOrder *-- Address : Destination
    
    TaxiOrder o-- Driver
    
    TaxiOrder --> TaxiOrderStatus
    
    Driver *-- PersonName : Name
    Driver *-- Car
    
    TaxiApi --> DriversRepository
    
    TaxiApi ..> TaxiOrder
    
    DriversRepository ..> Driver

    class Entity {
        <<Abstract>>
        +int Id
    }
    
    class ValueType {
        <<Abstract>>
    }
    
    class ITaxiApi {
        <<Interface>>
    }
    
    class TaxiOrder {
        +PersonName ClientName <<readonly>>
        +Address Start <<readonly>>
        +Address Destination <<readonly>>
        +Driver Driver <<readonly>>
        +TaxiOrderStatus Status <<readonly>>
        +DateTime CreationTime <<readonly>>
        +DateTime DriverAssignmentTime <<readonly>>
        +DateTime CancelTime <<readonly>>
        +DateTime StartRideTime <<readonly>>
        +DateTime FinishRideTime <<readonly>>
        +UpdateDestination(Address)
        +AssignDriver(Driver, DateTime)
        +UnassignDriver()
        +Cancel(DateTime)
        +StartRide(DateTime)
        +FinishRide(DateTime)
    }
    
    class PersonName {
        +string FirstName <<readonly>>
        +string LastName <<readonly>>
    }
    
    class Address {
        +string Street <<readonly>>
        +string Building <<readonly>>
    }
    
    class Car {
        +string Model <<readonly>>
        +string Color <<readonly>>
        +string PlateNumber <<readonly>>
    }
    
    class Driver {
        +PersonName Name <<readonly>>
        +Car Car <<readonly>>
    }
    
    class DriversRepository {
        +GetDriver(int) Driver
    }
    
    class TaxiApi {
        -DriversRepository driversRepo
        -Func~DateTime~ currentTime
        -int idCounter
        +CreateOrderWithoutDestination(string, string, string, string) TaxiOrder
        +UpdateDestination(TaxiOrder, string, string)
        +AssignDriver(TaxiOrder, int)
        +UnassignDriver(TaxiOrder)
        +GetDriverFullInfo(TaxiOrder) string
        +GetShortOrderInfo(TaxiOrder) string
        +Cancel(TaxiOrder)
        +StartRide(TaxiOrder)
        +FinishRide(TaxiOrder)
    }
    
    class TaxiOrderStatus {
        <<Enumeration>>
        WaitingForDriver
        WaitingCarArrival
        InProgress
        Finished
        Canceled
    }
```
