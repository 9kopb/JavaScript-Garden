## Цикл `for in`

Как и оператор `in`, цикл `for in` проходит по всей цепочке прототипов, обходя свойства объекта.

> **Примечание:** Цикл `for in` **не** обходит те свойства объекта, у которых внутренний атрибут `enumerable` установлен в `false`; как пример - свойство `length` у массивов

    // Подпортим Object.prototype
    Object.prototype.bar = 1;

    var foo = {moo: 2};
    for(var i in foo) {
        console.log(i); // печатает и bar и moo
    }

Так как изменить поведение цикла `for in` как такового не представляется возможным, то для фильтрации нежелательных свойств объекта внутри этого цикла используется метод [`hasOwnProperty`](#object.hasownproperty) из `Object.prototype`.

> **Примечание:**  Цикл `for in` всегда обходит всю цепочку прототипов полностью: таким образом, чем больше прототипов (слоёв наследования) в цепочке, тем медленнее работает цикл.

### Использование `hasOwnProperty` в качестве фильтра

    // всё то же foo из примера выше
    for(var i in foo) {
        if (foo.hasOwnProperty(i)) {
            console.log(i);
        }
    }

Это единственно правильная версия использования такого цикла. За счёт использования `hasOwnProperty` будет выведено одно **только** свойство `moo`. Если же вы уберёте проверку `hasOwnProperty`, код станет нестабилен и, если кто-то позволил себе изменить прототипы встроенных типов, такие как `Object.prototype`, у вас возникнут непредвиденные ошибки.

Один из самых популярных фреймворков [Prototype][1] использует упомянутое расширение `Object.prototype` — и если вы его подключаете — ни в коем случае не забывайте использовать `hasOwnProperty` внутри всех циклов `for in` — иначе у вас гарантированно возникнут проблемы.

### Рекомендации

Рекомендация одна — **всегда** используйте `hasOwnProperty`. Пишите код, который будет в наименьшей мере зависеть от окружения, в котором он будет запущен — не стоит гадать, расширял кто-то прототипы или нет и используется ли в нём та или иная библиотека.

[1]: http://www.prototypejs.org/

