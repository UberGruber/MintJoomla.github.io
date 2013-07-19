---
layout: doc
title:  "Создание полей Cobalt 8. Часть 1. Основы и базовые функции."
date:   2013-07-16 18:53:38
tags: поля
---

##Введение.

В серии статей про создание полей планируется подробно рассмотреть все основные вопросы создания своих полей для конструктора контента Cobalt 8 на примере простого текстового поля.

Планируемые статьи:

**Часть 1. Основы и базовые функции.**
	
Основные принципы создания полей. Создание простого текстового поля с минимально необходимым функционалом (ввод, вывод, возможность полнотекстового поиска).

**Часть 2. Фильтрация и сортировка.**

Основные принципы построения полей с возможностью фильтрации и сортировки. Создание простого текстового поля с возможностью фильтрации и сортировки.

**Часть 3. Шаблоны.**

Основные принципов построения собственных шаблонов поля. Создание примера шаблона поля.

**Часть 4. Языковые пакеты.**

Способы создания языковых пакетов. Создание языковых пакетов для русского и английского языков.

**Часть 5. Импорт и экспорт.**

**Часть 6. Установочный пакет.**

Основные принципы создания установочных пакетов Joomla. Создания установочного пакета.

Результатом каждой статьи должно стать полностью работоспособное поле с определенным функционалом. К каждой статье будет прилагаться установочный пакет.

Во всех примерах будет рассматриваться создание поля с именем `simpletext`.

##Основные принципы построения полей.

Все поля CCK Cobalt хранятся в папке `components/com_cobalt/fields`. Каждое поле хранится в собственной папке. Имя папки должно быть одинаковым с именем поля и состоять только из латинских букв в нижнем регистре.

Таким образом, все файлы поля `simpletext` должны находиться в папке `components/com_cobalt/fields/simpletext`.

Папка `components/com_cobalt/fields/simpletext` **обязательно** должна содержать следующие подпапки и файлы:

1. `simpletext.xml` - `.xml` файл, содержит метаданные и параметры шаблонов поля.
2. `simpletext.php` - `.php` файл, содержит главный класс поля. Осуществляет обработку метаданных и параметров поля из файла `.xml` посредством предопределенных методов класса. Иными словами, методы класса поля обрабатывают логику работы поля и подготавливают набор необходимых переменных для работы шаблонов поля. Рекомендуется всю "логику" поля включать в методы класса поля, а HTML код поля- в файлы шаблонов.
3. `/tmpl/input/default.php` - файл шаблона по умолчанию для формы ввода.
4. `/tmpl/output/default.php` - файл шаблона по умолчанию для формы вывода в полном виде статьи или в списке статей.

Также в папке `components/com_cobalt/fields/simpletest` **могут находиться** следующие подпапки и файлы:

1. `/assets` - папка для хранения вспомогательных файлов, например `.css` файлов таблиц стилей, `.js` файлов JavaScript и других необходимых файлов. Имя этой папки является рекомендуемым. Важно помнить, что файлы из этой папки не подключаются Cobaltом автоматически, их необходимо подключать принудительно.
2. `/language` - папка для хранения .ini языковых файлов этого поля.
   Имена файлов должны быть в следующем формате `[код языка].com_cobalt_field_[имя поля].ini` (например `en-GB.com_cobalt_field_simpletext.ini` или `ru-RU.com_cobalt_field_simpletext.ini`). При установке поля языковые файлы из этой папки будут перенесены установщиком в папку `language` Joomla.     

3. `/tmpl/filter/default.php` - файл шаблона по умолчанию для вывода в форме фильта. 

4. `simpletext.png` - файл- иконка поля размером 16x16 px.
    Обычно мы используем иконки из набора [Fugue Icons][1], но Вы можете использовать любые иконки размером 16x16 px. Этот файл не является обязательным. Если его нет, по умолчанию будет показана иконка `text.png`.

[1]: http://p.yusukekamiyamane.com/

## Файл simpletext.xml

Содержит метаданные и параметры поля.

