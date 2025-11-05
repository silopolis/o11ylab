# Observability lab in containers

* Run all in Docker, orchestrated by Compose


## Components

* Prometheus
  * Upstream `Dockerfile`
  * Configuration in `./prometheus`
    * Listens on default port `9090`
    * Rules:
      * Recording in `./prometheus/rules/recording/*.rules.yml`
      * Alerting in `./prometheus/rules/alerting/*.rules.yml`
    * Scrape configs:
      * Prometheus statically defined in `scrape_configs`
        * Self scraping
        * Blackbox:
          * `http_200` on `/-/ready/`
          * `prom_instant_query` on dummy `query=1`
      * Other static targets defined in in `./prometheus/scrape_configs/*.scrape.yml`
        * Prometheus demo service instances
        * Docker (on host)
        * Node
  * Data in `data/prometheus`  
    Must be `chmod ugo+rwX` because Prometheus runs as `nobody:nogroup`
* Prometheus demo service  
  Generates demo metrics  
  3 instances
* Prometheus Blackbox exporter
  * Modules:
    * `http_200`: checks readyness of an HTTP target
    * `prom_instant_query`: checks `status: success` present in JSON reply
  * Checks Prometheus readyness and basic query success
* Grafana
  * Configuration in `./grafana`
    * Listens on default port `3000`
    * Default user/pwd `admin/admin`
  * Data in `data/grafana`
    Must be `chmod ugo+rwX` because Prometheus runs as `grafana` user (472)


