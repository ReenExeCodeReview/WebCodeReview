## Код нужно писать в одном стиле

В PHP сообществе существует два стандарта оформления [PSR-1](http://www.php-fig.org/psr/psr-1/) и [PSR-2](http://www.php-fig.org/psr/psr-2/)

Переводы на русский [PSR-1](http://svyatoslav.biz/misc/psr_translation/#_PSR-1) и [PSR-2](http://svyatoslav.biz/misc/psr_translation/#_PSR-2)

### Есть инструменты для проверки стандартов которые легко подключить в проект через composer
* [CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) (+ можно настроить для PHPStorm)
* [Mess Detector](https://github.com/phpmd/phpmd) (+ можно настроить для PHPStorm)
* [PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer) (автоматизация приведения кода в нужный стиль, замена кавычек array() -> []) ([Пример замены в открытом проекте](https://github.com/ruflin/Elastica/pull/1141))


## На стыке JS & HTML
* Селекторы которые используем в JS лучше начинать с `js`
* Данные правильно хранить в атрибутах `data-` (HTML5)

##### Пример:
```html
    <ul id="js-basket-list">
        <li class="js-basket-item" data-basket-id="17">
            ...
        </li>
    <ul>
```

## Лучшие практики

### Явные
**1 Операции фильтрации данных (иногда обработки) лучше делать на стороне БД**
```php
$sql = <<<'SQL'
    SELECT DISTINCT `status`
    FROM ...
SQL;

$list = fetchAll($sql);
```

Вместо

```php
$sql = <<<'SQL'
    SELECT `status`
    FROM ...
SQL;

$list = array_unique(fetchAll($sql));
```

**2 Использование встроенных функций**
```php
if (strcasecmp($requestAlias, $storedAlias) === 0) {
    ...
}
```
вместо
```php
if (strtolower($requestAlias) === strtolower($storedAlias)) {
    ...
}
```
**3 Тонкий контроллер, толстая модель**

Код нужно писать в сервисах или моделях, а контроллер уже использует их

### Плохие запахи
**1 Запросы к БД в цикле**
```php
        $result = [];
        foreach ($baskets as $basket) {
            $result[$basket->getId()] = fetchProduct($basket->getProductCode());
        }
```

Лучше:
```php
        $productCodes = [];
        foreach ($baskets as $basket) {
            $productCodes[] = $basket->getProductCode();
        }
        
        $result = fetchProducts($productCodes);
```

**2 Лишние конструкции** `else` | `elseif` | `else if`
```php
public function iHaveCookie($nameCookie)
{
    if ($this->getSession()->getCookie($nameCookie)){
        return true;
    } else {
        throw new \Exception("Cookie with name $nameCookie not found");
    }
}
```
Лучше:
```php
public function iHaveCookie($nameCookie)
{
    if ($this->getSession()->getCookie($nameCookie)){
        return true;
    }
         
    throw new \Exception("Cookie with name $nameCookie not found");
}
```
В больших функциях - это ранее возвращение результата `early return`

**3 Перемешаны методы и свойства с разными областями видимости**
```
    private function resolve()
    {
        ...
    }
    
    public function find()
    {
        ...
    }

    private function findByCode()
    {
        ...
    }

    private $id;
```

**4 Двойные проверки**
```
if (isset($variable) && $variable) {
    ...
}
```
правильнее заменить
```
if (!empty($variable)) {
    ...
}
```

**5 Проверки с двойным отрицанием**
```php
$variable = getSomeThing();
if (!empty($variable)) {
    ...
}
```
правильнее заменить
```php
$variable = getSomeThing();
if ($variable) {
    ...
}
```
[Приведение значений к "bool" прямое, через конструкции или функции](http://php.net/manual/ru/types.comparisons.php)

Все свойства и константы нужно объявлять сверху
Дальше public методы
Дальше protected и private

## Покрытие тестами
Основной функционал нужно покрывать тестами и учитывать это время при обсуждении задачи и оценке

Пример для интернет магазинов - это добавления в корзину и оформления заказа

## Контроль версий
В коммитах нужно размещать ссылку на задачу:
Примеры:
* Для GitHub это [Autolinked references and URLs](https://help.github.com/articles/autolinked-references-and-urls/)
```
Add PSR-1 & PSR-2 #1
```
* Для проектов с JIRA это название проекта и номер задачи
```
move to new framework (HD-15)
```

### Можно дополнительно глянуть:
* [Right Qay](http://www.phptherightway.com/)
* [Best Practices](https://phpbestpractices.org/)
* [Javascript Best Practices](https://github.com/stevekwan/best-practices/blob/master/javascript/best-practices.md)
