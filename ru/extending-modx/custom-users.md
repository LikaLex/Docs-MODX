---
title: Расширения для пользователей
_old_id: '355'
_old_uri: 2.x/developing-in-modx/advanced-development/extending-moduser
---

## Intended Audience

This article is for developers who are looking to add additional data to their MODX users and functionality to the related classes.

Although this is possible via a less integrated approach by simply adding a database table that includes a foreign key relation back to the original MODX users table, the approach outlined here is for a more thorough integration via extending the core modUser class.

Приведенные здесь шаги носят технический характер и основаны на базовой платформе MODX xPDO. Вы должны быть знакомы с объектами и методами xPDO (например, [getObject](extending-modx/xpdo/class-reference/xpdo/xpdo.getobject "xPDO.getObject")), прежде чем пытаться использовать это руководство.

### See Also

- [Обратный инжиниринг классов xPDO из существующей таблицы базы данных](extending-modx/xpdo/custom-models/generating-the-model/reverse-engineer "Reverse Engineer xPDO Classes from Existing Database Table")
- [More Examples of xPDO XML Schema Files](extending-modx/xpdo/custom-models/defining-a-schema/more-examples "More Examples of xPDO XML Schema Files")
- [Генерация кода модели](extending-modx/xpdo/custom-models/generating-the-model "Generating the Model Code")

## Общее представление

By extending the MODx Revolution authentication layer we can build very complex and varied user subsystems, e.g. for social networking, user management systems, or other applications not yet conceptualized. This ability to extend the modUser class is just one example of how to extend a core class – a similar approach could be used to extend any MODX core class.

## Purpose

Extending modUser is for those situations when user authentication or user interaction need to be extended or enhanced, e.g. for easier custom authentication.

## The Rules

Расширение modUser *не* означает, что мы добавляем что-либо в таблицу *modx*_users в базе данных. Вместо этого мы будем добавлять отдельную связанную таблицу, привязанную к исходной таблице через внешний ключ. Ни в коем случае расширенное приложение не должно пытаться полностью заменить класс modUser. Мы используем класс modUser в качестве нашей основы. Единственный признак того, что пользователь был расширен, будет найден путем изменения class_key с «modUser» на расширенное имя класса.

Ваше расширение должно использоваться для доступа к **вашему расширению** . Если пользователь (объект) не был расширен, не позволяйте вашему расширению взаимодействовать с ним; иначе говоря: позвольте вашему расширению умереть.

MODx Revolution уже работает с пользователями и, вероятно, не нуждается в особой помощи. Хотя мы можем использовать ваше расширение для *наших* данных, не пишите «раздутый» код, который просто повторяет код, уже предоставленный в классе modUser. Другими словами, используйте ресурсы Revolution для ваших расширенных пользователей, но не создавайте код для замены modUser.

Наконец, ознакомьтесь с [modUser](developing-in-modx/other-development-resources/class-reference/moduser "modUser"), прежде чем начинать программировать. Вы можете предположить, что некоторые методы не являются взаимно-однозначными, например, атрибуты, которые можно назначать для контекста, ресурса и т.д. Обычно для доступа к методам modUser используются указания modUser.

## Steps to extending modUser

### 1. ) Create the schema and generate a model

The first thing we need to accomplish, is to create an extended user schema which extends modUser. Please note that there is no aggregate relation upwards from your "main" class which is extending modUser.

#### Simple Example

