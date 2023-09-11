Prometheus and Grafana:

Prometheus is an open-source monitoring and alerting toolkit designed for reliability and scalability. Grafana, on the other hand, is an open-source analytics and monitoring platform that integrates well with Prometheus. You can use Prometheus to scrape metrics from your containers and applications and then visualize them in Grafana dashboards.

Let's set up Prometheus and Grafana for monitoring WordPress, phpMyAdmin, and MySQL containers. 
This step-by-step guide assumes you have Docker and Docker Compose installed on your system. We'll use Docker Compose to set up both Prometheus and Grafana containers.

**Step 1: Create a Directory for Configuration**

Create a directory to store your configuration files for Prometheus and Grafana. For example:

```bash
mkdir monitoring
cd monitoring
```

**Step 2: Create a `docker-compose.yml` File**

Inside the `monitoring` directory, create a `docker-compose.yml` file for setting up Prometheus and Grafana:

```yaml
version: '3'
services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus:/etc/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:7.5.7
    user: "472:472"  # Add this line
    ports:
      - 3000:3000
    restart: unless-stopped
    volumes:
      - ./grafana/provisioning/datasources:/etc/grafana/provisioning/datasources
      - grafana-data:/var/lib/grafana
    networks:
      - monitoring

networks:
  monitoring:
    driver: bridge

volumes:
  grafana-data:
    driver: local
```

This `docker-compose.yml` file defines two services, Prometheus and Grafana, and configures their respective containers. It also sets up a Docker network named `monitoring` for communication between containers.

**Step 3: Create Configuration Files**

Create the configuration files for Prometheus and Grafana:

- **Prometheus Configuration (`prometheus.yml`):**

  Create a file named `prometheus.yml` inside the `prometheus` directory:

  ```yaml
  global:
    scrape_interval: 15s

  scrape_configs:
    - job_name: 'prometheus'
      static_configs:
        - targets: ['localhost:9090']

    - job_name: 'wordpress'
      static_configs:
        - targets: ['wordpress:80']

    - job_name: 'phpmyadmin'
      static_configs:
        - targets: ['phpmyadmin:80']
  ```

  This configuration tells Prometheus to scrape metrics from itself, as well as from your WordPress and phpMyAdmin containers.


- **Grafana Configuration (`grafana.ini`):**

  Create a file named `grafana.ini` inside the `grafana` directory:

  ```ini
  [server]
  root_url = %(protocol)s://%(domain)s:%(http_port)s/grafana/

  [security]
  admin_user = admin
  admin_password = adminpassword
  ```

  This sets up the Grafana admin user and password.

**Step 4: Start the Containers**

Run the following command to start the Prometheus and Grafana containers:

```bash
docker-compose up -d
```

This will start the containers in detached mode.

**Step 5: Access Prometheus and Grafana**

- Prometheus: You can access the Prometheus web interface at http://localhost:9090/. This is where you can configure and query metrics.

- Grafana: Access the Grafana web interface at http://localhost:3000/. Log in with the admin user and password you set in the `docker-compose.yml` file. From Grafana, you can create dashboards to visualize your container metrics.

**Step 6: Configure Data Sources and Dashboards (Grafana)**

In Grafana, you'll need to configure Prometheus as a data source and create dashboards to monitor your containers. Here's a basic outline:

- Log in to Grafana (http://localhost:3000/).
- Go to "Configuration" > "Data Sources" > "Add Data Source."
- Choose "Prometheus" and configure it to connect to `http://prometheus:9090`.

Now you can create dashboards and panels in Grafana to visualize your container metrics. You can find pre-made Grafana dashboards for Docker and Prometheus metrics on the Grafana website or create custom dashboards to suit your needs.

Remember to customize and secure Grafana and Prometheus configurations for production use. Additionally, adjust the Prometheus configuration in `prometheus.yml` to scrape metrics from your containers as needed.
