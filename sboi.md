## Практика 1: Сбои

### 1. Описание предметной области:
- *Устройство (Device):* Каждое устройство задается уникальным числовым номером (DeviceId) и имеет наименование (DeviceName).
- *Сбой (Failure):* Каждый сбой содержит ссылку на пострадавшее устройство (DeviceId), точную дату инцидента (FailureDate), а также категорию сбоя (FailureType).
- *Категория сбоя (FailureType):* Строго фиксированный перечень типов неполадок, определяющий характер проблемы.
- *Генератор отчетов (ReportMaker):* Фильтрует весь массив зарегистрированных сбоев, отбирает среди них только серьезные критические инциденты, произошедшие ранее указанной целевой даты, и формирует финальный список имен проблемных устройств.

### 2. Диаграмма классов:
```mermaid
classDiagram

    class FailureType {
        <<enum>>
        UnexpectedShutdown = 0
        ShortNonResponding = 1
        HardwareFailures = 2
        ConnectionProblems = 3
    }

    class Device {
        +int DeviceId
        +string? DeviceName
        +Device(int id, string? name)
    }

    class Failure {
        +FailureType FailureType
        +int DeviceId
        +DateTime FailureDate
        +bool IsSerious
        +Failure(FailureType failureType, int deviceId, DateTime failureDate)
    }

    class ReportMaker {
        +FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~ devices) List~string~
        +FindDevicesFailedBeforeDate(List~Device~ devices, List~Failure~ failures, DateTime targetDate) List~string~
    }

    Failure --> FailureType : хранит категорию сбоя
    ReportMaker ..> Device : извлекает имена устройств
    ReportMaker ..> Failure : фильтрует по дате и критичности сбоя
```
