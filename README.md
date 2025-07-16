# Local Grafana Stack

Môi trường monitoring hoàn chỉnh với Grafana, Prometheus, Loki, và Tempo.

## Các thành phần

-   **Grafana** (port 3000): Dashboard và visualization
-   **Prometheus** (port 9090): Metrics collection và storage
-   **Loki** (port 3100): Log aggregation
-   **Tempo** (port 3200): Distributed tracing
-   **Promtail**: Log collection agent
-   **Node Exporter** (port 9100): System metrics

## Khởi chạy

### Trên Windows (PowerShell):

```powershell
# Khởi động tất cả services
docker-compose up -d

# Xem logs
docker-compose logs -f

# Dừng tất cả services
docker-compose down

# Dừng và xóa volumes (mất data)
docker-compose down -v
```

### Trên Linux/macOS (với make):

```bash
# Khởi động tất cả services
make up

# Xem logs
make logs

# Dừng tất cả services
make down

# Dừng và xóa volumes (mất data)
make clean
```

## Truy cập

-   **Grafana**: http://localhost:3000
    -   Username: admin
    -   Password: admin
-   **Prometheus**: http://localhost:9090
-   **Loki**: http://localhost:3100
-   **Tempo**: http://localhost:3200

## Data Persistence

Tất cả data được lưu trữ trong Docker volumes:

-   `prometheus-data`: Prometheus metrics data
-   `loki-data`: Loki logs data
-   `tempo-data`: Tempo traces data
-   `grafana-data`: Grafana dashboards và settings
-   `promtail-data`: Promtail positions file

## Cấu hình

### Prometheus

-   File cấu hình: `prometheus/prometheus.yml`
-   Scrape các services: prometheus, node-exporter, grafana, loki, tempo

### Loki

-   File cấu hình: `loki/loki-config.yml`
-   Storage: Local filesystem
-   Retention: Có thể cấu hình trong limits_config

### Tempo

-   File cấu hình: `tempo/tempo.yml`
-   OTLP receivers: gRPC (4317), HTTP (4318)
-   Storage: Local filesystem

### Promtail

-   File cấu hình: `promtail/promtail-config.yml`
-   Thu thập logs từ /var/log và docker containers

### Grafana

-   Datasources được tự động provision
-   Kết nối với Prometheus, Loki, và Tempo
-   Tích hợp traces-to-logs và traces-to-metrics

## Sử dụng

1. Khởi động stack: `docker-compose up -d`
2. Truy cập Grafana tại http://localhost:3000
3. Login với admin/admin
4. Kiểm tra datasources đã được tự động thêm
5. Tạo dashboards hoặc import dashboards có sẵn
6. Xem metrics trong Prometheus: http://localhost:9090
7. Query logs trong Loki thông qua Grafana
8. Xem traces trong Tempo thông qua Grafana

## Troubleshooting

```bash
# Kiểm tra trạng thái containers
docker-compose ps

# Xem logs của service cụ thể
docker-compose logs grafana
docker-compose logs prometheus
docker-compose logs loki
docker-compose logs tempo

# Restart service cụ thể
docker-compose restart grafana

# Kiểm tra network
docker network ls
```

## Mở rộng

-   Thêm Alertmanager cho alerting
-   Thêm các exporters khác (mysql-exporter, redis-exporter, etc.)
-   Cấu hình remote storage cho production
-   Thêm authentication và SSL/TLS
