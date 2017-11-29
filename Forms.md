# Forms

#### Certification requirements

* [Forms creation](#forms-creation)
* [Forms handling](#forms-handling)
* [Form types](#form-types)
* [Forms rendering with Twig](#forms-rendering-with-twig)
* [Forms theming](#forms-theming)

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