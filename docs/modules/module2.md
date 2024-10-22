---

# Module 2: Magento Module Development

## Overview

### Objectives

- Learn how to create custom modules in Magento 2.
- Understand module declaration and configuration.
- Learn about Composer and module dependencies.
- Get familiar with Dependency Injection (DI) and the Object Manager.

---

## 2.1 Creating a Custom Module

### Study

- **Magento Module Structure:**

  - Each module resides in `app/code/<Vendor>/<Module>/`.
  - Key components:
    - `registration.php`: Registers the module.
    - `etc/module.xml`: Declares the module and its version.
    - `composer.json`: Module dependencies and autoloading.

- **Module Registration:**

  - **registration.php**:

    ```php
    <?php
    use \Magento\Framework\Component\ComponentRegistrar;

    ComponentRegistrar::register(
        ComponentRegistrar::MODULE,
        'Vendor_Module',
        __DIR__
    );
    ```

- **Module Declaration:**

  - **module.xml**:

    ```xml
    <?xml version="1.0"?>
    <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
        <module name="Vendor_Module" setup_version="1.0.0">
            <sequence>
                <module name="Magento_Store"/>
            </sequence>
        </module>
    </config>
    ```

- **Composer Configuration:**

  - **composer.json**:

    ```json
    {
      "name": "vendor/module",
      "description": "Custom Magento 2 module",
      "require": {
        "php": "^7.4||^8.1",
        "magento/framework": "*"
      },
      "type": "magento2-module",
      "version": "1.0.0",
      "license": [
        "OSL-3.0",
        "AFL-3.0"
      ],
      "autoload": {
        "files": [
          "registration.php"
        ],
        "psr-4": {
          "Vendor\\Module\\": ""
        }
      }
    }
    ```

### Practical Exercise

**Task:** Create a custom module named `Vendor_HelloWorld`.

#### Steps:

1. **Create Module Directory:**

   ```bash
   mkdir -p app/code/Vendor/HelloWorld
   ```

2. **Create `registration.php`:**

   **File:** `app/code/Vendor/HelloWorld/registration.php`

   ```php
   <?php
   use \Magento\Framework\Component\ComponentRegistrar;

   ComponentRegistrar::register(
       ComponentRegistrar::MODULE,
       'Vendor_HelloWorld',
       __DIR__
   );
   ```

3. **Create `module.xml`:**

   **File:** `app/code/Vendor/HelloWorld/etc/module.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
       <module name="Vendor_HelloWorld" setup_version="1.0.0">
           <sequence>
               <module name="Magento_Store"/>
           </sequence>
       </module>
   </config>
   ```

4. **Create `composer.json`:**

   **File:** `app/code/Vendor/HelloWorld/composer.json`

   ```json
   {
     "name": "vendor/helloworld",
     "description": "Hello World module for Magento 2",
     "require": {
       "php": "^7.4||^8.1",
       "magento/framework": "*"
     },
     "type": "magento2-module",
     "version": "1.0.0",
     "license": [
       "OSL-3.0",
       "AFL-3.0"
     ],
     "autoload": {
       "files": [
         "registration.php"
       ],
       "psr-4": {
         "Vendor\\HelloWorld\\": ""
       }
     }
   }
   ```

5. **Enable Module and Upgrade:**

   ```bash
   bin/magento module:enable Vendor_HelloWorld
   bin/magento setup:upgrade
   ```

6. **Verify Module Status:**

   ```bash
   bin/magento module:status
   ```

   - Ensure `Vendor_HelloWorld` is enabled.

#### Outcome

- A basic custom module is created and registered with Magento.
- Understanding of module structure and registration.

---

## 2.2 Dependency Injection and Avoiding the Object Manager

### Study

- **Dependency Injection (DI):**

  - A design pattern where a class requests dependencies from external sources rather than creating them.
  - Promotes loose coupling and easier testing.
  - In Magento, dependencies are injected via constructor.

- **Object Manager:**

  - Responsible for instantiating objects in Magento.
  - Direct use is discouraged outside of core framework code.
  - Best practice is to use DI and let the Object Manager resolve dependencies.

- **Defining Dependencies:**

  - Declare dependencies in the constructor of your class.
  - Type-hint the class or interface you need.
  - Magento's DI container will automatically inject the required object.

### Practical Exercise

**Task:** Create a simple model that demonstrates dependency injection.

#### Steps:

1. **Create a Model Class:**

   **File:** `app/code/Vendor/HelloWorld/Model/Greeting.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Model;

   class Greeting
   {
       public function getMessage()
       {
           return 'Hello, World!';
       }
   }
   ```

2. **Create a Controller That Uses the Model:**

   **File:** `app/code/Vendor/HelloWorld/Controller/Index/Index.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Controller\Index;

   use Magento\Framework\App\Action\Action;
   use Magento\Framework\App\Action\Context;
   use Vendor\HelloWorld\Model\Greeting;

   class Index extends Action
   {
       protected $greeting;

       public function __construct(
           Context $context,
           Greeting $greeting
       ) {
           $this->greeting = $greeting;
           parent::__construct($context);
       }

       public function execute()
       {
           echo $this->greeting->getMessage();
       }
   }
   ```

3. **Define Frontend Route:**

   **File:** `app/code/Vendor/HelloWorld/etc/frontend/routes.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
       <router id="standard">
           <route id="helloworld" frontName="helloworld">
               <module name="Vendor_HelloWorld"/>
           </route>
       </router>
   </config>
   ```

4. **Access the Controller:**

   - Visit `http://magento.local/helloworld/index/index` in your browser.
   - You should see "Hello, World!" displayed.

#### Outcome

- Demonstrated how to use dependency injection in Magento.
- Avoided direct use of the Object Manager.
- Improved understanding of best practices in module development.

---

[Next Module >>](module3.md)