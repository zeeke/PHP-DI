# PHP-DI - Simple PHP dependency injection library
by Matthieu Napoli

[![Build Status](https://secure.travis-ci.org/mnapoli/PHP-DI.png)](http://travis-ci.org/mnapoli/PHP-DI)

* Project home [http://github.com/mnapoli/PHP-DI](http://github.com/mnapoli/PHP-DI)

### Introduction

The aim of this library is to make [Dependency Injection]
(http://en.wikipedia.org/wiki/Dependency_injection)
as simple as possible with PHP.

No fancy features, but no overhead. The simpler the better.

#### Pros

* Annotations!
* As little configuration as possible
* Doesn't need getters/setters
* Doesn't need any change to your existing code (you can give it a shot easily)
* Fully compatible PHP 5.3/5.4

#### Cons

* You have to write a line of code in the constructor of your classes
(i'm looking for a solution about that)
* *Not* production ready for now

### Why?

Using the singleton design pattern may be practical at first, but it comes with several disadvantages,
the main one being that it's not testable.

By using dependency injection, you can develop using contracts and not care what implementation
will be used. As the dependency can be injected by the "user", your class can be tested with mocks.

### Basic example

Say you have this class:

    class Class2 {
    }

An instance of Class2 can be automatically injected in another class very simply:

    use DI\Annotations\Inject;

    class Class1 {
        /**
         * @Inject
         * @var Class2
         */
        private $class2;

        public function __construct() {
            \DI\DependencyManager::getInstance()->resolveDependencies($this);
        }
    }

### Using interfaces or abstract types?

If you have something like:

    class Class1 {
		/**
		 * @Inject
		 * @var MyInterface
		 */
		private $myProperty;

and:

    class MyInterface {
    }
	class TheImplementationToUse implements MyInterface {
	}

PHP-DI will fail to inject "myProperty" because the type is an interface (MyInterface).

You have to do the mapping between the interface (or abstract class) and the implementation to use.
This can be done with a configuration file (di.ini):

	; Type mapping for injection
	di.implementation.map["MyInterface"] = "TheImplementationToUse"

And in your code (Bootstrap for example):

	DependencyManager::getInstance()->setConfiguration('di.ini');

### How are instances created?

A factory is used to create the instances that are injected.

By default, the strategy used is the Singleton pattern, which means that only one
instance of each class is instantiated.

This can be configured to a different strategy, or even to use a different factory.

In the near future, more configurations (via annotations) will be available very easily.

### Requirements

* __PHP 5.3__ or higher
* Using an autoloading system is recommended (as in most of the major frameworks)

### Zend Framework

Are you using Zend Framework? Check out the official quickstart with
Dependency Injection already configured: [zf-quickstart-di](https://github.com/mnapoli/zf-quickstart-di)

### Contribute

To run the project, get [composer](http://getcomposer.org/doc/00-intro.md):

    $ curl -s http://getcomposer.org/installer | php
	$ php composer.phar install
