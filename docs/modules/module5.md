---

# Module 5: Frontend Development and Theming

*(Due to the length of the content, I'll provide up to Module 5 here. Please let me know if you'd like me to continue with the remaining modules.)*

---

# Module 5: Frontend Development and Theming

## Overview

### Objectives

- Understand Magento 2 theme structure and hierarchy.
- Learn how to create a custom theme.
- Customize layouts, templates, and static files.
- Override parent theme styles using LESS.

---

## 5.1 Creating a Custom Theme

### Study

- **Magento Theme Structure:**

  - Themes are located in `app/design/frontend/<Vendor>/<theme>/`.
  - Key components:
    - `theme.xml`: Theme declaration.
    - `registration.php`: Registers the theme.
    - `etc/view.xml`: Configuration for images and other view-related settings.
    - `web/`: Contains static assets (CSS, JavaScript, images).

- **Theme Hierarchy and Inheritance:**

  - Child themes inherit from parent themes.
  - Allows for reusing and overriding existing styles and templates.

### Practical Exercise

**Task:** Create a custom theme that extends Magento Blank theme.

#### Steps:

1. **Create Theme Directory:**

   ```bash
   mkdir -p app/design/frontend/Vendor/custom_theme
   ```

2. **Create `theme.xml`:**

   **File:** `app/design/frontend/Vendor/custom_theme/theme.xml`

   ```xml
   <?xml version="1.0"?>
   <theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
     <title>Vendor Custom Theme</title>
     <parent>Magento/blank</parent>
   </theme>
   ```

3. **Create `registration.php`:**

   **File:** `app/design/frontend/Vendor/custom_theme/registration.php`

   ```php
   <?php
   use \Magento\Framework\Component\ComponentRegistrar;

   ComponentRegistrar::register(
       ComponentRegistrar::THEME,
       'frontend/Vendor/custom_theme',
       __DIR__
   );
   ```

4. **Apply the Theme:**

   - In the Magento Admin, navigate to **Content** > **Design** > **Configuration**.
   - Edit the default store view.
   - Set **Applied Theme** to 'Vendor Custom Theme'.
   - Flush the cache:

     ```bash
     bin/magento cache:flush
     ```

5. **Customize the Theme:**

   - **Override Templates:**

     - Copy template files from `vendor/magento/module-catalog/view/frontend/templates/` to `app/design/frontend/Vendor/custom_theme/Magento_Catalog/templates/`.

   - **Modify Layouts:**

     - Create layout override files in `app/design/frontend/Vendor/custom_theme/Magento_Catalog/layout/`.

   - **Add Static Files:**

     - Place CSS and JavaScript files in `app/design/frontend/Vendor/custom_theme/web/css/` and `web/js/` respectively.

#### Outcome

- Created and applied a custom theme.
- Understood theme inheritance and customization.

---

## 5.2 Overriding Parent Theme Styles with LESS

### Study

- **LESS in Magento:**

  - Magento uses LESS preprocessor for styles.
  - Allows variables, mixins, and nesting.

- **Overriding Styles:**

  - Use `_extend.less` or `_theme.less` to override parent styles.
  - Place these files in `web/css/source/` directory of your theme.

### Practical Exercise

**Task:** Change the background color of the header.

#### Steps:

1. **Create the Directory:**

   ```bash
   mkdir -p app/design/frontend/Vendor/custom_theme/web/css/source
   ```

2. **Create `_theme.less`:**

   **File:** `app/design/frontend/Vendor/custom_theme/web/css/source/_theme.less`

   ```less
   @primary__color: #FF6600;

   .page-header {
     background-color: @primary__color;
   }
   ```

3. **Deploy Static Content:**

   ```bash
   bin/magento setup:static-content:deploy
   ```

   - For developer mode, you may need to clear static files:

     ```bash
     rm -rf pub/static/frontend/*
     rm -rf var/view_preprocessed/*
     ```

4. **Verify Changes:**

   - Refresh the frontend and confirm the header background color has changed.

#### Outcome

- Successfully overridden parent theme styles using LESS.
- Gained understanding of Magento's frontend styling mechanisms.

---

[Next Module >>](module6.md)