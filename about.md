---
description: >-
  About Docket Cache and some relevant information.
---

# About

## Prologue

The Docket cache is a persistent WordPress Object Cache that is stored as a plain PHP code. Intends to provide an alternative option for those who can't use Redis or Memcached server.

Rather than using [serialize](https://www.php.net/manual/en/function.serialize.php) and [unserialize](https://www.php.net/manual/en/function.unserialize.php) a PHP object to store into flat files, this plugin stores data by converting the object into plain PHP code which results in faster data retrieval and better performance with Zend OPcache enabled.

## Manifesto

When it comes to reliable persistent Object Cache in WordPress, [Redis](https://redis.io) or [Memcached](https://memcached.org) comes on top. However, those solutions require knowledge of server and rarely available at low cost or shared hosting servers

The only solution is to store the object caches into files. With WordPress, exporting the PHP objects are not easy, most plugin that implements file-based solution will `serialize` and `unserialize` the object to store and retrieve the data.

Docket Cache takes a better approach by turning the object cache into plain PHP code. This solution is faster since WordPress can use the cache directly without running other operations.

## How Versions Work

Versions are as follows: Year.Month.Day

* Year: Two digits representation of a year.
* Month: Two digits representation of a month.
* Day: Two digits representation of a day.

## Contributions

Anyone can contribute to Docket Cache. Please do so by posting issues when you've found something that is unexpected or sending a pull request for improvements.

* [Report issues](https://wordpress.org/support/plugin/docket-cache/)
* [Github Repo](https://github.com/nawawi/docket-cache/)

## License

{% hint style="info" %}
Docket cache is an Open Source Software under the MIT License.
{% endhint %}

Docket Cache

Copyright (c) 2020-present [Nawawi Jamili](https://github.com/nawawi)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Credits

Some parts of the Docket Cache code are borrowed from different open-source projects.\
The full list can be found here [https://github.com/nawawi/docket-cache/blob/master/credits.tx](https://github.com/nawawi/docket-cache/blob/master/credits.txt).

