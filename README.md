# Managing Secrets with Vault and Consul

## Want to learn how to build this?
It  a Docker Compose file (YAML) for setting up a service named "vault" using Docker. This service seems to be running HashiCorp Vault, a tool for managing secrets and protecting sensitive data.

Here's a breakdown of the key elements in your Docker Compose file:


```yaml
version: '3.8'

services:

  vault:
    build:
      context: ./vault
      dockerfile: Dockerfile
    ports:
      - 8200:8200
    volumes:
      - ./vault/config:/vault/config
      - ./vault/policies:/vault/policies
      - ./vault/data:/vault/data
      - ./vault/logs:/vault/logs
    environment:
      - VAULT_ADDR=http://127.0.0.1:8200
      - VAULT_API_ADDR=http://127.0.0.1:8200
    command: server -config=/vault/config/vault-config.json
    cap_add:
      - IPC_LOCK
```

1. **Service Name:**
   - The service is named "vault."

2. **Docker Build Configuration:**
   - The service is built using a Dockerfile located in the "./vault" directory.

3. **Port Mapping:**
   - The Vault service inside the Docker container is mapped to port 8200 on the host machine.

4. **Volume Mounts:**
   - Various directories from the host machine are mounted as volumes inside the container. These include configurations, policies, data, and logs.
     - `./vault/config` is mounted to `/vault/config` inside the container.
     - `./vault/policies` is mounted to `/vault/policies` inside the container.
     - `./vault/data` is mounted to `/vault/data` inside the container.
     - `./vault/logs` is mounted to `/vault/logs` inside the container.

5. **Environment Variables:**
   - Environment variables are set for the Vault server:
     - `VAULT_ADDR` is set to "http://127.0.0.1:8200."
     - `VAULT_API_ADDR` is set to "http://127.0.0.1:8200."

6. **Command:**
   - The command to start the Vault server is specified with the `-config=/vault/config/vault-config.json` flag, indicating the location of the configuration file.

7. **Capabilities:**
   - The `cap_add` section specifies additional Linux capabilities for the container. In this case, it adds the capability `IPC_LOCK`, which is often used by applications that need to lock memory.

Please note that the effectiveness of this configuration depends on the specific requirements and security considerations of your Vault setup. Additionally, ensure that the necessary Vault configuration file (`vault-config.json`) is present in the `./vault/config` directory on your host machine.

1. Build the images and run the containers:

    ```sh
    $ docker-compose up -d --build
    ```

1. You can now interact  Vault . View the UIs at [http://localhost:8200/ui](http://localhost:8200/ui)
