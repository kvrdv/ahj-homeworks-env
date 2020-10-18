1. Если не устанавливаются пакеты через yarn:
```shell
npm config set registry https://registry.npmjs.org/
```

2. Создаем пакет [yarn](https://yarnpkg.com/configuration/manifest):
```shell
yarn init
```
* package.json
* yarn-lock.json
* .gitignore
* .browserslistrc
* .appveyor.yml

3. Добавляем [webpack](https://webpack.js.org/concepts/):
```shell
yarn add --dev webpack webpack-cli
```
* webpack.config.js

4. Добавляем [eslint](https://eslint.org/docs/user-guide/getting-started):
```shell
yarn add --dev eslint
yarn run eslint --init
```
* .eslintignore
* .eslintrc.json

5. Добавляем [babel](https://babeljs.io/setup#installation):
```shell
yarn add --dev babel babel-cli
```
* .babelrc

6. Добавляем [jest](https://jestjs.io/docs/ru/getting-started):
```shell
yarn add --dev jest
```

7. Добавляем [webpack-dev-server](https://github.com/webpack/webpack-dev-server):
```shell
yarn add --dev webpack-dev-server
```

# Нетология
## Продвинутый JavaScript в браузере
## Рабочее окружение
[Задание](https://github.com/netology-code/ahj-homeworks/tree/master/env)

---

### Yarn

#### Легенда

Вы работаете с новой командой, в которой в качестве пакетного менеджера используется `yarn`. От верстальщика вам пришёл небольшой макет, для которого необходимо собрать инфраструктуру.

#### Описание

Соберите инфраструктуру проекта на базе Webpack, ESLint, Babel, Jest, Webpack Dev Server.

Обратите внимание, что картинка, указанная в `index.html` должна попадать в итоговую сборку (без необходимости явного её импорта в `index.js`). Кроме того, иконка (`favicon.ico`) тоже должна быть в дистрибутиве.

Исходники к задаче: https://github.com/netology-code/ahj-homeworks/tree/master/yarn-cd

**Важно: данная задача не предполагает развёртывания в AppVeyor и GitHub Pages**

**В качестве результата пришлите проверяющему ссылку на ваш GitHub-проект.**

---

### Continuous Deployment

#### Легенда

Пора развернуть настроенный вами шедевр. Для этого прекрасно подойдёт связка из AppVeyor и GitHub Pages.

#### Описание

Воспользуйтесь пошаговой инструкцией к лекции, чтобы развернуть тестирование, сборку и deployment на AppVeyor и GitHub Pages.

Обратите внимание, что команда `yarn test` (не забудьте написать скрипт `test`) выдаёт ненулевой код завершения. Настройте свой проект так, чтобы без тестов код завершения был 0 (команда `yarn test` проходила без ошибки).

**Обратите внимание: в лекции приведены описания для `npm` вам же нужно использовать `yarn`.**

Не забудьте поставить бейджик со статусом в `README.md`.

**В качестве результата пришлите проверяющему ссылку на ваш GitHub-проект.**

---

### Разделение конфигураций (задача со звёздочкой)

Важно: эта задача не является обязательной

#### Легенда

На данный момент у нас одна конфигурация Webpack: для разработки и для production. В больших проектах конфигурации чаще всего разделяют, в том числе потому, что для production применяются плагины оптимизации, выполнение которых может занять достаточно длительное время.

#### Описание

Следуя инструкциям из лекции, разделите конфигурацию на три части:
* общая
* prod (в prod нужно указать только `mode: 'production'` и настройки оптимизации для плагинов Terser и OptimizeCSSAssets)
* dev

**В качестве результата пришлите проверяющему ссылку на ваш GitHub-проект.**