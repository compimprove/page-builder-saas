<p><a target="_blank" href="https://app.eraser.io/workspace/aMz8oRWJzptFhYK6wvNx" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

## 1. Tổng quan (Overview)
### 1.1 Mục đích (Purpose)
Hệ thống Web Builder SaaS cho phép người dùng tạo website nhanh chóng dựa trên template có sẵn, không cần kiến thức kỹ thuật, đồng thời hỗ trợ mở rộng tính năng thông qua plugin và tích hợp dịch vụ bên thứ ba.

### 1.2 Đối tượng sử dụng (Target Users)
- **Khách hàng cuối:** tạo và quản trị website của riêng mình
- **Quản trị viên hệ thống (Admin):** quản lý nền tảng, template, plugin, gói dịch vụ
- **Đội vận hành nội bộ:** giám sát, bảo trì và hỗ trợ kỹ thuật
### 1.3 Mục tiêu & Phạm vi (Goals & Scope)
**Trong phạm vi:**

- Tạo website trong 1–3 phút từ template
- Quản trị nội dung và giao diện website
- Hệ thống plugin mở rộng
- Hỗ trợ thương mại điện tử cơ bản
- Vận hành ở quy mô ≥ 100.000 website

**Ngoài phạm vi:**

- Thiết kế website theo yêu cầu riêng lẻ
- Tính năng thương mại điện tử phức tạp (ERP, OMS nâng cao)
- Custom code cho từng khách hàng
---

## 2. Yêu cầu chức năng (Functional Requirements)
### 2.1. Tính năng cốt lõi (Core Features)
- Chọn và xem demo template theo ngành nghề
- Tạo website tự động (template, domain, SSL, tài khoản)
- Quản trị website: nội dung, giao diện, media, SEO
- Mở rộng tính năng thông qua plugin
- Back-office admin quản lý toàn bộ hệ thống
### 2.2. Vai trò & Phân quyền (User Roles & Permissions)
| Vai trò | Quyền hạn |
| ----- | ----- |
| Admin | Quản lý website, template, plugin, gói dịch vụ, thống kê |
| Customer | Tạo và quản trị website của mình |
| User | Người dùng duyệt website, đọc blog, mua hàng. |
### 2.3. Luồng xử lý chính (System Workflows – High Level)
- **Luồng customer:**
 Chọn template → đăng ký → tạo website → quản trị nội dung → cài plugin
- **Luồng admin:**
 Quản lý template → cấu hình plugin → quản lý gói dịch vụ → theo dõi hệ thống
- **Luồng user:
**Đăng nhập -> đọc blog -> xem sản phẩm -> mua hàng
- **Luồng lỗi / ngoại lệ:**
 Lỗi khởi tạo website → rollback → thông báo người dùng / ghi log
---

## 3. Yêu cầu phi chức năng (Non-Functional Requirements)
- **Hiệu năng:**
 Thời gian tạo website < 90 giây, website tải nhanh, tối ưu SEO
- **Khả năng mở rộng (Scalability):**
 Hỗ trợ ≥ 100.000 website, thiết kế multi-tenant
- **Tính sẵn sàng (Availability):**
 Uptime mục tiêu ≥ 99.9%
- **Bảo mật (Security):**
 Xác thực, phân quyền, bảo vệ dữ liệu tenant, SSL mặc định
- **Độ tin cậy (Reliability):**
 Chịu lỗi, retry, không ảnh hưởng website khách khi deploy
- **Khả năng bảo trì (Maintainability):**
 Logging tập trung, monitoring, backup & restore, dễ nâng cấp
- **Tuân thủ (Compliance):**
 Sẵn sàng đáp ứng GDPR và các tiêu chuẩn bảo mật phổ biến khi mở rộng
## 4. High-Level Architecture
- Microservice với kubernetes
- Ước tính quy mô 100k website dựa trên các platform tương tự:
    - **80k website thuộc blog/company/landing page**
        - ~60–70%: < 20 visits/ ngày
        - ~20–30%: 20–100 visits/ ngày
        - ~5–8%: 100–500 visits/ ngày
        - <2%: > 500 visits/ ngày

    - **20k website thuộc ecommerce.**
        - 50–60% → 50–200 visits/day
        - 25–30% → 200–800 visits/day
        - 8–12% → 800–2,000 visits/day
        - <3% → >2,000 visits/day
            - Add to cart rate: 8%



![Highlevel architecture](/.eraser/aMz8oRWJzptFhYK6wvNx___mwaSmXiQHibHjUS2HrVs23OrXqt2___---figure---u9Jt_OSEiyqAEHq6Hx5Zi---figure---bS86xj6PkxPG7OVnf7QB7Q.png "Highlevel architecture")

### 4.1 Phân tích flow khách hàng tạo website




### 4.2 Phân tích flow khách hàng truy cập trang web
![Truy cập trang web](/.eraser/aMz8oRWJzptFhYK6wvNx___mwaSmXiQHibHjUS2HrVs23OrXqt2___---figure---bUCGBixvfEvus1jwkHX_0---figure---uOLoyD8-8A_OEPr5S4QQJw.png "Truy cập trang web")



### 4.3 Phân tích flow user mua sản phẩm ở ecommerce site.
### 
## 5. Database Design
![Admin Model](/.eraser/aMz8oRWJzptFhYK6wvNx___mwaSmXiQHibHjUS2HrVs23OrXqt2___---diagram---ItKlQ1KfG7Jx_hYkpvxJU---diagram---dAMVNnsxZUnYN7S3MqK-hg.png "Admin Model")

![Content Model](/.eraser/aMz8oRWJzptFhYK6wvNx___mwaSmXiQHibHjUS2HrVs23OrXqt2___---diagram---DgtmEow8mxjAug5KKkmxw---diagram---4SYAVnLhHCATNTnmMIsZpQ.png "Content Model")




<!-- eraser-additional-content -->
## Diagrams
<!-- eraser-additional-files -->
<a href="/Page Builder-Model-1.eraserdiagram" data-element-id="b-4QIey4-0thJL13BR2fc"><img src="/.eraser/aMz8oRWJzptFhYK6wvNx___mwaSmXiQHibHjUS2HrVs23OrXqt2___---diagram----cf683a910f0b7c12ce13ea8735c3101d-Model.png" alt="" data-element-id="b-4QIey4-0thJL13BR2fc" /></a>
<a href="/Page Builder-Content Model-2.eraserdiagram" data-element-id="QHZH9nOkRYv7T_ydbaisc"><img src="/.eraser/aMz8oRWJzptFhYK6wvNx___mwaSmXiQHibHjUS2HrVs23OrXqt2___---diagram----ecab0b84e30b315bda264404ad37f57c-Content-Model.png" alt="" data-element-id="QHZH9nOkRYv7T_ydbaisc" /></a>
<a href="/Page Builder-Admin Model-3.eraserdiagram" data-element-id="69BAXwmTNL9YCYDToAUF6"><img src="/.eraser/aMz8oRWJzptFhYK6wvNx___mwaSmXiQHibHjUS2HrVs23OrXqt2___---diagram----5b0f8116370bbbc8343aaf057a76dc72-Admin-Model.png" alt="" data-element-id="69BAXwmTNL9YCYDToAUF6" /></a>
<!-- end-eraser-additional-files -->
<!-- end-eraser-additional-content -->
<!--- Eraser file: https://app.eraser.io/workspace/aMz8oRWJzptFhYK6wvNx --->