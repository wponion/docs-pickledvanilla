# Getting Started

![](.gitbook/assets/banner.jpg)

## What ?

domQ is an absurdly small jQuery alternative for modern browsers \(IE11+\) that provides jQuery-style syntax for manipulating the DOM. Utilizing modern browser features to minimize the codebase, developers can use the familiar chainable methods at a fraction of the file size. 100% feature parity with jQuery isn't a goal, but domQ comes helpfully close, covering most day to day use cases.

## Why ?

> I was very bored with jquery script and wanted to try something else🙈.

Well as a web developer i wanted to move out from jQuery and use Vanilla JS. When i realized i had to write multiple lines of code for a single action thats when i decided to create a lightweight javascript library that can do most things that `jQuery` can.

## Comparison

| Size | domQ | domQ + Dizzle | Zepto | jQuery Slim |
| :--- | :---: | :---: | :---: | :---: |
| Unminified | 48.7 KB | 80.2 KB | 58.7 KB | 250 KB |
| Minified | 21.5 KB | 32.4 KB | 26 KB | 70.6 KB |
| Minified & Gzipped | 7.91KB | 11.71KB | 9.8 KB | 24.4 KB |

| Features | domq | domQ + Dizzle | Zepto | jQuery Slim |
| :--- | :---: | :---: | :---: | :---: |
| Supports Older Browsers | ❌ | ❌ | ❌ | ✔ |
| Supports Modern Browsers | ✔ | ✔ | ✔ | ✔ |
| Actively Maintained | ✔ | ✔ | ❌ | ✔ |
| Namespaced Events | ✔ | ✔ | ❌ | ✔ |
| jQuery Selectors | ✔ | ✔ | ⚠️\(Experimental Feature\) | ✔ |
| \*\* Animation | ✔ | ✔ | ⚠️\(Custom Workaround\) ️ | ❌ |

{% hint style="info" %}
 domQ uses [WebAnimation's](https://github.com/web-animations/web-animations-js) API
{% endhint %}

## Usage

Get domQ from CloudFlare or jsDelivr and use it like this:

### jsDelivr

1. **domQ** : [jsDelivr](https://cdn.jsdelivr.net/npm/domq/dist/domq.standalone.umd.min.js)
2. **domQ + Dizzle** : [jsDelivr](https://cdn.jsdelivr.net/npm/domq/dist/domq.bundled.umd.min.js)

```markup
<script src="https://cdn.jsdelivr.net/npm/domq/dist/domq.bundled.umd.min.js"></script>
<script>
  domQ(function () {
    domQ('html').addClass ( 'domq-works' );
    domQ('<footer>Appended with domQ</footer>').appendTo ( document.body );
  });
</script>
```

domQ is also available through [npm](https://npmjs.com/) as the [`domq`](https://npmjs.com/package/domq) package:

```text
npm install --save domq
```

That you can then use like this:

```javascript
import domq from "domq";

domq(function () {
  domq('html').addClass ( 'domq-works' );
  domq('<footer>Appended with domQ</footer>').appendTo ( document.body );
});
```

## 📝 Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

[Checkout CHANGELOG.md](https://github.com/varunsridharan/domq/blob/docs/CHANGELOG.md)

## 🤝 Contributing

If you would like to help, please take a look at the list of [issues](https://github.com/varunsridharan/domq/blob/docs/issues).

## 💰 Sponsor

[I](https://sva.onl/twitter/) fell in love with open-source in 2013 and there has been no looking back since! You can read more about me [here](https://sva.onl/website/). If you, or your company, use any of my projects or like what I’m doing, kindly consider backing me. I'm in this for the long run.

* ☕ How about we get to know each other over coffee? Buy me a cup for just [**$9.99**](https://sva.onl/buymeacoffee)
* ☕️☕️ How about buying me just 2 cups of coffee each month? You can do that for as little as [**$9.99**](https://sva.onl/buymeacoffee)
* 🔰 We love bettering open-source projects. Support 1-hour of open-source maintenance for [**$24.99 one-time?**](https://sva.onl/paypal)
* 🚀 Love open-source tools? Me too! How about supporting one hour of open-source development for just [**$49.99 one-time ?**](https://sva.onl/paypal)

## 📜 License & Conduct

* [**MIT**](https://github.com/varunsridharan/domq/blob/docs/LICENSE) © [Varun Sridharan](https://github.com/varunsridharan/domq/blob/docs/website)
* [Code of Conduct](https://github.com/varunsridharan/domq/blob/docs/code-of-conduct.md)

## 📣 Feedback

* ⭐ This repository if this project helped you!😉
* Create An[🔧 Issue](https://github.com/varunsridharan/domq/blob/docs/issues) if you need help / found a bug

## Connect & Say 👋

* **Follow** me on[👨‍💻 Github](https://sva.onl/github/) and stay updated on free and open-source software
* **Follow** me on[🐦 Twitter](https://sva.onl/twitter/) to get updates on my latest open source projects
* **Message** me on[📠 Telegram](https://sva.onl/telegram/)
* **Follow** my pet on [Instagram](https://www.instagram.com/sofythelabrador/) for some _dog-tastic_ updates!

