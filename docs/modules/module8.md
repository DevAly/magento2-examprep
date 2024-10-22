# Module 8: Plugins and Dependency Injection

## Overview

### Objectives

- Understand how to use plugins (interceptors) to modify behavior.
- Learn about Dependency Injection (DI) in Magento.
- Configure di.xml for object preferences and plugins.
- Best practices for using plugins and dependency injection.

---

## 8.1 Using Plugins to Extend Functionality

### Study

- **What Are Plugins?**

  - Also known as interceptors.
  - Allows modifying the behavior of public methods in classes.
  - Types of plugin methods:
    - `before`: Executes before the original method.
    - `after`: Executes after the original method.
    - `around`: Wraps around the original method.

- **When to Use Plugins vs. Observers:**

  - Use plugins to modify specific methods.
  - Use observers to respond to events.

### Practical Exercise

**Task:** Create a plugin to modify the `getName()` method of the `Product` model.

#### Steps:

1. **Create the Plugin Class:**

   **File:** `app/code/Vendor/HelloWorld/Plugin/ProductPlugin.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Plugin;

   class ProductPlugin
   {
       public function afterGetName(\Magento\Catalog\Model\Product $subject, $result)
       {
           return $result . ' - Customized';
       }
   }
   ```

2. **Configure the Plugin in di.xml:**

   **File:** `app/code/Vendor/HelloWorld/etc/di.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
     <type name="Magento\Catalog\Model\Product">
       <plugin name="product_name_plugin" type="Vendor\HelloWorld\Plugin\ProductPlugin"/>
     </type>
   </config>
   ```

3. **Test the Plugin:**

   - View a product page.
   - The product name should display with ' - Customized' appended.

#### Outcome

- Learned how to create and configure plugins.
- Modified the behavior of a core class method.

---

## 8.2 Understanding Dependency Injection

### Study

- **Dependency Injection (DI):**

  - Design pattern for supplying dependencies to classes.
  - Promotes loose coupling and testability.
  - In Magento, implemented via constructor injection.

- **Object Manager:**

  - Responsible for instantiating classes and resolving dependencies.
  - Should not be used directly in custom code.

- **Defining Preferences:**

  - Use `di.xml` to set a preference (replacement) for a class or interface.

### Practical Exercise

**Task:** Override a core class by setting a preference.

#### Steps:

1. **Create the Class:**

   **File:** `app/code/Vendor/HelloWorld/Model/CustomProduct.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Model;

   class CustomProduct extends \Magento\Catalog\Model\Product
   {
       public function getName()
       {
           return parent::getName() . ' - From Custom Product';
       }
   }
   ```

2. **Set the Preference:**

   **File:** `app/code/Vendor/HelloWorld/etc/di.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
     < preference for="Magento\Catalog\Model\Product" type="Vendor\HelloWorld\Model\CustomProduct"/>
   </config>
   ```

3. **Test the Change:**

   - Refresh the frontend product page.
   - The product name should reflect the change.

#### Outcome

- Learned how to use dependency injection to override classes.
- Understood the importance of DI in Magento's architecture.

---

[Next Module >>](module9.md)