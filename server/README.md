# 201901
Bước 1: lấy các file cấu hình Docker theo đường dẫn
git clone https://github.com/joeymasip/docker-django.git
Bước 2: move tất cả file và thư mục vào thư mục gốc project muốn triển khai (cùng cấp với file manage.py)
"gồm: +  1 thư mục chứa Docker chứa thư mục con Django -> trong thư mục này chứa file requirements.txt - file này chứa các thư viện cần cài đặt và file cấu hình Dockerfile
        + file docker-compose.yml
        + file .env chứa 1 số thông tin cấu hình
"
Bước 3: chạy docker bên trong thư mục project
docker-compose up -d
-d để chạy các container ngầm
Bước 4: Cập nhật cấu hình database
mở file settings.py và thay đổi giá trị biến DATABASES như sau:
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'docker_django_db',
        'USER': 'dbuser',
        'PASSWORD': 'dbpw',
        'HOST': 'mysql',
        'PORT': '3306',
        'TEST': {
            'NAME': 'docker_django_db_test',
        },
    }
}
"(chú ý là user - password trong file cấu hình này phải có trong file docker-compose.yml
và giá trị của 'HOST' phải là tên 1 node csdl trong docker-compose.yml)"
Bước 5: thực hiện migrate dữ liệu
Để thực hiện các lệnh trong python như migrate, createsuperuser ... cần vào bash của django
Lệnh như sau:
docker-compose exec django bash
Sau đó thực hiện migrate
python manage.py migrate
Bước 6: quản lý CSDL với phpmyadmin
như file cấu hình bên, port chạy phpmyadmin là: 8082
Vào http://localhost:8082 - nếu máy nào cài docker trực tiếp
"còn không thì vào địa chỉ: http://[IP CUA MAY]:8082
xem địa chỉ của máy (cài docker-toolbox) bằng lệnh: docker-machine ip"
Đăng nhập vào phpmyadmin với các thông tin:
- server: IPcủa máy + Port container chứa MySQL - như file cấu hình PORT là 8306
- user và password như trong file là: root / docker_root