Самый простой пример, который мы можем себе представить, - это желание добавить один дополнительный атрибут к пользовательским данным. В базе данных это будет означать, что у нас есть отдельная таблица с 2 столбцами: одна для отношения внешнего ключа обратно к таблице **modx_users** и другой столбец, содержащий наш новый «дополнительный» атрибут, например, *facebook_url*:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<model package="extendeduser" baseClass="xPDOObject" platform="mysql" defaultEngine="MyISAM" tablePrefix="ext_">
       <!-- расширяем класс modUser -->
       <object class="extUser" extends="modUser">
               <composite alias="Data" local="id" class="Userdata" foreign="userdata_id" cardinality="one" owner="local" />
       </object>
       <object class="Userdata" table="data" extends="xPDOSimpleObject">
               <field key="userdata_id" dbtype="int" precision="11" phptype="integer" null="false" attributes="unsigned"/>
               <field key="facebook_url" dbtype="varchar" precision="100" phptype="string" null="true" />
               <index alias="userdata_id" name="userdata_id" primary="false" unique="true" type="BTREE">
                   <column key="userdata_id" length="" collation="A" null="false" />
               </index>
               <aggregate alias="extUser" local="userdata_id" foreign="id" cardinality="one" owner="foreign" />
       </object>
</model>
```

Обратите внимание, что расширение класса modUser происходит в пределах одного этого *объекта*. Также обратите внимание, что мы указываем префикс для нашей вспомогательной таблицы в узле *модели*: **ext_**

#### Более сложный пример

Обратите внимание, что бит *index="unique"* устарел - объявление индекса должно попадать в свой собственный узел, как в примере выше.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<model package="extendeduser" baseClass="xPDOObject" platform="mysql" defaultEngine="MyISAM" tablePrefix="ext_">
   <!-- наследуем класс пользователя MODX и расширяем его -->
   <object class="extUser" extends="modUser">
       <composite alias="Phones" local="id" foreign="user" cardinality="many" owner="local" />
       <composite alias="Table2" local="id" foreign="user" cardinality="many" owner="local" />
   </object>
   <!-- добавляем номера телефона пользователя -->
   <object table="phone_numbers" extends="xPDOSimpleObject">
       <field key="user" dbtype="int" phptype="integer" null="false" default="0" index="index" />
       <field key="areacode" dbtype="varchar" precision="3" phptype="string" null="false" default="" />
       <field key="number" dbtype="varchar" precision="7" phptype="string" null="false" default="" />
       <aggregate alias="extUser" local="user" foreign="id" cardinality="one" owner="foreign" />
   </object>
   <!-- расширяем пользователя -->
   <object table="table2" extends="xPDOSimpleObject">
       <field key="user" dbtype="int" phptype="integer" null="false" default="0" index="index" />
       <field key="myspaceurl" dbtype="varchar" precision="255" phptype="string" null="false" />
       <aggregate alias="extUser" local="user" foreign="id" cardinality="one" owner="foreign" />
   </object>
</model>
```

You will need to parse and create the model map associated with this schema. As this process is out of the scope of this topic, please refer to [Using Custom Database Tables in your 3rd Party Components](extending-modx/tutorials/using-custom-database-tables "Using Custom Database Tables in your 3rd Party Components") for further information.

### 2.) Edit the extuser.class.php

Чтобы получить доступ к расширенному классу, мы должны сообщить modUser, что данный пользователь был расширен. Таблица *modx*_users в базе данных содержит поле специально для этой цели: class_key. Значением по умолчанию в этом поле является modUser. Когда пользователи добавляются на ваш сайт с использованием вашего расширения, нам нужно «принудительно» ввести имя «основного» класса в схеме, а именно extUser в нашем примере.

Edit the extuser.class.php file created when you generated the model. The specific file is the one found in the top of the model tree (you should see a mysql directory) in this same folder. Edit the file to resemble the following:

```php
<?php
/**
* @package extendeduser
* @subpackage user.mysql
*/
class extUser extends modUser {
   function __construct(xPDO & $xpdo) {
       parent::__construct($xpdo);
       $this->set('class_key','extUser');
   }
}
?>
```

### 3.) Создайте (или отредактируйте) *extension_packages в Системных настройках*

Перейдите к системным настройкам в системном меню менеджера и найдите [extension_packages](building-sites/settings/extension_packages "extension_packages").

**Если ключ уже существует**, добавьте в массив json

