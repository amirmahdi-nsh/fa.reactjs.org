---
id: code-splitting
title: تکه تکه کردن کد
permalink: docs/code-splitting.html
---

## بسته‌بندی کردن (bundling) {#bundling}

در بیشتر برنامه های ری‌اکت فایل ها توسط ابزار هایی مثل [Webpack](https://webpack.js.org/) یا [Browserify](http://browserify.org/) بسته بندی می شوند

پروسه‌ی بسته‌بندی کردن به این شکل است که فایل های import شده دنبال می‌شود و همه‌ی آن‌ها در یک فایل با نام "bundle" ادغام می‌شوند. این فایل bundle می‌تواند در صفحه‌ی وب بارگذاری شود تا کل برنامه را به یک‌باره اجرا کند.

#### مثال {#example}

**App:**

```js
// app.js
import { add } from './math.js';

console.log(add(16, 26)); // 42
```

```js
// math.js
export function add(a, b) {
  return a + b;
}
```

**Bundle:**

```js
function add(a, b) {
  return a + b;
}

console.log(add(16, 26)); // 42
```

> نکته:
>
> بسته‌های شما در نهایت با این نمونه تفاوت زیادی خواهد داشت.

اگر شما از [Gatsby](https://www.gatsbyjs.org/) ،[Next.js](https://nextjs.org/) ،[Create React App](https://create-react-app.dev/) یا ابزار های مشابه استفاده می‌کنید، به‌طور پیش‌فرض Webpack تنظیم شده‌است تا برنامه‌ی شما را بسته‌بندی کند.

در غیر این صورت، لازم است که پروسه‌ی بسته‌بندی را خودتان تنظیم و راه‌اندازی کنید. برای نمونه راهنمایی‌های [نصب](https://webpack.js.org/guides/installation/) و [شروع به کار](https://webpack.js.org/guides/getting-started/) را از مستندات Webpack مشاهده کنید.

## تکه‌تکه کردن کد {#code-splitting}

بسته‌بندی بسیار خوب است، اما همواره با رشد و بزرگ شدن برنامه‌تان، فایل bundle شما نیز بزرگ می‌شود. به ویژه اگر شما از کتابخانه‌های جانبی استفاده کنید. 
لازم است که همیشه چشمتان به کدهایی باشد که به فایل بسته اضافه می‌کنید. اگر به صورت تصادفی حجم فایل بسته زیاد شود، درنتیجه زمان بارگذاری اولیه برنامه شما طولانی می‌شود.

برای جلوگیری از درگیر شدن با مشکلات بسته‌ی بزرگ، بهتر است که قبل از ایجاد مشکل شروع به "تکه‌تکه" کردن فایل بسته‌تان کنید.
 [تکه‌تکه کردن کد](https://webpack.js.org/guides/code-splitting/) امکانی است که توسط کتابخانه‌های بسته‌بندی کننده مثل Webpack و Browserify (توسط
[factor-bundle](https://github.com/browserify/factor-bundle)) پشتیبانی می‌شود به این‌صورت که می‌توانند چندین فایل bundle ایجاد کنند که هنگام اجرای برنامه بصورت پویا بارگذاری شوند.

تکه‌تکه کردن برنامه‌تان به شما کمک می‌کند که تنها چیز‌هایی که در حال حاضر کاربر نیاز دارد را به روش "lazy-load" بارگیری کنید که به صورت جشم‌گیری کارایی برنامه ی شما را افزایش می‌دهد. با وجود این‌که شما مقدار کلی کد برنامه خود را کاهش نداده‌اید، حجم کد موردنیاز برای بارگذاری اولیه کاهش پیدا می‌کند. زیرا از بارگیری کدی که ممکن است کاربر اصلا نیازی به آن نداشته باشد، جلوگیری کرده‌اید.


## `import()` {#import}

بهترین راه برای شروع استفاده از تکه‌تکه کردن کد در برنامه‌تان استفاده از سینتکس `import()` پویا است.


**قبل:**

```js
import { add } from './math';

console.log(add(16, 26));
```

**بعد:**

```js
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

هنگامی که Webpack با این سینکتس برخورد می کند، بصورت خودکار شروع به تکه‌تکه کردن کد برنامه ی شما می‌کند. اگر شما از Create React App استفاده می‌کنید، این در حال حاضر برای شما تنظیم شده‌است و شما می‌توانید همین حالا [از اینجا شروع به استفاده از آن کنید](https://create-react-app.dev/docs/code-splitting/). همچنین به‌صورت پیش‌فرض توسط [Next.js](https://nextjs.org/docs/advanced-features/dynamic-import) نیز پشتیبانی می‌شود.

اگر شما Webpack را خودتان تنظیم و راه‌اندازی کرده‌اید، ممکن است بخواهید [راهنمای تکه‌تکه کردن کد Webpack](https://webpack.js.org/guides/code-splitting/) را بخوانید. پیکربندی Webpack شما احتمالا چیزی [شبیه به این](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269) باشد.

هنگام استفاده از [Babel](https://babeljs.io/)، شما باید اطمینان حاصل کنید که Babel می‌تواند سینتکس import پویا را parse کند ولی آن را تغییر ندهد. در این راستا شما به [babel-plugin-syntax-dynamic-import](https://classic.yarnpkg.com/en/package/@babel/plugin-syntax-dynamic-import) نیاز خواهید داشت.


## `React.lazy` {#reactlazy}

> نکته:
>
> `React.lazy` و Suspense هنوز برای رندر کردن در سمت سرور در دسترس نیست. اگر شما می خواهید کد تان را در برنامه‌ای که سمت سرور رندر می‌شود تکه تکه کنید، ما به شما [Loadable Components](https://github.com/gregberge/loadable-components) را پیشنهاد می‌کنیم که [راهنمای خوبی برای تکه‌تکه کردن bundle در رندر سمت سرور](https://loadable-components.com/docs/server-side-rendering/) دارد.

تابع `React.lazy` به شما اجازه می‌دهد که یک import پویا را به عنوان یک component معمولی رندر کنید.

**قبل:**


```js
import OtherComponent from './OtherComponent';
```

**بعد:**

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

هنگامی که کامپوننت فوق رندر می شود، بصورت خودکار فایل bundle ای که حاوی `OtherComponent` است را بارگیری می‌کند.

`React.lazy` یک تابع به عنوان ورودی می‌گیرد که یک import() پویا را صدا می‌کند. که یک promise برمی‌گرداند که resolves آن یک ماژول با خروجی default ای است که حاوی یک کامپوننت ری‌اکت است.

کامپوننت lazy باید درون یک کامپوننت `Suspense` رندر شود، که به ما امکان نمایش یک مجتوای جایگزین (fallback) هنگامی که کامپوننت در حال بارگذاری است، می‌دهد.

```js
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

prop `fallback` هر المان ری‌اکتی که شما می‌خواهید در بازه‌ی صبر کردن تا بارگیری کامپوننت اصلی رندر کنید را به عنوان ورودی می‌پذیرد. شما می‌توانید هر جایی کامپوننت `Suspense` را در بالای کامپوننت lazy قرار دهید. حتی شما می‌توانید چندین کامپوننت lazy را داخل یک کامپوننت `Suspense` قرار دهید.

```js
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

### مرز‌های خطا (Error Boundaries)‌ {#error-boundaries}

اگر یک ماژول در هنگام بارگیری با مشکل مواجه شود (برای مثال، به خاطر مشکلات شبکه)، خطا می دهد. شما می توانید خطا‌های این‌چنینی را مدیریت کنید که تجربه کاربری خوبی را نشان داده و بازیابی را با [مرز های خطا](/docs/error-boundaries.html) مدیریت کنید. هنگامی که مرز خطای‌تان را ساختید، شما می‌توانید از آن در هر جایی در بالای کامپوننت lazy تان برای نمایش حالت خطا هنگامی که مشکلی در شبکه وجود دارد، استفاده کنید.

```js
import React, { Suspense } from 'react';
import MyErrorBoundary from './MyErrorBoundary';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```

## تکه‌تکه کردن کد بر اساس مسیر‌ها {#route-based-code-splitting}

تشخیص این‌که در کدام سطح از برنامه‌تان از تکه‌تکه کردن کد استفاده کنید می‌تواند کمی مشکل باشد. شما می‌خواهید اطمینان حاصل کنید که جاهایی را انتخاب کنید که bundle ها را بصورت مساوی تقسیم کنید، اما تجربه‌ی کاربر را مختل نکنید.

مسیر‌ها نقطه شروع خوبی هستند. بیشتر افراد در محیط وب به زمان‌بر بودن انتقال از صفحه‌ای به صفحه دیگر عادت کرده‌اند. شما هم معمولا تمام صفحه را یک‌باره مجدد رندر می‌کنید، بنابراین دور از ذهن به‌نظر می‌رسد که کاربران شما با دیگر المان‌های صفحه در حال کار کردن باشند.

این‌جا یک مثال از چگونگی راه‌اندازی تکه‌تکه کردن کد بر‌پایه‌ی مسیر (route-based code splitting) در داخل برنامه‌تان با استفاده از کتابخانه‌هایی مثل [React Router](https://reacttraining.com/react-router/) با `React.lazy` را مشاهده می‌کنید.

```js
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

## export های نامگذاری شده {#named-exports}

در حال حاضر `React.lazy` صرفا export های پیش‌فرض را پشتیبانی می‌کند، اگر ماژولی که شما می‌خواهید import کنید از export نام‌گذاری شده استفاده میکند، شما می‌توانید یک ماژول میانجی ایجاد کنید که آن را به‌صورت پیش‌فرض export می‌کند. این تضمین می‌کند که ساختار درختی برنامه همچنان به‌صورت صحیح کار می‌کند و شما کامپوننت بدون استفاده‌ای را درخواست نکرده‌اید.

```js
// ManyComponents.js
export const MyComponent = /* ... */;
export const MyUnusedComponent = /* ... */;
```

```js
// MyComponent.js
export { MyComponent as default } from "./ManyComponents.js";
```

```js
// MyApp.js
import React, { lazy } from 'react';
const MyComponent = lazy(() => import("./MyComponent.js"));
```
