[![Build status](https://ci.appveyor.com/api/projects/status/yx5i9j4gto2g9pxx/branch/main?svg=true)](https://ci.appveyor.com/project/kvrdv/ahj-homeworks-env/branch/main)

## Нетология. Продвинутый JavaScript в браузере

[Задание "Рабочее окружение"](https://github.com/netology-code/ahj-homeworks/tree/master/env)

---

## Гайд по развертыванию рабочего окружения:

### 0. Если не устанавливаются пакеты через yarn:

```shell
npm config set registry https://registry.npmjs.org/
```

---

### 1. Создать необходимые файлы и папки:

- Папка `src`, с соответствующей структурой и файлами

- Запустить команду `yarn init` > создается файл `package.json`

- Файл [.gitignore](https://github.com/github/gitignore/blob/master/Node.gitignore)

- Файл `.browserslistrc`:

```shell
last 1 version
> 1%
maintained node versions
not dead
```

- Файл `.appveyor.yml`:

```shell
image: Ubuntu1804  # образ для сборки

stack: node 12  # окружение

branches:
  only:
    - master  # ветка git
  except:
      - gh-pages

cache: node_modules  # кеширование

install:
  - npm install  # команда установки зависимостей

build: off  # отключаем встроенную в appveyor систему сборки

build_script:
  - npm run build   # команда сборки

test_script:
  - npm run lint && npm test  # скрипт тестирования

deploy_script:
  - git config --global credential.helper store
  - git config --global user.name AppVeyor
  - git config --global user.email ci@appveyor.com
  - echo "https://$GITHUB_TOKEN:x-oauth-basic@github.com" > "$HOME/.git-credentials"
  - npx push-dir --dir=dist --branch=gh-pages --force --verbose
```

- Файл `.eslintignore`:

```shell
dist
coverage
docs
```

---

### 2. Установить [webpack](https://webpack.js.org/concepts/):

```shell
yarn add webpack webpack-cli
yarn add webpack-merge
```

- Прописать в `package.json`:

```shell
"scripts": {
    "build": "webpack --config webpack.prod.js"
},
```

- Создать файл `webpack.common.js`:

```shell
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "app.bundle.js",
    chunkLoading: false,
    wasmLoading: false,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
    new MiniCssExtractPlugin({
      filename: "[name].css",
    }),
  ],
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
      {
        test: /\.html$/i,
        loader: 'html-loader',
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
      {
        test: /\.(png|jpg|gif)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
            }
          }
        ]
      },
      {
        test: /\.ico$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]',
        },
      }
    ]
  }
}
```

- Создать файл `webpack.dev.js`:

```shell
const { merge } = require('webpack-merge');
const common = require('./webpack.common');

module.exports = merge(common, {
  mode: 'development',
  devtool: 'inline-source-map',
  devServer: {
    contentBase: './dist',
  },
});
```

- Создать файл `webpack.prod.js`:

```shell
const { merge } = require('webpack-merge');
const TerserPlugin = require('terser-webpack-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const common = require('./webpack.common');

module.exports = merge(common, {
  mode: 'production',
  optimization: {
    minimizer: [
      new TerserPlugin({}),
      new OptimizeCSSAssetsPlugin({}),
    ],
  },
});
```

- Установить плагины и лоадеры:

```shell
yarn add --dev html-webpack-plugin
yarn add terser-webpack-plugin optimize-css-assets-webpack-plugin mini-css-extract-plugin
yarn add --dev babel-loader css-loader html-loader url-loader file-loader
```

---

### 3. Установить [jest](https://jestjs.io/docs/ru/getting-started) и [babel](https://babeljs.io/setup#installation):

```shell
yarn add --dev jest babel-jest @babel/core @babel/cli @babel/preset-env
yarn add core-js@3
```

- Прописать в `package.json`:

```shell
"scripts": {
    "test": "jest",
    "lint": "eslint .",
},
```

- Создать файл `.babelrc`:

```shell
{
  "presets": [
    [
       "@babel/preset-env", {
           "useBuiltIns": "usage",
            "corejs": 3
        }
    ]
  ]
}
```

---

### 4. Установить [eslint](https://eslint.org/docs/user-guide/getting-started):

```shell
yarn add --dev eslint
yarn run eslint --init
```

```shell
How would you like to use ESLint?
-To check syntax, find problems, and enforce code style
What type of modules does your project use?
-JavaScript modules (import/export)
Which framework does your project use?
-None of this
Does your project use TypeScript?
-No
Where does your code run?
-Browser
How would you like to define a style for your project?
-Use a popular style guide
Which style guide do you want to follow?
-Airbnb
What format do you want your config file to be in?
-JSON
Would you like to install them now with npm?
-Yes
```

- Прописать в `.eslintrc.json`:

```shell
{
  "env": {
    "jest": true,
    "browser": true,
    "es6": true
  },
  "extends": "airbnb-base",
  "globals": {
    "Atomics": "readonly",
    "SharedArrayBuffer": "readonly"
  },
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module"
  },
  "rules": {
    // допускает ++
    "no-plusplus": "off",

    // допускает вывод в консоль
    "no-console": "off",

    "no-restricted-syntax": ["error", "LabeledStatement", "WithStatement"],

    // допускает знаки
    "no-mixed-operators": [
      "error",
      {
        "groups": [
          ["+", "*", "/", "%"],
          ["&", "|", "^", "~", "<<", ">>", ">>>"],
          ["==", "!=", "===", "!==", ">", ">=", "<", "<="],
          ["&&", "||"],
          ["in", "instanceof"]
        ],
        "allowSamePrecedence": true
      }
    ],

    // максимальная длина строки
    "max-len": ["error", { "code": 999 }]
  }
}
```

---

### 5. Установить [webpack-dev-server](https://github.com/webpack/webpack-dev-server):

```shell
yarn add --dev webpack-dev-server
```

- Прописать в `package.json`:

```shell
"scripts": {
  "start": "webpack-dev-server --config webpack.dev.js"
}
```

- Прописать в `webpack.common.js`:

```shell
module.exports = {
  //...
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    port: 9000
  }
};
```

---