```php
,{"extendeduser":{"path":"[[++core_path]]components/extendeduser/model/"}}
```

#### If the key does not exists

- Создайте новый системный параметр с названием extension_packages
- Key of extension_packages
- Fieldtype: Textfield
- Значение:

```php
[{"extendeduser":{"path":"[[++core_path]]components/extendeduser/model/"}}]
```

### 4.) Заключительный шаг: Создайте класс для доступа и использования вашего расширенного класса

Основная причина расширения базового класса заключается в том, чтобы вы могли легче взаимодействовать со своими расширенными данными. Таким образом, в какой-то момент в сниппете, плагине или пользовательских страницах менеджера (CMP, Custom Manager Page) вы будете работать с новыми данными.

#### Simple Example

Here's how you might interact with your extended data in a Snippet:

```php
$modx->addPackage('extendeduser', MODX_CORE_PATH . 'components/extendeduser/model/', 'ext_');
$user = $modx->getObject('extUser', 123); // где 123 - это идентификатор (ID) пользователя
$data = $user->getOne('Data'); // используйте псевдоним из схемы
// toArray распечатает все дополнительные данные, например, facebook_url
return print_r($data->toArray(), true);
```

#### More complex example

```php
<?php
/**
*  File        sample.class.php (требует MODx Revolution 2.x)
* Created on    Август 18, 2010
* Project        shawn_wilkerson
* @package     extendedUser
* @version    1.0
* @category    Расширение пользователя
* @author        W. Shawn Wilkerson
* @link        http://www.shawnWilkerson.com
* @copyright  Авторское право (c) 2010, W. Shawn Wilkerson.  Все права защищены.
* @license      GPL
*
*/
if (!class_exists('Sampleclass')) {
   class Sampleclass
   {
       function __construct(modX & $modx, array $config= array ()) {
           /* Импортируем modx по ссылке */
           $this->modx= & $modx;
           /* Создадим среду */
           $this->extPath= $modx->getOption('core_path',null, MODX_CORE_PATH).'components/extendeduser/';
           $this->modx->addPackage('extendeduser', $this->extPath .'model/', 'ut_');
           $this->_config= array_merge(array (
               'userID' => $this->modx->user->get('id'),
           ), $config);
           /* Определим пользователя */
           $this->userObj = $this->setUser($this->_config['userID']);
           $this->userID = $this->userObj->get('id');
       }
       function __destruct() {
           unset ($this->extPath, $this->userObj, $this->userID, $this->_config);
       }
       /**
        * Возвращает объект с типом Phone.
        */
       public function getPhoneObj() {
           $this->userObj->getOne('Phones');
           return $this->userObj->Phones;
       }
       /**
        * Возвращает экземпляр объекта modUser, по умолчанию - текущий пользователь.
        * @param $userID
        */
       public function getUserObj($userID) {
           return $this->modx->getObject('modUser', $userID);
       }
       /**
        * Устанавливает пользователя.
        * @param int $userID
        */
       public function setUser($userID){
           return $this->getUserObj($userID);
       }
   }
}
```

#### 5.) Accessing the class

In our example we will be accessing our extended user throughout our site, therefore we load it as a service as shown in the following example:

```php
<?php
$x = $modx->getService('extendeduser','Sampleclass',$modx->getOption('core_path',null, MODX_CORE_PATH).'components/extendeduser/',$scriptProperties);
if (!($x instanceof Extendeduser)) {
   $modx->log(modX::LOG_LEVEL_ERROR,'[Extendeduser] Could not load Extendeduser class.');
   $modx->event->output(true);
}
return;
```

## Стоит отметить

