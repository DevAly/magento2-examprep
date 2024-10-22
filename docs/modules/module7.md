# Module 7: Routing and Controllers

## Overview

### Objectives

- Understand Magento's routing mechanism.
- Learn how to create frontend and admin controllers.
- Implement custom URL routes.
- Forward and redirect requests within controllers.

---

## 7.1 Understanding Routing in Magento

### Study

- **Routing Configuration:**

  - Defined in `routes.xml`.
  - Maps incoming URLs to modules and controllers.

- **Route Structure:**

  - **Area:** `frontend` or `adminhtml`.
  - **Router ID:** Identifies the router (`standard`, `admin`).
  - **Route ID and Front Name:**

    ```xml
    <route id="helloworld" frontName="helloworld">
      <module name="Vendor_HelloWorld"/>
    </route>
    ```

### Practical Exercise

**Task:** Create a custom frontend route and controller.

#### Steps:

1. **Define the Route:**

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

2. **Create the Controller Action:**

   **File:** `app/code/Vendor/HelloWorld/Controller/Index/Forward.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Controller\Index;

   use Magento\Framework\App\Action\Action;
   use Magento\Framework\App\Action\Context;

   class Forward extends Action
   {
       public function execute()
       {
           $this->_forward('index');
       }
   }
   ```

3. **Access the Controller:**

   - Visit `http://magento.local/helloworld/index/forward`.
   - It should forward to `index` action.

#### Outcome

- Learned how to configure routes and create controllers.
- Implemented request forwarding within a controller.

---

## 7.2 Creating Admin Controllers and ACL

### Study

- **Admin Routing:**

  - Defined in `etc/adminhtml/routes.xml`.
  - Uses a different router ID (`admin`).

- **Access Control Lists (ACL):**

  - Defined in `etc/acl.xml`.
  - Controls access to admin controllers and resources.

### Practical Exercise

**Task:** Create an admin controller with proper ACL configuration.

#### Steps:

1. **Define the Admin Route:**

   **File:** `app/code/Vendor/HelloWorld/etc/adminhtml/routes.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
     <router id="admin">
       <route id="helloworld" frontName="helloworld">
         <module name="Vendor_HelloWorld"/>
       </route>
     </router>
   </config>
   ```

2. **Create the Admin Controller:**

   **File:** `app/code/Vendor/HelloWorld/Controller/Adminhtml/Index/Index.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Controller\Adminhtml\Index;

   use Magento\Backend\App\Action;
   use Magento\Backend\App\Action\Context;
   use Magento\Framework\View\Result\PageFactory;

   class Index extends Action
   {
       const ADMIN_RESOURCE = 'Vendor_HelloWorld::helloworld';

       protected $resultPageFactory;

       public function __construct(
           Context $context,
           PageFactory $resultPageFactory
       ) {
           $this->resultPageFactory = $resultPageFactory;
           parent::__construct($context);
       }

       public function execute()
       {
           return $this->resultPageFactory->create();
       }
   }
   ```

3. **Define ACL Resources:**

   **File:** `app/code/Vendor/HelloWorld/etc/acl.xml`

   ```xml
   <?xml version="1.0"?>
   <acl xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Acl/etc/acl.xsd">
     <resources>
       <resource id="Magento_Backend::admin">
         <resource id="Vendor_HelloWorld::helloworld" title="HelloWorld Module" />
       </resource>
     </resources>
   </acl>
   ```

4. **Add Menu Item:**

   **File:** `app/code/Vendor/HelloWorld/etc/adminhtml/menu.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:framework:Menu/etc/menu.xsd">
     <menu>
       <add id="Vendor_HelloWorld::helloworld" title="HelloWorld" module="Vendor_HelloWorld"
            sortOrder="100" parent="Magento_Backend::content" action="helloworld/index/index" resource="Vendor_HelloWorld::helloworld"/>
     </menu>
   </config>
   ```

5. **Access the Admin Page:**

   - Log in to the admin panel.
   - Navigate to **Content** > **HelloWorld**.

#### Outcome

- Learned how to create admin controllers with ACL.
- Secured admin routes and controlled access permissions.

---

[Next Module >>](module8.md)