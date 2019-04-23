# STUDY-ES6Polyfill
Study notes for ES6 polyfill implementation.

## A. Define Polyfill
1. Polyfill：**用於實現瀏覽器並不支援的原生 API 的程式碼。**
1. Polyfill 是 shim 的一種，情境限制在瀏覽器 API 上。**Polyfill 是一個用於瀏覽器 API 上的 shim。**
2. 一般實現的作法為：檢查瀏覽器對某 API 的支援度，若不支援，則載入對應的 polyfill 來實現該 API。

> **Reference**
> - **[前端“黑話”polyfill](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/29473/)**
> - [Polyfill (programming) | Wikipedia](https://en.wikipedia.org/wiki/Polyfill_(programming))

## B. ES6 Compatibility
若須在 IE 上支援 ES6，則必須：
1. **使用 transpiler 來將 ES6 語法轉譯為 ES5 語法**。
2. **導入 polyfill 函式庫來實現新的原生 API**。

> **Reference**
> - **[ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)**
> - **[ECMAScript 6 | HTML5 PLEASE](https://html5please.com/#ecmascript)**
> - [Polyfill 與 Transpiler | 你懂 JavaScript 嗎？](https://cythilya.github.io/2018/10/10/intro-2/#polyfill)

## C. Transpilers | TK
1. Babel `C-1`
2. Traceur `C-2`

> **Reference**
> - [原始碼到原始碼編譯器 | Wikipedia](https://zh.wikipedia.org/wiki/%E6%BA%90%E5%88%B0%E6%BA%90%E7%BC%96%E8%AF%91%E5%99%A8)
> - [The Super Tiny Compiler](https://github.com/jamiebuilds/the-super-tiny-compiler)

### C-1. Babel

> **Reference**
> - [Babel](https://babeljs.io/docs/en/index.html)

### C-2. Traceur

> **Reference**
> - [Traceur](https://github.com/google/traceur-compiler)

## D. Polyfills | TK
1. Babel/Polyfill `D-1`
2. core-js `D-2`
3. Polyfill.io `D-3`

> **Reference**
> - [ECMAScript 6 | HTML5 PLEASE](https://html5please.com/#ecmascript%206)

### D-1. Babel/Polyfill

> **Reference**
> - [@babel/polyfill | Babel](https://babeljs.io/docs/en/babel-polyfill/)
> - [https://javascript.info/polyfills#babel](https://javascript.info/polyfills#babel)

### D-2. core-js

> **Reference**
> - [core-js | GitHub](https://github.com/zloirock/core-js)

### D-3. Polyfill.io

> **Reference**
> - [Polyfill.io](https://polyfill.io/v3/)
