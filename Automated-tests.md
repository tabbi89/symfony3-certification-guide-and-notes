# Automated tests

#### Certification requirements

* [Unit tests with PHPUnit](#unit-tests)
* [Functional tests with PHPUnit](#functional-tests)
* [Client object](#client-object)
* [Crawler object](#crawler-object)
* Profile object
* Framework objects access
* Client configuration
* Request and response objects introspection
* PHPUnit bridge
* Handling legacy deprecated code

#### Extras

* Katas


## Unit tests with PHPUnit <a id="unit-tests"></a>

#### Links

[http://symfony.com/doc/3.0/testing.html#the-phpunit-testing-framework][doc-1]

[doc-1]: http://symfony.com/doc/3.0/testing.html#the-phpunit-testing-framework

#### You should know

* how to generate code coverage
* [how to use data providers][up-1]
* [how to test exceptions][up-2]
* [how to test PHP errors][up-3]
* [how to test output][up-4]
* [how to use fixtures][up-5]
* [how to mock objects][up-6]
* [how to mock traits and abstract classes][up-7]
* [what assetsions are available][up-8]
* [what annotations are available][up-9]

[up-1]: https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.data-providers
[up-2]: https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.exceptions
[up-3]: https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.errors
[up-4]: https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.output
[up-5]: https://phpunit.de/manual/current/en/fixtures.html
[up-6]: https://phpunit.de/manual/current/en/test-doubles.html#test-doubles.mock-objects
[up-7]: https://phpunit.de/manual/current/en/test-doubles.html#test-doubles.mocking-traits-and-abstract-classes
[up-8]: https://phpunit.de/manual/current/en/appendixes.assertions.html
[up-9]: https://phpunit.de/manual/current/en/appendixes.annotations.html

## Functional tests with PHPUnit <a id="functional-tests"></a>

#### Links

* [http://symfony.com/doc/3.0/testing.html#functional-tests][doc-2]

[doc-2]: http://symfony.com/doc/3.0/testing.html#functional-tests

#### You should know

* [that to test functional test you can use Symfony\Bundle\FrameworkBundle\Test\WebTestCase][fp-1]
* [how to execute `requests()` using WebTestCase][fp-2]
* [that the `request()` returns Crawler object][fp-3]
* that Crawler only works when the response is an XML or an HTML
* that to get the raw content response, call `$client->getResponse()->getContent()`
* how to use filter in Crawler object
* how to select form in Crawler object
* how to fill and submit form in Crawler object
* [usefull assertions][fp-4]

[fp-1]: https://github.com/symfony/framework-bundle/blob/3.0/Test/WebTestCase.php
[fp-2]: http://api.symfony.com/2.3/Symfony/Bundle/FrameworkBundle/Client.html#method_request
[fp-3]: http://api.symfony.com/3.0/Symfony/Component/DomCrawler/Crawler.html
[fp-4]: http://symfony.com/doc/3.0/testing.html#your-first-functional-test

## Client object <a id="client-object"></a>

#### Links

* [http://symfony.com/doc/3.0/testing.html#working-with-the-test-client][doc-3]

[doc-3]: http://symfony.com/doc/3.0/testing.html#working-with-the-test-client

#### You should know

* [that hardcoding the request URLs is a best practice for functional tests]
* what parameters are in `request()` method
* click() and submit() methods both return a Crawler object and are the best way to browse your application
* that you can use `request()` method to directly submit form
* that you can use `request()` method to directly upload file
* that to execute request in its own PHP process you can use `client->insulate()`
* [that you can browse by calling `back()`, `forward()`, `reload()`, `restart()`][co-1]
* [how to get access to client internal objects][co-2]


[co-1]: http://symfony.com/doc/3.0/testing.html#browsing
[co-2]: http://symfony.com/doc/3.0/testing.html#accessing-internal-objects


## Crawler object <a id="crawler-object"></a>

#### Links

* [http://symfony.com/doc/3.0/testing.html#the-crawler][doc-4]

[doc-4]: http://symfony.com/doc/3.0/testing.html#the-crawler