# Hướng dẫn Ngôn ngữ
## Thiết lập biến môi trường
> Tham khảo ngôn ngữ khác: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language

N8N_DEFAULT_LOCALE=vi-VN

## Thay thế gói editor-ui
Tải gói UI Editor từ mục releases với phiên bản tương ứng, sau đó ánh xạ đến đường dẫn thư mục UI Editor trong container docker

/usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/dist


## Lệnh docker kiểm tra đầy đủ
```shell
docker run -it --rm --name n8n \
-p 5678:5678 \
-v [đường_dẫn_đến_thư_mục_UI_đã_tải]:/usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/dist \
-e N8N_DEFAULT_LOCALE=vi-VN \
-e N8N_SECURE_COOKIE=false \
n8nio/n8n
```

## Giải thích các tham số
- `-it`: Chế độ tương tác và cấp phát TTY
- `--rm`: Tự động xóa container khi dừng
- `--name n8n`: Đặt tên cho container
- `-p 5678:5678`: Ánh xạ cổng từ máy host sang container
- `-v [đường_dẫn]:/usr/local/lib/...`: Liên kết thư mục UI đã tải về với thư mục trong container
- `-e N8N_DEFAULT_LOCALE=vi-VN`: Thiết lập ngôn ngữ mặc định
- `-e N8N_SECURE_COOKIE=false`: Tắt cookie bảo mật (cho môi trường phát triển)

## Sử dụng Docker Compose
Tạo file `docker-compose.yml` với nội dung sau:
```yaml
version: '3'

services:
  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    volumes:
      - ./n8n-data:/home/node/.n8n
      - ./editor-ui-dist:/usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/dist
    environment:
      - N8N_DEFAULT_LOCALE=vi-VN
      - N8N_SECURE_COOKIE=false
    # Các biến môi trường khác nếu cần
    # - DB_TYPE=postgresdb
    # - DB_POSTGRESDB_HOST=postgres
    # - DB_POSTGRESDB_PORT=5432
    # - DB_POSTGRESDB_DATABASE=n8n
    # - DB_POSTGRESDB_USER=n8n
    # - DB_POSTGRESDB_PASSWORD=password
```

Khởi động với lệnh:
```shell
docker-compose up -d
```

# Nguyên lý
> editor-ui hỗ trợ i18n, nhưng chưa mở gói ngôn ngữ

Thêm thủ công vi-VN.json vào thư mục i18n của editor-ui, sau đó biên dịch lại
Thiết lập ngôn ngữ trong môi trường để sử dụng tiếng Trung một cách bình thường

# Thêm gói ngôn ngữ khác
Vui lòng gửi PR tệp ngôn ngữ vào thư mục languages, github action sẽ tự động đóng gói trong phiên bản n8n tiếp theo
