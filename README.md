# Traefik

## Mở đầu

![alt text](https://images.viblo.asia/fce9ff11-3106-42c9-a27a-324e55df6b0c.png)

**Giới thiệu Traefik**
- Traefik là Reverse-proxy đời mới, cũng là load Balancer để  giúp cho việc deploy hệ thống microservice dễ dàng hơn.
- Tích hợp nhiều thành phần: Docker, Swarm mode, Kubernetes, Marathon, Consul, Etcd, Rancher, Amazon ESC .... Và tính **Tự động** là điểm quan trong 

## Traefik sinh ra để làm gì?

**Traefik và Microservices**

- Microservices architecture là một kiểu kiến trúc hệ thống để dựng lên application của chúng ta. Tại đó, application được định nghĩa dưới dạng một tập hợp các services. Mỗi service sẽ đảm nhiệm một phần chức năng trong hệ thống

**Cách tính năng**

- Traefik tự động cập nhật cấu hình mà không phải restart
- Hỗ trợ nhiều giải thuật load balancing
- Free HTTPS cho microservice với Let's Encrypt
- Websocket, HTTP/2, GRPC
- Chống quá tải với Circuit breakers
- Lưu access logs (JSON, CLF)
- Fast
- Có Rest API cho bạn sử dụng để để update các config.

**cấu hình cho Traefik qua 3 cách thức**:

1. File cấu hình .toml
2. Docker API
3. RESTful API

Sử dụng nhiều là cách 1 và 2

**Toml file**

Là file có đuôi mở rộng là .toml
cú pháp dạng key = value

- **Chú ý**
    + TOML phân biệt chữ hoa chữ thường.
    + File TOML phải là tài liệu Unicode được mã hóa UTF-8 hợp lệ.
    + Khoảng trắng được TOML hiểu là ký tự tab (0x09) hoặc dấu cách (0x20).
    + Dòng mới được hiểu là ký tự LF (0x0A) hoặc CRLF (0x0D0A).s
    + Comment đến hết dòng được sử dụng bằng dấu #.

**Docker API**

Các cấu hình của Traefik có thể được define với Docker api qua docker-compose. Sử dụng từ khóa command, labels. Trong đó, commands cần có --docker để khai báo sử dụng docker api cho traefik.

VD:

``
version: '3'

services:
    reverse-proxy:
        image: traefik/traefik:latest
        commands: --api --docker --
        ports:
            - 80:80
            - 443:443
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        labels:
            - traefik.enable=true
            - traefik.port=80
``

**RESTful API**

Enable rest api bằng cách thêm group rest vào file .toml:


# YAML

#### command
- api.insecure=true- cho phép truy cập bảng điều khiển Traefik - đơn giản hóa việc gỡ lỗi, nhưng sẽ bị vô hiệu hóa bên ngoài môi trường phát triển vì lý do bảo mật.
- providers.docker=true- cho phép khám phá cấu hình Docker
- providers.docker.exposedbydefault=false- không để lộ các dịch vụ Docker theo mặc định
- entrypoints.web.address=:80- tạo một entrypoint được gọi web, đang nghe:80