Структура файла должна быть следующей.

	<?xml version="1.0" encoding="utf-8"?>
	<cobaltfield>
		<name>Simple Test</name>
		<group>Simple From Element</group>
	
		<config>
			<fields name="params">
				<fieldset name="tmpl">
					
				</fieldset>
			</fields>
		</config>
	</cobaltfield>

Файл содержит 3 основных раздела, определяемых тэгами верхнего уровня: **name, group, config**.

### Раздел `<name>`

Определяет имя поля. Это имя будет показываться в выпадающем списке выбора типа поля.

### Раздел `<group>`

Определяет группу полей, к которой будет принадлежать поле.

Все поля объединяются в группы на основании содержания этого тэга.

Вы можете включить новое поле в одну из существующих предопределенных групп полей или создать свою собственную группу.

Если необходимо создать свою группу полей, достаточно просто поместить название этой группы внутри тэга `<group>...</group>`. Никаких дополнительных действий не требуется, Cobalt автоматически создаст эту группу.

Предопределенные группы полей CCK Cobalt.

Имя группы      		| Описание
------------------------|-------------
Simple Form Elements	| Простые поля типа text, checkbox, radio, select и пр.
Special Form Elements	| Поля для вывода данных в специальном формате типа telephone, url и пр. 
Media Form Elements		| Поля для работы с медиа контекстом типа gallery, image, video, audio, uploads и пр.
Commerce Form Elements	| Поля для электронной коммерции, работа этих полей основана на [SSI][2]
Exclusive Form Elements	| Различные эксклюзивные поля.
Relation Form Elements	| Поля для создания связей между элементами контекста и отображающие списки связанных элементов.

[2]: http://www.mintjoomla.com/blog/item/53-what-is-ssi-in-depth.html

### Раздел `<config>`

Определяет параметры поля.

Структура раздела `<config>` должна быть следующей

	<config>
		<fields name="params">
			<fieldset name="general" label="ST_LABEL1">
				<field name="param1" type="text" label="ST_PARAM1" />
			</fieldset>
			<fieldset name="playerlist" label="ST_LABEL2">
				<field name="param2" type="text" label="ST_PARAM2" />
			</fieldset>
		</fields>
	</config>

Таким образом, раздел `<config>` состоит из наборов групп полей, ограниченных тэгами `<fieldset>...</fieldset>` и наборов полей внутри группы, ограниченных тэгами `<field>...</field>` с обязательным атрибутом `name="params"`.

Количество наборов групп полей и количество полей внутри каждой группы не ограничено. Естественно, имена групп полей и самих полей должны быть уникальными.

Существует 3 предопределенных группы полей, которые используются ядром Cobalt:

#### Группа полей `tmpl`

Определяет месторасположение шаблонов поля.
		
	<fieldset name="tmpl">
		<field type="filelist" name="template_input" filter="php$" hide_none="1" hide_default="1" directory="/components/com_cobalt/fields/simpletext/tmpl/input" label="F_TMPLINPUT" default="default.php" />
		<field type="filelist" name="template_output_list" filter="php$" hide_none="1" hide_default="1" directory="/components/com_cobalt/fields/simpletext/tmpl/output" label="F_TMPLLIST" default="default.php" />
		<field type="filelist" name="template_output_full" filter="php$" hide_none="1" hide_default="1" directory="/components/com_cobalt/fields/simpletext/tmpl/output" label="F_TMPLFULL" default="default.php" />
	</fieldset>

где

- `type="filelist"` - тип параметра, в данном случае- список файлов;
- `name="template_input"` - имя параметра;
- `filters="php$"` - фильтр типа файла, в данном случае- только файлы с расширением .php;
- `hide_none="1"` - причет в списке файлов возможность не выбрать ни одного файла или выбрать пустое значение.
- `hide_default="1"` - прячет возможность выбора значения по умолчанию. Такой параметер приминим только если он наследует данные от других параметров.
- `directory="/components/com_cobalt/fields/simpletext/tmpl/input"` - месторасположение файла шаблона;
- `label="F_TMPLINPUT"` - языковая константа ярлыка параметра;
- `default="default.php"` - имя файла шаблона по умолчанию;
	
	
Первая строка определяет местоположение шаблона формы ввода поля.
	
