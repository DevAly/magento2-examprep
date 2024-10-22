---

# Module 3: Working with Database and Models

## Overview

### Objectives

- Understand Magento's database structure.
- Learn how to create models, resource models, and collections.
- Work with the declarative schema to define database tables.
- Perform CRUD (Create, Read, Update, Delete) operations using models.

---

## 3.1 Understanding the Database Structure

### Study

- **Magento's Database Architecture:**

  - Combines Entity-Attribute-Value (EAV) and flat table structures.
    - **EAV Model:**
      - Flexible schema for entities like products and categories.
      - Attributes stored in separate tables based on data type.
    - **Flat Tables:**
      - Denormalized for performance optimization.
      - Used for orders, invoices, etc.

- **Key Database Components:**

  - **Models:** Represent data entities.
  - **Resource Models:** Handle database interactions (CRUD operations).
  - **Collections:** Retrieve multiple records.

### Example

- **EAV Tables for Products:**

  - `catalog_product_entity`
  - `catalog_product_entity_varchar`
  - `catalog_product_entity_int`
  - `catalog_product_entity_text`
  - etc.

---

## 3.2 Creating Models and Resource Models

### Practical Exercise

**Task:** Create a custom entity with its own database table using the declarative schema.

#### Steps:

1. **Define the Database Table Using Declarative Schema:**

   **File:** `app/code/Vendor/HelloWorld/etc/db_schema.xml`

   ```xml
   <?xml version="1.0"?>
   <schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
     <table name="vendor_helloworld_item" resource="default" engine="innodb" comment="HelloWorld Items">
       <column xsi:type="int" name="item_id" nullable="false" identity="true" unsigned="true" comment="Item ID"/>
       <column xsi:type="varchar" name="title" nullable="false" length="255" comment="Title"/>
       <column xsi:type="text" name="content" nullable="false" comment="Content"/>
       <column xsi:type="timestamp" name="created_at" nullable="false" default="CURRENT_TIMESTAMP" on_update="false" comment="Creation Time"/>
       <constraint xsi:type="primary" referenceId="PRIMARY">
         <column name="item_id"/>
       </constraint>
     </table>
   </schema>
   ```

2. **Create the Model Class:**

   **File:** `app/code/Vendor/HelloWorld/Model/Item.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Model;

   use Magento\Framework\Model\AbstractModel;

   class Item extends AbstractModel
   {
       protected function _construct()
       {
           $this->_init(\Vendor\HelloWorld\Model\ResourceModel\Item::class);
       }
   }
   ```

3. **Create the Resource Model Class:**

   **File:** `app/code/Vendor/HelloWorld/Model/ResourceModel/Item.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Model\ResourceModel;

   use Magento\Framework\Model\ResourceModel\Db\AbstractDb;

   class Item extends AbstractDb
   {
       protected function _construct()
       {
           $this->_init('vendor_helloworld_item', 'item_id');
       }
   }
   ```

4. **Create the Collection Class:**

   **File:** `app/code/Vendor/HelloWorld/Model/ResourceModel/Item/Collection.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Model\ResourceModel\Item;

   use Magento\Framework\Model\ResourceModel\Db\Collection\AbstractCollection;

   class Collection extends AbstractCollection
   {
       protected function _construct()
       {
           $this->_init(
               \Vendor\HelloWorld\Model\Item::class,
               \Vendor\HelloWorld\Model\ResourceModel\Item::class
           );
       }
   }
   ```

5. **Run the Setup Upgrade:**

   ```bash
   bin/magento setup:upgrade
   ```

6. **Test the Model:**

   - Create a simple controller or script to perform CRUD operations.

   **Example Controller:** `app/code/Vendor/HelloWorld/Controller/Item/Test.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Controller\Item;

   use Magento\Framework\App\Action\Action;
   use Magento\Framework\App\Action\Context;
   use Vendor\HelloWorld\Model\ItemFactory;

   class Test extends Action
   {
       protected $itemFactory;

       public function __construct(
           Context $context,
           ItemFactory $itemFactory
       ) {
           $this->itemFactory = $itemFactory;
           parent::__construct($context);
       }

       public function execute()
       {
           // Create a new item
           $item = $this->itemFactory->create();
           $item->setTitle('Test Item');
           $item->setContent('This is a test content.');
           $item->save();

           // Load the item
           $loadedItem = $this->itemFactory->create()->load($item->getId());
           echo 'Loaded Item Title: ' . $loadedItem->getTitle();

           // Update the item
           $loadedItem->setTitle('Updated Test Item');
           $loadedItem->save();

           // Delete the item
           $loadedItem->delete();

           echo 'Item operations completed.';
       }
   }
   ```

7. **Define the Route for the Controller:**

   - Update `routes.xml` and create appropriate layout and templates if needed.

#### Outcome

- Created a custom database table using the declarative schema.
- Implemented model, resource model, and collection classes.
- Performed CRUD operations using the model.

---

[Next Module >>](module4.md)