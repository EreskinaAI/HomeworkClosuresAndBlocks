# Hometask по лекции № 4
## **Описать все отличия objective-c блоков от swift closure в виде чекпоинтов**


Что такое блок? В первую очередь, блок — это объект.

Блоки являются экземплярами класса NSObject (конкретные классы этих объектов не определенны), поэтому мы можем и вынуждены пользоваться методами класса NSObject — copy, retain, release и autorelease для блоков и управления памятью.

По умолчанию экземпляры блоков создаются не в куче, как можно было бы предположить, а в стеке. Поэтому при необходимости сделать отложенный вызов блока сначала его нужно скопировать в кучу. Причина, по которой блоки поместили в стек по умолчанию, — это скорость. В общем случае, когда время жизни блока меньше, чем у функции стека, содержащей его, это очень хорошая оптимизация.

Существует три вида блоков:

1)    Global blocks — блоки, не захватывающие никакого состояния (переменных). Располагаются в глобальной памяти (не в стеке, не в куче). 
     Следовательно, методы copy, retain, release и autorelease глобального блока ничего не делают.
     
     В SWIFT же это глобальные функции - у них есть имя и они не могут захватить значение

2)    Stack blocks - блоки, которые захватывает внешние переменные и размещается в стеке
     Метод retain ничего не делает для стекового блока
     
     В SWIFT это аналог вложенной функции. Она может захватывать любой из аргументов своей внешней функции, а также может захватывать любые константы и переменные, определенные во внешней функции

3)    Malloc blocks - блоки в куче.
После копирования блока из стека в кучу, блок может использоваться за пределами области действия в которой он был определен. 
Так же, после копирования, блок становится объектом к которому применяется подсчет ссылок. 
При определении блока stackBlock, необходимо отправить ему сообщение copy, которое не подставил ARC
Метод copy в свою очередь работает как retain для NSObject

     В SWIFT это похоже на выражения закрытия - у них нет имени и они могут захватывать значения из окружающего контекста

Декларация как blocks в Objective-C, так и closures в SWIFT происходит аналогичным образом в качестве:

- свойств
- параметров методов
- аргументов при вызове методов
- псевдонимов типов typedef/ typealias

Закрытия (closures) против блоков(blocks)

•    Закрытия Swift и блоки Objective-C совместимы, поэтому можно передавать закрытия Swift методам Objective-C, которые ожидают блоков;
•    Закрытия и функции Swift имеют один и тот же тип, поэтому можно даже передать имя функции Swift;
•    Закрытия имеют такую же семантику захвата, как и блоки, но отличаются одним ключевым образом: переменные изменяемы, а не копируются. 

P.S. Переменные — указатели на объекты с подсчетом ссылок (id, NSObject). Они задаются ключевым словом __block и являются изменяемыми. Работает это за счет копирования значения такой переменной в кучу (для них вызывается retain), и каждый блок хранит ссылку на эту переменную. Retain объекта происходит именно во время копирования блока в кучу, а не во время его создания, объявленные с ключевым словом __block. Для них не вызывается retain при копировании блока в кучу. Обычно это используется для избегания циклических ссылок (retain cycle).
Примечание: в качестве атрибута свойства следует указать copy, поскольку блок необходимо скопировать, чтобы отслеживать его захваченное состояние за пределами исходной области. Это не то, о чем нужно беспокоиться при использовании автоматического подсчета ссылок (ARC), так как это произойдет автоматически, но лучше всего, чтобы атрибут свойства отображал результирующее поведение. ARC нам в этом помогает, и при присваивании блока в переменную (__strong) произойдет копирование блока в кучу. В общем, благодаря ARC, в отличие от MRC, удается избегать крайне неприятных багов.





 

. 





