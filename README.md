
# AutoColorChange

背景色をゆっくりと変化させる効果のデモです。

[デモページ](https://todays-mitsui.github.com/auto-color-change/)

## Usage

### Step1. 様々な背景色を設定したクラスを用意

```css
/* 切り替えの基点になる色を設定 */
body.color0 { background-color: #346caa; }
body.color1 { background-color: #3386c8; }
body.color2 { background-color: #4aa45d; }
body.color3 { background-color: #8fb84f; }
body.color4 { background-color: #debc1c; }
body.color5 { background-color: #e09532; }
body.color6 { background-color: #eb7889; }
```

### Step2. 背景色をゆっくりと変化させるために transiton を5sで設定

```css
body { transition: background-color 5s linear; }
```

### Step3. 指定した要素のクラス名を貼り替える関数を定義

```javascript
// 背景色を設定したクラスを切り替えるためのクロージャを生成
// 現在設定されている色は index で保持する、
function initAutoColorChange($el, colorCount) {
  var index = 0;

  return function() {
    index = (index + 1) % colorCount;


    // "color*"にマッチするクラス名を全て削除
    // 今回の組み方では色数が10色以上担った場合に対応していない
    $el.removeClass(function(i, classNames) {
      return classNames.split(' ').map(function(className) {
        return className.match(/color\d+/);
      }).filter(Boolean).join(' ');
    });

    // 次のクラス名を付与
    $el.addClass('color'+index);
  }
}
```

### Step4. `setInterval` で5秒毎にクラス名を貼り替える

```javascript
jQuery(function($) {
  // 切り替え関数を取得
  // 対象は<body>、色数は7色
  var autoColorChange = initAutoColorChange($('body'), 7);

  autoColorChange()
  setInterval(autoColorChange, 5000); // 5000ms 毎に切り替えされるよう設定
});
```

## TODO

- 色数が10色以上になったらどうしよう...

Enjoy!!
