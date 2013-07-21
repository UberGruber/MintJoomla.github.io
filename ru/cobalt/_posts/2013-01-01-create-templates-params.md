---
layout: doc
title:  "Создание шаблонов - параметры"
date:   2013-07-16 18:53:38
tags: templates developer
---
<div class="alert">Для понимания этой темы необходимо ознакомится с темой <a href="/ru/cobalt/create-templates-general/">основы работы с шаблонами</a>.</div>

В этой статье хотелось бы более подробно остановиться на параметрах шаблонов.

Все параметры шаблона находятся в специальном файле с расширением *.xml* и именем, одинаковым с именем самого шаблона.

Для примера рассмотрим шаблон полного вида статьи *default*.

Файлы шаблонов полного вида статьи находятся в папке `views/record`.Полное имя файла состоит из префикса и названия шаблона. Для полного вида статьи префикс названия файла должен быть `default_record_`, а полное имя файла, соответственно будет `default_record_default.php`. Более подробную информацию по этому вопросу Вы можете найти в [уроке о работе с шаблонами Cobalt][u1]. Я даже порекомендовал бы его для обязательного предварительного изучения.

[u1]: http://cobalt-cck.ru/%D0%B4%D0%BE%D0%BA%D1%83%D0%BC%D0%B5%D0%BD%D1%82/%D1%81%D1%82%D0%B0%D1%82%D1%8C%D1%8F-%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8F/852-pavel-ivanov/16-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%BE%D0%B2-cobalt-ver-1-4.html

Параметры этого шаблона находятся в одноименном *XML* файле `default_record_default.xml`.


## Структура файла параметров шаблона

