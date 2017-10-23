# Dependency Injection

#### Certification requirements

* [Service container](#service-container)
* [Built-in services](#built-in-services)
* [Configuration parameters](#configuration-parameters)
* [Services registration](#services-registration)
* [Factories](#factories)
* [Compiler passes](#compiler-passes)
* [Services autowiring](#services-autowiring)

#### Extras

* Katas TODO

## Service container <a id="service-container"></a>

* <http://symfony.com/doc/3.0/service_container.html>

#### You should know

* that creating/configuring Services in the Container is done via XMl, PHP or YAML
* that symfony loads and builds configuration via `AppKernel::registerContainerConfiguration()`

		by default: app/config/config.yml
* be default all services are public in Symfony Standard Edition

		in 	Symfony Standard Edition 3.3 autowire was added and services are by default not public
		see more https://github.com/symfony/symfony-standard/blob/3.3/app/config/services.yml
* [to use `%` or `@` as a string they must be escaped][sc-1]

		'@@securepass' or '%%bar%%'
* [that you can use array as parameters][sc-2]
* [that you can use setter injection as well][sc-3]

		demo:
        class:     AppBundle\Demo
        calls:
            - [setMailer, ['@app.other_service']]
* that the Symfony service container also supports property injection
* [that there are two ways to alias service][sc-3]
* [that you can add deprecated message to services][sc-4]
		
		you should define custom message it is a recommendation
		the deprecation message is optional if not used it will use a default one
		
* [there is a service configurator][sc-5]

       configurators can be valid PHP classes or PHP collable
       when creating an instance with 'configurator' it will first call 'configure()' with that instance
       
* you can use `php bin/console debug:container` to debug container

		  The debug:container command displays all configured public services:

		    php bin/console debug:container
		
		  To get specific information about a service, specify its name:
		
		    php bin/console debug:container validator
		
		  By default, private services are hidden. You can display all services by
		  using the --show-private flag:
		
		    php bin/console debug:container --show-private
		
		  Use the --tags option to display tagged public services grouped by tag:
		
		    php bin/console debug:container --tags
		
		  Find all services with a specific tag by specifying the tag name with the --tag option:
		
		    php bin/console debug:container --tag=form.type
		
		  Use the --parameters option to display all parameters:
		
		    php bin/console debug:container --parameters
		
		  Display a specific parameter by specifying its name with the --parameter option:
		
		    php bin/console debug:container --parameter=kernel.debug
		    
* [how to work with service definitions][sc-6]

		In general service definitions are instructions how container should build a service
		
		you should not use get() to get a service that you want to inject as constructor argument, the service is not yet availabe, use Reference()
		
		you can change service definitions only before compiling the container
		
* [how to inject values based on complex expressions][sc-7]		
		you have access to 2 functions:
			service
			parameter
			
		and container variable (ContainerBuilder)
		
		This comples expressions can be used in:
		arguments, properties, as arguments with configurator and as arguments to calls
		
* [how to user factories][sc-8]

		class option has no effect on the resulting service when using factories only the result of factory is important
		
		factory: ['AppBundle\Email\NewsletterManager', create]
		factory: ['@app.newsletter_manager_factory', createNewsletterManager]
		
		but this could be used by compiler passes for some processing
		
* [how to use imports in dependency injection][sc-9]

		you cannot use parameters to build paths in imports dynamically
		
		this is a not working example:
		imports:
    		- { resource: '%kernel.root_dir%/parameters.yml' }

* that third-party bundles configuration is load using dependency injection extension

		extension looks for keyword in configuration and loads a proper extension
		
* [all injection types][sc-10]

		contructor
		setter
		
			calls:
			 	- [setMailer, ['@mailer']]
			 	
	 	properties
	 	
	 		properties:
             mailer: '@mailer'
 
 * [how to make services lazy][sc-11]

 		Symfony Standard Edition does not include package ocramius/proxy-manager container will just skip lazy flag
 		
 		To check if service is lazy dump(class_implements($service));
 		// "ProxyManager\Proxy\LazyLoadingInterface"

* [how to make service arguments/references optional][sc-12]
		
		Setting missing dependencies to null is possible in XML (on-invalid="null") and PHP
		
		Ignoring missing dependencies is possible in YAML PHP and XML (on-invalid="ignore")
		it will just skip call if service does not exist
		
		In general if service does not exist during compilation in the result file appDevDebugProjectContainer.php there will be no checking if service exist
		and if call will be removed completely. In construction injection we will have just null injected.
		
* [that the values between parameter tags in XML configuration files are not trimmed][sc-13]
		
		<parameter key="mailer.transport">
		    sendmail
		</parameter>
		
		will result:
		\n    sendmail\n
		
* that you can only set a parameter before the container is compiled
* parameters can contain arrays (in XML type="collection" must be set)
* that in XML and PHP constants are available in parameters
 
		In order to use constants you can import xml file from yaml
		
		imports:
			- { resource: file.xml }

		In symfony 3.3 constants are available in yaml see http://symfony.com/doc/current/service_container/parameters.html#constants-as-parameters
		
* in XML true, false and null are converted to proper PHP values

		by adding  type="string"
		
		<parameter key="mailer.some_parameter" type="string">
		
* [how to create parent services][sc-14]

		the shared abstract and tags attributes are not inherited from parent services
		
		you can override parent arguments using index_N notation all overrride all using full arguments notation
		
* [how to get request from container][sc-15]		
		inject request_stack and call getCurrentRequest() to get Request object
		
		for controllers as services just inject Request into action method as parameter
		
* [how to decorate services][sc-16]

		the decorator should be declared as private, as you will not need to retrieve it
		
		you can change 'decoration_inner_name'
		
		it there are more decorated services you can use 'decoration_priority' (higher will be first)
		
* [how to define non-shared services][sc-17]
* [how to inject instances in container instead of letting container to build them for you][sc-18]

		kernel is a synthetic service
		
		To make service synthetic
		synthetic: true
		and then use container->set();
		
* [how container is compiled][sc-19]

		compiling is done by Compiler Passes for example:
		
		- checking various issues with definitions
		- validation
		- optimization before caching (private and abstract services are removed)

[sc-1]: http://symfony.com/doc/3.0/service_container.html#service-parameters
[sc-2]: http://symfony.com/doc/3.0/service_container.html#array-parameters
[sc-3]: http://symfony.com/doc/3.0/service_container/alias_private.html#aliasing
[sc-4]: http://symfony.com/doc/3.0/service_container/alias_private.html#deprecating-services
[sc-5]: http://symfony.com/doc/3.0/service_container/configurators.html
[sc-6]: http://symfony.com/doc/3.0/service_container/definitions.html
[sc-7]: http://symfony.com/doc/3.0/service_container/expression_language.html
[sc-8]: http://symfony.com/doc/3.0/service_container/factories.html
[sc-9]: http://symfony.com/doc/3.0/service_container/import.html
[sc-10]: http://symfony.com/doc/3.0/service_container/injection_types.html
[sc-12]: http://symfony.com/doc/3.0/service_container/optional_dependencies.html
[sc-13]: http://symfony.com/doc/3.0/service_container/parameters.html
[sc-14]: http://symfony.com/doc/3.0/service_container/parent_services.html
[sc-15]: http://symfony.com/doc/3.0/service_container/request.html
[sc-16]: http://symfony.com/doc/3.0/service_container/service_decoration.html
[sc-17]: http://symfony.com/doc/3.0/service_container/shared.html
[sc-18]: http://symfony.com/doc/3.0/service_container/synthetic_services.html
[sc-19]: https://symfony.com/doc/current/components/dependency_injection/compilation.html


## Built-in services <a id="built-in-services">

* <http://symfony.com/doc/3.0/service_container.html#debugging-services>

#### You should know

* [if a private service is only used as an argument to just one other service, it won't be displayed by the debug:container --show-private][b-1]

		They are converted from services to inlined instantiations - new PrivateExample(). This increases the container's performance.

[b-1]: http://symfony.com/doc/3.0/service_container/alias_private.html#inlined-private-services

## Configuration parameters <a id="configuration-parameters"></a>

* <http://symfony.com/doc/3.0/service_container/parameters.html>
* <http://symfony.com/doc/3.0/components/dependency_injection.html#setting-up-the-container-with-configuration-files>

#### You should know

* that the values between parameter tags in XML configuration files are not trimmed

		<parameter key="mailer.transport">
		    sendmail
		</parameter>
		
		\n    sendmail\n
* that to escape `%` you have to use `%%`
* you can only set a parameter before the container is compiled
* that you can use array as parameters
* [that you can use contansts as parameters for XML and PHP format (from 3.3 YAML also is supported)][cp-1]	
		<?xml version="1.0" encoding="UTF-8" ?>
		<container xmlns="http://symfony.com/schema/dic/services"
		    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		    xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">
		
		    <parameters>
		        <parameter key="global.constant.value" type="constant">GLOBAL_CONSTANT</parameter>
		        <parameter key="my_class.constant.value" type="constant">My_Class::CONSTANT_NAME</parameter>
		    </parameters>
		</container>

* that in Symfony 3 you can import xml from yaml to use constants

		imports:
    		- { resource: parameters.xml }  

* that by default `true, false and null` in XML are converted to the PHP keywords
* [that there are 3 types of loaders: `XmlFileLoader, YamlFileLoader, PhpFileLoader ` for parameters and service configuration][cp-2]	
[cp-1]: http://symfony.com/doc/3.0/service_container/parameters.html#constants-as-parameters
[cp-2]: http://symfony.com/doc/3.0/components/dependency_injection.html#setting-up-the-container-with-configuration-files

## Services registration <a id="services-registration"></a>

* <http://symfony.com/doc/3.0/service_container.html#creating-configuring-services-in-the-container>

## Tags <a id="tags"></a>

* <http://symfony.com/doc/3.0/service_container/tags.html>
* <http://symfony.com/doc/3.0/reference/dic_tags.html>

## Semantic configuration <a id="semantic-configuration"></a>

* <http://symfony.com/doc/3.0/bundles/extension.html>

#### You should know

* the Extension class should implement the `ExtensionInterface`, but usually you would simply extend the `Extension`
* it has to live in the DependencyInjection namespace of the bundle,
* the name must fallow convenction `AcmeHelloBundle` would be called `AcmeHelloExtension`
* [that you can manually register extension `Bundle::getContainerExtension()`][sco-1]
* that the load() method doesn't get the actual container instance, but a copy
* that IniFileLoader can only be used to load parameters and it can only load them as strings
* for non shared bundles it does not make sense to add advanced configuration
* `$this->processConfiguration($configuration, $configs);` is used to validate, normalize and merge all the configuration arrays together
* `config:dump-reference` command dumps the default configuration of a bundle in the console 
* [how to define configuration tree][sco-2]

[sco-1]: http://symfony.com/doc/3.0/bundles/extension.html#manually-registering-an-extension-class
[sco-2]: http://symfony.com/doc/3.0/components/config/definition.html

## Factories <a id="factories"></a>

* <http://symfony.com/doc/3.0/service_container/factories.html>

## Compiler passes <a id="compiler-passes"></a>

* <http://symfony.com/doc/3.0/service_container/compiler_passes.html>

#### You should know

* separate compiler passes, you need to register them in the `build()` -> `$container->addCompilerPass`
* [compiler pass can be defined in extension by implementing `CompilerPassInterface`][cpp-1]
* that process() is called after all extensions are loaded
* that the process() method in the extension class is called during the optimization step
* work with services definition in a compiler pass and do not create service instances
* [how to controll pass ordering][cpp-2]

[cpp-1]: http://symfony.com/doc/3.0/components/dependency_injection/compilation.html#execute-code-during-compilation
[cpp-2]: http://symfony.com/doc/3.0/components/dependency_injection/compilation.html#controlling-the-pass-ordering

## Services autowiring <a id="services-autowiring"></a>

* <http://symfony.com/doc/3.0/components/dependency_injection/autowiring.html>