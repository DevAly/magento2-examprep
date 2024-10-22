# Module 12: Testing and Troubleshooting

## Overview

### Objectives

- Learn how to use Magento's logging and debugging tools.
- Understand how to troubleshoot common issues.
- Learn about Magento's testing frameworks.
- Implement unit tests and integration tests.
- Use Xdebug for step debugging.

---

## 12.1 Utilizing Logging and Reports

### Study

- **Magento Logging System:**

  - Uses Monolog library.
  - Logs are stored in `var/log/`.

- **Common Log Files:**

  - `system.log`: General system messages.
  - `exception.log`: Captures exceptions.

- **Enabling Developer Mode:**

  - Provides detailed error messages.
  - Enable with:

    ```bash
    bin/magento deploy:mode:set developer
    ```

- **Custom Logging:**

  - Use `Psr\Log\LoggerInterface` to log custom messages.

### Practical Exercise

**Task:** Add custom logging to a module and analyze log files.

#### Steps:

1. **Inject Logger into a Class:**

   **File:** `app/code/Vendor/HelloWorld/Model/Example.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Model;

   use Psr\Log\LoggerInterface;

   class Example
   {
       protected $logger;

       public function __construct(LoggerInterface $logger)
       {
           $this->logger = $logger;
       }

       public function execute()
       {
           $this->logger->info('HelloWorld Example executed.');
       }
   }
   ```

2. **Trigger the Logging:**

   - Call the `execute` method of the `Example` class.

3. **Check the Log Files:**

   - Open `var/log/system.log`.
   - Look for the logged message.

4. **Analyze Errors:**

   - Intentionally introduce an error (e.g., incorrect class name).
   - Check `var/log/exception.log` for the stack trace.

#### Outcome

- Learned how to log custom messages.
- Gained experience in analyzing log files to troubleshoot issues.

---

## 12.2 Writing Unit Tests

### Study

- **Magento Testing Frameworks:**

  - **PHPUnit:** For unit testing.
  - **Magento Functional Testing Framework (MFTF):** For functional tests.

- **Unit Testing Best Practices:**

  - Test individual units of code in isolation.
  - Use mocks and stubs as needed.

- **Test Directory Structure:**

  - Place tests in `app/code/Vendor/Module/Test/Unit/`.

### Practical Exercise

**Task:** Write a unit test for a simple model method.

#### Steps:

1. **Create the Class to Test:**

   **File:** `app/code/Vendor/HelloWorld/Model/Calculator.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Model;

   class Calculator
   {
       public function add($a, $b)
       {
           return $a + $b;
       }
   }
   ```

2. **Set Up PHPUnit Configuration:**

   - Use the provided `phpunit.xml.dist` in Magento root directory.
   - Ensure PHPUnit is installed.

3. **Write the Unit Test:**

   **File:** `app/code/Vendor/HelloWorld/Test/Unit/Model/CalculatorTest.php`

   ```php
   <?php
   namespace Vendor\HelloWorld\Test\Unit\Model;

   use PHPUnit\Framework\TestCase;
   use Vendor\HelloWorld\Model\Calculator;

   class CalculatorTest extends TestCase
   {
       public function testAdd()
       {
           $calculator = new Calculator();
           $this->assertEquals(5, $calculator->add(2, 3));
       }
   }
   ```

4. **Run the Test:**

   ```bash
   vendor/bin/phpunit -c dev/tests/unit/phpunit.xml.dist app/code/Vendor/HelloWorld/Test/Unit/Model/CalculatorTest.php
   ```

5. **Verify Test Results:**

   - Test should pass if the `add` method functions correctly.

#### Outcome

- Learned how to write and run unit tests.
- Understood the importance of automated testing.

---

## 12.3 Using Xdebug for Step Debugging

### Study

- **What is Xdebug:**

  - A PHP extension that provides debugging and profiling capabilities.
  - Allows you to set breakpoints and step through code.

- **Setting Up Xdebug:**

  - Install Xdebug extension for PHP.
  - Configure `php.ini` with Xdebug settings.

- **Integrating with IDE:**

  - Popular IDEs like PhpStorm support Xdebug.
  - Configure the IDE to listen for Xdebug connections.

### Practical Exercise

**Task:** Debug a Magento module using Xdebug.

#### Steps:

1. **Install Xdebug:**

   - Follow installation instructions for your PHP version.

2. **Configure `php.ini`:**

   ```ini
   [Xdebug]
   zend_extension="xdebug.so"
   xdebug.mode=debug
   xdebug.start_with_request=yes
   xdebug.client_host=127.0.0.1
   xdebug.client_port=9003
   ```

3. **Set Up IDE:**

   - In PhpStorm, enable listening for PHP Debug connections.
   - Set up a server configuration matching your Magento environment.

4. **Set Breakpoints:**

   - Open the PHP class you want to debug.
   - Click in the margin to set breakpoints.

5. **Trigger the Debug Session:**

   - Access the relevant page or execute the code that invokes the class.
   - The IDE should stop at the breakpoint.

6. **Step Through Code:**

   - Use the debug controls to step over, into, or out of code.
   - Inspect variables and stack traces.

#### Outcome

- Set up and used Xdebug to debug Magento code.
- Improved ability to troubleshoot complex issues.

---

[Next Module >>](module13.md)