Рассмотрим пример файла настроек.

	<?xml version="1.0" encoding="utf-8"?>
	<form>
		<name>Default</name>
		<author>MintJoomla</author>
		<creationDate>Febrary 2012</creationDate>
		<copyright>Copyright (C) 2005 - 2012 MintJoomla. All rights reserved.</copyright>
		<license>Commercial (http://www.mintjoomla.com/commercial-license)</license>
		<authorEmail>support@mintjoomla.com</authorEmail>
		<authorUrl>www.mintjoomla.com</authorUrl>
		<version>8.36</version>
		<description>Default article full view template</description>
	
		<fields name="tmpl_params">
			<fieldset name="name" label="Fieldset Nmae">
				<field>
					<option value="1">Value 1</option>
				</field>
			</fieldset>
		</fields>
		
		<fields name="tmpl_core">
			<fieldset name="name2" label="Fieldset Name 2">
				<field />
			</fieldset>
		</fields>
	</form>

Несколько важных моментов, которые надо понимать:

- Вся структура *XML* должна быть обязательно заключена в коневой тег `<form>`. 

- Верхняя группа параметров вида `<name>`, `<author>`, `<version>` и других нужна для заполнения информации об авторе этого шаблона и его контактных данных, чтобы пользователь легко мог найти автора и обратиться к нему при наличии вопросов.

- В файл параметров шаблона может быть добавлено только 2 раздела `<fields>` с фиксированными именами- `tmpl_params` и `tmpl_core`. Вы можете добавлять свои параметры в раздел `tmpl_params`. Раздел `tmpl_core` следует изменять с осторожностью, т.к. его может использовать ядро Cobalt. Иными словами, настройки раздела `tmpl_core` должны присутствовать обязательно.

- Каждый раздел `<fields>` может содержать сколько угодно элементов `<fieldset>`. Элемент `<fieldset>` позволяет объединять параметры в группы, что сильно упрощает их зрительное восприятие, особенно если параметров много. Объединив параметры в группы, Вы упростите их чтение.

## Группа параметров *fieldset*

	<fieldset name="name" label="Group Name" description="Group description"></fieldset>

допустимые атрибуты группы параметров *fieldset*

Название | Обязательный | Описание
---------|--------------|---------
name | да | должно быть уникально для всех групп настроек
label | да | отображается как заголовок группы параметров на форме настроек параметров шаблона
description | нет | дополнительное описание группы. Возможно использование HTML тегов в тексте этого атрибута. Например, если Вы хотите видеть в описании строку `This is <b>group</b> description`, то текст будет `This is &lt;b&gt;group&lt;/b&gt; description`. Также возможно использование языковых констант. Например, можно вставить в описание параметра ключ типа `MY_GRPUP_DESCR` и добавить перевод значения в языковой файл. Но с такой схемой будет неудобно распространять шаблоны, так как пока что шаблоны не имеют своих языковых файлов.
addfieldpath | нет | Атрибут указывает путь расположения PHP классов ваших собственных типов. Должен начинаться от корня Joomla. Например: `addfieldpath="/libraries/mint/forms/fields/`

## Параметр *field*

	<field name type label [class default description]>
		[<option value="1">Value 1</option>]
	</field>

Элемент `<field>` может содержать основные и дополнительные атрибуты.

Все атрибуты записываются в формате `имя="значение"`, атрибуты отделяются друг от друга пробелом. Состав дополнительных атрибутов зависит, в основном, от значения атрибута `type`.

Вот описание основных атрибутов элемента `<field>`

|Название | Обязательный | Описание|
|---------|--------------|---------|
|name | да | Идентификатор для получения значения этого параметра шаблона. Допустимы только латинские буквы в нижнем регистре без спецсимволов.	 
|type | да | Тип отображаемого элемента, например переключатель, выпадающий список или просто поле для ввода текста. Каждый тип имеет соответствующий PHP класс, который его прорисовывает. Таким образом, при желании можно создавать свои классы и добавлять любые типы элементов, которые только можно себе представить.|
|label | да | Ярлык параметра. Можно использовать языковые константы.|
|class | нет | CSS класс для вывода параметра, применяется только с элементами text, textarea, list, radio.|
|default | нет | Значение параметра по-умолчанию. Обратите внимание, что это значение только для заполнения формы. Это значение не передается в сам шаблон, пока пользователь не сохранил значение хотя бы один раз. Для того что бы получить это значение по умолчанию в самом шаблоне, надо его опустить как 2-й параметр. Например `$params->get('tmpl_params.my_parameter', 10)` где 10 это то что вернется, если пользователь ни разу не сохранял форму.|
|description | нет | Описание параметра. Его пользователь увидит в tooltip при наведении на ярлык параметра.|

## Опция *type*

Рассмотрим несколько значений атрибута *type*. На самом деле их существует гораздо больше, но этот основной набор покроет 99%-100% ваших нужд.

### Text

`type="text"`

Текстовое поле ввода. Превратится в `<input type="text">` в HTML.

Название | Обязательный | Описание
---------|--------------|---------
size | нет | Размер поля. Превратится в атрибут `size` элемента `<input>`. В Joomla 3.0 это практически не применяется. Даже при наличии этого элемента размер текстового поля все равно будет переписан в стилях.


### Textarea

`type="textarea"`

Область ввода произвольного текста.

Название | Обязательный | Описание
---------|--------------|---------
size | нет | Размер поля. Превратится в атрибут `size` элемента `<input>`. В Joomla 3.0 это практически не применяется. Даже при наличии этого элемента размер текстового поля все равно будет переписан в стилях.
rows | нет | Количество строк
cols | нет | Количество столбцов

### List

`type="list"`

Выпадающий список значений для выбора. В HTML превратится в `<select>`

Название | Обязательный | Описание
---------|--------------|---------
multi | нет | Определяет единственный или множественный выбор. Может быть `true` или `false`. По умолчанию равен `false`.

Так же как список значений, этот XML элемент может включать в себя элементы `<option>`. Например:


	<field name="search_mode" type="list" default="3" label="Режим поиска">
		<option value="1">Искать везде</option>
		<option value="2">Искать в категории</option>
		<option value="3">Искать в ветке</option>
	</field>


### Radio

`type="radio"`

Блок radio-кнопок.

Особых атрибутов нет, может включать в себя элементы `<option>`.

Например:

	<field name="search_title" type="radio" class="btn-group" default="1" label="Искать в заголовке">
		<option value="0">CNO</option>
		<option value="1">CYES</option>
	</field>


### Meresourcesfields

`type="meresourcesfields"`

Список полей из типов контента Cobalt. Это один из самых полезных и практичных параметров в контексте шаблонов кобальта. Предположим, Вам необходимо разместить значение какого-то поля в определенной позиции шаблона с отдельными стилями. Это легко можно сделать, просто вызвав это поле в шаблоне по его `ID`. Примерно так.

	<?php echo $item->fields_by_id[11]->result ?>

Где `11`- это `ID` поля. Читайте [эту статью][u1] для более подробной информации. 

Но если вы не знаете, какой будет `ID` или хотите предоставить пользователю возможность самому выбрать, где и какое поле разместить, вот тогда этот параметр и придет на помощь.

Название | Обязательный | Описание
---------|--------------|---------
key | нет |	Определяет, какой параметр поля будет сохраняться `id` или `key`. В зависимости от этого в шаблоне нужно будет использовать `$item->fields_by_id` или `$item->fields_by_key`. В чем разница и что нужно выбрать? Ответить просто- для шаблонов с многотиповостью необходимо использовать `key`, а для шаблонов с одним типом контента- `id`. Но мы рекомендуем использовать `id`, так как этот метод более универсален и в шаблоне всегда можно получить ключ поля по `id`. Читайте [этот урок][u1] для более подробной информации.
client | нет | Ограничение списка полей.  Может быть `list`, `full` и `all`. Если `list`- показываются только те поля типа контента, которые можно отображать в списке статей. Если `full`- показываются только те поля, которые можно отображать в полном виде статьи. Если `all`- показываются все поля. По умолчанию- `all`.
filters | нет | Позволяет включать в список поля только определенных типов. Например: `filters="'textarea','text','html'"`. Включает в список выбора только поля типов `textarea`, `text` и `html`
multi | нет | Определяет единственный или множественный выбор. Может быть `true` или `fale`. По умолчанию- `fale`. 
size | нет | Определяет максимальный размер высоты выводимого списка или количество видимых элементов выбора без прокручивания.

Внимание: шаблон основной опции *name* должен быть name="field_имя_поля"

### accesslevel

`type="accesslevel"`

Список уровней доступа типа public, registered, special, Дополнительные атрибуты отсутствуют, но при необходимости можно добавить в список дополнительный элемент  `<option>`. Например:

	<field name="add" type="accesslevel" default="3" label="Кто добавит?">
		<option value="0">Ни кто</option>
	</field>


### Folderlist

`type="folderlist"`

Выпадающей список папок из указанной директории.

Название | Обязательный | Описание
---------|--------------|---------
direcotry | да | Начальная директория для выбора. Указывается от корневой директории Joomla. Например: `directory="images/story"`
hide_none | нет | Спрятать элемент списка не выбора директории. Значение 1 или 0. По умолчанию- 0.
hide_default | нет | Спрятать элемент списка выбора наследования директории. Значение 1 или 0. По умолчанию- 0.

Пожалуйста, подпишитесь или установите наблюдение за этой статьей, нажав на кнопке с иконкой ![eye](http://cobalt-cck.ru/media/mint/icons/16/follow0.png). Мы будем дополнять ее новой информацией, и Вы сможете получать уведомления при изменениях или дополнениях этой статьи.