Вторая строка определяет местоположение шаблона формы вывода поля в списке статей.
	
Третья строка определяет местоположение шаблона формы вывода поля в полном виде статьи.

#### Группа полей `core`.

Определяет возможность сортировки по полю.

Включается в файл при необходимости сортировки по полю. Будет подробно рассматриваться в Часть 2 - Фильтрация и сортировка.

#### Группа полей `filter`.

Определяет возможность фильтрации по полю.

Включается в файл при необходимости фильтрации по полю. Будет рассматриваться в Часть 2 - Фильтрация и сортировка.

Остальные группы параметров можно добавлять по Вашей необходимости.

## Файл simpletext.php

Данный файл содержит программный код класса поля и должен начинаться со следующего блока кода

	<?php
	defined('_JEXEC') or die;
	require_once JPATH_ROOT.DS.'components/com_cobalt/library/php/fields/cobaltfield.php';
	class JFormFieldCSimpletext extends CFormField
	{
		[здесь должны располагаться методы класса поля]
	}

Название класса должно быть `JFormFieldCSimpletext`, где `JFormFieldC` есть обязательный префикс имени класса, а `Simpletext` есть имя поля, начинающееся с большой буквы.

Родительский класс `CFormField` находится в файле `/components/com_cobalt/library/php/fields/cobaltfield.php`

Для выполнения необходимых операций с полем (ввод, вывод, фильтрация по полю и пр.), класс поля должен содержать набор предопределенных методов.

### Предопределенные методы класса поля.

Сводная таблица предопределенных методов класса поля.

**Базовые функции поля.**

Метод | Краткое описание
---------|-------------
getInput() | Выводит форму ввода поля.
validate() | Проверяет правильность ввода значения поля перед его сохранением на сервере в базе данных.
onJSValidate() | Позволяет подключать проверку правильности ввода значения поля на форме ввода посредством JavaScript.
onPrepareSave() | Сохраняет значение поля в базе данных.
onPrepareFullTextSearch() | Сохраняет значение поля для полнотекстового поиска.
onRenderList() | Выводит значение поля в списке статей.
onRenderFull() | Выводит значение поля в полном виде статьи.

**Фильтрация и сортировка.** Будут рассматриваться в Часть 2- Фильтрация и сортировка.

Метод | Краткое описание
---------|-------------
onStoreValues() | Сохраняет значение поля для последующей фильтрации или сортировки по этому полю.
onRenderFilter()| Выводит поле в форме фильтра.
onFilterWhere() | Обрабатывает основной SQL запрос на получения списка статей что бы фильтровать его по установленным фильтром значением.
onFilterWornLabel() | Текст кторый покажется в сообщении о том что данный фильтр установлен.

**Импорт.** Будут рассматриваться в часть 5- Импорт и экспорт.

Метод | Краткое описание
---------|-------------
onImport() | ?????.
onImportData() | ?????.
onImportForm() | ?????.

Предопределенные функции родительского класса поля предназначены, в основном, для подготовки необходимых переменных для шаблонов поля. В шаблоне поля эти переменные можно получить через `$this`. Т.е. рекомендуется всю "логику" работы поля включать в методы класса поля, а HTML код поля- в файлы шаблонов.

### метод getInput()

Выводит форму ввода поля через шаблон в `fields/simpletext/tmpl/input/[filename].php` устнаовленый в натройках поля.

Код метода по умолчанию:

	public function getInput()
	{
		[код пользователя]
		return $this->_display_input();
	}

Перед `return $this->_display_input();` можно поставить свой фрагмент кода и установить необходимые переменные в `$this`. Потом к этим переменнм можно будет иметь доступ в файле шаблона.

### метод validate()

Проверяет правильность ввода значения поля перед его сохранением на сервере.

