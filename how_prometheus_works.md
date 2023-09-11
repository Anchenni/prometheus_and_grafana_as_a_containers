Prometheus scrapes metrics from your containers and other targets by using a mechanism called "Service Discovery" and "Exporters." Here's a simplified explanation of how Prometheus scrapes metrics from your containers:

1. **Service Discovery:**
   Prometheus uses a feature called service discovery to dynamically discover and monitor targets, such as your containers. There are various methods of service discovery in Prometheus:

   - **Static Configuration:** You can manually specify the targets in the `prometheus.yml` configuration file, as you've done. This is known as "static configuration."

   - **Dynamic Service Discovery:** Prometheus can also discover targets dynamically using service discovery mechanisms like DNS-based service discovery, Kubernetes service discovery, or file-based service discovery. This allows Prometheus to automatically find and monitor new containers as they come online.

2. **Exporters:**
   Prometheus itself doesn't collect metrics directly from your containers; instead, it relies on exporters. Exporters are small programs or services that run alongside your applications or services and expose metrics in a format that Prometheus can understand. Common exporters include the Node Exporter for host-level metrics and the Prometheus MySQL Exporter for MySQL-specific metrics.

3. **Scraping Process:**
   Once Prometheus knows where to find the targets (either through static configuration or dynamic service discovery), it uses HTTP or other protocols to periodically scrape the metrics endpoints exposed by the exporters. The exporters serve the metrics in a format called "Prometheus exposition format," which is a simple text-based format.

4. **Storage:**
   Prometheus stores the scraped metrics in its time-series database. It keeps track of these metrics over time, allowing you to query and visualize historical data.

5. **Alerting and Queries:**
   Prometheus can be configured to generate alerts based on predefined rules or to perform ad-hoc queries to retrieve specific information from the collected metrics. It has a powerful query language called PromQL (Prometheus Query Language) for this purpose.

In summary, Prometheus scrapes metrics from your containers through exporters that expose metrics in a standardized format. The exporters run alongside your applications or services and make the metrics available via HTTP or other protocols. Prometheus periodically queries these endpoints, stores the data, and allows you to query, visualize, and set up alerting based on the collected metrics.
