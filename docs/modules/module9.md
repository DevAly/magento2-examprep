# Module 9: Working with Cron Jobs

## Overview

### Objectives

- Understand how Magento's cron jobs work.
- Learn how to schedule custom cron jobs.
- Share configurations among cron jobs using groups.
- Best practices for cron job implementation.

---

## 9.1 Scheduling Custom Cron Jobs

### Study

- **Cron Configuration Files:**

  - Defined in `crontab.xml` within `etc` directory.
  - Schedules tasks to be executed at specified times.

- **Cron Job Elements:**

  - `job`: Defines the cron job.
  - `schedule`: Specifies when the job runs using cron syntax.
  - `group`: Optional, allows grouping of jobs.

### Practical Exercise

**Task:** Create a cron job that logs a message every hour.

#### Steps:

1. **Define the Cron Job:**

   **File:** `app/code/Vendor/HelloWorld/etc/crontab.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/crontab.xsd">
     <group id="default">
       <job name="vendor_helloworld_cron" instance="Vendor\HelloWorld\Cron\MyCron" method="execute">
         <schedule>0 * * * *</schedule>
       </job>
     </group>
   </config>
   ```

2. **Create the Cron Class:**

   **File:** `app/code/Vendor/HelloWorld/Cron/MyCron.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Cron;

   use Psr\Log\LoggerInterface;

   class MyCron
   {
       protected $logger;

       public function __construct(LoggerInterface $logger)
       {
           $this->logger = $logger;
       }

       public function execute()
       {
           $this->logger->info('Vendor_HelloWorld cron job executed.');
           return $this;
       }
   }
   ```

3. **Verify Cron Job Execution:**

   - Ensure Magento's cron is set up on your system.
   - Check the logs in `var/log/system.log` or `var/log/cron.log`.

#### Outcome

- Learned how to schedule custom cron jobs.
- Implemented logging within a cron job.

---

## 9.2 Using Cron Groups

### Study

- **Cron Groups:**

  - Allows grouping of cron jobs.
  - Each group can have different settings (e.g., timeouts).
  - Defined in `cron_groups.xml`.

- **Default Groups:**

  - `default`, `index`, `analytics`.

### Practical Exercise

**Task:** Create a custom cron group and assign a job to it.

#### Steps:

1. **Define the Cron Group:**

   **File:** `app/code/Vendor/HelloWorld/etc/cron_groups.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/cron_groups.xsd">
     <group id="custom_group">
       <schedule_generate_every>1</schedule_generate_every>
       <schedule_ahead_for>60</schedule_ahead_for>
       <schedule_lifetime>60</schedule_lifetime>
       <history_cleanup_every>10</history_cleanup_every>
       <history_success_lifetime>60</history_success_lifetime>
       <history_failure_lifetime>600</history_failure_lifetime>
       <use_separate_process>true</use_separate_process>
     </group>
   </config>
   ```

2. **Assign Job to the Group:**

   Modify `crontab.xml`:

   **File:** `app/code/Vendor/HelloWorld/etc/crontab.xml`

   ```xml
   <?xml version="1.0"?>
   <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/crontab.xsd">
     <group id="custom_group">
       <job name="vendor_helloworld_cron_custom" instance="Vendor\HelloWorld\Cron\MyCron" method="execute">
         <schedule>*/5 * * * *</schedule>
       </job>
     </group>
   </config>
   ```

3. **Verify Cron Job Execution:**

   - Ensure the cron for `custom_group` is running.
   - Check logs to confirm execution.

#### Outcome

- Learned how to create and use cron groups.
- Managed cron job configurations effectively.

---

[Next Module >>](module10.md)