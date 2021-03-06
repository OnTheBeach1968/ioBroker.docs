---
translatedFrom: en
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/dev/aliases.md
title: Псевдонимы
hash: NWUy6T4dxNPqgj/4GIA3p697eSGnvkF0Twd3+v0eRmg=
---
# Псевдонимы
Псевдонимы (псевдонимы) - это объекты виртуальных состояний, которые связаны с реальными состояниями.

## Сценарии использования
Часто реальные устройства будут сломаны, и пользователь должен заменить это устройство.
Кроме того, что оборудование будет заменено, адрес этого устройства будет изменен. Например. от `hm-rpc.0.ABC123` до `hm-rpc.0.QJU978`.

Поскольку старый адрес использовался во многих местах, таких как vis, javascript, сцены или другие, пользователь теперь должен найти все эти места и заменить их там.

Эта функция позволяет пользователю выделить псевдоним для физического устройства, а затем использовать этот псевдоним во всех случаях.
Поскольку устройство должно быть заменено, идентификатор должен быть изменен только в псевдониме.

Другой вариант использования этой функции - поддержка устройства в специальных интеллектуальных адаптерах, таких как iot или material.
С помощью псевдонимов можно создать необходимую структуру состояний, но значения будут считываться с физических устройств.

## Объяснение
Все состояния, созданные в пространстве имен объектов `alias.0`, будут управляться как псевдонимы.

Значение состояния псевдонима будет считываться из связанного состояния (цель), но значения объекта (как общие, собственные) будут считываться из состояния псевдонима сам.

Фактически объект `alias` будет отражать значение состояния целевого объекта.
Если это разрешено, оба состояния могут быть изменены и автоматически синхронизированы базовой системой ioBroker.
Также оба состояния могут использоваться для подписки в сценариях и должны вести себя абсолютно одинаково.

Вот пример такого объекта:

```
{
  "_id": "alias.0.Light.Device_1.WORKING",
  "type": "state",
  "common": {
    "alias": {
      "id": "admin.0.connected"
    },
    "name": "WORKING",
    "role": "indicator.working",
    "type": "boolean"
  },
  "native": {}
}
```

`native` всегда пуст, потому что ни одно устройство не находится за самим псевдонимом, и все настройки будут сохранены в `common`.

Но в `common.alias.id` хранится идентификатор, где значение состояния должно быть считано или записано.

Псевдоним выполняет автоматическое преобразование значения, если заданы минимальные / максимальные параметры для обоих объектов (псевдоним и целевой объект)

Например. если у псевдонима есть `min=0,max=100`, а у цели - `min=0,max=255`, то при чтении значение 10 из целевого состояния будет преобразовано в 3.9215686274509802, а записанное в псевдоним 10 будет преобразовано в 25.5.

Типы будут преобразованы автоматически. От строки к номеру, от числа к логическому значению и так далее. Зависит от типа псевдонима и цели.

Кроме того, функции записи и чтения могут быть определены в `common.alias`:

```
{
  "_id": "alias.0.Temperature.SET",
  "type": "state",
  "common": {
    "alias": {
      "id": "knx.0.6786878.value",
      "write": "(val * 9/5) + 32",
      "read": "(val − 32) * 5/9"
    },
    "unit": "°C",
    "name": "Temperature",
    "role": "value.temperature",
    "type": "number"
  },
  "native": {}
}
```

а также

```
{
  "_id": "knx.0.6786878.value",
  "type": "state",
  "common": {
    "unit": "°F",
    "name": "Temperature",
    "role": "value.temperature",
    "type": "number"
  },
  "native": {}
}
```

Если определены функции преобразования, автоматическое преобразование будет деактивировано. Только для чтения функция записи может быть опущена, соответственно, для функций только для записи - функция чтения.

Например.

```
{
  "_id": "alias.0.button",
  "type": "state",
  "common": {
    "alias": {
      "id": "knx.0.6786879.value",
      "write": "val ? 1 : 0"
    },
    "name": "Button",
    "role": "button",
    "type": "boolean"
  },
  "native": {}
}
```

а также

```
{
  "_id": "knx.0.6786879.value",
  "type": "state",
  "common": {
    "name": "KNX Switch",
    "role": "value",
    "type": "number",
    "min": 0,
    "max": 1
  },
  "native": {}
}
```

Подписки будут управляться автоматически. Если псевдоним будет подписан, значит, целевой идентификатор тоже будет подписан.

Идентификатор целевого устройства может быть изменен динамически (через администратора), и подписка будет обновлена для нового целевого идентификатора.