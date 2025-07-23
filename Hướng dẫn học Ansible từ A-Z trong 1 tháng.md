# Hướng dẫn học Ansible từ A-Z trong 1 tháng

## Lời giới thiệu

Chào mừng bạn đến với tài liệu hướng dẫn học Ansible toàn diện này! Ansible là một công cụ tự động hóa mạnh mẽ và linh hoạt, giúp đơn giản hóa đáng kể các tác vụ quản lý cấu hình, triển khai ứng dụng và tự động hóa quy trình IT. Dù bạn là một kỹ sư DevOps, quản trị viên hệ thống hay chỉ đơn giản là người muốn tìm hiểu về tự động hóa, Ansible đều là một kỹ năng vô cùng giá trị trong bối cảnh công nghệ hiện đại.

Tài liệu này được thiết kế đặc biệt cho người mới bắt đầu, cung cấp một lộ trình học tập rõ ràng trong vòng 1 tháng, kết hợp chặt chẽ giữa lý thuyết và thực hành. Mỗi tuần sẽ tập trung vào các chủ đề cụ thể, từ những khái niệm cơ bản nhất đến các kỹ thuật nâng cao, đảm bảo bạn có thể nắm vững Ansible và áp dụng vào công việc thực tế.

Chúng tôi tin rằng việc học đi đôi với hành là chìa khóa để thành công. Do đó, bên cạnh phần lý thuyết chi tiết, tài liệu còn bao gồm các bài tập thực hành cụ thể, giúp bạn củng cố kiến thức và xây dựng kinh nghiệm thực tế. Hãy chuẩn bị một môi trường lab (máy ảo hoặc cloud) và sẵn sàng bắt tay vào tự động hóa!

Chúc bạn có một hành trình học tập hiệu quả và thú vị với Ansible!

## 1. Ansible là gì? Tổng quan và các khái niệm cơ bản

Ansible là một công cụ tự động hóa mã nguồn mở được phát triển bởi Red Hat, được sử dụng rộng rãi trong lĩnh vực quản lý cấu hình, triển khai ứng dụng, và tự động hóa các tác vụ liên quan đến cơ sở hạ tầng IT. Điểm nổi bật của Ansible là sự đơn giản, dễ sử dụng và khả năng hoạt động theo cơ chế agentless (không cần cài đặt phần mềm đặc biệt trên các máy chủ được quản lý).

### 1.1. Tại sao nên sử dụng Ansible?

Trong môi trường IT hiện đại, việc quản lý hàng trăm, thậm chí hàng nghìn máy chủ, triển khai ứng dụng và duy trì cấu hình nhất quán là một thách thức lớn. Ansible ra đời để giải quyết những vấn đề này bằng cách cung cấp một giải pháp tự động hóa hiệu quả. Các lợi ích chính của Ansible bao gồm:

*   **Đơn giản và dễ học:** Ansible sử dụng cú pháp YAML (Yet Another Markup Language) để định nghĩa các tác vụ tự động hóa (gọi là Playbooks). YAML là một ngôn ngữ dễ đọc, gần gũi với ngôn ngữ tự nhiên, giúp người dùng nhanh chóng làm quen và viết các kịch bản tự động hóa mà không cần kiến thức lập trình sâu rộng.
*   **Agentless:** Đây là một trong những ưu điểm lớn nhất của Ansible. Không giống như nhiều công cụ quản lý cấu hình khác yêu cầu cài đặt một 


agent" trên các máy chủ, Ansible chỉ cần kết nối SSH (cho Linux/Unix) hoặc WinRM (cho Windows) để thực thi các tác vụ. Điều này giúp giảm thiểu tài nguyên tiêu thụ trên các máy chủ và đơn giản hóa việc triển khai.
*   **Mạnh mẽ và linh hoạt:** Ansible có thể tự động hóa một loạt các tác vụ, từ việc cung cấp tài nguyên trên đám mây, quản lý cấu hình hệ thống, triển khai ứng dụng đa tầng, đến việc tự động hóa mạng và bảo mật.
*   **Cộng đồng lớn và hệ sinh thái phong phú:** Ansible có một cộng đồng người dùng và nhà phát triển rất lớn. Điều này có nghĩa là có rất nhiều tài liệu, hướng dẫn, và các Roles (đơn vị mã tái sử dụng) có sẵn trên Ansible Galaxy, giúp bạn giải quyết các vấn đề phổ biến một cách nhanh chóng.

### 1.2. Kiến trúc và các thành phần chính

Kiến trúc của Ansible khá đơn giản và dễ hiểu, bao gồm các thành phần chính sau:

