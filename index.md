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

## Проблемы с модификаторами
* ...раздутый html

## Раздутый html
~~~
<table class="b-timetable b-timetable_sort_yes
    b-timetable_filter_yes b-timetable_ajax_yes
    b-timetable_preset_trans i-bem b-timetable_js_inited"
>
    ...
</table>
~~~

## Проблемы с модификаторами
* раздутый html
* у модификаторов бывают разные роли
* ...стили накладываются в непредсказуемом порядке
* ...миксы только усугубляют вышеперечисленные проблемы

## BEViS
* ...блоки и элементы &mdash; то же самое
* ...view описывает отображение блока (большой, жёлтый и т.д.)

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

## BEViS
* блоки и элементы &mdash; то же самое
* view описывает отображение блока (большой, жёлтый и т.д.)
* ...state описывает состояние блока (нажатый, задизейбленный и т.д.)

## State
~~~
<!--BEM-->
<button class="button button_size_large button_theme_action button_pressed">
    Найти
</button>

<!--BEViS-->
<button class="button_large-action _pressed">
    Найти
</button>
~~~

## BEViS
* блоки и элементы &mdash; то же самое
* view описывает отображение блока (большой, жёлтый и т.д.)
* state описывает состояние блока (нажатый, задизейбленный и т.д.)
* view явно подчиняет state
* ...миксов нет

## Проблемы с view
* ...необходимо создавать новое view для незначительных визуальных изменений
* ...неудобно генерировать json
* ...нельзя поменять в рантайме

## Форма
<img src="pictures/form-collapsed.png" style="width: 800px"/>

## Форма
<img src="pictures/form-expanded.png" style="width: 800px"/>

## Форма
<img src="pictures/form-mobile.png" style="height: 300px" />

## Боль
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

## Разметка
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

## Реализация (миксины на stylus)
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

## На самом деле даже
~~~
.Form .Button {
    font-size: 13px;
    background: #fc0;
}

.Form_expanded .Button {
    font-size: 15px;
}
~~~

## Есть нюансы
* ...каскад

## Каскад
~~~
<main class="Content">
    <form class="Form">
        <button class="Button">
            Найти
        </button>
    </form>
</main>
~~~

## Каскад
~~~
// Form.styl
.Form .Button {
    Button_size: large;
}

// Content.styl
.Content .Button {
    Button_size: normal;
}
~~~

## Миксы!
~~~
<main class="Page">
    <form class="Form">
        <button class="Button Form__submit">
            Найти
        </button>
    </form>
</main>
~~~

## Миксы!
~~~
.Form__submit.Button {
    Button_size: large;
}

.Content__button.Button {
    Button_size: normal;
}
~~~

## Есть нюансы
* каскад
* наложение разных модификаторов друг на друга

## Наложение
~~~
Button_size($size) {
    font: 13px/18px Arial;
}

Button_theme($theme) {
    font-family: Verdana;
}
~~~

## Разделение ответственности
~~~
Button_size($size) {
    // font-size, margin, padding
}

Button_theme($theme) {
    // font-family, background, color, ...
}
~~~

## Есть нюансы
* каскад
* наложение разных модификаторов друг на друга
* разделить ответственность не всегда получается

## Нельзя разделить
~~~
Button_size($size) {
    border-radius: ...
}

Button_shape($shape) {
    border-radius: ...
}
~~~


## Можно объединить
~~~
Button_geometry($size, $shape) {
    if ($size == normal && $shape == round) {
        border-radius: 10px;
    }
    if ($size == large && $shape == round) {
        border-radius: 20px;
    }
}
~~~

## Или добавить где-то дополнительный параметр
~~~
Button_size($size) {
    // как есть, без border-radius
}

Button_shape($shape, $size = normal) {
    border-radius: 10px;
}
~~~

## Есть нюансы
* каскад
* наложение разных модификаторов друг на друга
* разделить ответственность не всегда получается
* разные значения одного модификатора задают разные свойства

## Разные значения
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
* наложение разных модификаторов друг на друга
* разделить ответственность не всегда получается
* разные значения одного модификатора задают разные свойства
* ...иногда нужно задать дополнительные свойства

## Можно прямо так
~~~
.Form__submit.Button {
    Button_theme: action;
    Button_size: normal;

    width: 100px;
}
~~~

## Но лучше так
~~~
.Form__submit.Button {
    Button_theme: action;
    Button_size: normal;

    Button_width: medium;
}
~~~

## Есть нюансы
* каскад
* наложение разных модификаторов друг на друга
* разделить ответственность не всегда получается
* разные значения одного модификатора задают разные свойства
* иногда нужно задать дополнительные свойства

## Что дальше? (а может быть и нет)
~~~
.Button {
    @prop size {
        @value normal {
            ...
        }
    }

    @allow width;
}
~~~

## Итого
* ...осмысленный, лёгкий и читаемый html
* ...возможность легко сменить оформление в зависимости от контекста
* ...применение модификаторов локализовано и управляемо
* ...бонус: в сборку автоматически попадает только то, что используется

{:.shout}
## Всё
