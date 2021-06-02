---
layout: post
title:  "How to create a phpunit feature test in laravel 8"
tags: laravel phpunit feature-test php testing developer-testing
categories: laravel php
---

# How to create a phpunit feature test in laravel 8

**Phpunit** is a unit testing framework created by Sebastian Bergmann. Laravel has built-in feature for testing using phpunit.

After installing a laravel application you can see a folder named **tests**. In that **tests** folder you can see two folders named **Feature** and **Unit**. **Feature** folder will contain tests used to test certain features like creating a user in the admin dashboard.
Your majority of tests will be written in **Feature** folder as feature tests.

# How to run phpunit tests

For running phpunit either **vendor/bin/phpunit** or  **php artisan test** can be used. I prefer the php artisan test use to run tests since it creates a beautiful response, but vendor/bin/phpunit method is suggested by many.

# Before running phpunit test

Create a **.env.testing** file in the root directory and add .env.testing to your **.gitignore** file. The config values stored in this file will be used while phpunit tests instead of .env. Set APP_URL=http://localhost in the .env.testing file.

# Creating a new unit test

```

	php artisan make:test AdminRegistrationTest

```

Run this in terminal in the application root directory. A file named AdminRegistrationTest will be created in the directory **tests/Feature**.


```

	<?php

	namespace Tests\Feature;

	use App\Models\User;
	use Illuminate\Foundation\Testing\RefreshDatabase;
	use Tests\TestCase;

	class AdminRegistrationTest extends TestCase
	{
	    use RefreshDatabase;

	    /**
	     * Test admin user create form
	     *
	     * @return void
	     */
	    public function test_admin_user_create()
	    {
	        $this->seed();

	        $user = User::find(1);

	        $response = $this->actingAs($user)
	            ->post('/admin/create', [
	                'name' => 'Deffrin Joseph',
	                'email' => 'deffrin@gmail.com',
	                'password' => 'password',
	                'password_confirmation' => 'password'
	            ]);

	        $response->assertRedirect('/admin/list');
	    }
	}

```
	The above test will run

	1, database seeders
	
	```
		$this->seed();
	```

	2, find the user having id equal to 1
	```
		$user = User::find(1);
	```

	3, create a session for that user
	```
		$this->actingAs($user) 
	```

	4, submit a post request to the /admin/create url. name, email, password, password_confirmation must be the names of the form fields

	```
		->post('/admin/create', [
	                'name' => 'Deffrin Joseph',
	                'email' => 'deffrin@gmail.com',
	                'password' => 'password',
	                'password_confirmation' => 'password'
	            ])
	```

	5, assert after creating admin whether the page redirects to /admin/list page or not

	```
		$response->assertRedirect('/admin/list');
	```



After creating tests as mentioned earlier run **php artisan test** all your tests will run then.

Creating unit tests while developing application is a best practice the developers need to follow. For everyone who uses Laravel to build web applications can utilize phpunit and more stable applications and make maintaining the application much simple.