*   **Control Node:** Đây là máy chủ mà Ansible được cài đặt và từ đó bạn sẽ chạy các lệnh và Playbooks. Bất kỳ máy tính nào có cài đặt Python và Ansible đều có thể trở thành Control Node.
*   **Managed Nodes (hoặc Hosts):** Đây là các máy chủ (hoặc thiết bị mạng) mà Ansible quản lý. Ansible không yêu cầu cài đặt bất kỳ agent nào trên các máy chủ này.
*   **Inventory:** Là một file (thường ở định dạng INI hoặc YAML) chứa danh sách tất cả các Managed Nodes. Bạn có thể nhóm các máy chủ lại với nhau (ví dụ: `webservers`, `databases`) để dễ dàng quản lý và thực thi tác vụ trên một nhóm cụ thể.
*   **Modules:** Là các đơn vị mã nhỏ mà Ansible sử dụng để thực hiện các tác vụ cụ thể trên Managed Nodes. Mỗi module chịu trách nhiệm cho một công việc nhất định, ví dụ: `apt` để quản lý gói trên Ubuntu/Debian, `yum` cho CentOS/RHEL, `copy` để sao chép file, `service` để quản lý dịch vụ.
*   **Tasks:** Là một hành động cụ thể được thực hiện bởi một module. Ví dụ: một task có thể là "sử dụng module `apt` để cài đặt gói `nginx`".
*   **Playbooks:** Đây là trái tim của Ansible. Playbook là các file YAML định nghĩa một tập hợp các Plays. Mỗi Play sẽ ánh xạ một nhóm Managed Nodes tới một tập hợp các Tasks. Nói cách khác, Playbook là nơi bạn viết kịch bản cho quy trình tự động hóa của mình.
*   **Handlers:** Là các tác vụ đặc biệt, chỉ được kích hoạt khi một task khác thông báo rằng có sự thay đổi. Ví dụ phổ biến nhất là khởi động lại một dịch vụ sau khi file cấu hình của nó đã được cập nhật.
*   **Roles:** Là một cách để tổ chức các Playbooks, variables, templates, và các file khác thành một cấu trúc thư mục chuẩn. Roles giúp mã Ansible của bạn trở nên gọn gàng, dễ quản lý, tái sử dụng và chia sẻ.
*   **Facts:** Là thông tin về hệ thống của Managed Nodes mà Ansible tự động thu thập khi chạy một Playbook. Các facts này bao gồm thông tin về hệ điều hành, địa chỉ IP, bộ nhớ, CPU, v.v. và có thể được sử dụng trong Playbooks.
*   **Ansible Vault:** Là một tính năng cho phép bạn mã hóa các dữ liệu nhạy cảm như mật khẩu, khóa API trong các Playbooks của mình.

## 2. Lộ trình học tập chi tiết trong 1 tháng




Dưới đây là lộ trình học Ansible chi tiết trong 1 tháng, được chia thành các tuần, mỗi tuần tập trung vào các chủ đề cụ thể và đi kèm với các bài tập thực hành để củng cố kiến thức.

### Tuần 1: Giới thiệu và các khái niệm cơ bản

**Mục tiêu:** Hiểu rõ Ansible là gì, kiến trúc, các thành phần cơ bản và cách thiết lập môi trường.

**Lý thuyết:**

*   **Ngày 1-2:** Giới thiệu về Ansible, vai trò của nó trong tự động hóa IT. Tìm hiểu về kiến trúc Control Node, Managed Nodes và Inventory. Nắm vững các ưu điểm của Ansible so với các công cụ khác.
*   **Ngày 3-4:** Đi sâu vào các khái niệm cốt lõi: Modules, Tasks, Playbooks và Plays. Hiểu cách Ansible sử dụng SSH để giao tiếp với Managed Nodes. Hướng dẫn cài đặt Ansible trên Control Node.
*   **Ngày 5-7:** Tìm hiểu về Ad-hoc commands – cách thực thi các lệnh Ansible đơn lẻ để thực hiện các tác vụ nhanh chóng. Nắm vững cách cấu hình SSH Key-based authentication để Ansible có thể kết nối mà không cần mật khẩu.

**Thực hành:**

