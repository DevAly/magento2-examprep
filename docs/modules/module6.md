# Module 6: JavaScript Customizations

## Overview

### Objectives

- Understand Magento's JavaScript framework and RequireJS.
- Learn how to create and customize JavaScript modules.
- Implement custom widgets and extend existing ones.
- Learn how to translate strings in JavaScript.

---

## 6.1 Working with RequireJS and JavaScript Modules

### Study

- **Magento's Use of RequireJS:**

  - Magento 2 uses RequireJS for asynchronous JavaScript module loading.
  - Organizes JavaScript code into modules for better maintainability.
  - Defines dependencies explicitly.

- **Defining JavaScript Modules:**

  - Modules are defined using `define` function.
  - Syntax:

    ```javascript
    define(['dependency1', 'dependency2'], function (dep1, dep2) {
      // Module code
      return function () {
        // Return a function or object
      };
    });
    ```

- **Requiring Modules:**

  - Use `require` to load modules:

    ```javascript
    require(['module_name'], function (module) {
      // Use the module
    });
    ```

- **Configuration with `requirejs-config.js`:**

  - Located in `view/frontend/requirejs-config.js`.
  - Used to map module aliases and paths.

### Practical Exercise

**Task:** Create a custom JavaScript module and load it on a specific page.

#### Steps:

1. **Create the JavaScript Module:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/web/js/custom-module.js`

   ```javascript
   define(['jquery'], function ($) {
     'use strict';
     return function (config, element) {
       $(element).text('Hello from Custom Module!');
     };
   });
   ```

2. **Update `requirejs-config.js`:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/requirejs-config.js`

   ```javascript
   var config = {
     map: {
       '*': {
         customModule: 'Vendor_HelloWorld/js/custom-module'
       }
     }
   };
   ```

3. **Add the Module to a Layout:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/layout/helloworld_index_index.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
     <body>
       <referenceContainer name="content">
         <block class="Magento\Framework\View\Element\Template" name="custom_block" template="Vendor_HelloWorld::custom.phtml">
           <arguments>
             <argument name="jsLayout" xsi:type="array">
               <item name="components" xsi:type="array">
                 <item name="customComponent" xsi:type="array">
                   <item name="component" xsi:type="string">Vendor_HelloWorld/js/custom-module</item>
                 </item>
               </item>
             </argument>
           </arguments>
         </block>
       </referenceContainer>
     </body>
   </page>
   ```

4. **Create the Template File:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/templates/custom.phtml`

   ```html
   <div data-bind="scope: 'customComponent'">
     <!-- ko template: getTemplate() --><!-- /ko -->
   </div>
   ```

5. **Verify on Frontend:**

   - Navigate to the `helloworld` page.
   - The text "Hello from Custom Module!" should display.

#### Outcome

- Learned how to create and configure custom JavaScript modules.
- Implemented RequireJS configuration and module loading.

---

## 6.2 Extending and Overriding JavaScript Components

### Study

- **Magento UI Components:**

  - JavaScript components used for dynamic UI elements.
  - Configured via JSON configurations in layouts or templates.

- **Extending Components:**

  - Use mixins to extend existing components.
  - Configure mixins in `requirejs-config.js`.

- **Creating Mixins:**

  - Introduce new behavior without modifying the original module.

### Practical Exercise

**Task:** Extend the Magento validation component to add a custom validation rule.

#### Steps:

1. **Create the Mixin Module:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/web/js/validation-mixin.js`

   ```javascript
   define(['jquery'], function ($) {
     'use strict';

     return function (originalValidation) {
       $.validator.addMethod(
         'custom-rule',
         function (value, element) {
           return value === 'custom';
         },
         $.mage.__('Please enter "custom"')
       );
       return originalValidation;
     };
   });
   ```

2. **Update `requirejs-config.js`:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/requirejs-config.js`

   ```javascript
   var config = {
     config: {
       mixins: {
         'mage/validation': {
           'Vendor_HelloWorld/js/validation-mixin': true
         }
       }
     }
   };
   ```

3. **Use the Custom Rule in a Form:**

   - Add `data-validate="{required:true, 'custom-rule':true}"` to an input field in a template.

4. **Test the Validation:**

   - Try submitting the form with different input values to see the validation in action.

#### Outcome

- Learned how to extend JavaScript components using mixins.
- Implemented a custom validation rule.

---

## 6.3 Translating Strings in JavaScript

### Study

- **Magento's Translation Mechanism:**

  - Uses `mage/translate` library.
  - Translations are defined in CSV files under `i18n` directory.

- **Translating Strings:**

  - Use `$.mage.__('Your string here')` in JavaScript code.

### Practical Exercise

**Task:** Implement translation for strings in a JavaScript module.

#### Steps:

1. **Modify the JavaScript Module:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/web/js/custom-module.js`

   ```javascript
   define(['jquery', 'mage/translate'], function ($, $t) {
     'use strict';
     return function (config, element) {
       $(element).text($t('Hello from Custom Module!'));
     };
   });
   ```

2. **Create Translation File:**

   **File:** `app/code/Vendor/HelloWorld/i18n/en_US.csv`

   ```csv
   "Hello from Custom Module!","Hello from Custom Module!"
   ```

   **File:** `app/code/Vendor/HelloWorld/i18n/fr_FR.csv`

   ```csv
   "Hello from Custom Module!","Bonjour du Module Personnalisé!"
   ```

3. **Test the Translation:**

   - Switch the store view to French.
   - Verify that the text changes accordingly.

#### Outcome

- Learned how to implement translations in JavaScript.
- Understood the use of `mage/translate` for localization.

---

[Next Module >>](module7.md)