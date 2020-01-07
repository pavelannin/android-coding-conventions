# Соглашение по оформлению кода на языке Java
Это соглашение расширяет соглашение [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) и 
[Android Java Style Guide](https://source.android.com/setup/contribute/code-style).

## Содержание
1. [Общие положения](#common_rules)
1. [Длина строки](#linelength)
1. [Правила именования](#naming)
    1. [Наименование свойств](#property_naming)
1. [Аннотации](#annotation)
1. [Классы](#class)
    1. [Структура класса](#class_member_order)
1. [Документирование кода](#documentation_comments)

## <a name='common_rules'>Общие положения</a>
- избегать использования аббревиатур в наименованиях;
- в файле не должно быть предупреждений.

## <a name='linelength'>Длина строки</a>
- максимальная длина строки: 140 символов.

## <a name='naming'>Правила именования</a>
- compile time константы именуются в стиле SCREAMING_SNAKE_CASE;
- любые другие свойства именуются в стиле lowerCamelCase;
- методы именуются в стиле lowerCamelCase;
- классы именуются в стиле UpperCamelCase.

### <a name='property_naming'>Наименование свойств</a>
- недопустимо использование префиксов для свойств класса: `m`, `s`, и т.д.

## <a name='annotation'>Аннотации</a>
- все методы и свойства класса имеющие объектные типы и модификаторы видимости: `public`, `default` 
и `protected`, должны аннотироваться: `@Nullable` или `@NonNull`.

## <a name='class'>Классы</a>
- классы которые не планируется наследовать должны быть помечены модификатором `final`.

### <a name='class_member_order'>Структура класса</a>
1. константы;
1. свойства: `abstract`, `public`, `default`, `protected`, `private`;
1. конструкторы;
1. абстрактные методы;
1. переопределенные методы родительского класса (рекомендуемо в том же порядке, в каком они следуют в 
родительском классе);
1. реализации методов интерфейсов (рекомендуемо в том же порядке, в каком они следуют в описании класса, 
соблюдая при этом порядок описания этих методов в самом интерфейсе);
1. методы: `public`, `default`, `protected`, `private`;
1. nested классы: `public`, `default`, `protected`, `private`;
1. nested static классы: `public`, `default`, `protected`, `private`.

## <a name='documentation_comments'>Документирование кода</a>
- документироваться должны все классы/свойства/методы имеющие модификаторы видимости: `public`, `default`, `protected`;
- не использовать в документации для классов авторства и даты создания файла.