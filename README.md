# Executing Ansible Playbooks with Handlers and Flush Handlers

This guide demonstrates how to execute Ansible playbooks focusing on handlers and flush handlers, and discusses the benefits of using flush handlers.

## Prerequisites

Ensure the following prerequisites are met before proceeding:

## Workflow

**![](https://lh7-us.googleusercontent.com/docsz/AD_4nXdEyfaVBSkIVnbyIKXbbNBU5vdM7dlvtAvwlMJ6sOUAkkOhPulSz5is9KdUC3JhccR_LnKhntkicXT6mduVToqKXsl0TOVpaTF6wZyKcdx6YGv9-6S1QllGOT1fdVGuWq7aq_WlTAXLvrNVsI4IrbSm7wtU?key=L4XayPA1sB7Vrq7Z5fyZgg)**


1. **Ansible Installed**: Ensure Ansible is installed on your system. You can install Ansible using the package manager of your operating system or via Python's pip package manager.

   ```bash
   # For Ubuntu/Debian
   sudo apt update
   sudo apt install ansible

   # For CentOS/RHEL
   sudo yum install ansible

   # Using pip (Python package manager)
   pip install ansible



## Playbooks

1. **play_with_flush.yml**: This playbook configures and deploys Nginx and uses the `flush_handlers` directive to immediately apply changes and reload the Nginx configuration.
2. **play_without_flush.yml**: This playbook configures and deploys Nginx without using the `flush_handlers` directive, resulting in the handler being executed only at the end of the playbook run.

## Prerequisites

- Ansible installed on the control node.
- An Ubuntu server with Docker installed.

## Setup

1. Clone the repository:
    ```bash
    git clone https://github.com/Aurelie-Kamgang/handler-demo.git
    cd handler-demo
    ```

2. Ensure that the server has the necessary permissions and is accessible via SSH.

## Running the Playbooks

### play_with_flush.yml

This playbook installs Docker, pulls the Nginx image, creates a container, and sets up a custom Nginx configuration. It uses the `flush_handlers` directive to immediately apply the changes.

1. Run the playbook:
    ```bash
    ansible-playbook play_with_flush.yml 
    ```

2. Observe the output to see the handler being executed immediately after the configuration changes.

### play_without_flush.yml

This playbook performs similar tasks but does not use the `flush_handlers` directive. The handler is only executed at the end of the playbook.

1. Run the playbook:
    ```bash
    ansible-playbook play_without_flush.yml 
    ```

2. Observe the output to see the handler being executed at the end of the playbook run.

## Key Differences and Benefits

- **Immediate Application of Changes**: The `play_with_flush.yml` playbook uses the `flush_handlers` directive to restart Nginx immediately after configuration changes, ensuring the new settings are applied right away. This is beneficial in scenarios where immediate feedback is needed or where services must be updated without delay.
- **Deferred Handler Execution**: The `play_without_flush.yml` playbook restarts Nginx only after all tasks are completed, which might be suitable in scenarios where changes can wait until the entire configuration process is done. This reduces the number of service restarts but might delay the application of critical updates.

## Verification

To verify that the Nginx server is running and serving the custom HTML page, you can use `curl`:

1. Run the following command on the server:

- for play_without_flush.yml:
  
    ```bash
    curl localhost
    ```
- for play_with_flush.yml:

    ```bash
    curl http://localhost:443
    ```

2. You should see the custom HTML content if the configuration was applied successfully.

## Troubleshooting

Troubleshooting: If changes made by the playbook are not reflecting as expected, check logs (/var/log/nginx/error.log, /var/log/apache2/error.log, etc.) for any errors or warnings. Ensure that your playbook's tasks, handlers, and paths are correctly defined.

## Understanding Handlers and Flush Handlers:

Handlers: Handlers in Ansible are special tasks that are triggered by other tasks using the notify directive. They are typically used to restart services or perform actions that need to be triggered after specific changes.
Flush Handlers: By default, handlers are executed at the end of a play. However, if you want to apply changes immediately and not wait until the end of the play, you can use meta: flush_handlers after making configuration changes. This ensures that any handlers triggered by notify directives are executed immediately.
Benefits of Using Flush Handlers:

Immediate Application of Changes: Flush handlers ensure that changes to configurations (like updating Nginx or Apache settings) are applied immediately without waiting for the playbook to complete.
Real-time Configuration Updates: Useful when you need to see the impact of configuration changes immediately, especially in environments where services need to be updated or restarted swiftly.
