# Console

#### Certification requirements

* [Built-in commands](#built-in-commands)
* [Custom commands](#custom-commands)
* [Configuration](#configuration)
* [Options and arguments](#options-and-arguments)
* [Input and Output objects](#input-and-output)
* [Built-in helpers](#built-in-helpers)
* [Console events](#console-events)
* [Verbosity levels](#verbosity-levels)

#### Extras

* [Katas](#katas)


## Build-in commands <a id="built-in-commands"></a>

#### Links

[http://symfony.com/doc/3.0/components/console/usage.html#built-in-commands][doc-1]

[doc-1]: http://symfony.com/doc/3.0/components/console/usage.html#built-in-commands

Symfony Standard Edition console commands (output of `php bin/console`):

	Usage:
	  command [options] [arguments]
	
	Options:
	  -h, --help            Display this help message
	  -q, --quiet           Do not output any message
	  -V, --version         Display this application version
	      --ansi            Force ANSI output
	      --no-ansi         Disable ANSI output
	  -n, --no-interaction  Do not ask any interactive question
	  -e, --env=ENV         The environment name [default: "dev"]
	      --no-debug        Switches off debug mode
	  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

	Available commands:
	  about                                   Displays information about the current project
	  help                                    Displays help for a command
	  list                                    Lists commands
	 assets
	  assets:install                          Installs bundles web assets under a public directory
	 cache
	  cache:clear                             Clears the cache
	  cache:pool:clear                        Clears cache pools
	  cache:warmup                            Warms up an empty cache
	 config
	  config:dump-reference                   Dumps the default configuration for an extension
	 debug
	  debug:config                            Dumps the current configuration for an extension
	  debug:container                         Displays current services for an application
	  debug:event-dispatcher                  Displays configured listeners for an application
	  debug:router                            Displays current routes for an application
	  debug:swiftmailer                       [swiftmailer:debug] Displays current mailers for an application
	  debug:translation                       Displays translation messages information
	  debug:twig                              Shows a list of twig functions, filters, globals and tests
	 doctrine
	  doctrine:cache:clear-collection-region  Clear a second-level cache collection region.
	  doctrine:cache:clear-entity-region      Clear a second-level cache entity region.
	  doctrine:cache:clear-metadata           Clears all metadata cache for an entity manager
	  doctrine:cache:clear-query              Clears all query cache for an entity manager
	  doctrine:cache:clear-query-region       Clear a second-level cache query region.
	  doctrine:cache:clear-result             Clears result cache for an entity manager
	  doctrine:database:create                Creates the configured database
	  doctrine:database:drop                  Drops the configured database
	  doctrine:database:import                Import SQL file(s) directly to Database.
	  doctrine:ensure-production-settings     Verify that Doctrine is properly configured for a production environment.
	  doctrine:generate:crud                  [generate:doctrine:crud] Generates a CRUD based on a Doctrine entity
	  doctrine:generate:entities              [generate:doctrine:entities] Generates entity classes and method stubs from your mapping information
	  doctrine:generate:entity                [generate:doctrine:entity] Generates a new Doctrine entity inside a bundle
	  doctrine:generate:form                  [generate:doctrine:form] Generates a form type class based on a Doctrine entity
	  doctrine:mapping:convert                [orm:convert:mapping] Convert mapping information between supported formats.
	  doctrine:mapping:import                 Imports mapping information from an existing database
	  doctrine:mapping:info
	  doctrine:query:dql                      Executes arbitrary DQL directly from the command line.
	  doctrine:query:sql                      Executes arbitrary SQL directly from the command line.
	  doctrine:schema:create                  Executes (or dumps) the SQL needed to generate the database schema
	  doctrine:schema:drop                    Executes (or dumps) the SQL needed to drop the current database schema
	  doctrine:schema:update                  Executes (or dumps) the SQL needed to update the database schema to match the current mapping metadata.
	  doctrine:schema:validate                Validate the mapping files.
	 generate
	  generate:bundle                         Generates a bundle
	  generate:command                        Generates a console command
	  generate:controller                     Generates a controller
	 lint
	  lint:twig                               Lints a template and outputs encountered errors
	  lint:xliff                              Lints a XLIFF file and outputs encountered errors
	  lint:yaml                               Lints a file and outputs encountered errors
	 router
	  router:match                            Helps debug routes by simulating a path info match
	 security
	  security:check                          Checks security issues in your project dependencies
	  security:encode-password                Encodes a password.
	 server
	  server:log                              Starts a log server that displays logs in real time
	  server:run                              Runs a local web server
	  server:start                            Starts a local web server in the background
	  server:status                           Outputs the status of the local web server for the given address
	  server:stop                             Stops the local web server that was started with the server:start command
	 swiftmailer
	  swiftmailer:email:send                  Send simple email message
	  swiftmailer:spool:send                  Sends emails from the spool
	 translation
	  translation:update                      Updates the translation file

#### You should know

* [that in console component there are built-in list and help commands][com-1]
* [how shortcut syntax works][com-2]

[com-1]: http://symfony.com/doc/3.0/components/console/usage.html#built-in-commands
[com-2]: http://symfony.com/doc/3.0/components/console/usage.html#shortcut-syntax

## Custom commands <a id="custom-commands"></a>

#### Links

[http://symfony.com/doc/3.0/console.html][doc-2]

[doc-2]: http://symfony.com/doc/3.0/console.html

#### Youd should know

* [how command lifecycle works][cc-1]
* that there is only one method required execute()
* [how to test single command using CommandTester][cc-2]
* [how to test whole console application using ApplicationTester][cc-3]
* [that setCode(collable $code) overrides the code defined in the execute() method][cc-4]
* [that input validation is executed after initialize and interact (if input isInteractive)][cc-5]
* [that execute returns null or 0 if everything went fine, or an error code][cc-6]
 
[cc-1]: http://symfony.com/doc/3.0/console.html#command-lifecycle
[cc-2]: http://symfony.com/doc/3.0/console.html#testing-commands
[cc-3]: http://api.symfony.com/3.0/Symfony/Component/Console/Tester/ApplicationTester.html
[cc-4]: http://api.symfony.com/3.1/Symfony/Component/Console/Command/Command.html#method_setCode
[cc-5]: https://github.com/symfony/console/blob/3.0/Command/Command.php#L251
[cc-6]: http://api.symfony.com/3.1/Symfony/Component/Console/Command/Command.html#method_run

## Configuration <a id="configuration"></a>

#### Links

[http://symfony.com/doc/3.0/console.html#configuring-the-command][doc-2]

[doc-3]: http://symfony.com/doc/3.0/console.html#configuring-the-command

## Options and arguments <a id="options-and-arguments"></a>

#### Links

[http://symfony.com/doc/3.0/console/input.html#using-command-arguments][doc-3]
[http://symfony.com/doc/3.0/components/console/console_arguments.html][doc-4]

[doc-3]: http://symfony.com/doc/3.0/console/input.html#using-command-arguments
[doc-4]: http://symfony.com/doc/3.0/components/console/console_arguments.html

#### Youd should know

* [that to add argument you have to call `addArgument` method][oa-2]
* [that to configure all at once you can call setDefinition(array|InputDefinition $definition)][oa-1]
* that there are 3 types of InputArgument
* that you can combine InputArgument for an argument
* that InputArgument::IS_ARRAY should be used last because it can container limitless number of arguments
* [that you can use options in console commands][oa-3]
* [that to add InputOption you can call `addOption` method][oa-4]
* that options are always optional
* that for InputOption::VALUE_OPTIONAL you cannot distinguish if command was used without value or without option in both cases you will get null value
* [that Symfony Console application follows docopt standards][oa-5]
* [how console arguments and options are handled][oa-6]


[oa-1]: http://api.symfony.com/3.1/Symfony/Component/Console/Command/Command.html#method_setDefinition
[oa-2]: http://api.symfony.com/3.1/Symfony/Component/Console/Command/Command.html#method_addArgument
[oa-3]: http://symfony.com/doc/3.0/console/input.html#using-command-options
[oa-4]: http://api.symfony.com/3.1/Symfony/Component/Console/Command/Command.html#method_addOption
[oa-5]: http://docopt.org/
[oa-6]: http://symfony.com/doc/3.0/components/console/console_arguments.html

## Input and Output objects <a id="input-and-output"></a>

#### Links

* [http://symfony.com/doc/3.0/console.html#console-input][doc-5]
* [http://symfony.com/doc/3.0/console.html#executing-the-command][doc-6]

[doc-5]: http://symfony.com/doc/3.0/console.html#console-input
[doc-6]: http://symfony.com/doc/3.0/console.html#executing-the-command

#### You should know

* [what are the parameters for `write` or `writeln`][io-1]

[io-1]: http://api.symfony.com/3.0/Symfony/Component/Console/Output/OutputInterface.html#method_writeln

## Built-in helpers <a id="built-in-helpers"></a>

#### Links

* [http://symfony.com/doc/3.0/components/console/helpers/index.html][doc-7]

[doc-7]: http://symfony.com/doc/3.0/components/console/helpers/index.html

Symfony 3 current available helpers:

	Formatter Helper
	Process Helper
	Progress Bar
	Question Helper
	Table
	Debug Formatter Helper

## Console events <a id="console-events"></a>

#### Links

* [http://symfony.com/doc/3.0/components/console/events.html][doc-8]

[doc-8]: http://symfony.com/doc/3.0/components/console/events.html

#### You should know

* [purpose of ConsoleEvents::COMMAND -> ConsoleCommandEvent][ce-1]
* [how to disable command in listener and return code 113][ce-2]
* that is is possible to enable command in next listener after it was disabled
* [purpose of ConsoleEvents::EXCEPTION -> ConsoleExceptionEvent][ce-3]
* [purpose of ConsoleEvents::TERMINATE -> ConsoleTerminateEvent][ce-4]
* [that ConsoleEvents::TERMINATE is dispatched when an exception is thrown by the command]

[ce-1]: http://symfony.com/doc/3.0/components/console/events.html#the-consoleevents-command-event
[ce-2]: http://symfony.com/doc/3.0/components/console/events.html#disable-commands-inside-listeners
[ce-3]: http://symfony.com/doc/3.0/components/console/events.html#the-consoleevents-exception-event
[ce-4]: http://symfony.com/doc/3.0/components/console/events.html#the-consoleevents-terminate-event

## Verbosity levels <a id="verbosity-levels"></a>

#### Links

* [http://symfony.com/doc/3.0/console/verbosity.html][doc-9]

[doc-9]: http://symfony.com/doc/3.0/console/verbosity.html

#### You should know

* [what are options for verbosity levels][vl-1]
* how to disable any output
* how to check verbosity levels inside command
* [how to add verbosity levels to `writeln()` or `write`][vl-2]
* [what are the verbosity semtantic methods for output][vl-3]
* that full exception stacktrace is displayed for >= OutputInterface::VERBOSITY_VERBOSE

[vl-1]: http://symfony.com/doc/3.0/console/verbosity.html
[vl-2]: http://api.symfony.com/3.0/Symfony/Component/Console/Output/OutputInterface.html#method_writeln
[vl-3]: http://api.symfony.com/3.0/Symfony/Component/Console/Output/Output.html#method_isQuiet

## Katas <a id="katas"></a>

* Create custom command in Symfony Standard Edition
	1. Discover how they are automatically autoloaded in Bundle
	2. Register 3 listeners for all console events and play with them
	3. Add container to command
	4. Use all verbosity levels in command
	5. Add arguments and use short syntax for them
	6. Add options and play with all of them
	7. Add `--` to separate options and arguments 
