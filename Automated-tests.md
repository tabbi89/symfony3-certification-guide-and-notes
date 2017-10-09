# Automated tests

#### Certification requirements

* [Unit tests with PHPUnit](#unit-tests)
* [Functional tests with PHPUnit](#functional-tests)
* [Client object](#client-object)
* [Crawler object](#crawler-object)
* [Profiler object](#profiler-object)
* [Framework objects access](#framework-objects-access)
* [Client configuration](#client-configuration)
* Request and response objects introspection
* [PHPUnit bridge](#phpunit-bridge)
* Handling legacy deprecated code

#### Extras

* Katas (TODO)


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

#### You should know

* [how to traverse the DOM of an HTML/XML document][co-1]
* [how to extract information][co-2]
* [how to use links][co-3]
* [how to use forms][co-4]
* [that when adding or removing collection of forms you have to add the values to the raw data array][co-5] 	


[co-1]: http://symfony.com/doc/3.0/testing.html#traversing
[co-2]: http://symfony.com/doc/3.0/testing.html#extracting-information
[co-3]: http://symfony.com/doc/3.0/testing.html#links
[co-4]: http://symfony.com/doc/3.0/testing.html#forms
[co-5]: http://symfony.com/doc/3.0/testing.html#adding-and-removing-forms-to-a-collection

## Profiler object <a id="profiler-object"></a>

#### Links

* [http://symfony.com/doc/3.0/testing.html#accessing-the-profiler-data][doc-5]
* [http://symfony.com/doc/3.0/testing/profiling.html][doc-6]

[doc-5]: http://symfony.com/doc/3.0/testing.html#accessing-the-profiler-data
[doc-6]: http://symfony.com/doc/3.0/testing/profiling.html

#### You should know

* that profiler is enabled in test environment
* [how to speed up test bu not collecting profiler data][po-1]
* that when you disable collecting data in configuration you can enable it per test `$client->enableProfiler()`

[po-1]: http://symfony.com/doc/3.0/testing/profiling.html#speeding-up-tests-by-not-collecting-profiler-data

## Framework objects access <a id="framework-objects-access"></a>

#### Links

* [http://symfony.com/doc/3.0/testing.html#accessing-internal-objects][doc-7]
* [http://symfony.com/doc/3.0/testing.html#accessing-the-container][doc-8]

[doc-7]: http://symfony.com/doc/3.0/testing.html#accessing-internal-objects
[doc-8]: http://symfony.com/doc/3.0/testing.html#accessing-the-container

#### You should know

* that to get container you can use `$client->getContainer()`
* that when you use `$client->insulate()` or using real HTTP requests returned container will be different

## Client configuration <a id="client-configuration"></a>

#### Links

* [http://symfony.com/doc/3.0/testing.html#testing-configuration][doc-9]

[doc-9]: http://symfony.com/doc/3.0/testing.html#testing-configuration

#### You should know

* that by default swiftmailer delivery is disabled
* [that in the test env you shoulde have enabled `framework.test`][cc-1]
* [how to configure client][cc-2]

[cc-1]: http://symfony.com/doc/3.0/reference/configuration/framework.html#reference-framework-test
[cc-2]: https://github.com/symfony/framework-bundle/blob/3.0/Test/WebTestCase.php#L31

## PHPUnit bridge <a id="phpunit-bridge"></a>

#### Links

* [http://symfony.com/doc/3.0/components/phpunit_bridge.html][doc-10]

[doc-10]: http://symfony.com/doc/3.0/components/phpunit_bridge.html

#### You should know

* what features it has
* [how to mark tests as legacy][pb-1]
* that `SYMFONY_DEPRECATIONS_HELPER` is set to 0 by default
* that making `SYMFONY_DEPRECATIONS_HELPER` will make bridge to ignore any deprecation notices
* that using this is usefull for deprecated interfaces
* [how to work with time-sensitive tests][pb-2]
* [that ClockMock mocks PHP built-in `time(), microtime(), sleep() and usleep()`][pb-3]

[pb-1]: http://symfony.com/doc/3.0/components/phpunit_bridge.html#mark-tests-as-legacy
[pb-2]: http://symfony.com/doc/3.0/components/phpunit_bridge.html#time-sensitive-tests
[pb-3]: http://api.symfony.com/3.0/Symfony/Bridge/PhpUnit/ClockMock.html