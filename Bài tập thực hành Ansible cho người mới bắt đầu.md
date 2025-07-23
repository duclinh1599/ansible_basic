# Bài tập thực hành Ansible cho người mới bắt đầu

Các bài tập này được thiết kế để đi kèm với lộ trình học Ansible trong 1 tháng, giúp bạn củng cố kiến thức lý thuyết thông qua thực hành trực tiếp.

## Tuần 1: Giới thiệu và các khái niệm cơ bản

### Bài 1.1: Chuẩn bị môi trường
- **Mục tiêu:** Cài đặt Control Node và Managed Nodes, thiết lập kết nối SSH.
- **Các bước:**
  1. Cài đặt một máy ảo Linux (Ubuntu Server 22.04 LTS hoặc CentOS Stream 9) làm Control Node. Đảm bảo máy ảo có kết nối internet.
  2. Cài đặt Ansible trên Control Node theo hướng dẫn chính thức (ví dụ: `sudo apt update && sudo apt install ansible` cho Ubuntu).
  3. Tạo 2-3 máy ảo Linux khác (cùng hệ điều hành với Control Node hoặc khác) làm Managed Nodes. Đảm bảo các Managed Nodes có thể được truy cập từ Control Node qua mạng.
  4. Trên Control Node, tạo cặp khóa SSH (`ssh-keygen`).
  5. Sao chép khóa công khai SSH từ Control Node sang tất cả các Managed Nodes (`ssh-copy-id user@managed_node_ip`). Đảm bảo bạn có thể SSH vào Managed Nodes mà không cần mật khẩu.
  6. Tạo file `inventory.ini` trong thư mục làm việc của bạn với nội dung sau (thay thế IP và user của bạn):
    ```ini
    [webservers]
    web1 ansible_host=192.168.1.10 user=your_user
    web2 ansible_host=192.168.1.11 user=your_user

    [databases]
    db1 ansible_host=192.168.1.12 user=your_user
    ```

### Bài 1.2: Kiểm tra kết nối và Ad-hoc commands cơ bản
- **Mục tiêu:** Kiểm tra kết nối Ansible và thực thi các lệnh Ad-hoc đơn giản.
- **Các bước:**
  1. Kiểm tra kết nối đến tất cả các host trong inventory:
     `ansible all -i inventory.ini -m ping`
  2. Thực thi lệnh `uptime` trên nhóm `webservers`:
     `ansible webservers -i inventory.ini -m command -a 


uptime"`
  3. Cài đặt gói `htop` trên nhóm `databases` (sử dụng module `apt` cho Ubuntu hoặc `yum` cho CentOS):
     `ansible databases -i inventory.ini -m apt -a "name=htop state=present"` (Ubuntu)
     `ansible databases -i inventory.ini -m yum -a "name=htop state=present"` (CentOS)
  4. Xóa gói `htop` vừa cài đặt:
     `ansible databases -i inventory.ini -m apt -a "name=htop state=absent"` (Ubuntu)
     `ansible databases -i inventory.ini -m yum -a "name=htop state=absent"` (CentOS)

## Tuần 2: Playbooks và Variables

### Bài 2.1: Viết Playbook đầu tiên - Cài đặt Web Server
- **Mục tiêu:** Viết Playbook để tự động cài đặt và cấu hình Nginx/Apache trên các Managed Nodes.
- **Các bước:**
  1. Tạo file `webserver_setup.yml` với nội dung sau (ví dụ cho Nginx trên Ubuntu):
    ```yaml
    ---
    - name: Cài đặt và cấu hình Nginx
      hosts: webservers
      become: yes
      tasks:
        - name: Cập nhật apt cache
          apt: update_cache=yes

        - name: Cài đặt Nginx
          apt: name=nginx state=present

        - name: Khởi động và bật Nginx
          service: name=nginx state=started enabled=yes

        - name: Triển khai trang web mặc định
          copy:
            content: "<h1>Chào mừng đến với Ansible!</h1>"
            dest: /var/www/html/index.html
    ```
  2. Chạy Playbook:
     `ansible-playbook -i inventory.ini webserver_setup.yml`
  3. Kiểm tra kết quả bằng cách truy cập IP của `web1` và `web2` trên trình duyệt.

