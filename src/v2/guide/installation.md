---
title: التنصيب
type: guide
order: 1
vue_version: 2.5.16
gz_size: "30.90"
---

### ملاحظة عن التوافق

يرجى ملاحظة أن Vue **لا تدعم** متصفح إنترنت إكسبلورر 8 فأقل، لأنه يستخدم مميزات خاصة من الإصدار الخامس من ECMAScript والذي لا يتيح التحديثات الدورية لهذه المميزات. على أي حال, فإنها تدعم [جميع المتصفحات الاخرى المتوافقة مع الاصدار الخامس من ECMAScript](https://caniuse.com/#feat=es5).

### ملاحظات عن الإصدار

آخر إصدار مستقر: {{vue_version}}

ملاحظات أخرى تخص الإصدارات الأخرى متاحة على [GitHub](https://github.com/vuejs/vue/releases).

## أدوات مطوري Vue

عند استخدام Vue, ننصح أيضاً بتنصيب [أدوات مطوري Vue](https://github.com/vuejs/vue-devtools#vue-devtools) في المستعرض الخاص بك, والتي تسمح لك بتفحص وتصحيح تطبيقات Vue في واجهة مستخدم ذات قابلية عالية.

## باستخدام تضمين وسم `<script>` مباشرة

ببساطة قم بتحميل وتضمين Vue باستخدام وسم script. سيتم تسجيل Vue كمتغير عالمي.

<p class="tip">لا تستخدم الإصدار المصغر أثناء التطوير. سوف تفوتك كل التحذيرات الرائعة لأغلب المشكلات.</p>

<div id="downloads">
  <a class="button" href="/js/vue.js" download>نسخة التطوير</a><span class="light info">مع التحذيرات الكاملة ووضع تصحيح الأخطاء.</span>

  <a class="button" href="/js/vue.min.js" download>نسخة الإنتاج</a><span class="light info">دون تحذيرات، وبحجم {{gz_size}} كيلو بايت مصغرة و مضغوطة باستخدام gzip</span>
</div>

### شبكة توزيع المحتوى

لأغراض التعلم أو عمل النماذج المصغرة، يمكنك استخدام الإصدار الأحدث :

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
للإنتاج، ننصح بالربط مع إصدار مخصص حتى لا يحدث تعارض مع الإصدارات الأحدث:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0/dist/vue.js"></script>
```

إذا كنت تستخدم وحدات JavaScript، يوجد أيضاً إصدار متوافق مع وحدات JavaScript:

``` html
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.0/dist/vue.esm.browser.js'
</script>
```

يمكنك استعراض الكود المصدري الخاص برزمة NPM من هنا [cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue/).

Vue أيضاً متوفرة على [unpkg](https://unpkg.com/vue@{{vue_version}}/dist/vue.js) و [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) (cdnjs تأخذ بعض الوقت للتزامن، لذا قد لا يتوفر آخر إصدار بعد).

قم بالتأكد بأن تقرأ [الإصدارات المختلفة من Vue](#Explanation-of-Different-Builds) وأن تستخدم **النسخة الخاصة بالإنتاج** في موقعك المنشور، وإستبدال `vue.js` بـ `vue.min.js`. هذا الإصدار أقل في الحجم ومحسن للمزيد من السرعه بدلاً من خبرة التطوير.

## NPM ( مدير حزم Node.js )

NPM هي الأداة الموصى بإصتخدامها عند بناء تطبيقات كبيرة الحجم بإستخدام Vue. فهي تعمل جيداً مع أدوات التجميع مثل [Webpack](https://webpack.js.org/) و [Browserify](http://browserify.org/). Vue أيضاً توفر أدوات خاصة بالإصدار مثل [Single File Components](single-file-components.html).

``` bash
# latest stable
$ npm install vue
```

## CLI (أدوات سطر الأوامر)

Vue توفر [أدوات سطر الأوامر الرسمية](https://github.com/vuejs/vue-cli) لمساعدتك على إنشاء تطبيقات الصفحة الواحدة بشكل سريع. سوف تستهلك فقط بضع دقائق للبدء ببناء تطبيقات جاهز للنشر. أنظر [توثيق أدوات سطر الأوامر لـVue](https://cli.vuejs.org) لمزيد من المعلومات.

<p class="tip">إستخدام أدوات سطر الأوامر يتطلب أن يكون لديك معرفة بـNode.js وأدوات البناء المرتبطة بها. إذا كنت جديد على Vue أو أدوات بناء الواجهات الأمامية، فنحن نقترح بشدة أن تقوم بإتباع <a href="./">الدليل</a> بدون إستخدام أي أدوات بناء قبل بدءك بإستخدام أدوات سطر الأوامر.</p>

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/real-world-vue-js/vue-cli" target="_blank" rel="noopener" title="Vue CLI">قم بمشاهدة فيديو توضيحي على Vue Mastery</a></div>

## توضيح الإصدارات المختلفة

في [مجلد `dist/` الخاص بحزمة NPM](https://cdn.jsdelivr.net/npm/vue/dist/) سوف تجد العديد من الإصدارات الخاصة بـVue.js. إليك نبذة عن الفرق بينهم:

| | UMD | CommonJS | ES Module (for bundlers) | ES Module (for browsers) |
| --- | --- | --- | --- | --- |
| **الإصدار الكامل** | vue.js | vue.common.js | vue.esm.js | vue.esm.browser.js |
| **إصدار بيئة التشغيل فقط** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js | - |
| **الإصدار الكامل (الإنتاج)** | vue.min.js | - | - | vue.esm.browser.min.js |
| **إصدار بيئة التشغيل (الإنتاج)** | vue.runtime.min.js | - | - | - |

### مصطلحات

- **الكامل**: الإصدارات التي تحتوي على كلِ من المترجم وبيئة العمل.

- **المترجم**: الكود المسئول عن ترجمة قوالب الكلمات لدوال Javascript الخاصة الخاصة بالتصدير.

- **Runtime**: code that is responsible for creating Vue instances, rendering and patching virtual DOM, etc. Basically everything minus the compiler.

- **[UMD](https://github.com/umdjs/umd)**: UMD builds can be used directly in the browser via a `<script>` tag. The default file from jsDelivr CDN at [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue) is the Runtime + Compiler UMD build (`vue.js`).

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS builds are intended for use with older bundlers like [browserify](http://browserify.org/) or [webpack 1](https://webpack.github.io). The default file for these bundlers (`pkg.main`) is the Runtime only CommonJS build (`vue.runtime.common.js`).

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: starting in 2.6 Vue provides two ES Modules (ESM) builds:

  - ESM for bundlers: intended for use with modern bundlers like [webpack 2](https://webpack.js.org) or [Rollup](https://rollupjs.org/). ESM format is designed to be statically analyzable so the bundlers can take advantage of that to perform "tree-shaking" and eliminate unused code from your final bundle. The default file for these bundlers (`pkg.module`) is the Runtime only ES Module build (`vue.runtime.esm.js`).

  - ESM for browsers (2.6+ only): intended for direct imports in modern browsers via `<script type="module">`.

### وقت التشغيل + المترجمة vs. وقت التشغيل فقط

إذا أحتجت ترجمة القوالب في جهة العميل, سوف تحتاج المترجمة  و البناء الكامل.

``` js
// هذا يتطلب المترجم
new Vue({
  template: '<div>{{ hi }}</div>'
})

// هذا لا يتطلب
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

عند إستخدام `vue-loader` أو `vueify`, القوالب بداخل ملفات `*.vue` تكون مترجمة من قبل داخل JavaScript في وقت البناء. أنت لا تحتاج المترجم في الحزمة النهائية, و , و بالتالي يمكنك إستخدام بناء وقت-التشغيل فقط. 

نظرًا لأن إصدارات وقت التشغيل فقط أقل مساحة بنسبة 30 ٪ تقريبًا من نظيراتها كاملة الإنشاء ، فيجب عليك استخدامها كلما استطعت. إذا كنت لا تزال ترغب في استخدام الإصدار الكامل بدلاً من ذلك ، فأنت بحاجة إلى تكوين اسم مستعار في الحزمة الخاصة بك:

#### حزمة الويب

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': require.resolve('vue/dist/vue.esm.js')
    })
  ]
})
```

#### Browserify

أضف ملف `package.json` الخاص بمشروعك : 

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

#### Parcel

أضف ملف `package.json` الخاص بمشروعك : 

``` js
{
  // ...
  "alias": {
    "vue" : "./node_modules/vue/dist/vue.common.js"
  }
}
```

### وضع التطوير / وضع الإنشاء

Development/production modes are hard-coded for the UMD builds: the un-minified files are for development, and the minified files are for production.
أوضاع التطوير/الإنشاء ثابتة في إنشاءات UMD: الملفات غير المصغرة للتطوير ، والملفات المصغرة للإنتاج. 

وحدات CommonJS و ES Module مخصصة للحزم ، لذلك لا نقدم إصدارات مصغرة لها. ستكون مسؤولاً عن تصغير الحزمة النهائية بنفسك.

تحافظ إصدارات CommonJS و ES Module أيضًا على عمليات التحقق الأولية لـ `process.env.NODE_ENV` لتحديد الوضع الذي يجب أن تعمل فيه. يجب استخدام حزم المترجم المناسبة لاستبدال متغيرات البيئة هذه للتحكم في الوضع الذي سيتم تشغيل Vue فيه. تسمح عملية process.env.NODE_ENV` التي تحتوي على نصوص حرفية أيضًا للمُصغرات مثل UglifyJS بإسقاط كتل التعليمات البرمجية للتطوير فقط ، مما يقلل من حجم الملف النهائي.

