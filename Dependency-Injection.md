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
        

[sc-1]: http://symfony.com/doc/3.0/service_container.html#service-parameters
[sc-2]: http://symfony.com/doc/3.0/service_container.html#array-parameters

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

## Services autowiring <a id="services-autowiring"></a>

* <http://symfony.com/doc/3.0/components/dependency_injection/autowiring.html>