---
title: الموجه
type: دليل
order: 501
---

# حزمة التوجيه الرسمية

  بالنسبة لمعظم تطبيقات الصفحة الواحدة ، يوصى باستخدام [حزمة التوجيه المدعمة رسمياً vue-router](https://github.com/vuejs/vue-router). لمزيد من التفاصيل، انظر إلى [وثائق حزمة التوجيه](https://router.vuejs.org/).

## موجه بسيط من الصفر

إذا كنت تحتاج فقط إلى موجه بسيط للغاية ولا ترغب في تضمين مكتبة التوجيه كاملة الميزات ، فيمكنك القيام بذلك عن طريق عرض مكون على مستوى الصفحة ديناميكيًا مثل هذا:

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

بالاقتران مع HTML5 تاريخ API ، يمكنك إنشاء موجه أساسي جدًا ولكنه يعمل بكامل طاقته من جانب العميل. لمعرفة ذلك عمليًا ، تحقق من [مثال التطبيق هذا](https://github.com/chrisvfritz/vue-2.0-simple-routing-example)

## الدمج مع حزم التوجيه الاخرى

إذا كنت تفضل استخدام حزمة توجيه اخرى ، مثل [Page.js](https://github.com/visionmedia/page.js) أو [Director](https://github.com/flatiron/director)، التكامل [سهل بشكل مشابه](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs). إليك [مثال كامل](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs باستخدام Page.js.
