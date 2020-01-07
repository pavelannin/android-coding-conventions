# Соглашение по написанию кода на android-проектах
За основу взято [соглашение предложенное HeadHunter](https://github.com/hhru/android-style-guide/blob/master/docs/android-style-guide.md).

## Содержание
1. [Структура каталогов](#directory_structure)
1. [Правила именования](#naming)
1. [Константы](#constants)
1. [Фрагменты](#fragments)
1. [Логирование](#log)
1. [Общие соглашения](#common)

## <a name='directory_structure'>Структура каталогов</a>
- Java файлы должны располагаться в директории `src/{main|androidTest|test|...}/java`;
- Kotlin файлы должны располагаться в директории `src/{main|androidTest|test|...}/kotlin`;
- Gradle файлы должны располагаться в директории `gradle`, за исключением файлов: `settings.gradle` и `build.gradle`.

В чистых Kotlin проектах структуру пакета необходимо опускать до имени проекта:
```kotlin
package project_name
```
Если в проекте используется Java код, то имя пакета должно соответствовать полной форме:
```kotlin
package com.github.pavelannin.project_name
```

В проектах, где Kotlin используется вместе с Java, исходные файлы Kotlin должны находиться в том же пакете, 
что и исходные файлы Java.

## <a name='naming'>Правила именования</a>
- модули именуются в стиле lower_snake_case;
- пакеты именуются в стиле lower_snake_case;
- файлы с исходным кодом именуются в стиле UpperCamelCase;
- файлы ресурсов именуются в стиле lower_snake_case;
- классы, которые расширяют компонент Android, должны заканчиваться именем компонента:
```
FooActivity
FooFragment
FooService
FooDialog
```

## <a name='constants'>Константы</a>
- для констант, которые используются как **ключи к `SharedPreferences`** необходимо придерживаться следующих соглашений:
    - имя константы должно начинаться с префикса `PREF_`;
    - значение константы должно формироваться по следующему правилу: `{module package name}.{flag}`. 
    Это необходимо, что бы ключи не пересекались, если используется один файл SharedPreferences для нескольких модулей;
    ```kotlin
    const val PREF_FOO_ENABLED = "com.github.pavelannin.project_name.module_name.foo_enabled"
    ```
 
- для констант, которые используются как **тэг при логировании** необходимо именовать константу `LOG_TAG`. 
Если названием для константы является название класса, и оно не помещается в 23 символа (ограничение LogCat-a), 
то можно удалить символы с конца строки;
    ```kotlin
    const val LOG_TAG = "InternetAvailabilityInt"
    ```
  
- для констант, которые используются как **тэг для экземпляра класса `Fragment`**, стоит именовать тэг `TAG` 
и оставлять его с публичным доступом. При добавлении экземпляра `Fragment` в `FragmentManager` необходимо использовать этот тэг;

- константы, которые используются как значения ключей для объектов типа `Bundle`, должны иметь модификатор `private` и именоваться: 
    - с префиксом `ARGS_`, если объект используется для передачи аргументов в объект класса `Fragment` и 
    сохранения аргументов внутри метода `onSaveInstanceState` при условии, что они имутабельны;
    - с префиксом `EXTRA_`, если объект используется для передачи аргументов в объект класса `Intent` и 
        сохранения аргументов внутри метода `onSaveInstanceState` при условии, что они имутабельны;
    - с префиксом `STATE_`, если объект используется для сохранения состояния экрана внутри метода `onSaveInstanceState`.
    
- константы, которые используются как **аргумент `requestCode` в методе `startActivityForResult`**, 
именуются с префиксом `REQUEST_CODE_`;

- константы, которые используются как **аргумент `requestCode` в методе `requestPermissions`**, 
именуются с префиксом `PERMISSION_REQUEST_CODE_`;

- при первой имплементации интерфейса `Serializable` для класса проставлять значение константы `serialVersionUID` равной **`-1L`**.
    ```kotlin
        const val serialVersionUID = -1L
    ```
  
## <a name='fragments'>Фрагменты</a>
- создавать экземпляры класса `Fragment` всегда необходимо через статический метод `newInstance` самого класса 
с передачей всех аргументов в этот метод. Аргументы не должны быть типа `Bundle`.

## <a name='log'>Логирование</a>
- при логировании всегда проставлять тэг: `Timber.tag(LOG_TAG).d("Some message")`; 

- если требуется залогировать параметры, используем интерполяцию строк в Kotlin-е:
```kotlin
Timber.tag(LOG_TAG).d("Some message with params: $param1, $param2 and $param3")
```

- при использовании статического метода `Timber.e(args)` в качестве аргументов всегда передавать экземпляр класса `Throwable`, 
даже несмотря на то, что в качестве аргументов можно передать только строку текста. 
Это требование необходимо, для логирования исключений в аналитические метрики. 
    ```kotlin
    Timber.tag(LOG_TAG).e(Exception("Unknown currency code: $currency"))
    ```
    Если уже есть экземпляр класса `Throwable`, то можно использовать метод `Timber.e(t: Throwable, message: String)`
    ```kotlin
    try {
        foo()
    } catch (error: Exception) {
        Timber.tag(LOG_TAG).e(error, "Message optional")
    }
    ```
  
## <a name='common'>Общие соглашения</a>
- при описании условий вида `if-else` сначала описываем позитивный сценарий, а потом негативный. 

То есть, НЕ так:

```kotlin
if (!myList.contains(1)) {
    negativeScenario()
} else {
    positiveScenario()
}
```
А вот так:

```kotlin
if (myList.contains(1)) {
    positiveScenario()
} else {
    negativeScenario()
}