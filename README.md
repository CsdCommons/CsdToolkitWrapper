CsdToolkitWrapper
=================

Introduction
------------

Zend Framework 2 module that wraps the "PHP Toolkit for IBMI i" library.  The goal
of the module is to make it easier to include and configure the ToolkitService 
for use in multiple projects.

Requirements
------------
  
Please see the [composer.json](composer.json) file.
Also, the "PHP Toolkit for IMBI i" library must be in the include path or 
explicitly included using `require_once()` or `include_once()`.  For more information
on installing the Toolkit, the readme [can be found here](https://github.com/zendtech/IbmiToolkit).

Installation
------------

Run the following `composer` command:

```console
$ composer require "csd-commons/csd-toolkitwrapper" : "~0.1"
```

Alternately, manually add the following to your `composer.json`, in the `require` section:

```javascript
"require": {
    "csd-commons/csd-toolkitwrapper" : "~0.1"
}
```

And then run `composer update` to ensure the module is installed.

Finally, add the module name to your project's `config/application.config.php` under the `modules`
key:

```php
return array(
    /* ... */
    'modules' => array(
        /* ... */
        'CsdToolkitWrapper',
    ),
    /* ... */
);
```

Configuration
=============

There are 3 .dist files in the config directory for you to modify.  One for
production, one for test, and one for development.  Copy the .dist file over to
your `config\autoload` directory and modify any of the settings in the file.

ToolkitService Options
----------------------

Modify the config option `csdToolkitWrapper_options` to set the default options
to use after the ToolkitService object has been instantiated.

Information for the following options 
[can be found here](http://files.zend.com/help/Zend-Server-6-IBMi/zend-server.htm#setting_the_xml_service_options.htm):

```php
'csdToolkitWrapper_options' => array(
    'arrayIntegrity' => true,
    'ccsidAfter' => null,
    'ccsidBefore' => null,
    'cdata' => true,
    'customControl' => '',
    'dataStructureIntegrity' => true,
    'debug' => true,
    'debugLogFile' => "toolkit_debug.log",
    'encoding' => 'ISO-8859-1',
    'httpTransportUrl' => null,
    'idleTimeout' => '',
    'internalKey' => "/tmp/MyInternalKey",
    'license' => false,
    'parseDebugLevel' => null,
    'parseOnly' => false,
    'paseCcsid' => null,
    'performance' => false,
    'plugSize' => '512K',
    'plugPrefix' => 'iPLUG',
    'prestart' => false,
    'sbmjobCommand' => null,
    'sbmjobParams' => 'ZENDSVR6/ZSVR_JOBD/XTOOLKIT',
    'schemaSep' => '.',
    'stateless' => false,
    'trace' => false,
    'transport' => false,
    'transportType' => 'ibm_db2',
    'useHex' => false,
    'v5r4' => false,
    'XMLServiceLib' => 'ZENDSVR6'
),
```

ToolkitService Connection Configuration
---------------------------------------

Modify the config option `csdToolkitWrapper_config` to set some default 
settings for the ToolkitService connecting to the IBMi.

Example:

```php
'csdToolkitWrapper_config' => array(
    'transport' => 'ibm_db2',
    'database' => 'DATABASE_NAME',
    'options' => array(
        'isPersistent' => false,
        'db2_i5_naming' => false,
    ),
    'platform_options' => array(
        'quote_identifiers' => false
    ),
    'curlib' => "SANE DEFAULT CURRENT LIBRARY",
    'liblist' => "SANE DEFAULT LIBRARY LIST",
),
```

ZF2 Events
==========

ZF2 Services
============

### Factories

### CsdToolkitWrapper\Factory\IbmToolkitServiceFactory

This factory creates the main IbmToolkit Service instance.  The factory pulls
together the configuration options and connection parameters used to instantiate
and configure the ToolkitService object.  The returned IbmToolkit object is used
to call CL commands and RPG programs.

### CsdToolkitWrapper\Factory\ParameterServiceFactory

This factory creates an instance of `Doctrine\Common\Annotations\AnnotationReader`
and passes it to the newly created instance of the `CsdToolkitWrapper\Service\ParameterService`.
The ParameterService class has two main methods for building parameter objects 
for use in program calls.

1. buildDataStructureParams($entity = null)
    - This method will attempt to use the AnnotationReader to build out a data
    structure based on the given Entity class.
    
2. getParam($type, $config = array())
    - This method will return a Parameter object of the given type configured with
    the given config array.

### Abstract Factories

### CsdToolkitWrapper\Factory\ParameterAbstractFactory

This factory attempts to return a parameter object of type
CsdToolkitWrapper\Parameter\<requested_param_type>.

Usage
=====


