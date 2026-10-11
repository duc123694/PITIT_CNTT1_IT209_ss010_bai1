# Bài 1: Báo cáo kỹ thuật phân tích kiến trúc Container vs Virtual Machine

### 1. Bảng so sánh VN vs Container:

| Tiêu chí | Virtual Machine (VM) | Container (Docker) |
| --- | --- | --- |
| **Dung lượng Image** | Vài GB đến hàng chục GB | Vài MB đến vài trăm MB |
| **Thời gian khởi động** | Vài phút (phải boot Guest OS) | Vài mili giây / giây (khởi chạy process) |
| **Hao phí OS** | Cao (tốn overhead cho Guest OS) | Rất thấp (gần như bằng 0 overhead) |
| **Mức độ cô lập** | Cô lập mức Phần cứng (Hardware-level) | Cô lập mức OS (Process-level qua Namespaces/cgroups) |
| **Khả năng mở rộng** | Hàng chục VM trên 1 Server | Hàng trăm / hàng nghìn Container trên 1 Server |

### 2. Vai trò của Namespaces 
- **Namespaces:** Đảm nhiệm việc cô lập góc nhìn về tài nguyên của hệ thống (như Process ID, Network, Mount points, User IDs). Nhờ có Namespaces, mỗi container có một không gian tên riêng biệt, khiến tiến trình bên trong container chỉ thấy được các tài nguyên của riêng nó mà không nhìn thấy hoặc tác động đến các tiến trình hay mạng của container khác hoặc của hệ thống host.
- **Control Groups (cgroups):** Đảm nhiệm việc giới hạn và hạn chế mức sử dụng tài nguyên phần cứng (như CPU, Memory, I/O). Nhờ có cgroups, hệ thống có thể kiểm soát chính xác lượng tài nguyên tối đa mà một container được phép tiêu thụ, ngăn chặn việc một container bị lỗi hoặc chạy nặng làm cạn kiệt tài nguyên của toàn bộ máy chủ.


### 3. Các thông số từ đầu ra lệnh `docker info`
- Các thông số:
  - Server Version: 29.8.2
  - Cgroup Driver: systemd - version: 2
  - Số lượng Containers: 5
  - Số lượng Images: 6

- Kết quả:
```bash
dev_docker@ip-172-31-42-200:~$ docker info
Client: Docker Engine - Community
 Version:    29.8.2
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.0
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.6.0
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 5
  Running: 2
  Paused: 0
  Stopped: 3
 Images: 6
 Server Version: 29.8.2
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: ee2735368117d2eb259779949d5e75cdafec9761
 runc version: v1.5.1-0-g8f2685a4
 init version: de40ad0
 Security Options:
  apparmor
   Profile: default
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 7.0.0-1014-aws
 Operating System: Ubuntu 26.04.1 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 908.6MiB
 Name: ip-172-31-42-200
 ID: a23d521e-857e-419b-8be1-705c2ad873e4
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables
  EnableUserlandProxy: true
  UserlandProxyPath: /usr/bin/docker-proxy
```
