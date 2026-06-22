# Практика: GraphViz

## 1. Описание предметной области и сущностей
Статический класс DotGraphBuilder служит точкой входа и создаёт GraphMaker, который хранит граф (Graph) и отвечает за добавление узлов и рёбер,
возвращая NodeBuilder и EdgeBuilder соответственно.
Эти билдеры хранят ссылки на GraphMaker и на конкретный GraphNode или GraphEdge, 
позволяют через метод With настроить атрибуты с помощью NodeProps или EdgeProps
(каждый из которых содержит словарь свойств и метод Set для их применения)
и вернуть управление обратно в GraphMaker для продолжения цепочки;
в конце вызов Build() генерирует итоговую DOT-строку через DotFormatWriter.
Разделение NodeProps (color, fontsize, label, shape) и EdgeProps 

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class DotGraphBuilder {
        <<static>>
        +DirectedGraph(string name) GraphMaker
        +UndirectedGraph(string name) GraphMaker
    }

    class GraphMaker {
        -Graph _g
        +AddNode(string name) NodeBuilder
        +AddEdge(string a, string b) EdgeBuilder
        +Build() string
    }

    class NodeBuilder {
        -GraphMaker _maker
        -GraphNode _node
        +With(Action~NodeProps~ action) GraphMaker
        +AddNode(string name) NodeBuilder
        +AddEdge(string a, string b) EdgeBuilder
        +Build() string
    }

    class EdgeBuilder {
        -GraphMaker _maker
        -GraphEdge _edge
        +With(Action~EdgeProps~ action) GraphMaker
        +AddNode(string name) NodeBuilder
        +AddEdge(string a, string b) EdgeBuilder
        +Build() string
    }

    class NodeProps {
        -Dictionary~string, string~ _props
        +Color(string color) NodeProps
        +FontSize(int val) NodeProps
        +Label(string label) NodeProps
        +Shape(NodeShape shape) NodeProps
        -Set(GraphNode node) void
    }

    class EdgeProps {
        -Dictionary~string, string~ _props
        +Color(string color) EdgeProps
        +FontSize(int val) EdgeProps
        +Label(string label) EdgeProps
        +Weight(double weight) EdgeProps
        -Set(GraphEdge edge) void
    }

    class NodeShape {
        <<enumeration>>
        Box
        Ellipse
        Circle
        Diamond
        Plaintext
        Point
        Polygon
        Record
    }

    class Graph
    class GraphNode
    class GraphEdge
    class DotFormatWriter

    DotGraphBuilder ..> GraphMaker : создает
    GraphMaker *-- Graph : владеет
    GraphMaker --> NodeBuilder : создает
    GraphMaker --> EdgeBuilder : создает
    GraphMaker ..> DotFormatWriter : использует
    NodeBuilder --> GraphMaker : ссылается
    NodeBuilder --> GraphNode : ссылается
    NodeBuilder ..> NodeProps : создает
    EdgeBuilder --> GraphMaker : ссылается
    EdgeBuilder --> GraphEdge : ссылается
    EdgeBuilder ..> EdgeProps : создает
    NodeProps ..> GraphNode : использует в Set
    EdgeProps ..> GraphEdge : использует в Set
    NodeProps ..> NodeShape : использует
```
