# Соглашение по оформлению кода на языке XML

## Содержание
1. [Общие положения](#common_rules)
1. [Длина строки](#linelength)
1. [Правила именования](#naming)
    1. [Drawable](#drawable_naming)
    1. [Menu](#menu_naming)
    1. [Anim](#anim_naming)
    1. [Anim](#anim_naming)
1. [Правила именования Id](#id_naming)
    1. [Color](#color_id_naming)
    1. [String](#string_id_naming)
    1. [View](#view_id_naming)
1. [Порядок следования элементов](#modifier_order)
1. [Текстовые поля](#text_fields)

## <a name='common_rules'>Общие положения</a>
- избегать использования аббревиатур в наименованиях;
- файл должен начинаться с декларации, корневой элемент должен отделяться от декларации одной пустой строкой;
- элементы должны разделяться между собой одной пустой строкой;
- свойства элемента не должны разделяться между собой; 
- по возможности следует использовать самозакрывающиеся теги;
- в файле не должно быть предупреждений.
```xml
<?xml version="1.0" encoding="utf-8"?>

<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/fooMainContainer"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/fooTitleTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        tools:text="Title" />

</FrameLayout>
```

## <a name='linelength'>Длина строки</a>
- максимальная длина строки: 120 символов.

## <a name='naming'>Правила именования</a>
- имя файла должно быть в стиле lower_snake_case.

### <a name='drawable_naming'>Drawable</a>
- имя файла имеет маску {название ресурса}\_{описание }\_{свойства: цвет, размер, селектор и пр.};

- сокращенные наименования ресурсов:

| Полное название | Сокращенное название |
| --------------- | -------------------- |
| icon            | ic                   |
| background      | bg                   |

- последовательность указания свойств:
1. цвет;
1. размер;
1. селектор;
1. прочие.
```
ic_favorite_gray_24dp.xml
bg_button_blue_pressed.xml
```

### <a name='menu_naming'>Menu</a>
- имя имеет маску {имя экрана (исключая тип: `activity` или `fragment`)}_{описание (опционально)}:
```
resume_contacts.xml
vacancy_actions.xml
```

### <a name='anim_naming'>Anim</a>
- имя имеет маску {описание}\_{свойства: вид анимации(fade, transform и пр.), время действия (опционально)}:
```
enter_from_left_250ms.xml
hide_icon_fade_out.xml
```

## <a name='id_naming'>Правила именования Id</a>
### <a name='color_id_naming'>Color</a>
- идентификаторы записываются в стиле lower_snake_case;
- идентификатор должен обозначать наименование оттенка цвета.

### <a name='string_id_naming'>String</a>
- идентификаторы записываются в стиле lower_snake_case.

### <a name='view_id_naming'>View</a>
- идентификаторы записываются в стиле lowerCamelCase;
- идентификаторы имеют маску {название файла}{название элемента}{тип виджета}. Название элемента отражает его предназначение. 
Тип виджета допускается обобщать, если нет необходимости понимать какой это конкретно тип 
(например: `ConstraintLayout` допускается назвать `Container`);
- имена должны присутствовать у всех элементов в разметке. 
```xml
    <EditText
        android:id="@+id/profilePhoneEditText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:inputType="phone" />
```

## <a name='modifier_order'>Порядок следования элементов</a>
1. id;
1. style/theme;
1. layout_width/layout_height/layout_weight;
1. атрибуты (из пространства `android`) отсортированы в алфавитном порядке;
1. атрибуты (из других пространств, например `app`) отсортированы в алфавитном порядке;
1. атрибуты (из пространства `tools`) отсортированы в алфавитном порядке.

## <a name='text_fields'>Текстовые поля</a>
- все тексты должны быть вынесены в ресурсы;
- исключение составляет текст для верстки, помеченный xml схемой `tools`.