### Bài 2.2: Sử dụng Variables và Facts
- **Mục tiêu:** Tùy chỉnh Playbook bằng Variables và khám phá Facts.
- **Các bước:**
  1. Sửa đổi `webserver_setup.yml` để sử dụng variables cho nội dung trang web và cổng lắng nghe (nếu có).
    ```yaml
    ---
    - name: Cài đặt và cấu hình Nginx với Variables
      hosts: webservers
      become: yes
      vars:
        welcome_message: "Xin chào từ {{ ansible_hostname }}!"
      tasks:
        - name: Cập nhật apt cache
          apt: update_cache=yes

        - name: Cài đặt Nginx
          apt: name=nginx state=present

        - name: Khởi động và bật Nginx
          service: name=nginx state=started enabled=yes

        - name: Triển khai trang web tùy chỉnh
          copy:
            content: "{{ welcome_message }}"
            dest: /var/www/html/index.html

        - name: Hiển thị facts về hệ điều hành
          debug:
            var: ansible_distribution

        - name: Hiển thị facts về địa chỉ IP
          debug:
            var: ansible_facts.ipv4_addresses
    ```
  2. Chạy Playbook và quan sát output của `debug` module.
  3. Chạy Playbook với extra variable để ghi đè `welcome_message`:
     `ansible-playbook -i inventory.ini webserver_setup.yml -e "welcome_message='Đây là thông điệp mới!'"`

### Bài 2.3: Sử dụng Handlers
- **Mục tiêu:** Thêm Handlers vào Playbook để khởi động lại dịch vụ khi cấu hình thay đổi.
- **Các bước:**
  1. Sửa đổi `webserver_setup.yml` để thêm một task thay đổi file cấu hình Nginx (ví dụ: `/etc/nginx/nginx.conf`) và một handler để khởi động lại Nginx.
    ```yaml
    ---
    - name: Cài đặt và cấu hình Nginx với Handler
      hosts: webservers
      become: yes
      tasks:
        - name: Cập nhật apt cache
          apt: update_cache=yes

        - name: Cài đặt Nginx
          apt: name=nginx state=present

        - name: Cấu hình Nginx (ví dụ: thay đổi port)
          copy:
            content: | # Nội dung cấu hình Nginx mới
              user www-data;
              worker_processes auto;
              events {
                  worker_connections 1024;
              }
              http {
                  include /etc/nginx/mime.types;
                  default_type application/octet-stream;
                  sendfile on;
                  keepalive_timeout 65;
                  server {
                      listen 8080;
                      root /var/www/html;
                      index index.html index.htm;
                  }
              }
            dest: /etc/nginx/nginx.conf
          notify: Khởi động lại Nginx

        - name: Khởi động và bật Nginx
          service: name=nginx state=started enabled=yes

      handlers:
        - name: Khởi động lại Nginx
          service: name=nginx state=restarted
    ```
  2. Chạy Playbook và quan sát khi nào handler được kích hoạt.

## Tuần 3: Roles và Templates

### Bài 3.1: Tạo và sử dụng Roles
- **Mục tiêu:** Tổ chức Playbook thành Roles để dễ quản lý và tái sử dụng.
- **Các bước:**
  1. Trên Control Node, tạo một thư mục dự án mới (ví dụ: `my_ansible_project`).
  2. Trong thư mục dự án, tạo một Role mới cho webserver:
     `ansible-galaxy init roles/webserver`
  3. Di chuyển các tasks từ `webserver_setup.yml` (Bài 2.1) vào file `roles/webserver/tasks/main.yml`.
  4. Tạo một Playbook chính (ví dụ: `site.yml`) trong thư mục dự án để gọi Role `webserver`:
    ```yaml
    ---
    - name: Triển khai Webserver bằng Role
      hosts: webservers
      become: yes
      roles:
        - webserver
    ```
  5. Chạy Playbook chính:
     `ansible-playbook -i inventory.ini site.yml`

### Bài 3.2: Sử dụng Jinja2 Templates
- **Mục tiêu:** Tạo file cấu hình động bằng Jinja2 Templates.
- **Các bước:**
  1. Trong Role `webserver`, tạo thư mục `templates` (`roles/webserver/templates`).
  2. Tạo file `nginx.conf.j2` trong thư mục `templates` với nội dung cấu hình Nginx sử dụng variables (ví dụ: port, root directory):
    ```jinja2
    user www-data;
    worker_processes auto;
    events {
        worker_connections 1024;
    }
    http {
        include /etc/nginx/mime.types;
        default_type application/octet-stream;
        sendfile on;
        keepalive_timeout 65;
        server {
            listen {{ nginx_port | default(80) }};
            root {{ nginx_root | default('/var/www/html') }};
            index index.html index.htm;
        }
    }
    ```
  3. Trong `roles/webserver/tasks/main.yml`, thay thế task `copy` cấu hình Nginx bằng task `template`:
    ```yaml
    - name: Cấu hình Nginx bằng template
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify: Khởi động lại Nginx
    ```
  4. Định nghĩa `nginx_port` và `nginx_root` trong `roles/webserver/vars/main.yml` hoặc trong Playbook chính.
  5. Chạy Playbook và kiểm tra cấu hình Nginx trên Managed Nodes.

