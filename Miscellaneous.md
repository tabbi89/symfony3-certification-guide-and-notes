# Miscellaneous

#### Certification requirements

* [Error handling](#error-handling)
* [Code debugging](#code-debugging)
* [Deployment best practices](#deployment-best-practices)

#### Extras

* Katas (TODO)

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

* [http://symfony.com/doc/3.0/deployment.html][doc-3]

[doc-3]: http://symfony.com/doc/3.0/deployment.html