*   **Bài 1.1: Chuẩn bị môi trường**
    *   Cài đặt một máy ảo Linux (Ubuntu Server 22.04 LTS hoặc CentOS Stream 9) làm Control Node. Đảm bảo máy ảo có kết nối internet.
    *   Cài đặt Ansible trên Control Node theo hướng dẫn chính thức (ví dụ: `sudo apt update && sudo apt install ansible` cho Ubuntu).
    *   Tạo 2-3 máy ảo Linux khác (cùng hệ điều hành với Control Node hoặc khác) làm Managed Nodes. Đảm bảo các Managed Nodes có thể được truy cập từ Control Node qua mạng.
    *   Trên Control Node, tạo cặp khóa SSH (`ssh-keygen`).
    *   Sao chép khóa công khai SSH từ Control Node sang tất cả các Managed Nodes (`ssh-copy-id user@managed_node_ip`). Đảm bảo bạn có thể SSH vào Managed Nodes mà không cần mật khẩu.
    *   Tạo file `inventory.ini` trong thư mục làm việc của bạn với nội dung sau (thay thế IP và user của bạn):
        ```ini
        [webservers]
        web1 ansible_host=192.168.1.10 user=your_user
        web2 ansible_host=192.168.1.11 user=your_user

        [databases]
        db1 ansible_host=192.168.1.12 user=your_user
        ```

*   **Bài 1.2: Kiểm tra kết nối và Ad-hoc commands cơ bản**
    *   Kiểm tra kết nối đến tất cả các host trong inventory:
        `ansible all -i inventory.ini -m ping`
    *   Thực thi lệnh `uptime` trên nhóm `webservers`:
        `ansible webservers -i inventory.ini -m command -a "uptime"`
    *   Cài đặt gói `htop` trên nhóm `databases` (sử dụng module `apt` cho Ubuntu hoặc `yum` cho CentOS):
        `ansible databases -i inventory.ini -m apt -a "name=htop state=present"` (Ubuntu)
        `ansible databases -i inventory.ini -m yum -a "name=htop state=present"` (CentOS)
    *   Xóa gói `htop` vừa cài đặt:
        `ansible databases -i inventory.ini -m apt -a "name=htop state=absent"` (Ubuntu)
        `ansible databases -i inventory.ini -m yum -a "name=htop state=absent"` (CentOS)

### Tuần 2: Playbooks và Variables

**Mục tiêu:** Nắm vững cách viết Playbook để tự động hóa các tác vụ phức tạp và sử dụng Variables để làm cho Playbook linh hoạt hơn.

**Lý thuyết:**

*   **Ngày 8-9:** Đi sâu vào cấu trúc của Playbook: `hosts`, `become`, `tasks`, và các module. Bắt đầu viết Playbook đầu tiên để cài đặt và cấu hình một dịch vụ web (Nginx/Apache).
*   **Ngày 10-11:** Tìm hiểu về Variables – cách định nghĩa và sử dụng chúng trong Playbook để tùy chỉnh hành vi. Phân biệt các loại variables như Host variables, Group variables và Extra variables.
*   **Ngày 12-14:** Giới thiệu về Facts – thông tin tự động thu thập về Managed Nodes. Sử dụng module `debug` để kiểm tra variables và facts. Nắm vững khái niệm Handlers và cách chúng được kích hoạt khi có sự thay đổi.

**Thực hành:**

*   **Bài 2.1: Viết Playbook đầu tiên - Cài đặt Web Server**
    *   Tạo file `webserver_setup.yml` với nội dung sau (ví dụ cho Nginx trên Ubuntu):
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
    *   Chạy Playbook:
        `ansible-playbook -i inventory.ini webserver_setup.yml`
    *   Kiểm tra kết quả bằng cách truy cập IP của `web1` và `web2` trên trình duyệt.

*   **Bài 2.2: Sử dụng Variables và Facts**
    *   Sửa đổi `webserver_setup.yml` để sử dụng variables cho nội dung trang web và cổng lắng nghe (nếu có).
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
    *   Chạy Playbook và quan sát output của `debug` module.
    *   Chạy Playbook với extra variable để ghi đè `welcome_message`:
        `ansible-playbook -i inventory.ini webserver_setup.yml -e "welcome_message=\'Đây là thông điệp mới!\\'"`

*   **Bài 2.3: Sử dụng Handlers**
    *   Sửa đổi `webserver_setup.yml` để thêm một task thay đổi file cấu hình Nginx (ví dụ: `/etc/nginx/nginx.conf`) và một handler để khởi động lại Nginx.
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
    *   Chạy Playbook và quan sát khi nào handler được kích hoạt.

### Tuần 3: Roles và Templates

**Mục tiêu:** Tổ chức mã Ansible hiệu quả bằng Roles và tạo file cấu hình động bằng Jinja2 Templates. Bảo vệ dữ liệu nhạy cảm với Ansible Vault.

**Lý thuyết:**

