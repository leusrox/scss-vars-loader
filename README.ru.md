# @funboxteam/scss-vars-loader

[![npm](https://img.shields.io/npm/v/@funboxteam/scss-vars-loader.svg)](https://www.npmjs.com/package/@funboxteam/scss-vars-loader)

Лоадер для Вебпака, добавляющий в обрабатываемые файлы переменную `$b` с именем текущего БЭМ-блока в качестве значения.

## Мотивация

При разработке веб-приложений мы используем БЭМ, в том числе и на файловой системе.
Иногда блоки получаются довольно-таки развесистыми, и вдруг всплывает необходимость их переименования. Чтобы не грепать
по всем файлам и не заменять имя блока, мы решили унифицировать его в SCSS-файлах, используя везде переменную `$b`. 

Помимо прочего такой подход позволяет генерировать SCSS-файлы по шаблону, не заботясь о правильном имени селектора в них.  

## Установка

Установить лоадер в проекте:

```bash
npm install --save-dev @funboxteam/scss-vars-loader
```

Подключить в конфиге Вебпака так, чтобы он вызывался перед sass-loader:

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          // ...
          'sass-loader',
          '@funboxteam/scss-vars-loader',
          // ...
        ],
      },
    ],
  },
};
```

## Использование

После настройки переменную `$b` можно использовать при описании стилей:

```scss
.#{$b}__elem_mod_value {
  color: red;
}
```

## Детали реализации

Во время сборки в начало каждого обрабатываемого файла добавляется подобная строка:

```scss
$b: 'blockname';
```

Чтобы правильно определить имя блока, лоадер проходит по пути до обрабатываемого файла справа навлево 
в поисках первой папки, имя которой не начинается с `_`. Найдя такую папку он и использует её имя 
в качестве значения для `$b`.

[![Sponsored by FunBox](https://funbox.ru/badges/sponsored_by_funbox_centered.svg)](https://funbox.ru)
