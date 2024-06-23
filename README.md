# Demonstration of Ansible Handlers without and with flush_handlers in Nginx Docker Deployment

This repository contains two Ansible playbooks for deploying and configuring Nginx inside Docker containers on a single Ubuntu server. The playbooks demonstrate the crucial difference and the benefit of using handlers with and without the `flush_handlers` directive.

## Workflow

**![](https://lh7-us.googleusercontent.com/docsz/AD_4nXdEyfaVBSkIVnbyIKXbbNBU5vdM7dlvtAvwlMJ6sOUAkkOhPulSz5is9KdUC3JhccR_LnKhntkicXT6mduVToqKXsl0TOVpaTF6wZyKcdx6YGv9-6S1QllGOT1fdVGuWq7aq_WlTAXLvrNVsI4IrbSm7wtU?key=L4XayPA1sB7Vrq7Z5fyZgg)**

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

Before install collection commuity of docker with command:

``ansible-galaxy collection install community.docker``

### play_with_flush.yml

This playbook installs Docker, pulls the Nginx image, creates a container, and sets up a custom Nginx configuration. It uses the `flush_handlers` directive to immediately apply the changes.

1. Run the playbook:
    ```bash
    ansible-playbook play_with_flush.yml -i hosts.ini
    ```

2. Observe the output to see the handler being executed immediately after the configuration changes.

### play_without_flush.yml

This playbook performs similar tasks but does not use the `flush_handlers` directive. The handler is only executed at the end of the playbook.

1. Run the playbook:
    ```bash
    ansible-playbook play_without_flush.yml -i hosts.ini
    ```

2. Observe the output to see the handler being executed at the end of the playbook run.

## Key Differences and Benefits

- **Immediate Application of Changes**: The `avec_flush.yml` playbook uses the `flush_handlers` directive to restart Nginx immediately after configuration changes, ensuring the new settings are applied right away. This is beneficial in scenarios where immediate feedback is needed or where services must be updated without delay.
- **Deferred Handler Execution**: The `sans_flush.yml` playbook restarts Nginx only after all tasks are completed, which might be suitable in scenarios where changes can wait until the entire configuration process is done. This reduces the number of service restarts but might delay the application of critical updates.

## Verification

To verify that the Nginx server is running and serving the custom HTML page, you can use `curl`:

1. Run the following command on the server:
    ```bash
    curl localhost
    ```

2. You should see the custom HTML content if the configuration was applied successfully.

## Troubleshooting

- Ensure that Docker is installed and running on the server.
- Check that the Ansible playbooks have the correct permissions and are executable.
- Verify that the Nginx container is running using `docker ps`.

## Logs

To view the logs of the Nginx container, use:
```bash
docker logs nginx_with_handler
docker logs nginx_without_handler
