# STUDY-ES6Polyfill
Study notes for ES6 polyfill implementation.

---

## A. ES6 Compatibility
若須在 IE 上支援 ES6，則必須：
1. **導入 polyfill 函式庫來實現新的原生 API**。`<B>`
1. **使用 transpiler 來將 ES6 語法轉譯為 ES5 語法**。`<C>` 

> **Reference**
> - **[ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)**
> - **[ECMAScript 6 | HTML5 PLEASE](https://html5please.com/#ecmascript)**
> - [Polyfill 與 Transpiler | 你懂 JavaScript 嗎？](https://cythilya.github.io/2018/10/10/intro-2/#polyfill)

---

## B. Polyfill
1. Polyfill：**用於實現瀏覽器並不支援的原生 API 的程式碼。**
1. Polyfill 是 shim 的一種，情境限制在瀏覽器 API 上。**Polyfill 是一個用於瀏覽器 API 上的 shim。**
2. 一般實現的作法為：檢查瀏覽器對某 API 的支援度，若不支援，則載入對應的 polyfill 來實現該 API。
1. core-js `<B-1>`
1. Polyfill.io `<B-2>`

> **Reference**
> - **[前端“黑話”polyfill](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/29473/)**
> - [Polyfill (programming) | Wikipedia](https://en.wikipedia.org/wiki/Polyfill_(programming))

### B-1. core-js
1. 應用最廣泛的 polyfill 函式庫，被許多套件引用作為 polyfill 核心（如 Babel）。
1. 可彈性引入需要的 polyfill。
1. 可避免變動到全域命名空間。
1. 安裝：
    
    ```
    // global version
    npm install --save core-js@3.0.1
    // version without global namespace pollution
    npm install --save core-js-pure@3.0.1
    // bundled global version
    npm install --save core-js-bundle@3.0.1
    ```

> **Reference**
> - **[core-js | GitHub](https://github.com/zloirock/core-js)**

### B-2. Polyfill.io
1. 自動化 polyfill js 打包線上服務。
1. 使用其所提供的 API get 資源，將欲支援的功能作為參數傳遞，即可獲得適用於該瀏覽器所需的所有 polyfill minified bundle 檔。
1. 優點：
    - 省事
    - 根據當下所使用的瀏覽器自動偵測，省去其他瀏覽器的 polyfill。
1. 缺點：
    - 站外資源無法控管
    - 多一次額外的 request

> **Reference**
> - [Polyfill.io](https://polyfill.io/v3/)

---

## C. Transpilers
1. Babel `<C-1>`
1. Traceur `<C-2>`

> **Reference**
> - [原始碼到原始碼編譯器 | Wikipedia](https://zh.wikipedia.org/wiki/%E6%BA%90%E5%88%B0%E6%BA%90%E7%BC%96%E8%AF%91%E5%99%A8)
> - [The Super Tiny Compiler](https://github.com/jamiebuilds/the-super-tiny-compiler)


### C-1. Babel | TK
1. Setup & Transpiling `<C-1-1>`
1. Babel/Polyfill `<C-1-2>`
1. Configure Babel `<C-1-3>`
1. Babel setup with C# / .NET `<C-1-4>`
    
> **Reference**
> - [Babel](https://babeljs.io/docs/en/index.html)

#### C-1-1. Setup & Transpiling
1. 使用 **npm** 安裝 core library：
    
    ```
    npm install --save-dev @babel/core
    ```
    `--save-dev` : 僅安裝於開發環境
    `@babel/core` : babel transpiler core package
2. Transpiling:
    1. by JS require (CommonJS API):
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
        將 `src` 資料夾下的程式碼轉譯到 `lib` 資料夾下。
        但此指令需要額外的參數來定義應如何轉譯程式碼，否則程式碼在上述轉譯後，與原始碼並無差別。（見 `<Plugins>` & `<Presets>`。）
1. Plugins: 
    - 用來指示 Babel 如何轉譯原始碼的小型 JS 程式。
    - 可自訂。
    - 用於將 ES6+ 箭頭函式轉譯為 ES5 語法的 Babel 官方 plugin：`@babel/plugin-transform-arrow-functions`。
        
        ```
        npm install --save-dev @babel/plugin-transform-arrow-functions

        ./node_modules/.bin/babel src --out-dir lib --plugins=@babel/plugin-transform-arrow-functions
        ```
1. Presets:
    - 預設好的 plugin 組合。
    - 可自訂。
    - 包含所有支援現代 JS 語法 plugin 的 Babel 官方 preset `@env`，搭配 browserlist 可以指定瀏覽器支援版本。
    - 可以設定參數，只載入特定 plugin。
    - stage：過新的 preset，可能仍在草案階段。分成 0-4 五個階段。
    - 安裝：
        - by CLI tool:
            
            ```
            npm install --save-dev @babel/preset-env

            ./node_modules/.bin/babel src --out-dir lib --presets=@babel/env
            ```
        - by Configuration:
            在專案根目錄建立 `babel.config.js` 設定檔。
            
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
            現在 `env` preset 將只會載入目標瀏覽器不支援的轉譯 plugin。
            
> **Reference**
> - [Usage Guide | Babel](https://babeljs.io/docs/en/usage)
> - [@babel/core](https://babeljs.io/docs/en/babel-core)
> - [@babel/cli](https://babeljs.io/docs/en/babel-cli)
> - [Babel - 走向 JavaScript 的嶄新未來 | JS 生態系及週邊工具整理](https://ithelp.ithome.com.tw/articles/10194314)

#### C-1-2. Babel/Polyfill | TK
1. `@babel/polyfill` 包含 **core-js** 和一個客製化的 **regenerator runtime** `<C-1-2-1>`。
1. 若不需要 instance methods (ex. Array.prototype.includes)，可改用 **transform runtime plugin**，避免更動全域 scope。`<C-1-2-2>`
1. 安裝：
    ```
    npm install --save @babel/polyfill
    ```
    **Polyfill 必須要原始碼之前先執行，因此要安裝在正式環境下。**
    
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
          },
          useBuiltIns: "usage",
        },
      ],
    ];

    module.exports = { presets };
    ```
    `useBuiltIns: "usage"`: Babel 會檢查程式碼，尋找目標環境所缺少的功能，並只引入需要的 polyfill。若未設置為 `usage`，則 Babel 會在所有程式碼之前的進入點，一次性引入完整的 polyfill。

> **Reference**
> - [Polyfill | Usage Guide | Babel](https://babeljs.io/docs/en/usage#polyfill)
> - [Babel Polyfills | JS Info](https://javascript.info/polyfills#babel)
> - [@babel/polyfill | Babel](https://babeljs.io/docs/en/babel-polyfill/)

#### C-1-2-1. Regenerator | TK

> **Reference**
> - [regenerator | GitHub](https://github.com/facebook/regenerator)

#### C-1-2-2. Transform Runtime Plugin | TK

> **Reference**
> - [@babel/plugin-transform-runtime | Babel](https://babeljs.io/docs/en/babel-plugin-transform-runtime)

#### C-1-3. Configure Babel | TK

> **Reference**
> - [Configure Babel | Babel](https://babeljs.io/docs/en/configuration)
> - [如何正確的設置 babel (Late 2018)](https://nereuseng.github.io/2018/11/27/babel-usage/)

#### C-1-4. Babel Setup with C# / .NET | TK

> **Reference**
> - [Interactive Setup Guide | Babel](https://babeljs.io/setup.html#installation)

### C-2. Traceur | TK

> **Reference**
> - [Traceur](https://github.com/google/traceur-compiler)
