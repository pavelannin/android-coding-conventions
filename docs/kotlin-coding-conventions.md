# Соглашение по оформлению кода на языке Kotlin
Это соглашение расширяет соглашение предложенное [командой разработки Kotlin](https://kotlinlang.org/docs/reference/coding-conventions.html).
За основу взято [соглашение предложенное RedMadRobot](https://github.com/RedMadRobot/kotlin-style-guide).

## Содержание
1. [Общие положения](#common_rules)
1. [Длина строки](#linelength)
1. [Правила именования](#naming)
1. [Порядок следования модификаторов](#modifier_order)
1. [Форматирование выражений](#expression_formating)
1. [Функции](#function)
1. [Аннотации](#annotation)
1. [Структура класса](#class_member_order)
1. [Форматирование лямбда-выражений](#lambda_formating)
1. [Использование условных операторов](#condition_operator)
1. [Документирование кода](#documentation_comments)
1. [Файлы](#files)
1. [Использование Properties](#properties)
    1. [Форматирование properties](#properties_formatting)
    1. [Functions VS Properties](#functions_vs_properties)

## <a name='common_rules'>Общие положения</a>
- избегать использования аббревиатур в наименованиях;
- в файле не должно быть предупреждений.

## <a name='linelength'>Длина строки</a>
- максимальная длина строки: 140 символов.

## <a name='naming'>Правила именования</a>
- неизменяемые поля в Companion Objects и compile time константы именуются в стиле SCREAMING_SNAKE_CASE;
- для полей View из Kotlin Extension используется стиль lowerCamelCase;
- любые другие свойства именуются в стиле lowerCamelCase;
- функции именуются в стиле lowerCamelCase;
- классы именуются в стиле UpperCamelCase.

## <a name='modifier_order'>Порядок следования модификаторов</a>
1. public / protected / private / internal;
1. expect / actual;
1. final / open / abstract / sealed / const;
1. external;
1. override;
1. lateinit;
1. tailrec;
1. vararg;
1. suspend;
1. inner;
1. enum / annotation;
1. companion;
1. inline;
1. infix;
1. operator;
1. data.

## <a name='expression_formating'>Форматирование выражений</a>
- при переносе на новую строку цепочки вызова методов символ `.` или оператор `?.` переносятся на следующую строку, 
`property` при этом разрешается оставлять на одной строке:
```kotlin
val collectionItem = source.collectionItems
    ?.dropLast(10)
    ?.sortedBy { it.progress }
```

- Elvis оператор `?:` при разрыве выражения также переносится на новую строку:
```kotlin
val promoItemDistanceTradeLink = promoItem.distanceTradeLinks?.appLink
    ?: String.EMPTY
```

- при описании свойства с делегатом, не помещающимися на одной строке, оставлять описание с открывающейся фигурной скобкой на одной строке, перенося остальное выражение на следующую строку:
```kotlin
private val promoItem: MarkPromoItem by lazy(mode = LazyThreadSafetyMode.NONE) {
    extractNotNull(BUNDLE_FEED_UNIT_KEY) as MarkPromoItem
}
```

## <a name='function'>Функции</a>
- всегда указывать тип функции явно (исключением являются вложенные функции);
- использование именованного синтаксиса аргументов остается на усмотрение разработчика. 
Стоит руководствоваться сложностью вызываемого метода: если вызов метода с переданными в него параметрами 
понятен и очевиден, нет необходимости использовать именованные параметры.

## <a name='annotation'>Аннотации</a>
- аннотации, относящиеся к файлу, располагаются перед package, с разделителем в виде пустой строки;
- аннотации располагаются над описанием сигнатуры, к которому они применяются;
- если к свойству/элементу enum/конструктору применяется только одна аннотация, указывается на той же строке:
```kotlin
@Inject lateinit var foo: Foo
```

## <a name='class_member_order'>Структура класса</a>
1. поля: abstract, override, public, internal, protected, private;
1. properties: abstract, override, public, internal, protected, private;
1. блок инициализации: init, вторичные конструкторы;
1. абстрактные методы;
1. переопределенные методы родительского класса (рекомендуемо в том же порядке, в каком они следуют в родительском классе);
1. реализации методов интерфейсов (рекомендуемо в том же порядке, в каком они следуют в описании класса, 
соблюдая при этом порядок описания этих методов в самом интерфейсе);
1. методы: public, internal, protected, private, private extension (для сторонних типов);
1. inner классы: public, internal, protected, private;
1. nested классы (interface, enum, sealed class, data class, class): public, internal, protected, private;
1. private extension методы для nested классов (располагаются под объектами расширения);
1. companion object.

## <a name='lambda_formating'>Форматирование лямбда-выражений</a>
- при возможности оставлять лямбда-выражение на одной строке, использовать `it` в качестве аргумента 
(если именованный аргумент не влияет на улучшение читабельности);
- при вызове функции высшего порядка, аргумент лямбда-выражения выносить за скобки, если это единственный 
аргумент лямбда-выражения:
```kotlin
list.filter { it > 10 }
```

- если выражение возможно написать с передачей метода по ссылке, передавать метод по ссылке:
```kotlin
foo(this::bar)
```

- при написании лямбда-выражений более чем в одну строку всегда использовать именованный аргумент, вместо `it` 
(исключение, если `it` обоснован примитивизированностью лямбда-выражения):
```kotlin
foo { param ->
    method1()
    method2()
}

```
- неиспользуемые параметры лямбда-выражений всегда заменять символом (underscore) `_`.

## <a name='condition_operator'>Использование условных операторов</a>
- не обрамлять `if else` выражения в фигурные скобки только если условный оператор `if else` помещается в одну строку;

- при возможности использовать условные операторы, как выражение:
```kotlin
return if (condition) foo() else bar()
```

- у оператора `when` для коротких выражений ветви условия размещать на одной строке с условием без фигурных скобок:
```kotlin
when (somenCondition) {
        0 -> fooFunction()
        1 -> barFunction()
        else -> exitFunction()
}
```

## <a name='documentation_comments'>Документирование кода</a>
- документироваться должны все классы/свойства/методы имеющие модификаторы видимости: public/internal/protected;

- свойства класса должны документироваться в описании класса:
```kotlin
/**
 * Описание класса.
 *
 * @property bar Описание свойства.
 */
class Foo {

    val bar: Int = 1
}
```

- не использовать в документации для классов авторства и даты создания файла.

##<a name='files'>Файлы</a>
- файл должен заканчиваться одной пустой строкой;
- файл должен именоваться именем класса если в нем описан единственный класс.

## <a name='properties'>Использование Properties</a>
### <a name='properties_formatting'>Форматирование properties</a>
- всегда указывать тип `property` явно;

- если `get`-блок помещается на одной строчке и не требует нескольких действий - можно писать его в одну строчку:
```kotlin
class User(
    private val firstName: String,
    private var lastName: String
) {
	
    val fullName: String get() = "$firstName $lastName"
}
```

- если `get`-блок не помещается на одной строчке - помещаем его под определением property, отделяя от следующего блока 
кода одной строчкой:
```kotlin
class User(
    private val firstName: String,
    private var lastName: String
) {

    val lastNameValue: Int
        get() = when (lastName) {
            null -> 100
            isNullOrBlank() -> 200
            else -> 300
        }

    val fullName: String get() = "$firstName $lastName"
}
```

- два `property` разделяются отдельной строкой если их нужно отделить логически друг от друга, или же у `property`
 есть выделенный `get`-блок:
```kotlin
class User(
    val firstName: String,
    val lastName: String
) {

    val firstNameValue: String get() = "First name: $firstName"
    val lastNameHashCode: Int? get() = lastName?.hashCode()

    val lastNameValue: Int
        get() = when (lastName) {
            null -> 100
            isNullOrBlank() -> 200
            else -> 300
        }

    val fullName: String
        get() {
            val superValue = firstName?.hashCode()?.plus(lastName?.hashCode() ?: 0)
            return "$superValue"
        }
}
```

### <a name='functions_vs_properties'>Functions VS Properties</a>
- `get`-блок `property` не должен содержать сложной логики. Если требуется описать некоторый сложный алгоритм, 
то лучше написать функцию;
- в `get`-блоках `property` не должны бросаться исключения. Если нужно бросить исключение в getter-е, 
лучше используйте функцию с говорящим названием;
- в `get`-блоках `property` не должны изменяться поля класса. Если требуется менять внутренние поля класса - 
нужно использовать функции.