1. Any pre existing user, will still have modUser as the class_key and therefore will **not** be extended or produce user objects of type extUser unless you change it
2. Дважды проверьте файл modx.mysql.schema.xml, чтобы убедиться, что вы не используете классы или псевдонимы, которые он уже использует, поскольку ваш файл заменит modUser по умолчанию, запрещающий вам доступ к таким элементам, как атрибуты пользователя (с псевдонимом Profile)
3. The extUser will **not** have a table created in the database, but the attached relations will
4. Таблица(ы) расширенных классов **должна(ы)** находиться в той же базе данных, что и обычная таблица *modx*_users
5. Признаки ошибки в шаге 3 (путь extension_packages):  
    1. Любой пользователь с class_key extUser вернет ошибку при входе в систему: «Пользователь не может быть найден ...». Если это администратор, обратитесь напрямую к вашей базе данных, верните ключ_класса в modUser, войдите в систему правильно, а затем измените путь на правильное представление пути.
    2. Прикрепленные к классу сниппеты будут работать с перебоями или вообще не работать
6. Чтобы посчитать свои данные (т.е. сколько телефонных номеров у этого человека), используйте любой вариант ниже (можно добавить любые критерии):

```php
    $this->modx->getCount('extPhones', array('user' => $this->userID));
    $this->modx->getCount('extPhones');
```

Вполне возможно иметь несколько активных расширенных систем ModUser одновременно. Было бы даже возможно расширить rpx расширение Джейсона Коварда в гибридную систему, используя преимущества обеих систем. Также вполне возможно иметь несколько расширенных приложений modUser, работающих автономно. Это может быть сделано, следуя подобному процессу для каждого из ваших расширений, изменяя только поле «class_key», чтобы отразить расширенный класс, принадлежащий каждому соответствующему пользователю.

## Suggested additional considerations

Файлы моделей можно редактировать с помощью методов и описаний. Взгляните на большую часть моделей MODx / xPDO, и вы увидите, как широко это сделано.

Этот процесс может быть автоматизирован и записан при входе пользователя. Для краткости лучше всего показать вам работу splittingred на github, где он предоставляет реальное приложение:

The plugins:

- [http://github.com/splittingred/modActiveDirectory/blob/master/core/components/activedirectory/elements/plugins/plugin.activedirectory.php](http://github.com/splittingred/modActiveDirectory/blob/master/core/components/activedirectory/elements/plugins/plugin.activedirectory.php)

The events:

- [http://github.com/splittingred/modActiveDirectory/blob/master/core/components/activedirectory/elements/events/onauthentication.php](http://github.com/splittingred/modActiveDirectory/blob/master/core/components/activedirectory/elements/events/onauthentication.php)
- [http://github.com/splittingred/modActiveDirectory/blob/master/core/components/activedirectory/elements/events/onusernotfound.php](http://github.com/splittingred/modActiveDirectory/blob/master/core/components/activedirectory/elements/events/onusernotfound.php)

## Extended modUser Classes currently Available

[modActiveDirectory](http://github.com/splittingred/modActiveDirectory) - это приложение, которое обеспечивает взаимодействие с контроллером домена Microsoft

[Расширение rpx](http://github.com/opengeek/engaged) позволяет людям войти через Facebook и другие социальные сети

## Modifying class_key

Всякий раз, когда вы изменяете ключ class_key для встроенного объекта MODX, вы должны знать, как меняется поведение. Ключ class_key влияет на то, какие агрегаты и композиты доступны объекту. Например, если у пользователя есть class_key «extUser», вы все равно можете получить объект, используя родительский класс:

```php
// Оба примера работают, когда у пользователя установлен class_key в "extUser":
$User = $modx->getObject('modUser', 123);
$User = $modx->getObject('extUser', 123);
```

However, the aggregates or composite relationships depend on the *stored value* of the class_key.

```php
$Data = $modx->newObject('Userdata');
$Data->set('facebook_url',$url); // и т.д.
$User->addOne($Data);
$User->save(); // это не сохранит связанные даннные, если class_key не имеет определенных отношений!
$User->set('class_key', 'extUser');
$User->save(); // теперь мы можем установить связанные данные
$User->addOne($Data);
$User->save(); // а теперь мы можем сохранить связанные данные
```
