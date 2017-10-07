# Miscellaneous

#### Certification requirements

* [Error handling](#error-handling)
* [Code debugging](#code-debugging)
* [Deployment best practices](#deployment-best-practices)
* [Process](#process)
* [Serializer](#serializer)
* [Data collectors](#data)
* [Web Profiler and Web Debug Toolbar](#profiler)
* [Internationalization and localization](#intl)

#### Extras

* [Katas](#katas)

## Error handling <a id="error-handling"></a>

#### Links

* [http://symfony.com/doc/3.0/controller/error_pages.html][doc-1]

[doc-1]: http://symfony.com/doc/3.0/controller/error_pages.html

#### You should know

* that in order to override exception pages from TwigBundle you have to put them into `app/Resources/TwigBundle/views/Exception/`
* that the exception pages shown in the development environment can also be customized `exception.html.twig` or `exception.json.twig`
* [that you can test error pages in dev environment (all that is included in Symfony Standard Edition)][eh-1]
* [how to override twig.exception_controller][eh-2]
* [what parameters are passed to default twig ExceptionController][eh-3]
* [how `kernel.exception` works][eh-4]
* that \Symfony\Component\Security\Http\Firewall\ExceptionListener handles various security-related exceptions (redirecting to login page, logging out ...)
* that kernel.exception throws GetResponseForExceptionEvent

[eh-1]: http://symfony.com/doc/3.0/controller/error_pages.html#testing-error-pages-during-development
[eh-2]: http://symfony.com/doc/3.0/controller/error_pages.html#overriding-the-default-exceptioncontroller
[eh-3]: http://api.symfony.com/3.0/Symfony/Bundle/TwigBundle/Controller/ExceptionController.html
[eh-4]: http://symfony.com/doc/3.0/controller/error_pages.html#working-with-the-kernel-exception-event

## Code debugging <a id="code-debugging"></a>

#### Links

* [http://symfony.com/doc/3.0/cookbook/debugging.html][doc-2]

[doc-2]: http://symfony.com/doc/3.0/cookbook/debugging.html

## Deployment best practices <a id="deployment-best-practices"></a>

#### Links

* [http://symfony.com/doc/3.0/deployment.html][doc-3]

[doc-3]: http://symfony.com/doc/3.0/deployment.html

#### You should know

* [common Post-Deployment Tasks][dp-1]
* [how to optimze autoloader][dp-2]
* [how to use composer classmap][dp-3]
* that default, the Symfony Standard Edition uses Composer's autoloader in the autoload.php file

[dp-1]: http://symfony.com/doc/3.0/deployment.html#common-post-deployment-tasks
[dp-2]: http://symfony.com/doc/3.0/deployment.html#c-install-update-your-vendors
[dp-3]: https://symfony.com/doc/current/performance.html#use-composer-s-class-map-functionality

## Process <a id="process"></a>

#### Links

* [http://symfony.com/doc/3.0/components/process.html][doc-4]

[doc-4]: http://symfony.com/doc/3.0/components/process.html

## Serializer <a id="serializer"></a>

#### Links

* [http://symfony.com/doc/3.0/serializer.html][doc-5]
* [http://symfony.com/doc/3.0/components/serializer.html][doc-6]

[doc-5]: http://symfony.com/doc/3.0/serializer.html
[doc-6]: http://symfony.com/doc/3.0/components/serializer.html

## Data collectors <a id="data"></a>

#### Links

* http://symfony.com/doc/3.0/profiler/data_collector.html

## Web Profiler and Web Debug Toolbar <a id="profiler"></a>

#### Links

* http://symfony.com/doc/3.0/reference/configuration/web_profiler.html

## Internationalization and localization <a id="intl"></a>

#### Links

* http://symfony.com/doc/3.0/translation.html
* http://symfony.com/doc/3.0/best_practices/i18n.html

## Katas <a id="katas"></a>

* Override all error pages and exceptions and test them
* Play with serializer (create custom normalizers, add groups)
* Play with pluralization and translations in twig
	* Change translations domain
	* Change directory where are located translations
	* Debug missing translations
* Create custom data collector
	* Discover symfony data collectors  	

