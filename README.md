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
    - 站外資源無法控管（解法：下載其 GitHub 程式，部署到自己的 server。）
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

### C-1. Babel
1. **Babel 採用 npm，預設是在支援 [CommonJS](https://github.com/nizniz187/STUDY-JS-Module-Tools#c-commonjs) `require` 語法的 runtime 下執行模組載入。如要在此架構下作為 production 打包 / 運行，需要其他打包工具如 [webpack](https://github.com/nizniz187/STUDY-JS-Module-Tools#e-webpack--tk)。**
1. Setup & Transpiling `<C-1-1>`
1. Polyfill `<C-1-2>`
1. Configuration `<C-1-3>`
1. Babel setup with C# / .NET `<C-1-4>`
    
> **Reference**
> - [Babel](https://babeljs.io/docs/en/index.html)
> - [Babel Polyfills | JS Info](https://javascript.info/polyfills)

#### C-1-1. Setup & Transpiling
1. 安裝：使用 **npm** 安裝 core library `@babel/core`。
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
    - `@babel/preset-env`：包含所有支援現代 JS 語法 plugin 的 Babel 官方 preset。搭配 browserlist 可以指定瀏覽器支援版本，同時支援 polyfill 功能 `<C-1-2-2>`。
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

#### C-1-2. Polyfill
1. Babel 使用 **core-js** 來實現 polyfill，與之相關的套件分為三個：
    1. @babel/polyfill `<C-1-2-1>`
    1. @babel/preset-env `<C-1-2-2>`
    1. @babel/runtime `<C-1-2-3>`
1. **Tanspile + Polyfill**：
    - 在舊版，polyfill 跟 transpile 是分開設定的。因此若設定使用 `babel-preset-es2015`，預設只轉譯語法，需額外引入 `babel-polyfill` 才會加入 polyfill 功能。
    - Babel 7 推出了 `@babel/preset-env`，棄用了以年為單位的 preset（`babel-preset-es2015`、`babel-preset-es2016`、`babel-preset-es2017`、`babel-preset-latest`）；使用 `@babel/preset-env` 同時就會引入 `core-js`，意味著不須再額外引入 `@babel/polyfill`，並可直接在 `@babel/preset-env` 中設定是否加入 polyfill 功能。 `<C-1-2-2>`

> **Reference**
> - **[Babel | core-js](https://github.com/zloirock/core-js#babel)**
> - [Upgrade to Babel 7 | Babel](https://babeljs.io/docs/en/v7-migration)

#### C-1-2-1. @babel/polyfill
1. `@babel/polyfill` 包含 **core-js 的穩定功能（全域版、無 ES 草案）** 和一個客製化的 **[regenerator runtime](https://github.com/nizniz187/STUDY-RegeneratorRuntime)**。
1. 停留在版本 `core-js@2`，基本上已不建議使用。
1. 若不需要 instance methods (ex. Array.prototype.includes)，可改用 **transform runtime plugin**，避免更動全域 scope。`<C-1-2-3>`
1. 安裝：
    ```
    npm install --save @babel/polyfill
    ```
    **Polyfill 必須要在原始碼之前先執行，因此要安裝在正式環境下。**

> **Reference**
> - [Polyfill | Usage Guide | Babel](https://babeljs.io/docs/en/usage#polyfill)
> - [@babel/polyfill | Babel](https://babeljs.io/docs/en/babel-polyfill/)
> - **[@babel/polyfill | core-js](https://github.com/zloirock/core-js#babelpolyfill)**

#### C-1-2-2. @babel/preset-env
1. 使用 **core-js 全域版**。
1. 利用 `useBuiltIns` 屬性來設定是否引入 polyfill 功能並自動最佳化。
    - `useBuiltIns: 'entry'`：依據 JS 進入點的 `core-js` 設定，自動替換成目標環境所需的 polyfill 模組。
    - `useBuiltIns: 'usage'`：檢查程式碼，尋找目標環境所缺少的功能，並只引入需要的 polyfill。
        - 預設引入穩定功能的 polyfill。
        - **小風險：由於 Babel 無法判斷程式碼中的資料型態，因此有可能載入錯誤的 polyfill。欲避免此風險，可使用 `useBuiltIns: 'entry'`，或手動引入每個 polyfill。**
    - `useBuiltIns: 'disable'`：不使用 polyfill。（預設）
1. 建議設置 `corejs` 屬性來指定 core-js 版本。例如：`corejs: '3.0'`。

> **Reference**
> - [@babel/preset-env | Babel](https://babeljs.io/docs/en/babel-preset-env)
> - **[@babel/preset-env | core-js](https://github.com/zloirock/core-js#babelpreset-env)**
> - **[如何正確的設置 babel (Late 2018)](https://nereuseng.github.io/2018/11/27/babel-usage/)**

#### C-1-2-3. @babel/runtime
1. **共用生成的 helper code**：Babel 在轉譯過程中，會生成許多 helper 通用程式碼；這些程式碼存在於轉譯後的各個模組中，但其實內容完全相同可共用。這個模組讓所有 helper 關連到 `@babel/runtime` 模組，避免轉譯後重複宣告。
1. **創造出沙盒環境**：實現 `core-js-pure` 功能，自動將新的 JS 語法取代成從 `core-js` 引入的版本，避免變動全域命名空間。
    - 適用於函式庫開發。
1. 預設使用穩定功能的 polyfill。

> **Reference**
> - **[@babel/plugin-transform-runtime | Babel](https://babeljs.io/docs/en/babel-plugin-transform-runtime)**
> - [@babel/runtime | Babel](https://babeljs.io/docs/en/babel-runtime)
> - **[@babel/runtime | core-js](https://github.com/zloirock/core-js#babelruntimev)**

#### C-1-3. Configuration
1. 有兩種設定檔格式，可同時或單獨使用。
    1. Project-wide: `babel.config.js`
        - 放在與 `package.json` 相同的根目錄下。Babel 在轉譯時會自動在此根目錄中搜尋此設定檔。
        - 適用於通用設定；能使 `node_modules` 下的所有 plugin 和 preset 輕鬆套用設定。
    2. File-relative: `.babelrc` / `.babelrc.js` / 在 `package.json` 檔案中設定 `babel` 屬性
        - Babel 在轉譯時會從轉譯的文件向上搜尋設定檔，並由下往上合併、 override 設定檔。
        - 只會套用到所屬套件中的檔案

> **Reference**
> - [Configure Babel | Babel](https://babeljs.io/docs/en/configuration)
> - **[Config Files | Babel](https://babeljs.io/docs/en/config-files#project-wide-configuration)**

#### C-1-4. Babel Setup with C# / .NET | TK

> **Reference**
> - [Interactive Setup Guide | Babel](https://babeljs.io/setup.html#installation)

#### C-1-5. Performance
1. 測試 PA JS 轉譯：
    - 原始檔案：457KB（ES5 語法）
    - 轉譯條件：相容 IE 9、Edge 14、FF 60、Chorme 67、Safari 10.3 以上；`useBuiltIns: "usage"`
    - 轉譯耗時約 5 秒
    - 產出檔案：574KB（僅增加 `core-js` 引入語法）

### C-2. Traceur | TK

> **Reference**
> - [Traceur](https://github.com/google/traceur-compiler)
