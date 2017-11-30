# Forms

#### Certification requirements

* [Forms creation](#forms-creation)
* [Forms handling](#forms-handling)
* [Form types](#form-types)
* [Forms rendering with Twig](#forms-rendering-with-twig)
* [Forms theming](#forms-theming)
* [CSRF protection](#csrf-protection)
* [Handling file upload](#handling-file-upload)
* [Built-in form types](#built-in-form-types)
* [Data transformers](#data-transformers)
* [Form events](#form-events)
* [Form type extensions](#form-type-extensions)

#### Extras

* Katas TODO

## Forms creation <a id="forms-creation"></a>

* <http://symfony.com/doc/3.0/forms.html#creating-a-simple-form>

#### You should know

* You can build form in controller using `createFormBuilder($task)->add('task', TextType::class)->getForm();`
* You can build from in controller using `createForm(TaskType::class, $task);`
* Form can be created using array `$this->createFormBuilder(['message' => 'Optional value'])->add('message, 'TextType::class')->getForm();`

		By default form assumes that you want to work with arrays instead of an objects

		If you don't:
		pass object to createFormBuilder or sencond argument createForm
		or declare data_class in form type
		
		Form will return array $form->getData()
		
* You can add constraints directly in form builder when you use array as object in form

## Forms handling <a id="forms-handling"></a>

* <http://symfony.com/doc/3.0/forms.html#handling-form-submissions>
* <http://symfony.com/doc/3.0/best_practices/forms.html#handling-form-submits>

#### You should know

* `createView()` should be called after handlerRequest is called. Because changes in *_SUBMIT will not be triggered like validation !
* `handleRequest()` recognizes if form was submitted and writes back data to form object/array

		After that object/array are validated
		
* `$form->isValid()` if validation was successful.

		It returns false is form was not submitted

* To control more about data that was submited use `submit()` function
* Second argument in `submit()` [clear missing](http://api.symfony.com/3.0/Symfony/Component/Form/FormInterface.html#method_submit) - whether to set fields to NULL when they are missing in the submitted data
* The recommended way is to use the same action for rendering form and handling form submission
* The recommended way is to use `$form->isSubmitted()` and `$form->isValid()` because flow is more readable otherwise it looks like it is always processed even for GET requests

## Form types <a id="form-types"></a>

* <http://symfony.com/doc/3.0/forms.html#creating-form-classes>
* <http://symfony.com/doc/3.0/forms.html#built-in-field-types>

## Forms rendering with Twig <a id="forms-rendering-with-twig"></a>

* <http://symfony.com/doc/3.0/form/rendering.html>

#### You should know

* Form can be rendered with just one line {{ form(form) }}
* `form_start()` will also include the correct enctype property if the form contains upload fields
* `form_end()` will also include csrf token
* `form_errors(form)` renders any errors to the whole form
* `form_row(form.dueDate)` renders label, error and html
* To access current data `form_row(form.dueDate)`
* `form_label(form.dueDate)` renders label
* `form_widget(form.dueDate)` renders html
* `form_errors(form.dueDate)` renders error
* To change label in HTML add second argument to form_label `{{ form_label(form.task, 'Task Description') }}`
* To get name attribute you need to use `{{ form.task.vars.full_name }}`

## Forms theming <a id="forms-theming"></a>

* <http://symfony.com/doc/3.0/form/form_customization.html#what-are-form-themes>
* <http://symfony.com/doc/3.0/form/form_themes.html>

#### You should know

* Symfony comes with built-in form themes:

		form_div_layout.html.twig, wraps each form field inside a <div> element.
		form_table_layout.html.twig, wraps the entire form inside a <table> element and each form field inside a <tr> element.
		bootstrap_3_layout.html.twig, wraps each form field inside a <div> element with the appropriate CSS classes to apply the default Bootstrap 3 CSS framework styles.
		bootstrap_3_horizontal_layout.html.twig, it's similar to the previous theme, but the CSS classes applied are the ones used to display the forms horizontally (i.e. the label and the widget in the same row).
		foundation_5_layout.html.twig, wraps each form field inside a <div> element with the appropriate CSS classes to apply the default Foundation CSS framework styles.
		
* `form_label()` for a checkbox/radio field doesn't show anything if using bootstrap form theme. It is included in `form_widget()`
* If you are rendering integer field twig will use `integer_widget` block from `form_div_layout.html.twig`
* Block `integer_widget ` uses `form_widget_simple` block

		{# form_div_layout.html.twig #}
		{% block integer_widget %}
		    {% set type = type|default('number') %}
		    {{ block('form_widget_simple') }}
		{% endblock integer_widget %}
* Form Attributes has it's own block `widget_attributes `
* In twig a theme is a single template file with all blocks and fragments
* In PHP a theme is folder with blocks and fragments as files
* How to customize blocks

		textarea -> textarea_widget
		integer -> integer_widget
		
		text -> text_errors
		
		It all refers to: widget, label, errors, row	
* Form Theming in Twig
	* Inside the same Template as the Form

			{% form_theme form _self %}
			
			This specific block theme cannot be reused

	* Inside a separate Template

			{% form_theme form 'form/fields.html.twig' %}

	* Multiple Templates

			{% form_theme form with ['common.html.twig', 'form/fields.html.twig'] %}
			
			it will use the first defined block from left to right
			
			or inside bundles
			
			{% form_theme form with ['common.html.twig', ' AcmeFormExtraBundle:form:fields.html.twig'] %}
			
	* Child Forms
	
			{% form_theme form 'form/fields.html.twig' %}

			{% form_theme form.child 'form/fields_child.html.twig' %}

* [How to use referencing blocks][ft-1]

		{% use 'form_div_layout.html.twig' with integer_widget as base_integer_widget %}

* How to use parent references

		{# app/Resources/views/Form/fields.html.twig #}
		{% extends 'form_div_layout.html.twig' %}
		
		{% block integer_widget %}
		    <div class="integer_widget">
		        {{ parent() }}
		    </div>
		{% endblock %}

* Be default twig uses div layout `form_div_layout.html.twig`, it can be changed

		# app/config/config.yml
		twig:
		    form_themes:
		        - 'form/fields.html.twig'
			        
* How to customize an individual Field
		
		{% block _product_name_widget %}
		    <div class="text_widget">
		        {{ block('form_widget_simple') }}
		    </div>
		{% endblock %}
		

		During building Form block_name can be defined
		
		$builder->add('name', TextType::class, array(
        'block_name' => 'custom_name',
	    ));
	    
    
* [How to customize a collection prototype][ft-2]

		{% form_theme form _self %}

		{% block _tasks_entry_widget %}
		    <tr>
		        <td>{{ form_widget(form.task) }}</td>
		        <td>{{ form_widget(form.dueDate) }}</td>
		    </tr>
		{% endblock %}

* [How to add a flag to fields][ft-3]

		{% use 'form_div_layout.html.twig' with form_label as base_form_label %}

		{% block form_label %}
		    {{ block('base_form_label') }}
		
		    {% if required %}
		        <span class="required" title="This field is required">*</span>
		    {% endif %}
		{% endblock %}
		
* You can add a variables in `form_widget`

		{{ form_widget(form.title, {'help': 'foobar'}) }}

[ft-1]: http://symfony.com/doc/3.0/form/form_customization.html#referencing-blocks-from-inside-the-same-template-as-the-form
[ft-2]: http://symfony.com/doc/3.0/form/form_customization.html#how-to-customize-a-collection-prototype
[ft-3]: http://symfony.com/doc/3.0/form/form_customization.html#adding-a-required-asterisk-to-field-labels

## CSRF protection <a id="csrf-protection"></a>

* <http://symfony.com/doc/3.0/form/csrf_protection.html>

#### You should know

* Hidden field is called `_token` by default
* Token is stored in session so if you render this field it will automatically starts the session
* Each CSRF token can be customized

		class TaskType extends AbstractType
		{
		    // ...
		
		    public function configureOptions(OptionsResolver $resolver)
		    {
		        $resolver->setDefaults(array(
		            'data_class'      => 'AppBundle\Entity\Task',
		            'csrf_protection' => true,
		            'csrf_field_name' => '_token',
		            // a unique key to help generate the secret token
		            'csrf_token_id'   => 'task_item',
		        ));
		    }
		
		    // ...
	}
	
* `csrf_token_id` is optional but adds extra security to the generated token
* CSRF tokens should be different for each user - do not cache them

	
## Handling file upload <a id="handling-file-upload"></a>

* <http://symfony.com/doc/3.0/controller/upload_file.html>
* <http://symfony.com/doc/3.0/reference/forms/types/file.html>

#### You should know

* In Symfony applications, uploaded files are objects of the UploadedFile class
* UploadedFile -> `getExtension()`, `getClientSize()`, `etClientOriginalName()` are considered not safe because a malicious user could tamper that information
* UploadedFile -> it's safer to use `guessExtension()` and generate unique file name

## Built-in form types <a id="built-in-form-types"></a>

* <http://symfony.com/doc/3.0/reference/forms/types.html>

## Data transformers <a id="data-transformers"></a>

* <http://symfony.com/doc/3.0/form/data_transformers.html>

#### You should know

* When a field has `inherit_data` option set DataTransformer cannot be applied.
* `CallbackTransformer` takes two callback functions (first original value info format)

		public function transform($issue)
		public function reverseTransform($issueNumber)
		
		You should throw TransformationFailedException - message will now be seen only invalid_message can write a message
	
* `addModelTransformer` method accepts any object that implements `DataTransformerInterface`

		Another way
		
		$builder->add(
		    $builder
		        ->create('tags', TextType::class)
		        ->addModelTransformer(...)
		);
		
* [different types of transformers][dt-1]
* As a general rule, the normalized data should contain as much information as possible

[dt-1]: http://symfony.com/doc/3.0/form/data_transformers.html#about-model-and-view-transformers

## Form events <a id="form-events"></a>

* <http://symfony.com/doc/3.0/form/events.html>
* <http://symfony.com/doc/3.0/form/dynamic_form_modification.html>

#### You should know

* On which step which event is triggered

		form.pre_set_data FormEvents::PRE_SET_DATA Model data -> Modify the data given during pre-population and adding or removing fields dynamically)
		form.post_set_data	FormEvents::POST_SET_DATA Model data -> This event is mostly here for reading data after having pre-populated the form.
		form.pre_bind	 FormEvents::PRE_SUBMIT Request data -> Change data from the request, before submitting the data to the form and add or remove form fields, before submitting the data to the form.
		form.bind	FormEvents::SUBMIT	Normalized data -> It can be used to change data from the normalized representation of the data.
		form.post_bind FormEvents::POST_SUBMIT View data -> It can be used to fetch data after denormalization.
		
* How to register an event listener

		->addEventListener(
	        FormEvents::PRE_SET_DATA,
	        function (FormEvent $event){...}
	    )
	    
	    or
	    
	    ->addEventSubscriber()
	    
	    class must implement EventSubscriberInterface
	    
* on `FormEvents::POST_SUBMIT` you cannot add or remove fields to the form
* on `FormEvents::POST_SUBMIT` `Symfony\Component\Form\Extension\Validator\EventListener\ValidationListener` automatically validate the denormalized object and to update the normalized representation as well as the view representations.
* to supress fully validation you need to stop propagation on `FormEvents::POST_SUBMIT` because even `validation_groups` is set to false there are still some integrity checks executed

		For example an uploaded file will still be checked to see if it is too large and the form will still check to see if non-existing fields were submitted
		
## Form type extensions <a id="form-type-extensions"></a>

* <http://symfony.com/doc/3.0/form/create_form_type_extension.html>

#### You should know

* The main 2 use-cases

		You want to add a specific feature to a single type (such as adding a "download" feature to the FileType field type);
		
		You want to add a generic feature to several types (such as adding a "help" text to every "input text"-like type).
		
* To create extension you have to extend `AbstractTypeExtension`

		This is the only method you must implement
		
		public function getExtendedType()
		
		it must retus FQCN of a form type
		
* You can have extra methods for your extension

		buildForm()
		buildView()
		configureOptions()
		finishView()
* A form type extension applying to TextType (i.e. whose getExtendedType method returns TextType::class) would apply to all of these form types
		
