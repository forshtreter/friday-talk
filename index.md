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
{:.shout}

## BEM
* блоки и элементы - ок
* ...с модификаторами сложнее

## Проблемы с модификаторами
* ...у модификаторов бывают разные роли
* ...модификаторов много и их взаимодействие не всегда предсказуемо

## Миксы
* еще более непредсказуемы, чем модификаторы

## BEViS
* блоки и элементы по-прежнему ок
* ...вместо модификаторов view и state
* ...миксов нет

## State
~~~
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
~~~
<!--BEM-->
<button class="button button_size_large button_theme_action">
    Найти
</button>

<!--BEViS-->
<button class="button_large-action">
    Найти
</button>
~~~

## View
~~~
.button_large-action {
    button_size_large();
    button_theme_action();
}
~~~

## Проблемы с View
* ...неудобно собирать
* ...нельзя поменять (а иногда надо)

## Итого
* BEM - гибко, но непредсказуемо
* BEViS - предсказуемо, но не гибко

## Проблема
{:.shout}

## Презентационные классы
~~~
<!--BEM-->
<button class="button button_theme_action button_size_large">
    Найти
</button>

<!--BEViS-->
<button class="button_action-large">
    Найти
</button>
~~~

## Презентационные классы
~~~
<button style="background: #fc0; font-size: 15px;">
    Найти
</button>
~~~

## Отображение может зависеть от контекста
* ...состояние родительского блока
* ...media queries
* ...порядок в дереве (:nth-child и т.п.)

## Что хочется
~~~
<form class="Form">
    <button class="Button">
        Найти
    </button>
</form>
~~~

## Что хочется
~~~
.Form .Button {
    size: normal;
    theme: action;
}

.Form_expanded .Button {
    size: large;
}
~~~

## CSS API
{:.shout}

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
~~~
.Form .Button {
    Button_size(normal);
    Button_theme(action);
}

.Form_expanded .Button {
    Button_size(large);
}
~~~

## На самом деле даже
~~~
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
~~~
<form class="Form">
    <button class="Button Form__submit">
        Найти
    </button>
</form>
~~~

## Вторая жизнь миксов
~~~
.Form__submit.Button {
    Button_size: normal;
    Button_theme: action;
}

.Form_expanded .Form__submit.Button {
    Button_size: large;
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
* некоторые свойства всё-таки могут зависеть от нескольких миксинов

## Можно объединить
~~~
Button_appearance($size, $theme = normal) {
    if ($size == normal && $theme == round) {
        border-radius: 10px;
    }
}
~~~

## Есть нюансы
* каскад
* наложение миксинов друг на друга
* некоторые свойства всё-таки могут зависеть от нескольких миксинов
* иногда всё-таки нужно добавить какие-то свойства вне API

## Надо так надо
~~~
.Form__submit.Button {
    Button_theme: action;
    Button_size: normal;

    width: 100px;
}
~~~

## Возможно когда-нибудь
~~~
.Button {
    @prop size {
        normal {
            font-size: 13px;
        }
    }

    @allow width;
}
~~~

## Итого
* ...отделили мух от котлет
* ...гибкость
* ...надёжность
* ...семантика
* ...в сборку попадает то, что используется

{:.shout}
## Всё
