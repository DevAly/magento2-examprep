---

# Module 4: EAV Model and Attribute Management

## Overview

### Objectives

- Understand the Entity-Attribute-Value (EAV) model.
- Learn how to create custom attributes.
- Manage attribute properties and configurations.
- Implement custom attribute input types.

---

## 4.1 Understanding the EAV Model

### Study

- **Entity-Attribute-Value (EAV) Model:**

  - Designed to handle entities with dynamic and extensible attributes.
  - Data is stored across multiple tables based on data type.
  - Commonly used for products, categories, customers.

- **EAV Structure:**

  - **Entity Table:** Contains entity identifiers.
  - **Attribute Tables:** Define attribute metadata.
  - **Value Tables:** Store attribute values (`_int`, `_varchar`, `_text`, etc.).

- **Advantages:**

  - Flexibility to add new attributes without altering the schema.
  - Supports multiple attribute types and complex data structures.

- **Disadvantages:**

  - Can impact performance due to complex queries.
  - Joins across multiple tables can be resource-intensive.

### Example

- **Product Attributes:**

  - Adding a new attribute like 'Brand' or 'Manufacturer' can be done without changing the core database schema.

---

## 4.2 Creating Custom Product Attributes

### Practical Exercise

**Task:** Add a custom product attribute programmatically using a Data Patch.

#### Steps:

1. **Create a Data Patch:**

   **File:** `app/code/Vendor/HelloWorld/Setup/Patch/Data/AddCustomProductAttribute.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Setup\Patch\Data;

   use Magento\Framework\Setup\ModuleDataSetupInterface;
   use Magento\Framework\Setup\Patch\DataPatchInterface;
   use Magento\Eav\Setup\EavSetupFactory;

   class AddCustomProductAttribute implements DataPatchInterface
   {
       private $moduleDataSetup;
       private $eavSetupFactory;

       public function __construct(
           ModuleDataSetupInterface $moduleDataSetup,
           EavSetupFactory $eavSetupFactory
       ) {
           $this->moduleDataSetup = $moduleDataSetup;
           $this->eavSetupFactory = $eavSetupFactory;
       }

       public function apply()
       {
           $eavSetup = $this->eavSetupFactory->create(['setup' => $this->moduleDataSetup]);

           $eavSetup->addAttribute(
               \Magento\Catalog\Model\Product::ENTITY,
               'custom_attribute_code',
               [
                   'type' => 'varchar',
                   'label' => 'Custom Attribute',
                   'input' => 'text',
                   'frontend' => '',
                   'backend' => '',
                   'required' => false,
                   'user_defined' => true,
                   'searchable' => true,
                   'filterable' => true,
                   'comparable' => true,
                   'visible_on_front' => true,
                   'used_in_product_listing' => true,
                   'unique' => false,
                   'apply_to' => '',
                   'global' => \Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface::SCOPE_GLOBAL,
               ]
           );
       }

       public static function getDependencies()
       {
           return [];
       }

       public function getAliases()
       {
           return [];
       }
   }
   ```

2. **Run the Data Patch:**

   ```bash
   bin/magento setup:upgrade
   ```

3. **Verify the Attribute:**

   - In the Magento Admin, navigate to **Stores** > **Attributes** > **Product**.
   - Search for 'Custom Attribute'.

4. **Assign the Attribute to an Attribute Set:**

   - Go to **Stores** > **Attributes** > **Attribute Set**.
   - Edit an attribute set (e.g., Default).
   - Drag and drop the new attribute into a group.

5. **Use the Attribute:**

   - Edit a product and set a value for the new attribute.
   - Save the product and verify that the attribute value is stored.

#### Outcome

- Successfully added a custom product attribute programmatically.
- Gained understanding of how EAV works in Magento.
- Learned how to manage attributes and attribute sets.

---

[Next Module >>](module5.md)