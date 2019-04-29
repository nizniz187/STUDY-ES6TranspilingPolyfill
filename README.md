# STUDY-ES6Polyfill
Study notes for ES6 polyfill implementation.

## A. ES6 Compatibility
若須在 IE 上支援 ES6，則必須：
1. **使用 transpiler 來將 ES6 語法轉譯為 ES5 語法**。`A-1` 
2. **導入 polyfill 函式庫來實現新的原生 API**。`A-2`

> **Reference**
> - **[ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)**
> - **[ECMAScript 6 | HTML5 PLEASE](https://html5please.com/#ecmascript)**
> - [Polyfill 與 Transpiler | 你懂 JavaScript 嗎？](https://cythilya.github.io/2018/10/10/intro-2/#polyfill)

### A-1. Transpilers

> **Reference**
> - [原始碼到原始碼編譯器 | Wikipedia](https://zh.wikipedia.org/wiki/%E6%BA%90%E5%88%B0%E6%BA%90%E7%BC%96%E8%AF%91%E5%99%A8)
> - [The Super Tiny Compiler](https://github.com/jamiebuilds/the-super-tiny-compiler)

### A-2. Polyfill
1. Polyfill：**用於實現瀏覽器並不支援的原生 API 的程式碼。**
1. Polyfill 是 shim 的一種，情境限制在瀏覽器 API 上。**Polyfill 是一個用於瀏覽器 API 上的 shim。**
2. 一般實現的作法為：檢查瀏覽器對某 API 的支援度，若不支援，則載入對應的 polyfill 來實現該 API。

> **Reference**
> - **[前端“黑話”polyfill](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/29473/)**
> - [Polyfill (programming) | Wikipedia](https://en.wikipedia.org/wiki/Polyfill_(programming))


## B. Babel | TK
1. Setup & Transpiling `B-1`
1. Babel/Polyfill `B-2`
1. Configure Babel `B-3`
1. Babel setup with C# / .NET `B-4`
    
> **Reference**
> - [Babel](https://babeljs.io/docs/en/index.html)

### B-1. Setup & Transpiling
1. Install core library w/ npm
    ```
    npm install --save-dev @babel/core
    ```
    `--save-dev` : install in dev environment only  
    `@babel/core` : babel transpiler core package
2. Transpiling:
    1. by JS require:
        ```
        const babel = require("@babel/core");

        babel.transform("code", optionsObject);
        ```
    1. by CLI tool: 
        ```
        npm install --save-dev @babel/cli
        ```
        `@babel/cli` : babel command line tool
        
        ```
        ./node_modules/.bin/babel src --out-dir lib
        ```
        Transpile the source code under `src` to `lib`.  
        But this command needs options to define how the code to be transpiled, or it would just output the same as the source code. (See `Plugins` & `Presets`.)
1. Plugins: 
    - Small JS programs that instruct Babel on how to carry out transformations to the code.
    - Custimizable.
    - Official plugin for transform ES6+ arrow functions to ES5: `@babel/plugin-transform-arrow-functions`
        ```
        npm install --save-dev @babel/plugin-transform-arrow-functions

        ./node_modules/.bin/babel src --out-dir lib --plugins=@babel/plugin-transform-arrow-functions
        ```
1. Presets:
    - Pre-determined set of plugins.
    - Custumizable.
    - Official preset to include all plugins to support modern JS: `@env`.
    - Can take options to load the specific plugins only.
    - Installation:
        - by CLI tool:
            ```
            npm install --save-dev @babel/preset-env

            ./node_modules/.bin/babel src --out-dir lib --presets=@babel/env
            ```
        - by Configuration:
            Create `babel.config.js` config file in the root of the project.
            ```
            const presets = [
              [
                "@babel/env",
                {
                  targets: {
                    edge: "17",
                    firefox: "60",
                    chrome: "67",
                    safari: "11.1",
                  }
                },
              ],
            ];

            module.exports = { presets };
            ```
            Now the env preset will only load transformation plugins for features that are not available in our target browsers.
            
> **Reference**
> - [Usage Guide | Babel](https://babeljs.io/docs/en/usage)
> - [@babel/core](https://babeljs.io/docs/en/babel-core)
> - [@babel/cli](https://babeljs.io/docs/en/babel-cli)

### B-2. Babel/Polyfill | TK

> **Reference**
> - [Babel Polyfills | JS Info](https://javascript.info/polyfills#babel)
> - [Polyfill | Usage Guide | Babel](https://babeljs.io/docs/en/usage#polyfill)
> - [@babel/polyfill | Babel](https://babeljs.io/docs/en/babel-polyfill/)

### B-3. Configure Babel | TK

> **Reference**
> - [Configure Babel | Babel](https://babeljs.io/docs/en/configuration)
> - [如何正確的設置 babel (Late 2018)](https://nereuseng.github.io/2018/11/27/babel-usage/)

### B-4. Babel Setup with C# / .NET | TK

> **Reference**
> - [Interactive Setup Guide | Babel](https://babeljs.io/setup.html#installation)

## C. Traceur | TK

> **Reference**
> - [Traceur](https://github.com/google/traceur-compiler)

## D. core-js | TK

> **Reference**
> - [core-js | GitHub](https://github.com/zloirock/core-js)

### E. Polyfill.io | TK

> **Reference**
> - [Polyfill.io](https://polyfill.io/v3/)
