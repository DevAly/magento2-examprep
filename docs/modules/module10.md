# Module 10: Cloud Environment Management

## Overview

### Objectives

- Understand Adobe Commerce Cloud architecture.
- Learn how to manage environments using the Cloud CLI.
- Configure environment variables and settings.
- Best practices for cloud deployment and scaling.

---

## 10.1 Understanding Adobe Commerce Cloud Architecture

### Study

- **Environments:**

  - **Integration:** Development environments.
  - **Staging:** Pre-production testing.
  - **Production:** Live site.

- **Cloud Features:**

  - Read-only file system for consistency.
  - Automated build and deploy processes.
  - Scalable infrastructure.

- **Configuration Files:**

  - ``.magento.app.yaml``: Application configuration.
  - ``.magento.env.yaml``: Environment-specific overrides.
  - ``.magento/routes.yaml``: Routing configuration.

### Example

- **Environment Hierarchy:**

  - `master` (Production)
    - `staging` (Staging)
      - `develop` (Integration)
        - Feature branches

---

## 10.2 Using the Cloud CLI for Environment Management

### Study

- **Cloud CLI Installation:**

  - Install using Composer:

    ```bash
    composer require --dev magento/magento-cloud-cli
    ```

- **Common Commands:**

  - **List Environments:**

    ```bash
    magento-cloud environment:list
    ```

  - **Create a New Branch:**

    ```bash
    magento-cloud environment:branch new-feature
    ```

  - **SSH into Environment:**

    ```bash
    magento-cloud ssh
    ```

  - **Set Environment Variables:**

    ```bash
    magento-cloud variable:set VARIABLE_NAME value
    ```

### Practical Exercise

**Task:** Create a new environment for development and deploy changes.

#### Steps:

1. **Create a New Branch Environment:**

   ```bash
   magento-cloud environment:branch new-feature
   ```

2. **Push Code to the New Environment:**

   ```bash
   git checkout -b new-feature
   # Make code changes
   git add .
   git commit -m "Implement new feature"
   git push origin new-feature
   ```

3. **Deploy the Environment:**

   - The push triggers a build and deployment on the cloud.

4. **Manage Environment Variables:**

   - Set a configuration variable:

     ```bash
     magento-cloud variable:set --level environment CONFIG_VAR value
     ```

5. **Access the Environment:**

   - View the URL provided by the cloud.
   - Use `magento-cloud ssh` to connect if needed.

#### Outcome

- Learned how to use the Cloud CLI for environment management.
- Created and managed a new development environment.

---

## 10.3 Configuring Environment Variables and Settings

### Study

- **Types of Variables:**

  - **Project Variables:** Apply to all environments.
  - **Environment Variables:** Specific to an environment.

- **Sensitive Data:**

  - Store sensitive configurations securely.
  - Use `RELATIONSHIPS` to access services like databases.

### Practical Exercise

**Task:** Add a sensitive configuration value to an environment.

#### Steps:

1. **Set an Environment-Specific Variable:**

   ```bash
   magento-cloud variable:set --level environment API_KEY your_api_key
   ```

2. **Access the Variable in Code:**

   ```php
   $apiKey = getenv('API_KEY');
   ```

3. **Avoid Committing Sensitive Data:**

   - Ensure that `env.php` and other sensitive files are not committed to version control.

#### Outcome

- Managed environment-specific configurations securely.
- Understood best practices for handling sensitive data in the cloud.

---

[Next Module >>](module11.md)