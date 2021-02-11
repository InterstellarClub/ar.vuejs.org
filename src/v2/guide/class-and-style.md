---
title: الفئات وربط الأنماط
type: guide
order: 6
---

من ضمن إحتياجات الربط هو تعديل حالة الفئة الخاصة بالعناصر والانماط المضمنة. حيث ان كلاً منهما عبارة عن خصائص، يمكننا استخدام الخاصية `v-bind` للتحكم بهم: كل ما علينا فعله هو معالجة النص النهائي مع التعبيرات الخاصة بنا. على أي حال، استخدام دمج سلاسل النصوص (concatenation) يعتبر امر مزعج ويتسبب في العديد من الاخطاء. لهذا السبب، قامت Vue بتوفير بعض التحسينات المميزة عند استخدام `v-bind` مع `class` و `style`. بالإضافة إلى النصوص، التعبيرات أيضاً يمكنها ان تقوم بمعالجة كائنات او مصفوفات.

## ربط فئات HTML
<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-dynamic-classes?friend=vuejs" target="_blank" rel="noopener" title="Free Vue.js Dynamic Classes Lesson">قم بمشاهدة درس الڤيديو مجاناً على Vue School</a></div>

### صيغة استخدام الكائن

يمكننا تمرير كائن إلى `v-bind:class` لتبديل حالة الفئة بشكل حيوي:

``` html
<div v-bind:class="{ active: isActive }"></div>
```

الصيغة بالأعلى تعني ان الفئة `active` سوف يتم تحديدها طبقاً لحقيقة القيمة [truthiness](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) الموجودة في خاصية البينات `isActive`.

يمكنك استخدام وتبديل العديد من الفئات عن طريق استخدام العديد من الحقول الاخرى. بالاضافة الى ذلك، الموجه `v-bind:class` يمكن ان يستخدم مع خاصية العنصر `class`. كما في المثال التالي:

``` html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```
مع خصائص البيانات التالية:

``` js
data: {
  isActive: true,
  hasError: false
}
```

سوف يتم معالجتها وعرضها كالتالي:

``` html
<div class="static active"></div>
```

عندما يتم تغيير قيمة `isActive` و `hasError`، يتم تحديث قائمة الفئات وفقاً لذلك. على سبيل المثال، إذا كانت قيمة `hasError` تساوي `true`، فإن قائمة الفئات للعنصر سوف تصبح `"static active text-danger"`.

والعنصر الذي يتم ربطه لا يتطلب ان يكون متضمن (inline):

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

هذا سوف يتم معالجته وعرضه بنفس النتيجة السابقة. كما يمكننا ايضاً ربط خاصية معالجة [computed property](computed.html) والتي تقوم بارجاع عنصر. وهذا يعتبر نمط قوي وشائع:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### صيغة المصفوفة

يمكننا تمرير مصفوفة الى `v-bind:class` لتطبيق قائمة من الفئات:

``` html
<div v-bind:class="[activeClass, errorClass]"></div>
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

سوف يعالج ويعرض كالتالي:

``` html
<div class="active text-danger"></div>
```

إذا كنت ترغب في أيضاً في تبديل الفئات في القائمة طبقاً لأحد الشروط، يمكنك فعل ذلك بإستخدام التعبير الثلاثي (ternary expression) كالتالي:

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
هذا سوف يقوم دائماً بتطبيق `errorClass`، ولكن سوف يقوم بتطبيق `activeClass` فقط اذا كانت قيمة `isActive` تساوي قيمة حقيقية.

على أي حال، يمكن أن يطول هذا  قليلاً إذا كان لديك العديد من الشروط. لهذا السبب، من الممكن استخدام صيغة الكائن في صيغة المصفوفة.

``` html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### الاستخدام مع الموكنات

> هذا القسم يفترض ان لديك معرفة بـ[مكونات Vue](components.html). لك مطلق الحرية في تخطي هذا الجزء والعودة إليه لاحقاً.

عندما تستخدم خاصية `class` على مكون مخصص، سوف يتم إضافة هذه الفئات إلى العنصر الرئيسي في المكون. إذا كان لهذا العنصر خصائص مسبقه، لن يتم استبدالها.

على سبيل المثال، اذا قمت بتعريف هذا المكون:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

وقمت بإضافة بعض الفئات عند إستخدامه:

``` html
<my-component class="baz boo"></my-component>
```

فإن كود HTML الذي سيتم معالجته وعرضه سيكون كالتالي:

``` html
<p class="foo bar baz boo">Hi</p>
```

أيضاً نفس الشيء سيحدث مع ربط الفئات:

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

عندما تكون قيمة `isActive` تساوي قيمة حقيقية، سيتم معالجة وعرض HTML كالتالي:

``` html
<p class="foo bar active">Hi</p>
```

## ربط الأنماط المضمنة

### صيغة الكائن

صيغة الكائن لـ`v-bind:style` مباشرة وواضحة - فهي تبدو مثل CSS بإستثناء أنها كائن چافاسكريبت. يمكنك إستخدام سواء طريقة camelCase أو kebab-case في كتابة الكود (قم بإستخدام علامات التنصيص مع kebab-case) لأسماء خواص CSS:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

هذا غالباً ما يكون فكرة جيدة لربط كائن الانماط مباشرة حتى ننتهي بكود أكثر نظافة ووضوح:

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
مرة أخرى، صيغة الكائن غالباً ما يتم إستخدامها مع الخواص المعالجة (computed properties) والتي تقوم بإرجاع كائن.

### صيغة المصفوفة

صيغة المصفوفة `v-bind:style` تمكنك من تطبيق أنماط متعددة لنفس العنصر:

``` html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### اضافة البادئات التلقائية

عند استخدامك لخصائص CSS التي تتطلب البادئات [vendor prefixes](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) في `v-bind:style`، على سبيل المثال `transform`، Vue سوف تقوم بالكشف التلقائي وتقوم بإضافة البادئات المطلوبة الى الانماط المطبقة.


### القيمة المتعددة

> 2.3.0+

بدئاً من الاصدار 2.3.0 يمكنك توفير مصفوفة مكونة من مجموعة من القيم (التي لها باطئة) لأنماط الخاصية، على سبيل المثال:

``` html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
هذا سوف قوم بمعالجة القيمة الاخيرة فقط في المصفوفة والتي يدعمها المستعرض. في هذا المثال، سوف يقوم بمعالجة `display: flex` للمستعرضات التي لا تدعم اصدار flexbox ذو البادئات

سيؤدي هذا فقط إلى عرض القيمة الأخيرة في المصفوفة التي يدعمها المتصفح. في هذا المثال ، سيتم عرض `display: flex` للمتصفحات التي تدعم الإصدار غير المثبت من flexbox.