*   **Ngày 15-16:** Giới thiệu về Roles – tại sao chúng lại quan trọng trong việc tổ chức dự án Ansible lớn. Nắm vững cấu trúc thư mục chuẩn của Roles (`tasks`, `handlers`, `templates`, `vars`, `defaults`, `files`).
*   **Ngày 17-18:** Thực hành viết Role đầu tiên để cài đặt và cấu hình một ứng dụng (ví dụ: MySQL). Học cách sử dụng Roles trong Playbook chính.
*   **Ngày 19-21:** Tìm hiểu về Jinja2 Templates – cách sử dụng chúng để tạo ra các file cấu hình động dựa trên variables và facts. Sử dụng module `template`. Giới thiệu về Ansible Vault để mã hóa và bảo vệ dữ liệu nhạy cảm.

**Thực hành:**

*   **Bài 3.1: Tạo và sử dụng Roles**
    *   Trên Control Node, tạo một thư mục dự án mới (ví dụ: `my_ansible_project`).
    *   Trong thư mục dự án, tạo một Role mới cho webserver:
        `ansible-galaxy init roles/webserver`
    *   Di chuyển các tasks từ `webserver_setup.yml` (Bài 2.1) vào file `roles/webserver/tasks/main.yml`.
    *   Tạo một Playbook chính (ví dụ: `site.yml`) trong thư mục dự án để gọi Role `webserver`:
        ```yaml
        ---
        - name: Triển khai Webserver bằng Role
          hosts: webservers
          become: yes
          roles:
            - webserver
        ```
    *   Chạy Playbook chính:
        `ansible-playbook -i inventory.ini site.yml`

