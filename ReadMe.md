Общая суть сборки:

Gulp - отвечает за сборку ассетов (картинок, стилей, html)
Rollup - занимается только сборкой javascript файлов, для чего он и предназначен

Шаги сборки:

1) Собираем по пути /src/styles/**/*.scss стили
(тут есть проблема, что если туда попадет файл то он будет добавлен в сборку, не зависимо от того используется он или нет)
rollup plugin позволяет точечно делать импорты и в этом плане он работает лучше, тк собирает только то что реально используется

2) Сборка JS
На этом шаге собирается javascript (с помощью Rollup), также из разрозненных файлов в один

3) Assets
На этом шаге также по маске копируются картинки из папки и обжимаются, отправляясь в dist директорию

4) HTML
Берем наш HTML файл и добавляем в него css & js подключение и отправляем его в dist папку


Выполняем установку gulp, командой `npm i gulp --save-dev`, --save-dev  или -D - означает что зависимость будет сохранена в девелоперские.
Далее создаем файл `gulpfile.json` в корне проекта.

В моем gulpfile можно увидеть следующие импорты

const gulp = require('gulp');
const concat = require('gulp-concat');
const less = require('gulp-less');
const inject = require('gulp-inject');
const rollup = require('rollup');
const image = require('gulp-image');

Это старый вид импорта он был до синтаксиса `import gulp from 'gulp';` и еще не везде обновлен, к сожалению
Поскольку это импорты, нужно все их установить командой `npm i -D %name`, вместо  %name укажите имена подключаемых модулей

Файл rollup.config.js - уехал в gulpfile.js и теперь вся конфигурация Rollup внутри этого файла. отдельный факйл конфигурации для Rollup можно удалить

SCSS файлами теперь занимается gulp, тк это в целом не задача Rollup, так более правильно. Однако если вы хотите использовать импорты стилей внтруи JS файлов, придется все же переложить на Rollup эту задачу, добавив ему плагин

В нашем файле `index.html` добавляем по примеру точки вставки стилей и js для плагина gulp , gulp-inject

```
<!DOCTYPE html>
<html>
<head>
  <title>My index</title>
  <!-- inject:css -->
  <!-- endinject -->
</head>
<body>

  <!-- inject:js -->
  <!-- endinject -->
</body>
</html>
```
