# Form Generator
This is a auto form generator for Laravel 4. This package allows you to auto generate a form from a model.

This forked version adds orderBy as an option to manually order your form fields.

- [Form Generator on Packagist](https://packagist.org/packages/robin-malfait/formgenerator)
- [Form Generator on GitHub](https://github.com/RobinMalfait/Laravel-auto-form-generator)

## Installation
To add this form generator to your Laravel application follow this steps:

Add the following to your `composer.json` file:

	"robin-malfait/formgenerator": "dev-master"

Then run `composer update` or `composer install` if you have not already installed packages.

Add below to the `providers` array in `app/config/app.php` configuration file (add at the end):

	'RobinMalfait\Formgenerator\FormgeneratorServiceProvider'

Add `'Formgenerator' => 'RobinMalfait\Formgenerator\Facades\Formgenerator',` to the `aliases` array also in `app/config/app.php`

## What's new
* You can now flag all fields with 'form-control' for Bootstrap 3 users.
```php
	'extras'	=> array(
		'*'		=> array('class' => 'form-control')
	)
```

* You may now use hidden fields and set values to those hidden elements. The label associated will be hidden.
```php
	'customers_id'	=> array(
		'type'		=>	'hidden',
		'value'		=>	2
	)
```

* The ability to add custom labels in the `extras` array:

	```'label' => 'Supercalifragilisticexpialidocious'```

## How to use it
Let's make a form now, you can either pass an object like `$user` OR you can pass `table_name` as a string instead of the $model variable like so:

```php
{{ Form::open() }}
	{{ Formgenerator::generate('table_name_here') }}
{{ Form::close() }}
```


With a $model object
```php
{{ Form::model($user) }}
	{{ Formgenerator::generate($user) }}
{{ Form::close() }}
```

As a second param you can pass an options array for example:
```php
{{ Form::model($user) }}
	{{ Formgenerator::generate($user, array(

		// If you want a specific type, put it in here, default is type from the database
		'types' => array(
			// Field Name 	=> Type
			'all_day' 		=> 'checkbox',
			
			// Support for hidden fields (auto-hides associated label) and setting values!
			'customers_id'			=> array(
				'type'		=>	'hidden',
				'value'		=>	2
			),

			// If you want a select field with options
			'first_name'	=> array(
				'type'		=> 'select',
				'options'	=> array(
					'Taftse' 	=> 'Taftse',
					'Robin'		=> 'Robin',
					'Jeffrey'	=> 'Jeffrey'
				),
			),
		),
		
        // If you want to manually order different from the columns order in the database/model 
        'orderBy'           =>array(
            "last_name",
            "first_name",
        ),

		// Add a class to a field
		'extras' => array(
			// Field Name   => array('key' => 'value'),
			'first_name' 			=> array(
				'class' 			=> 'span5',
				'content_before'	=> '<fieldset><legend>My Form</legend>'
			),
			'last_name'				=> array(
				'class' 			=> 'span5',
				'content_before'	=> '<br>'
			),
			'activated' 	=> array(
				'class' 		=> '',
				'content_after'	=> '</fieldset>'

				// Set a custom label if you want
				'label' => 'Supercalifragilisticexpialidocious'
			),


			// Wildcards, those will be added to every field except for the fields that are listed above.

			// If you specify the *form-control* class, all fields will have form-control applied their class list.
			// This is great for Bootstrap 3 users, but keep in mind, the above functionality will break due to
			// all fields being given a class.

			'*'				=> array(
				'class' 	=> 'span5 form-control'
			)
		),

		// Submit? Yes or no? Set the text and set a class if you want
		'submit' => array(
			'show' 	=> true,
			'text'  => 'Save',
			'class' => 'btn btn-success btn-large'
		),

		// Fields to not display!
		'exclude' => array(
			'event_type', 'id', 'created_at', 'updated_at', 'for_user_id'
		),

		// Show labels
		'showLabels' => true,

	)) }}
{{ Form::close() }}
```