---
title: نشر الإنتاج
type: guide
order: 404
---

> يتم تمكين معظم النصائح أدناه افتراضيًا إذا كنت تستخدم [Vue CLI](https://cli.vuejs.org). هذا القسم مناسب فقط إذا كنت تستخدم إعداد بناء مخصص.

## تشغيل وضع الإنتاج

أثناء التطوير، تقدم Vue الكثير من التحذيرات لمساعدتك في الأخطاء والمخاطر الشائعة. ومع ذلك، فإن سلاسل التحذير هذه تصبح غير مجدية في الإنتاج وتُضَخم حجم حمولة التطبيق. بالإضافة إلى ذلك، تحتوي بعض عمليات التحقق من التحذيرات هذه على تكاليف وقت تشغيل صغيرة يمكن تجنبها في وضع الإنتاج.

### دون أدوات البناء

إذا كنت تستخدم البنية الكاملة، أي تضمين Vue مباشرة عبر علامة البرنامج النصي بدون أداة إنشاء، تأكد من استخدام الإصدار المصغر (`vue.min.js`) للإنتاج. يمكن العثور على كلا الإصدارين في [دليل التثبيت](installation.html#Direct-lt-script-gt-Include).

### مع أدوات البناء

عند استخدام أداة إنشاء مثل Webpack أو Browserify، سيتم تحديد وضع الإنتاج بواسطة `process.env.NODE_ENV` داخل شفرة مصدر Vue، وسيكون في وضع التطوير افتراضيًا. توفر كلتا أدوات التصميم طرقًا للاستبدال هذا المتغير لتمكين وضع الإنتاج الخاص بـ Vue، وسيتم تجريد التحذيرات بواسطة المصغرات أثناء الإنشاء. تحتوي جميع قوالب `vue-cli` على هذه التكوينات المسبقة لك، لكن سيكون من المفيد معرفة كيفية القيام بذلك:

#### Webpack

في Webpack 4+، يمكنك استخدام خيار `mode`:

``` js
module.exports = {
  mode: 'production'
}
```

ولكن في Webpack 3 والإصدارات السابقة، ستحتاج إلى استخدام [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}
```

#### Browserify

- قم بتشغيل أمر التجميع باستخدام متغير البيئة `NODE_ENV` الفعلي الذي تم تعيينه على "الإنتاج". هذا يخبر `vueify` لتجنب  إعادة التحميل الساخنة وشفرة التطوير ذات الصلة.

- قم بتطبيق تحويل [envify](https://github.com/hughsk/envify) إلى الحزمة الخاصة بك. يسمح هذا لجهاز المصغر بتجميع كافة التحذيرات في شفرة مصدر Vue ملفوفة في كتل شرطية متغير env. فمثلا:

  ``` bash
  NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
  ```

- أو باستخدام [envify](https://github.com/hughsk/envify) مع Gulp:

  ``` js
  // Use the envify custom module to specify environment variables
  var envify = require('envify/custom')

  browserify(browserifyOptions)
    .transform(vueify)
    .transform(
      // Required in order to process node_modules files
      { global: true },
      envify({ NODE_ENV: 'production' })
    )
    .bundle()
  ```

- أو باستخدام [envify](https://github.com/hughsk/envify) مع Grunt و [grunt-browserify](https://github.com/jmreidy/grunt-browserify):

  ``` js
  // Use the envify custom module to specify environment variables
  var envify = require('envify/custom')

  browserify: {
    dist: {
      options: {
        // Function to deviate from grunt-browserify's default order
        configure: b => b
          .transform('vueify')
          .transform(
            // Required in order to process node_modules files
            { global: true },
            envify({ NODE_ENV: 'production' })
          )
          .bundle()
      }
    }
  }
  ```

#### Rollup

استخدم [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify( 'production' )
    })
  ]
}).then(...)
```

## التجميع المسبق للقوالب

عند استخدام القوالب داخل DOM أو سلاسل قوالب JavaScript، يتم تنفيذ التصيير من القالب إلى function أثناء التنقل. عادة ما يكون هذا سريعًا في معظم الحالات، ولكن من الأفضل تجنبه إذا كان التطبيق الخاص بك حساسًا للأداء.

إن أسهل طريقة لإعداد القوالب المسبقة هي استخدام [Single-File Components](single-file-components.html) - تقوم إعدادات الإنشاء المرتبطة تلقائيًا بإجراء التحويل المسبق نيابة عنك، وبالتالي فإن الشفرة المدمجة تحتوي على وظائف التصيير المترجمة بالفعل بدلاً من سلاسل قالب الخام.

إذا كنت تستخدم Webpack، وتفضل فصل ملفات JavaScript والقالب، فيمكنك استخدام [vue-template-loader](https://github.com/ktsn/vue-template-loader)، والذي يحول أيضًا ملفات القوالب إلى JavaScript تقديم وظائف خلال خطوة البناء.

## استخراج مكون CSS

عند استخدام مكونات أحادية الملف، يتم حقن المكونات الداخلية لـ CSS ديناميكيًا كعلامات `<style>` عبر JavaScript. هذا له تكلفة تشغيل صغيرة، وإذا كنت تستخدم التجسيد من جانب الخادم، فسيؤدي ذلك إلى "flash لمحتوى غير منسق". سيتسبب استخراج CSS عبر جميع المكونات في نفس الملف في تجنب هذه المشكلات، كما يؤدي إلى تحسين CSS وتخزينها مؤقتًا.

ارجع إلى وثائق أداة الإنشاء المعنية لترى كيف يتم ذلك:

- [Webpack + vue-loader](https://vue-loader.vuejs.org/en/configurations/extract-css.html) (the `vue-cli` webpack template has this pre-configured)
- [Browserify + vueify](https://github.com/vuejs/vueify#css-extraction)
- [Rollup + rollup-plugin-vue](https://vuejs.github.io/rollup-plugin-vue/#/en/2.3/?id=custom-handler)

## تتبع أخطاء وقت التشغيل

إذا حدث خطأ في وقت التشغيل أثناء تصيير أحد المكونات، فسيتم تمريره إلى وظيفة التكوين `Vue.config.errorHandler` العالمية إذا تم تعيينها. قد تكون فكرة جيدة الاستفادة من هذا الخطاف مع خدمة تتبع الأخطاء مثل [Sentry](https://sentry.io) ، والتي توفر [تكاملًا رسميًا](https://sentry.io/for/vue/) ل Vue.
