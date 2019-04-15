---
title: Пространства имен
_old_id: '208'
_old_uri: 2.x/developing-in-modx/advanced-development/namespaces
---

## Что такое пространства имен?

Пространства имен являются организационными элементами для компонентов. Они связывают лексиконные строки и пакеты друг с другом, а также предоставляют Revolution основной способ узнать, какие объекты принадлежат какому пакету.

## Usage

Revolution использует пути к пространствам имен, чтобы определить, куда загружать файлы компонентов сторонних производителей для пользовательских страниц менеджера, а также управлять строками пользовательских языков для этих компонентов сторонних производителей.

For example, if a Namespace called "quip" has a path of "/www/modx/core/components/quip/", then when the CMP is loaded from the [Action](extending-modx/menus/actions "Actions and Menus") with a value on the controller of "index", it will look for the index controller file in the namespace path, ie: "/www/modx/core/components/quip/index.php". This will then load in place of the Custom Manager Page. A useful value for the controller on the Action is "controllers/index", so that you can point it to "/www/modx/core/components/quip/controllers/index.php" (which is the standard practice for paths for Extras in MODX).

### Лексиконы в пространствах имен

Namespaces can be used to isolate Lexicons and Lexicon Topics. For example, when loading a Lexicon, you can specify the Namespace of the topic prior to the name of the topic with a colon. For example, to load the "comment" topic for the "quip" Namespace:

```php
$modx->lexicon->load('quip:comment');
```

Assuming we're in English, this will look for the lexicon/en/comment.inc.php file in the Namespace path for Quip. This allows Lexicon topics to be loaded dynamically based on the Namespace path, rather than requiring the topic files to be put inside the MODX core directories.

- [Internationalization](extending-modx/internationalization "Internationalization")
- [Settings](_legacy/administering-your-site/settings "Settings")
