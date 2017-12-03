# Templating with Twig

#### Certification requirements

* [Auto escaping](#auto-escaping)
* [Template inheritance](#template-inheritance)

## Auto escaping <a id="auto-escaping"></a>

* <https://twig.symfony.com/doc/2.x/tags/autoescape.html>
* <http://symfony.com/doc/3.0/templating/escaping.html>

#### You should know

* auto escaping in Twig is on by default
* that the output escaping assumes that content is being escaped for HTML output
* to render normally output you have to use `raw` filter
		
		{{ validVariable|raw }}
* in PHP templates escaping is not automatic
* to escape output in PHP templates you have to use `escape()`

		Hello <?php echo $view->escape($name)
* second argument in `escape()` enables what context should be escaped

		var myMsg = 'Hello <?php echo $view->escape($name, 'js') ?>';
* [Escaper Extension adds automatic output escaping to Twig][ae-1]
* Escaper Extension defines tags autoescape, and a filter raw
* to disable escaping for a block

		{% autoescape false %}
		    Everything will be outputted as is in this block
		{% endautoescape %}
* default escaping strategy for `autoescape` is html
* to escape js

		{% autoescape 'js' %}
		    Everything will be automatically escaped in this block
		    using the js escaping strategy
		{% endautoescape %}
* to explicitly escape html

		{% autoescape 'html' %}
		    Everything will be automatically escaped in this block
		    using the HTML strategy
		{% endautoescape %}
* Twig is smart enough to not escape an already escaped value by the escape filter

		{% autoescape 'html' %}
		    {{ var }}
		    {{ var|raw }}      {# var won't be escaped #}
		    {{ var|escape }}   {# var won't be double-escaped #}
		{% endautoescape %}
* Twig does not escape static expressions:

		{% set hello = "<strong>Hello</strong>" %}
		{{ hello }}
		{{ "<strong>world</strong>" }}
* Functions returning template data (like macros and parent) always return safe markup

[ae-1]: https://twig.symfony.com/doc/2.x/api.html#escaper-extension

## Template inheritance <a id="template-inheritance"></a>

* <https://twig.symfony.com/doc/2.x/tags/extends.html>
* <https://twig.symfony.com/doc/2.x/templates.html#template-inheritance>
* <http://symfony.com/doc/3.0/templating.html#template-inheritance-and-layouts>
* <http://symfony.com/doc/3.0/templating/inheritance.html>

#### You should know

* [the common way is to use three-level approach - considered as best-practice used by vendor bundles so that the base template for a bundle can be easily overridden][ti-1]

		app/Resources/views/base.html.twig - main layout
		blog/layout.html.twig - section 
		blog/index.html.twig - individual templates
* `{% extends %}` must be the first tag in a template
* `{% block %}` the more tags you have in your base template, the better
* child templates don't have to define all blocks
* another option to reause duplicated code is by [include](http://symfony.com/doc/3.0/templating.html#including-templates)
* to get content of parent block you can use `{{ parent() }}`
* templates by default can live in two locations

		app/Resources/views/ - application wide base teampltes
		vendor/path/to/CoolBundle/Resources/views/ - third party bundle templates
* [you can reference templates in bundle `AcmeBlogBundle:Blog:index.html.twig`][ti-2]

		AcmeBlogBundle:Blog:index.html.twig - src/Acme/BlogBundle/Resources/views/Blog/index.html.twig
		AcmeBlogBundle::layout.html.twig - src/Acme/BlogBundle/Resources/views/layout.html.twig
* [in order to use the same block multiple times in one template you can use function `block('name')`][ti-3]
		
		{{ block('title') }}
		
* [block function can also be used to print content from another template][ti-6]

		{{ block("title", "common_blocks.twig") }}
* block function can also be checked if is defined in context

		{% if block("footer") is defined %}
		
		{% if block("footer", "common_blocks.twig") is defined %}
		
* [you can use block nesting][ti-4]

		Per default, blocks have access to variables from outer scopes
* you can use block shortcuts

		 	
		{% block title page_title|title %}
* [you can use dynaminc inheritance][ti-5]

		{% extends some_var %}
		
		
* you can use list of templates - the first template that exists will be used

		{% extends ['layout.html', 'base_layout.html'] %}
		
* you can use conditional inheritance

		{% extends standalone ? "minimum.html" : "base.html" %}

* [to skip single inheritance - you can use horizontal reuse `use`][ti-7]


		{% extends "base.html" %}
	
		{% use "blocks.html" %} {# use blocks from this file #}
		
		{% block title %}{% endblock %}
		{% block content %}{% endblock %}
* the use tag only imports a template  if it does not extend another template
* if two imported templates with `use` tag define the same block, the latest one wins
* you can import blocks with different names

		{% extends "base.html" %}

		{% use "blocks.html" with valid as base_valid, valid2 as base_valid2 %}

[ti-1]: http://symfony.com/doc/3.0/templating/inheritance.html
[ti-2]: http://symfony.com/doc/3.0/templating.html#template-inheritance-and-layouts
[ti-3]: https://twig.symfony.com/doc/2.x/tags/extends.html#child-template
[ti-4]: https://twig.symfony.com/doc/2.x/tags/extends.html#block-nesting-and-scope
[ti-5]: https://twig.symfony.com/doc/2.x/tags/extends.html#dynamic-inheritance
[ti-6]: https://twig.symfony.com/doc/2.x/functions/block.html#block
[ti-7]: https://twig.symfony.com/doc/2.x/tags/use.html