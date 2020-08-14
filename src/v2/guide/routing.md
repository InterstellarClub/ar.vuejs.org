---
title: الموجه
type: دليل
order: 501
---

# المٌوجه الرسمي

  بالنسبة لمعظم تطبيقات الصفحة الواحدة ، يوصى باستخدام تنسيق <a href="https://github.com/vuejs/vue-router">مكتبة vue-router</a>. للمزيد من التفاصيل, انظر إلى <a href="https://router.vuejs.org">توثيق</a> vue-router
</p>

<h2 dir="rtl">موجه بسيط من الصفر</h2>

إذا كنت تحتاج فقط إلى موجه بسيط للغاية ولا ترغب في تضمين مكتبة التوجيه كاملة الميزات ، فيمكنك القيام بذلك عن طريق عرض مكون على مستوى الصفحة ديناميكيًا مثل هذا:
</p>

``` js
const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

<p dir="rtl">
بالاقتران مع HTML5 History API ، يمكنك إنشاء موجه أساسي جدًا ولكنه يعمل بكامل طاقته من جانب العميل. لمعرفة ذلك عمليًا ، تحقق من <a href="https://github.com/chrisvfritz/vue-2.0-simple-routing-example">مثال التطبيق هذا</a>
</p>  

<h2 dir="rtl">دمج أجهزة توجيه الطرف الثالث</h2>
<p dir="rtl">
  إذا كنت تفضل استخدام جهاز توجيه تابع لجهة خارجية ، مثل <a href="https://github.com/visionmedia/page.js">Page.js</a> أو <a href="https://github.com/flatiron/director">مخرج</a>، التكامل
  <a href="https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs">سهل بالمثل</a>. إليك
  <a href="https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs">مثال كامل</a> باستخدام Page.js.
</p>