#### Webpack

In Webpack 4+, you can use the `mode` option:
In Webpack 4+, you can use the `mode` option:
في حزمة الويب 4+, يمكنك إستخدام إختيار ال `mode`:

``` js
module.exports = {
  mode: 'production'
}
```

و لكن في حزمة الويب 3 أو أقل, ستحتاج إلى استخدام [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

إستخدم [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

قم بتقديم تحويل [envify](https://github.com/hughsk/envify) عالمي إلى حزمتك.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

شاهد أيضاً [Production Deployment Tips](deployment.html).

### CSP environments

تفرض بعض البيئات ، مثل Google Chrome تطبيقات ، سياسة أمان المحتوى (CSP) ، والتي تحظر استخدام `` الوظيفة الجديدة () '' لتقييم التعبيرات. يعتمد البناء الكامل على هذه الميزة لتجميع القوالب ، لذا فهو غير قابل للاستخدام في هذه البيئات.

من ناحية أخرى ، فإن بناء وقت التشغيل فقط متوافق تمامًا مع CSP. عند استخدام إصدار وقت التشغيل فقط مع [Webpack + vue-loader] (https://github.com/vuejs-templates/webpack-simple) أو [Browserify + vueify] (https://github.com/vuejs-templates / browserify-simple) ، سيتم تجميع القوالب الخاصة بك مسبقًا في وظائف "تقديم" التي تعمل بشكل مثالي في بيئات CSP.

## إصدار التطوير

**هام**: الملفات المبنية في مجلد `/dist` على GitHub تم فحصها فقط أثناء الإصدارات. لإستخدام Vue أخر إصدار سيكون من الكود المصدري على GitHub، سيتوجب عليك أن تقوم ببنائها بنفسك!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

وحدات UMD فقط هي المتاحة من Bower.

``` bash
# latest stable
$ bower install vue
```

## وحدات تحميل AMD

جميع إصدارات UMD يمكن إستخدامها كوحدة AMD.