### Bài 3.3: Bảo vệ dữ liệu nhạy cảm với Ansible Vault
- **Mục tiêu:** Mã hóa và sử dụng dữ liệu nhạy cảm với Ansible Vault.
- **Các bước:**
  1. Tạo một file chứa mật khẩu database (ví dụ: `vault_passwords.yml`):
     `ansible-vault create vault_passwords.yml`
     Nhập mật khẩu cho vault và sau đó nhập nội dung:
     ```yaml
     db_password: my_secret_password
     ```
  2. Trong một Playbook hoặc Role, sử dụng biến `db_password`.
  3. Chạy Playbook và cung cấp mật khẩu vault:
     `ansible-playbook -i inventory.ini site.yml --ask-vault-pass`

## Tuần 4: Các chủ đề nâng cao và tổng kết

### Bài 4.1: Sử dụng Loops và Conditions
- **Mục tiêu:** Áp dụng Loops và Conditions để thực hiện các tác vụ phức tạp hơn.
- **Các bước:**
  1. Viết một Playbook để tạo nhiều user trên các Managed Nodes sử dụng `loop`:
    ```yaml
    ---
    - name: Tạo nhiều user
      hosts: all
      become: yes
      tasks:
        - name: Tạo user {{ item.name }}
          user:
            name: "{{ item.name }}"
            state: present
            shell: "{{ item.shell | default('/bin/bash') }}"
          loop:
            - { name: "devuser", shell: "/bin/bash" }
            - { name: "testuser", shell: "/bin/sh" }
            - { name: "adminuser" }
    ```
  2. Viết một Playbook để cài đặt gói chỉ trên các hệ điều hành cụ thể sử dụng `when`:
    ```yaml
    ---
    - name: Cài đặt gói theo hệ điều hành
      hosts: all
      become: yes
      tasks:
        - name: Cài đặt Apache trên Debian/Ubuntu
          apt: name=apache2 state=present
          when: ansible_distribution == 'Ubuntu' or ansible_distribution == 'Debian'

        - name: Cài đặt Httpd trên CentOS/RHEL
          yum: name=httpd state=present
          when: ansible_distribution == 'CentOS' or ansible_distribution == 'RedHat'
    ```

### Bài 4.2: Khám phá Ansible Galaxy
- **Mục tiêu:** Tìm kiếm, cài đặt và sử dụng Roles từ Ansible Galaxy.
- **Các bước:**
  1. Truy cập [https://galaxy.ansible.com/](https://galaxy.ansible.com/) và tìm kiếm một Role bất kỳ (ví dụ: `geerlingguy.mysql`).
  2. Cài đặt Role đó trên Control Node:
     `ansible-galaxy install geerlingguy.mysql`
  3. Tạo một Playbook mới để sử dụng Role vừa cài đặt:
    ```yaml
    ---
    - name: Triển khai MySQL bằng Galaxy Role
      hosts: databases
      become: yes
      roles:
        - geerlingguy.mysql
    ```
  4. Đọc tài liệu của Role để biết cách cấu hình các biến cần thiết.
  5. Chạy Playbook và kiểm tra kết quả.

### Bài 4.3: Dự án cuối khóa - Triển khai ứng dụng Web đơn giản
- **Mục tiêu:** Tổng hợp kiến thức để triển khai một ứng dụng web hoàn chỉnh.
- **Các bước:**
  1. Chọn một ứng dụng web đơn giản (ví dụ: một ứng dụng Flask/Node.js/PHP nhỏ) có sử dụng database (MySQL/PostgreSQL).
  2. Thiết kế cấu trúc dự án Ansible của bạn (sử dụng Roles).
  3. Viết các Roles cần thiết:
     - Role cho Webserver (Nginx/Apache).
     - Role cho Database (MySQL/PostgreSQL).
     - Role cho ứng dụng (cài đặt dependencies, copy code, cấu hình).
  4. Sử dụng Jinja2 Templates cho các file cấu hình ứng dụng và database.
  5. Sử dụng Ansible Vault để bảo vệ mật khẩu database.
  6. Viết Playbook chính để gọi các Roles theo đúng thứ tự.
  7. Chạy Playbook và kiểm tra ứng dụng.

**Lưu ý:** Các bài tập này yêu cầu bạn có kiến thức cơ bản về Linux và SSH. Hãy tìm kiếm thêm tài liệu nếu bạn gặp khó khăn ở bất kỳ bước nào.

