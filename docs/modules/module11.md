# Module 11: Security and Compliance

## Overview

### Objectives

- Understand security best practices in Magento.
- Learn how to manage sensitive configurations.
- Implement secure coding standards.
- Learn about user roles and access control.
- Understand how to apply patches and updates.

---

## 11.1 Implementing Security Best Practices

### Study

- **Data Sanitization and Validation:**

  - Always validate and sanitize user inputs.
  - Use Magento's built-in data validation methods.
  - Prevent SQL injection, XSS, and CSRF attacks.

- **Avoid Direct Database Access:**

  - Use Magento's ORM (Models and Resource Models).
  - Avoid writing raw SQL queries.

- **Secure Coding Standards:**

  - Follow Magento's coding standards.
  - Use proper exception handling.
  - Avoid exposing sensitive information in error messages.

- **Use Nonce and Form Keys:**

  - Utilize form keys to prevent CSRF attacks in forms.
  - Use `$this->formKey->getFormKey();` to generate a form key.

### Practical Exercise

**Task:** Secure a custom form with form keys to prevent CSRF attacks.

#### Steps:

1. **Inject FormKey in Controller:**

   **File:** `app/code/Vendor/HelloWorld/Controller/Index/Form.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Controller\Index;

   use Magento\Framework\App\Action\Action;
   use Magento\Framework\App\Action\Context;
   use Magento\Framework\Data\Form\FormKey\Validator;

   class Form extends Action
   {
       protected $formKeyValidator;

       public function __construct(
           Context $context,
           Validator $formKeyValidator
       ) {
           $this->formKeyValidator = $formKeyValidator;
           parent::__construct($context);
       }

       public function execute()
       {
           if (!$this->formKeyValidator->validate($this->getRequest())) {
               // Invalid form key
               $this->messageManager->addErrorMessage('Invalid Form Key');
               return $this->_redirect('*/*/index');
           }

           // Process form data
       }
   }
   ```

2. **Add Form Key to Form:**

   **File:** `app/code/Vendor/HelloWorld/view/frontend/templates/form.phtml`

   ```php
   <form action="<?php echo $block->getFormAction(); ?>" method="post">
       <?php echo $block->getFormKeyHtml(); ?>
       <!-- Form fields -->
       <button type="submit">Submit</button>
   </form>
   ```

3. **Test the Form:**

   - Submit the form with and without the form key.
   - Ensure that submissions without a valid form key are rejected.

#### Outcome

- Implemented security measures to protect against CSRF attacks.
- Gained understanding of secure coding practices in Magento.

---

## 11.2 Managing Sensitive Configurations

### Study

- **Storing Sensitive Data Securely:**

  - Use environment variables for sensitive data.
  - Do not hardcode API keys or passwords in code.

- **Using the `app/etc/env.php` File:**

  - Contains database credentials and other sensitive configurations.
  - Should not be tracked in version control (e.g., Git).

- **Configuration Management:**

  - Use `config.php` for system configuration that can be tracked.
  - Use `env.php` for environment-specific and sensitive settings.

### Practical Exercise

**Task:** Securely store and access a third-party API key.

#### Steps:

1. **Set Environment Variable:**

   - In Adobe Commerce Cloud:

     ```bash
     magento-cloud variable:set --level environment API_KEY your_api_key
     ```

   - In local development, set the environment variable in your server or IDE.

2. **Access the API Key in Code:**

   ```php
   $apiKey = getenv('API_KEY');
   ```

3. **Use the API Key:**

   - When making API requests, include the `$apiKey` securely.

4. **Ensure `env.php` Is Not in Version Control:**

   - Check `.gitignore` to confirm `app/etc/env.php` is ignored.

#### Outcome

- Learned how to manage sensitive configurations securely.
- Avoided exposing sensitive data in code repositories.

---

## 11.3 User Roles and Access Control

### Study

- **Admin User Roles and Permissions:**

  - Define custom user roles with specific permissions.
  - Use ACL to control access to resources.

- **Creating Custom Roles:**

  - Navigate to **System** > **Permissions** > **User Roles**.
  - Create a new role and assign relevant resources.

- **Assigning Roles to Users:**

  - Navigate to **System** > **Permissions** > **All Users**.
  - Assign the custom role to a user account.

### Practical Exercise

**Task:** Create a custom admin role for content editors.

#### Steps:

1. **Create a New Role:**

   - Go to **System** > **Permissions** > **User Roles**.
   - Click **Add New Role**.
   - **Role Information:**
     - **Role Name:** Content Editor
     - **Your Password:** [Enter your admin password]
   - **Role Resources:**
     - Select **Custom**.
     - Expand **Content**.
     - Check permissions for **Pages**, **Blocks**, **Media**.

2. **Create a New User:**

   - Go to **System** > **Permissions** > **All Users**.
   - Click **Add New User**.
   - Fill in user information.
   - Assign the **Content Editor** role.

3. **Test the User Account:**

   - Log in with the new user credentials.
   - Verify that only the permitted sections are accessible.

#### Outcome

- Successfully created a custom admin role with restricted permissions.
- Improved security by following the principle of least privilege.

---

[Next Module >>](module12.md)