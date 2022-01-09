# clone-inago
いなごFlyer に感化を受けて作りました。

WebSocket で Bybit の約定履歴を受信して、1分足のチャートを作って表示してます。

ページを開いた時は約定履歴のデータがまだないため何も表示されませんが、しばらく待つとチャートが表示されると思います。

## 予定
+ まだBybitしかないので他の取引所を追加する。
+ 一定期間で大量の取引があれば音を出す。
+ 清算データもリアルタイムで取得・表示する。

## Demo
[https://noitaro.github.io/clone-inago-demo/](https://noitaro.github.io/clone-inago-demo/)

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