Код метода по умолчанию

	public function validate($value, $record, $type, $section)
	{
		[код пользователя]
		return parent::validate($value, $record, $type, $section);
	}

Например, для проверки значения поля на то что был введен правильный емайл адрес можно использовать следующий пример кода:

	public function validate($value, $record, $type, $section)
	{
		if ($value && !JMailHelper::isEmailAddress($value))
		{
			$this->setError(JText::sprintf('E_ENTEREDINCORRECT', $this->label));
			return FALSE;
		}
		return parent::validate($value, $record, $type, $section);
	}

Проверка на обязательность поля произойдет в родительском классе `parent::validate()`.

Если вы обнаружили ошибку то не возвращайте `TRUE` или `FALSE`, необходимо принудительно установить ошибку ввода через `$this->setError()`.

### метод onJSValidate()

Добавляет проверку правильности ввода значения поля на форме ввода посредством JavaScript. Этот метод должен возвращать строку и срабатывает в момент нажатия кнопок _Добавить_ или _Приметь_.

Вы можете легко получать доступ к Вашему полю, присваивая ему любой ID в файлах шаблонов.

Например, проверка на необходимость ввода значения поля может выглядеть так:

	public function onJSValidate()
	{
		$js = '';
		if ($this->required)
		{
			$js .= "\n\t\t if($('field_{$this->id}').value == '') {
				hfid.push({$this->id}); 
				isValid = false; 
				errorText.push('" . JText::sprintf('CFIELDREQUIRED', $this->label) . "');
			}";
		}
		return $js;
	}

Конструкция кода JavaScript очень простая

	if(<expression>)
	{
		hfid.push(12); 
		isValid = false; 
		errorText.push('This field is required');
	}

Если `<expression>` есть `TRUE` или, другими словами, если есть ошибки ввода, необходимы всего 3 действия:

1. Добавить в массив `hfid` ID поля с ошибкой. Это необходимо для подсветки этого поля. ID поля который можно получить из `$this->id`.
2. Установить переменную `isValid` в `FALSE` для остановки отправки поля.
3. Добавить сообщение об ошибке в глобальный массив ошибок `errorText.push('This field is required');` для отображения ошибки ввода. Не забудте пропустить текст ошибки через `JText::_()` для многоязыковой поддержки.

### метод onPrepareSave()

Сохраняет значение поля в базе данных.

В дальнейшем это значение можно получить в любом другом методе, например `onRenderList` или `onRenderFull`, в переменной `$this->value`.

Вы можите вернуть или строку илил массив данных. Нет необходимости его релиализовать. Это будет сделано автоматически. Вернете массив то и получите массив. Вернете строку то и получите строку в `$this->value`.

Код метода по умолчанию:
 
	public function onPrepareSave($value, $record, $type, $section)
	{
		[код пользователя]
		return $value;
	}

### метод onPrepareFullTextSearch()

Сохраняет значение поля для полнотекстового поиска.

Код метода по умолчанию:

	public function onPrepareFullTextSearch($value, $record, $type, $section)
	{
		[код пользователя]	
		return $value;
	}

Должен возвращать всегда только строку или одноуровневый массив, в котором каждое значение массива является строкой.

### метод onRenderList()

Выводит поле в списке статей через шаблон в `fields/simpletext/tmpl/output/[filename].php` устнаовленый в натройках поля.

Код метода по умолчанию:

	public function onRenderList($record, $type, $section)
	{
		[код пользователя]
		return $this->_display_output('list', $record, $type, $section);
	}

Метод, аналогичный методу `onRenderFull`.

### метод onRenderFull()

Выводит поле в полном виде статьи через шаблон в `fields/simpletext/tmpl/output/[filename].php` устнаовленый в натройках поля.

Код метода по умолчанию:

	public function onRenderFull($record, $type, $section)
	{
		[код пользователя]
		return $this->_display_output('full', $record, $type, $section);
	}

Установочный пакет поля Simple Text находится в прикрепленном файле.