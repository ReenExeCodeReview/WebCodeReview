# WebCodeReview

### Код должен быть описан в одном стиле - так его будет проще читать.
В PHP сообществе существует два стандарта оформления [PSR-1](http://www.php-fig.org/psr/psr-1/) и [PSR-2](http://www.php-fig.org/psr/psr-2/)
Переводы на русский [PSR-1](http://svyatoslav.biz/misc/psr_translation/#_PSR-1) и [PSR-2](http://svyatoslav.biz/misc/psr_translation/#_PSR-2)

##### Есть инструменты для проверки стандартов которые легко подключить в проект через [composer]
* [CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) (+ можно настроить для PHPStorm)
* [Mess Detector](https://github.com/phpmd/phpmd) (+ можно настроить для PHPStorm)
* [PHP-CS-Fixer] (https://github.com/FriendsOfPHP/PHP-CS-Fixer) (автоматизация приведения кода в нужный стиль, замена кавычек array() -> []) ([Пример замены в открытом проекте](https://github.com/ruflin/Elastica/pull/1141))