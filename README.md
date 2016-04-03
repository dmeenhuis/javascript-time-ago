# react-time-ago

[![NPM Version][npm-badge]][npm]
[![Build Status][travis-badge]][travis]

International relative date/time formatter along with a React component.

Formats a date to something like:

  * just now
  * 5m
  * 15 min
  * 25 minutes
  * half an hour ago
  * an hour ago
  * 2h
  * yesterday
  * 2d
  * 1wk
  * 2 weeks ago
  * 3 weeks
  * half a month ago
  * 1mo
  * 2 months
  * half a year
  * 1 year
  * 2yr
  * 5 years ago
  * … or whatever else

## Installation

**This package hasn't been released to npm yet. It will be released this week.**

```bash
npm install react-time-ago --save
```

This package assumes that the [`Intl`][Intl] global object exists in the runtime. `Intl` is present in all modern browsers _except_ Safari (which can be solved with the Intl polyfill).

Node.js 0.12 has the `Intl` APIs built-in, but only includes the English locale data by default. If your app needs to support more locales than English, you'll need to [get Node to load the extra locale data](https://github.com/nodejs/node/wiki/Intl), or (a much simpler approach) just install the Intl polyfill.

If you decide you need the Intl polyfill then [here are some basic installation and configuration instructions](#intl-polyfill-installation)

## Usage

```js
import react_time_ago from 'react-time-ago'

// Load locale specific relative date/time messages
import { short as english } from 'react-time-ago/locales/en'
import { long as russian }  from 'react-time-ago/locales/ru'

// Load number pluralization functions for the locales.
// (the ones that decide if a number is gonna be 
//  "zero", "one", "two", "few", "many" or "other")
// http://cldr.unicode.org/index/cldr-spec/plural-rules
//
// If you are already using `react-intl` in your project
// and have already `require()`d `react-intl` locale data
// for these locales then this step is unnecessary
global.IntlMessageFormat = require('intl-messageformat')
require('intl-messageformat/dist/locale-data/en')
require('intl-messageformat/dist/locale-data/ru')

// Add locale specific relative date/time messages
react_time_ago.locale('en', english)
react_time_ago.locale('ru', russian)

// Initialization complete.
// Ready to format relative dates and times.

const time_ago_english = new react_time_ago('en-US')
console.log(time_ago_english.format(new Date()))

const time_ago_russian = new react_time_ago('ru-RU')
console.log(time_ago_russian.format(new Date()))
```

## Intl polyfill installation

To install the Intl polyfill (supporting 200+ languages):

```bash
npm install intl --save
```

Then configure the Intl polyfill:

  * [Node.js](https://github.com/andyearnshaw/Intl.js#intljs-and-node)
  * [Webpack](https://github.com/andyearnshaw/Intl.js#intljs-and-browserifywebpack)
  * [Bower](https://github.com/andyearnshaw/Intl.js#intljs-and-bower)

## CLDR

This library currently comes with English and Russian localization.

This library is compatible with [Unicode CLDR][CLDR] (Common Locale Data Repository) which is an industry standard and is basically a collection of formatting rules for all locales (date, time, currency, measurement units, numbers, etc).

[Example for en-US-POSIX locale](https://github.com/unicode-cldr/cldr-dates-full/blob/master/main/en-US-POSIX/dateFields.json)

```js
{
  "main": {
    "en-US-POSIX": {
      "dates": {
        "fields": {
          …
          "day": {
            "displayName": "day", // `displayName` field is not used
            "relative-type--1": "yesterday", // is optional
            "relative-type-0": "today", // is optional
            "relative-type-1": "tomorrow", // is optional
            "relativeTime-type-future": {
              "relativeTimePattern-count-one": "in {0} day",
              "relativeTimePattern-count-other": "in {0} days"
            },
            "relativeTime-type-past": {
              "relativeTimePattern-count-one": "{0} day ago",
              "relativeTimePattern-count-other": "{0} days ago"
            }
          },
          …
        }
      }
    }
  }
}
```

To add support for a specific language one should download the corresponding JSON file from [CLDR dates repository](https://github.com/unicode-cldr/cldr-dates-full/blob/master/main) and add the data from that file to the library:

```js
import react_time_ago from 'react-time-ago'
import russian from './CLDR/cldr-dates-full/main/ru/dateFields.json'

react_time_ago.locale('ru', russian.main.ru.dates.fields)

const time_ago = new react_time_ago('ru')
const text = time_ago.format(new Date())
```

## Customization

One is free to customize the output by supplying his own locale data to the library. If that's not enough then create an issue and maybe an appropriate solution can be implemented.

## Contributing

After cloning this repo, ensure dependencies are installed by running:

```sh
npm install
```

This module is written in ES6 and uses [Babel](http://babeljs.io/) for ES5
transpilation. Widely consumable JavaScript can be produced by running:

```sh
npm run build
```

Once `npm run build` has run, you may `import` or `require()` directly from
node.

After developing, the full test suite can be evaluated by running:

```sh
npm test
```

While actively developing, one can use (personally I don't use it)

```sh
npm run watch
```

in a terminal. This will watch the file system and run tests automatically 
whenever you save a js file.

When you're ready to test your new functionality on a real project, you can run

```sh
npm pack
```

It will `build`, `test` and then create a `.tgz` archive which you can then install in your project folder

```sh
npm install [module name with version].tar.gz
```

## License

[MIT](LICENSE)
[npm]: https://img.shields.io/npm/v/react-time-ago.svg?style=flat-square
[npm-badge]: https://www.npmjs.org/package/react-time-ago
[travis]: https://travis-ci.org/halt-hammerzeit/react-time-ago
[travis-badge]: https://img.shields.io/travis/halt-hammerzeit/react-time-ago/master.svg?style=flat-square
[CLDR]: http://cldr.unicode.org/
[Intl]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl