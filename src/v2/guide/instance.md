---
title: كائن Vue
type: guide
order: 3
---

## إنشاء كائن Vue

كل تطبيق Vue يبدأ بإنشاء **كائن Vue** ويتم إنشائه بإستخدام الدالة `Vue`:

```js
var vm = new Vue({
  // options
})
```

على الرغم من عدم ارتباطه بشكل وثيق بـ[نمط MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel)، فقد استلهم تصميم Vue جزئياً من ذلك. وكقاعدة عامة، غالباً ما نستخدم المتغير `vm` (اختصاراً لـViewModel) للإشارة الى كائن Vue.

عندما تقوم بإنشاء كائن Vue، يجب عليك تمرير كائن يحتوي على الخيارات **options object**. أغلب هذا الدليل يشرح كيف نستطيع استخدام هذه الخيارات لإنشاء والتحكم في السلوك المرغوب فيه. وكمرجع، يمكنك تصفح القائمة الكاملة للخيارات المتاحة في صفحة [مرجع API](../api/#Options-Data).

تطبيق Vue يتكون من **كائن Vue رئيسي** والذي يتم إنشائه من خلال `new Vue`، منظم بشكل إختياري في هئية شجرة من المكونات المتداخلة والقابلة لإعادة الاستخدام. على سبيل المثال، شجرة المكونات لتطبيق المهام `Todo App` قد تبدو على النحو الآتي:

```
Root Instance---------------------> المكون الرئيسي
└─ TodoList-----------------------> مكون فرعي  من المكون الرئيسي
   ├─ TodoItem--------------------> TodoList مكون فرعي من
   │  ├─ DeleteTodoButton---------> TodoItem مكون فرعي من
   │  └─ EditTodoButton-----------> TodoItem مكون فرعي من
   └─ TodoListFooter--------------> TodoList مكون فرعي من
      ├─ ClearTodosButton---------> TodoListFooter مكون فرعي من
      └─ TodoListStatistics-------> TodoListFooter مكون فرعي من
```

سوف نتحدث عن [نظام المكونات](components.html) بالتفصيل لاحقاً. الان، يجب فقط ان تعرف ان جميع المكونات في Vue هي أيضاً نماذج من كائن Vue، وتبقل أيضاً نفس كائن الخيارات (بإستثناء بعض الخيارات الخاصة بالمكون الرئيسي).

## كائن البيانات و الدوال

من ضمن الخيارات التي نقوم بإمدادها إلى كائن Vue هي الكائن `data` والذي يحتوي على الخصائص والبيانات الخاصة بالمكون.

عندما يتم إنشاء كائن Vue، يقوم بإضافة جميع الخصائص الموجودة في كائن الخيارات المسمى `data` إلى **نظام التفاعل**  الخاص بـVue. وعندما يتم تغيير القيم الخاصة بهذه الخصائص، سيتم التفاعل في واجهة المستخدم لتحديث القيم إلى القيم الجديدة.

```js
// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
```

عندما يتم تغيير هذه البيانات، سوف تقوم واجهة المستخدم بالتحديث. يجب الإنتباه إلى ان الخصائص المتواجدة في كائن البيانات `data` تصبح **تفاعلية** فقط عندما يتم إضافتها داخل كائن Vue عن إنشائه. هذا يعني انه اذا قمت على سبيل المثال بإضافة الخاصية بالشكل التالي:

```js
vm.b = 'hi'
```

عندها التغييرات التي حدثت على الخاصية `b` لن تفعل اي تحديث في الواجهة. إذا كنت تعرف انك ستحتاج إلى خاصية ما لاحثاً، يجب عليك ان تضعها في كائن `data` وإعطائها قيمة مبدئية حتى وان كانت قيمة فارغة. على سبيل المثال:

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

الاستثناء الوحيد لهذه القاعدة هو استخدام الدالة `Object.freeze()`، والتي تمنع الخصائص الحالية من التغيير، والذي يعني ايضاً ان نظام التفاعل لن يستطيع _تتبع_ التغييرات.

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- this will no longer update `foo`! -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

بالاضافة الى خصائص البيانات `data`، كائنات Vue تقوم بتوفير عدد من الخصائص والدوال المفيدة. هذه الخصائص والدوال يتم بدئها ب `$` للتفرقة بينها وبين الخصائص المعرفة من قبل المستخدم. على سبيل المثال:

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
  // This callback will be called when `vm.a` changes
})
```

في المستقبل، يمكنك استشارة [دليل API](../api/#Instance-Properties) لقائمة كاملة بكل الخصائص والدوال المتاحة من قبل كائن Vue.

## خطافات دورة حياة الكائن

<div class="vueschool"><a href="https://vueschool.io/lessons/understanding-the-vuejs-lifecycle-hooks?friend=vuejs" target="_blank" rel="noopener" title="درس مجاني عن خطافات دورة حياة Vue.js">مشاهدة الدرس المجاني على Vue School</a></div>

كل كائن Vue يمر عبر سلسلة من خطوات التهيئة عندما يتم إنشائه - على سبيل المثال، يحتاج الى اعداد مراقبة للبيانات، تجميع القالب، تركيب الكائن على DOM و تحديث DOM عند تغيير البيانات. على طول الطريق، يعمل أيضاً على تشغيل وظائف تسمى **خطافات دورة الحياة** ، مما يتيح للمستخدمين الفرصة لإضافة التعليمات البرمجية الخاصة بهم في مراحل محددة.

على سبيل المثال، الخطاف  [`created`](../api/#created) يمكن استخدامه لتشغيل كود برمجي بعد إنشاء الكائن بنجاح:

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```
يوجد ايضاً خطافات اخرى والتي يتم استدعائها في مراحل مختلفة من دورة حياة الكائن، مثل [`mounted`](../api/#mounted)، [`updated`](../api/#updated) و [`destroyed`](../api/#destroyed). جميع خطافات دورة الحياة يتم الاشارة اليها باستخدام `this` الخاص بالكائن والتي تشير الى كائن Vue الذي يستدعيها.

لا تقم باستخدام [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) على احد خصائص الخيارات او دالة مرجعية، مثل `created: () => console.log(this.a)` او `vm.$watch('a', newValue => this.myMethod())`. نظراً لان دالة السهم `arrow function` لا تحتوي على `this`، `this` سوف يتم معاملتها مثل اي متغير اخر وسيتم البحث عنه بشكل معجمي من خلال النطاقات الرئيسية حتى يتم العقور عليه وغالباً ما ينتج عنه أخطاء مثل `Uncaught TypeError: Cannot read property of undefined` او `Uncaught TypeError: this.myMethod is not a function`.

## مخطط دورة الحياة

بالاسفل يوجد مخطط لدورة حياة الكائن. ليس عليك ان تفهم كل ما يحدث في الوقت الحالي، ولكن عندما تتعلم ستبني المزيد من المعرفة، يمكنك ان تعتبره كمرجع مفيد.

![The Vue Instance Lifecycle](/images/lifecycle.png)