*   **Bài 3.2: Sử dụng Jinja2 Templates**
    *   Trong Role `webserver`, tạo thư mục `templates` (`roles/webserver/templates`).
    *   Tạo file `nginx.conf.j2` trong thư mục `templates` với nội dung cấu hình Nginx sử dụng variables (ví dụ: port, root directory):
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
                root {{ nginx_root | default(\'/var/www/html\') }};
                index index.html index.htm;
            }
        }
        ```
    *   Trong `roles/webserver/tasks/main.yml`, thay thế task `copy` cấu hình Nginx bằng task `template`:
        ```yaml
        - name: Cấu hình Nginx bằng template
          template:
            src: nginx.conf.j2
            dest: /etc/nginx/nginx.conf
          notify: Khởi động lại Nginx
        ```
    *   Định nghĩa `nginx_port` và `nginx_root` trong `roles/webserver/vars/main.yml` hoặc trong Playbook chính.
    *   Chạy Playbook và kiểm tra cấu hình Nginx trên Managed Nodes.

*   **Bài 3.3: Bảo vệ dữ liệu nhạy cảm với Ansible Vault**
    *   Tạo một file chứa mật khẩu database (ví dụ: `vault_passwords.yml`):
        `ansible-vault create vault_passwords.yml`
        Nhập mật khẩu cho vault và sau đó nhập nội dung:
        ```yaml
        db_password: my_secret_password
        ```
    *   Trong một Playbook hoặc Role, sử dụng biến `db_password`.
    *   Chạy Playbook và cung cấp mật khẩu vault:
        `ansible-playbook -i inventory.ini site.yml --ask-vault-pass`

### Tuần 4: Các chủ đề nâng cao và tổng kết

**Mục tiêu:** Nâng cao kỹ năng Ansible với Loops, Conditions, khám phá Ansible Galaxy và thực hiện một dự án cuối khóa.

**Lý thuyết:**

*   **Ngày 22-23:** Tìm hiểu về Loops (`with_items`, `with_dict`, `with_sequence`) để lặp lại các tác vụ. Nắm vững cách sử dụng điều kiện (`when` statement) để kiểm soát việc thực thi task.
*   **Ngày 24-25:** Giới thiệu về Ansible Galaxy – kho lưu trữ các Roles có sẵn. Học cách tìm kiếm, cài đặt và sử dụng các Roles này. Tìm hiểu các best practices trong Ansible để xây dựng dự án hiệu quả và dễ bảo trì.
*   **Ngày 26-28:** Tổng kết toàn bộ kiến thức đã học. Thực hiện một dự án nhỏ cuối khóa để tổng hợp tất cả các kỹ năng. Giới thiệu về Ansible Tower/AWX (tùy chọn) để quản lý tự động hóa ở quy mô lớn.

**Thực hành:**

*   **Bài 4.1: Sử dụng Loops và Conditions**
    *   Viết một Playbook để tạo nhiều user trên các Managed Nodes sử dụng `loop`:
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
                shell: "{{ item.shell | default(\'/bin/bash\') }}"
              loop:
                - { name: "devuser", shell: "/bin/bash" }
                - { name: "testuser", shell: "/bin/sh" }
                - { name: "adminuser" }
        ```
    *   Viết một Playbook để cài đặt gói chỉ trên các hệ điều hành cụ thể sử dụng `when`:
        ```yaml
        ---
        - name: Cài đặt gói theo hệ điều hành
          hosts: all
          become: yes
          tasks:
            - name: Cài đặt Apache trên Debian/Ubuntu
              apt: name=apache2 state=present
              when: ansible_distribution == \'Ubuntu\' or ansible_distribution == \'Debian\'

            - name: Cài đặt Httpd trên CentOS/RHEL
              yum: name=httpd state=present
              when: ansible_distribution == \'CentOS\' or ansible_distribution == \'RedHat\'
        ```

*   **Bài 4.2: Khám phá Ansible Galaxy**
    *   Truy cập [https://galaxy.ansible.com/](https://galaxy.ansible.com/) và tìm kiếm một Role bất kỳ (ví dụ: `geerlingguy.mysql`).
    *   Cài đặt Role đó trên Control Node:
        `ansible-galaxy install geerlingguy.mysql`
    *   Tạo một Playbook mới để sử dụng Role vừa cài đặt:
        ```yaml
        ---
        - name: Triển khai MySQL bằng Galaxy Role
          hosts: databases
          become: yes
          roles:
            - geerlingguy.mysql
        ```
    *   Đọc tài liệu của Role để biết cách cấu hình các biến cần thiết.
    *   Chạy Playbook và kiểm tra kết quả.

*   **Bài 4.3: Dự án cuối khóa - Triển khai ứng dụng Web đơn giản**
    *   Chọn một ứng dụng web đơn giản (ví dụ: một ứng dụng Flask/Node.js/PHP nhỏ) có sử dụng database (MySQL/PostgreSQL).
    *   Thiết kế cấu trúc dự án Ansible của bạn (sử dụng Roles).
    *   Viết các Roles cần thiết:
        *   Role cho Webserver (Nginx/Apache).
        *   Role cho Database (MySQL/PostgreSQL).
        *   Role cho ứng dụng (cài đặt dependencies, copy code, cấu hình).
    *   Sử dụng Jinja2 Templates cho các file cấu hình ứng dụng và database.
    *   Sử dụng Ansible Vault để bảo vệ mật khẩu database.
    *   Viết Playbook chính để gọi các Roles theo đúng thứ tự.
    *   Chạy Playbook và kiểm tra ứng dụng.

**Lưu ý:** Các bài tập này yêu cầu bạn có kiến thức cơ bản về Linux và SSH. Hãy tìm kiếm thêm tài liệu nếu bạn gặp khó khăn ở bất kỳ bước nào.

## 3. Tài nguyên học tập bổ sung

Để hỗ trợ quá trình học tập của bạn, dưới đây là một số tài nguyên hữu ích:

*   **Tài liệu chính thức của Ansible:** [https://docs.ansible.com/](https://docs.ansible.com/) – Nguồn thông tin đáng tin cậy và đầy đủ nhất về Ansible.
*   **Viblo:** Các bài viết về Ansible (ví dụ: series "Tìm hiểu về Ansible") – Cung cấp các bài viết tiếng Việt chất lượng, dễ hiểu.
*   **Red Hat:** Các tài liệu và hướng dẫn về Ansible Automation Platform – Cung cấp cái nhìn sâu sắc về Ansible trong môi trường doanh nghiệp.
*   **YouTube:** Có rất nhiều kênh hướng dẫn Ansible cho người mới bắt đầu, ví dụ như kênh của TechWorld with Nana, freeCodeCamp.org.
*   **Udemy/Coursera:** Các khóa học trực tuyến về Ansible từ cơ bản đến nâng cao, giúp bạn có lộ trình học tập bài bản và được hướng dẫn bởi các chuyên gia.

## Kết luận

Việc nắm vững Ansible sẽ mở ra nhiều cơ hội trong sự nghiệp IT của bạn, đặc biệt trong các lĩnh vực DevOps, quản trị hệ thống và tự động hóa. Với lộ trình học tập và các bài tập thực hành được cung cấp trong tài liệu này, bạn đã có một nền tảng vững chắc để bắt đầu hành trình chinh phục Ansible. Hãy kiên trì, thực hành thường xuyên và đừng ngần ngại khám phá thêm các tính năng mạnh mẽ của công cụ này. Chúc bạn thành công!

