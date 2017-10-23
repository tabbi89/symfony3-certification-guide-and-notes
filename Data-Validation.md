# Data Validation

#### Certification requirements

* [PHP object validation](#php-object-validation)
* [Built-in validation constraints](#built-in)
* [Validation scopes](#validation-scopes)
* [Validation groups](#validation-groups)
* [Group sequence](#group-sequance)
* [Custom callback validators](#custom-callback)
* [Violations builder](#violations-builder)

#### Extras

* Katas TODO

## PHP object validation <a id="php-object-validation"></a>

* <http://symfony.com/doc/3.0/validation.html#the-basics-of-validation>

#### You should know

* Validation component is based on JSR303 Bean Validation specification
* validation rules can by applied with YAML, XML, annotations, or PHP
* protected and private properties can also be validated, as well as "getter" methods
* during validation `ConstraintViolationList` is returned
		
		ConstraintViolationList->_toString() displays:
		
		AppBundle\Author.name:
    		This value should not be blank
* Symfony Standard Edition has enabled Validation by default but you have to enable annotation

		# app/config/config.yml
		framework:
		    validation: { enable_annotations: true }
		    
* each validation error is represented by `ConstraintViolation`    
* validator uses Constraints to validate input data

		Basic constraints
		String constraints
		Number constraints
		Comparison constraints
		Date constraints
		Collection constraints
		File constraints
		Financial and other Number constraints
		Other Constraints
* constraints can be applied to properties, public getters or entire class
		
		getters validations is applied to the return value of a method
		public methods: get is has -> getters
* class constraints: Callback is a generic constraint that can be applied to the whole class
* [how to create custom validation constraint][dv-1]

		To enable constraint as annotation you have to add @Annotation annotation in the constraint
		to define validator with dependencies you have to use tag: validator.constraint_validator

		to define constraint to whole class:
		
		public function getTargets()
		{
		    return self::CLASS_CONSTRAINT;
		}


[dv-1]: http://symfony.com/doc/3.0/validation/custom_constraint.html

## Built-in validation constraints <a id="built-in"></a>

* <http://symfony.com/doc/3.0/reference/constraints.html>

## Validation scopes <a id="validation-scopes"></a>

* <http://symfony.com/doc/3.0/validation/custom_constraint.html#class-constraint-validator>

## Validation groups <a id="validation-groups"></a>

* <http://symfony.com/doc/3.0/validation/groups.html>

		There are two basic validation groups
		
		Default and Object (ex. User)
		ex. User but it only applies to object name
		Default applies to all referenced classes that belong to no other group
		
## Group sequence <a id="group-sequance"></a>

* <http://symfony.com/doc/3.0/validation/sequence_provider.html>

#### You should know

* when you define Default group sequance you will make circural reference use class name group

		this is because Default now reference the group sequence
		
		
* for complex group sequances you can use group sequance provider a class should implement `GroupSequenceProviderInterface`

		public function getGroupSequence()
	    {
	        $groups = array('User');
	
	        if ($this->isPremium()) {
	            $groups[] = 'Premium';
	        }
	
	        return $groups;
	    }
	    
	    
	   	 next you have to enable this
	   	 
	   	 # src/AppBundle/Resources/config/validation.yml
		 AppBundle\Entity\User:
		    group_sequence_provider: true
		    
		 or by using annotation:
		 
		 /**
		 * @Assert\GroupSequenceProvider
		 */
		class User implements GroupSequenceProviderInterface
		{}
		 
## Custom callback validators <a id="custom-callback"></a>

* <http://symfony.com/doc/3.0/validation/custom_constraint.html>
* <http://symfony.com/doc/3.0/reference/constraints/Callback.html>
* <https://knpuniversity.com/screencast/question-answer-day/custom-validation-property-path>

## Violations builder <a id="violations-builder"></a>

* <http://symfony.com/doc/3.0/validation/custom_constraint.html#creating-the-validator-itself>