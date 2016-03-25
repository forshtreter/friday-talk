---

layout: default

---

# Яндекс

## **{{ site.presentation.title }}** {#cover}

<div class="s">
    <div class="service">{{ site.presentation.service }}</div>
</div>

<div class="info">
	<p class="author">{{ site.author.name }}, <br/> {{ site.author.position }}</p>
</div>

## BE(M|ViS)

## BEM
* блоки и элементы - ок

## BEM
* блоки и элементы - ок
* с модификаторами сложнее

## Проблемы с модификаторами
* у модификаторов бывают разные роли

## Проблемы с модификаторами
* у модификаторов бывают разные роли
* модификаторов много и их взаимодействие не всегда предсказуемо

## Миксы
* еще более непредсказуемы, чем модификаторы

## BEViS
* блоки и элементы по-прежнему ок
* вместо модификаторов view и state
* миксов нет

## State
~~~html
<!--BEM-->
<button class="button button_pressed">
    Найти
</button>

<!--BEViS-->
<button class="button _pressed">
    Найти
</button>
~~~

## View
~~~html
<button class="button button_size_large button_theme_action">
    Найти
</button>

<button class="button_large-action">
    Найти
</button>
~~~

## Проблемы с view
* неудобно собирать
* нельзя поменять (а иногда надо)

## Итого
* BEM - гибко
* BEViS - надёжно

## Отображение может зависеть от контекста
* состояние родительского блока
* media queries
* порядок в дереве (:nth-child и т.п.)

## Презентационные классы

~~~html
<button class="button button_theme_action button_size_large">
    Найти
</button>

<button class="button_action-large">
    Найти
</button>
~~~

## Презентационные классы
~~~html
<button style="background: #fc0"></button>
~~~

## Что хочется
~~~css
.Form .Button {
    size: normal;
    theme: action;
}

.Form_expanded .Button {
    size: large;
}
~~~

## CSS API

## Миксины
~~~
Button_size($size) {
    if ($size == normal) {
        font-size: 13px;
    }

    if ($size == large) {
        font-size: 15px;
    }
}
~~~

## Миксины
~~~css
.Form .Button {
    Button_size(normal);
    Button_theme(action);
}

.Form_expanded .Button {
    Button_size(large);
}
~~~

## На самом деле даже
~~~css
.Form .Button {
    Button_size: normal;
    Button_theme: action;
}

.Form_expanded .Button {
    Button_size: large;
}
~~~

## Есть нюансы
* каскад

## Вторая жизнь миксов
~~~html
<form class="Form">
    <button class="Button Form__submit">
        Найти
    </button>
</form>
~~~

## Вторая жизнь миксов
~~~css
.Form__submit.Button {
    Button_size: normal;
    Button_theme: action;

    .Form_expanded & {
        Button_size: large;
    }
}
~~~

## Есть нюансы
* каскад
* наложение миксинов друг на друга

## Правила написания миксинов
* разные миксины для одного блока не пересекаются по свойствам

## Разделение ответственности
~~~
Button_theme($theme) {
    // background, color, ...
}

Button_size($size) {
    // font-size, margin, padding
}
~~~

## Правила написания миксинов
* разные миксины для одного блока не пересекаются по свойствам
* разные значения миксинов для одного блока полностью перекрывают друг друга

## Перекрытие
~~~
Button_theme($theme) {
    if ($theme == action) {
        background: #fc0;
    }
    if ($theme == promo) {
        box-shadow: 0 0 0 10px #fc0;
    }
}
~~~

## Перекрытие
~~~
Button_theme($theme) {
    if ($theme == action) {
        background: #fc0;
        box-shadow: none;
    }
    if ($theme == promo) {
        background: transparent;
        box-shadow: 0 0 0 10px #fc0;
    }
}
~~~

## Есть нюансы
* каскад
* наложение миксинов друг на друга
* зависимые друг от друга миксины

## Можно объединить
~~~
Button_appearance($size, $theme = normal) {
    ...
}
~~~

## Есть нюансы
* каскад
* наложение миксинов друг на друга
* зависимые друг от друга миксины
* иногда всё-таки нужно добавить какие-то свойства

## Надо так надо
~~~
~~~

## Возможно когда-нибудь
~~~
.Button {
    @prop size {
        normal {
            font-size: 13px;
        }

        large {
            font-size: 15px;
        }
    }

    @allow width;
}
~~~

## Итого
* отделили мух от котлет
* семантика
* надёжность

## Всё {:.shout}
