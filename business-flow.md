# **EVN SYSTEMS \-** 

# **TÀI LIỆU PHÂN TÍCH YÊU CẦU NGHIỆP VỤ & KỸ THUẬT (BRD/SRS)**

**Dự án:** Hệ thống EVN systems & API Cung cấp Thông tin Sản lượng Điện tiêu thụ Khách hàng

**Khối ngành:** Năng lượng / Điện lực & Tích hợp Dịch vụ Công Quốc gia / Thị trường Điện

**Vai trò:** Senior Business Analyst (BA)

**Phiên bản:** 4.0

**Ngày cập nhật:** 22/07/2026

## **1\. TỔNG QUAN DỰ ÁN (PROJECT OVERVIEW)**

### **1.1. Bối cảnh & Mục tiêu (Context & Objectives)**

Trong bối cảnh chuyển đổi số quốc gia và hiện đại hóa ngành điện lực, việc quản lý, dự báo và chia sẻ dữ liệu **sản lượng điện tiêu thụ của khách hàng** đóng vai trò nòng cốt. Hệ thống được xây dựng nhằm thu thập, chuẩn hóa dữ liệu từ các hệ thống đo đếm thông minh (AMR/MDMS), Billing và Core Điện lực, từ đó cung cấp các cổng giao tiếp API bảo mật cho ba nhóm đối tác bên ngoài chiến lược:

1. **Khách hàng dùng điện cá nhân (Tích hợp qua Ứng dụng VNeID \- C06 Bộ Công an):** Cho phép công dân tra cứu minh bạch chỉ số sản lượng điện tiêu thụ hàng ngày, hàng tháng trực tiếp trên ứng dụng Định danh điện tử Quốc gia VNeID.  
2. **Cơ quan Quản lý Nhà nước (Government Agencies \- Bộ Công Thương, Sở Công Thương, Tổng cục Thống kê, Ủy ban Nhân dân Tỉnh/Thành phố):** Cung cấp bức tranh toàn cảnh về tổng sản lượng điện tiêu thụ theo từng cấp hành chính (Toàn quốc, Vùng kinh tế, Tỉnh/Thành phố) kết hợp với **Mô hình Dự báo Sản lượng (AI/ML Load Forecasting)** trong ngắn, trung và dài hạn (Tháng, Quý, Năm) phục vụ quy hoạch an ninh năng lượng và phát triển kinh tế.  
3. **Đơn vị Mua bán điện & Điều độ Hệ thống điện (Power Trading & System Dispatch \- EPTC / NSMO / A0):** Cung cấp dữ liệu sản lượng tiêu thụ thực tế theo chu kỳ (Hourly/Real-time) và biểu đồ phụ tải dự báo (Load Curve Forecast) theo thời gian thực và ngày tới (Day-ahead/Week-ahead) để đơn vị Mua bán điện chủ động điều phối các nguồn cung cấp (Thủy điện, Nhiệt điện, Năng lượng tái tạo, Nhập khẩu), chào giá thị trường điện và đảm bảo công suất phát lên lưới cân bằng chính xác với nhu cầu tiêu thụ.

**Mục tiêu của hệ thống:**

* **Chuẩn hóa & Tập trung hóa (Centralization):** Hợp nhất dữ liệu sản lượng từ các Tổng công ty Điện lực (EVN miền Bắc, Trung, Nam, Hà Nội, TP.HCM) về Data Lake / OLAP chung.  
* **Cung cấp API Tích hợp liên thông (Multi-Party API Integration):** Xây dựng các chuẩn API bảo mật cấp quốc gia (mTLS, Encrypted Payload, Token Exchange VNeID, High-performance Streaming API for Power Trading) đáp ứng hàng triệu lượt tra cứu mỗi ngày.  
* **Trí tuệ nhân tạo & Dự báo phụ tải (AI-driven Load Forecasting):** Tích hợp Pipeline dự báo phụ tải điện tiêu thụ dựa trên dữ liệu chuỗi thời gian (Time-series ML models) phục vụ điều độ thị trường điện và hoạch định chính sách.

### **1.2. Phạm vi dự án (Scope)**

* **In-Scope:**  
  * Pipeline thu thập & tổng hợp dữ liệu sản lượng đo đếm điện (Data Ingestion từ MDMS/AMR).  
  * Mô hình Định danh & Ánh ánh tài khoản: Ánh xạ giữa Số Định danh cá nhân (CCCD) từ VNeID với Mã khách hàng dùng điện (Mã PE/PB).  
  * Bộ API Tra cứu Sản lượng Điện dành cho Khách hàng trên VNeID (Daily & Monthly).  
  * Bộ API Báo cáo Tổng hợp & Dự báo Sản lượng dành cho Cơ quan Nhà nước (Government Portal API).  
  * **Bộ API Truyền nhận Sản lượng Thực tế & Biểu đồ Dự báo Phụ tải dành cho Đơn vị Mua bán điện (Power Trading & Grid Balance API).**  
  * Engine Dự báo Sản lượng bằng AI/ML (Time-series Forecasting: Hourly Load Profile, Day-Ahead, Month, Quarter, Year).  
  * Cơ chế bảo mật quốc gia & chuyên ngành: mTLS, Data Masking PII, Mã hóa AES-256/TLS 1.3, Rate Limiting nâng cao, High-Availability Streaming.  
* **Out-of-Scope:**  
  * Thu tiền điện hoặc thanh toán trực tiếp trên VNeID (Sẽ phát triển ở giai đoạn tiếp theo).  
  * Thay đổi hạ tầng phần cứng của các công tơ điện tử ngoài hiện trường.  
  * Trực tiếp điều khiển đóng ngắt hoặc sa thải phụ tải tự động tại các trạm biến áp.

## **2\. STAKEHOLDERS & USER PERSONAS**

| Nhóm Nhân sự / Hệ thống | Vai trò / Tổ chức | Nhu cầu & Yêu cầu trọng tâm |
| :---- | :---- | :---- |
| **Công dân / Khách hàng dùng điện** | Người dân truy cập qua App VNeID | Tra cứu minh bạch sản lượng điện hàng ngày/tháng, nhận cảnh báo khi sản lượng tăng đột biến, giao diện trực quan. |
| **VNeID Integration Gateway** | C06 \- Bộ Công an | Yêu cầu API chuẩn hóa cao, bảo mật mTLS, phản hồi nhanh (![][image1]), tuân thủ Luật Bảo vệ dữ liệu cá nhân. |
| **Cơ quan Quản lý Nhà nước** | Bộ Công Thương, Sở Công Thương các Tỉnh, Tổng cục Thống kê | Cần dữ liệu tổng sản lượng điện tiêu thụ chính xác theo Tỉnh/Vùng/Toàn quốc; Báo cáo dự báo phụ tải điện (Tháng, Quý, Năm) để lập quy hoạch năng lượng. |
| **Đơn vị Mua bán điện & Điều độ** | Công ty Mua bán điện (EPTC), Trung tâm Điều độ Hệ thống điện (NSMO) | Cần dữ liệu sản lượng tiêu thụ thực tế theo từng giờ (Hourly) và Biểu đồ Phụ tải Dự báo Ngày tới/Tuần tới để tính toán lập lịch huy động nguồn phát, cân bằng lưới điện và thanh toán thị trường điện. |
| **BI & Data Science Team** | Nội bộ Điện lực / Trung tâm Điều độ | Vận hành mô hình AI/ML dự báo sản lượng, kiểm soát sai số dự báo (MAPE), giám sát luồng dữ liệu realtime. |
| **System Admin / Security Team** | Đơn vị vận hành hệ thống | Quản lý API Gateway, phân quyền dữ liệu theo phạm vi địa lý (Data Scope), mã hóa PII, giám sát DDoS và Rate Limit. |

## **3\. KIẾN TRÚC LUỒNG DỮ LIỆU & TÍCH HỢP (SYSTEM ARCHITECTURE)**

\[Hệ thống Công tơ Đo đếm AMR / MDMS / Billing Điện lực\]  
                         │  
                         ▼ (Kafka Event Stream / Hourly Batch ETL)  
\[Data Lake & Aggregation Engine (ClickHouse / Enterprise OLAP)\]  
                         │  
                         ├─────────────────────────────────────────┐  
                         ▼                                         ▼  
           \[Real-time / Historical Data\]               \[AI/ML Forecasting Engine\]  
                         │                             (Prophet / LSTM / XGBoost)  
                         │                                         │  
                         ├─────────────────────────────────────────┘  
                         ▼  
                \[Redis Cluster Cache\]  
                         │  
                         ▼  
        ┌────────────────────────────────┐  
        │       API GATEWAY LAYER        │  
        │ \- mTLS Auth & OAuth2 Server    │  
        │ \- Citizen-to-Customer Mapping  │  
        │ \- Rate Limiting & Audit Log    │  
        └──────────────┬─────────────────┘  
                       │  
         ┌─────────────┼──────────────────────────────┐  
         ▼             ▼                              ▼  
\[VNeID Gateway\]  \[Government Portal\]   \[Power Trading Platform (EPTC/NSMO)\]  
(Khách hàng)     (Bộ/Sở Công Thương)    (Điều phối nguồn phát & Thị trường)

## **4\. YÊU CẦU CHỨC NĂNG CHI TIẾT (FUNCTIONAL REQUIREMENTS \- FR)**

### **4.1. Module Tích hợp VNeID & Định danh Khách hàng (VNeID Citizen Mapping)**

* **FR-01 (Ánh xạ Định danh Công dân):** Hệ thống phải phối hợp với VNeID Gateway để ánh xạ từ Citizen\_ID (Số CCCD) sang danh sách các Customer\_Code (Mã số KH dùng điện PE/PB) mà công dân đó đứng tên chính chủ hoặc được ủy quyền.  
* **FR-02 (Tra cứu Sản lượng Ngày & Tháng):** Trả về chỉ số công tơ đầu kỳ, cuối kỳ, tổng điện năng tiêu thụ (kWh) theo từng ngày trong tháng hoặc theo các kỳ hóa đơn tháng lịch sử.  
* **FR-03 (Cảnh báo Sản lượng Đột biến):** Cho phép tính toán mức tiêu thụ trung bình và gắn flag cảnh báo nếu sản lượng điện tiêu thụ trong ngày vượt ![][image2] so với trung bình 7 ngày trước đó.

### **4.2. Module Báo cáo & Dự báo Sản lượng cho Cơ quan Nhà nước (Gov Analytics & AI Forecast)**

* **FR-04 (Tổng hợp Sản lượng Hành chính):** Cung cấp số liệu tổng sản lượng điện tiêu thụ (kWh/MWh/GWh) được phân rã theo Cấp hành chính (Toàn quốc, Vùng kinh tế, Tỉnh/Thành phố) và Nhóm mục đích sử dụng (Sinh hoạt, Sản xuất công nghiệp, Thương mại, Hành chính).  
* **FR-05 (Mô hình Dự báo AI/ML Sản lượng):** Tích hợp Pipeline dự báo tự động sản lượng điện cho Tháng tiếp theo (![][image3]), Quý tiếp theo (![][image4]), và Năm tiếp theo (![][image5]).  
* **FR-06 (Phân quyền theo Thẩm quyền Hành chính \- Data Scope Isolation):** Đảm bảo tính bảo mật và phân quyền truy cập dữ liệu địa lý theo quy định nhà nước.

### **4.3. Module Kết nối Mua bán điện & Cân bằng Lưới điện (Power Trading & Grid Balancing)**

* **FR-07 (Đồng bộ Biểu đồ Phụ tải Thực tế Theo giờ \- Hourly Load Profile Integration):** Hệ thống phải tổng hợp và cung cấp biểu đồ sản lượng điện tiêu thụ thực tế theo từng chu kỳ giờ (![][image6]) được gom nhóm theo Nút phụ tải, Trạm biến áp 110kV/220kV, hoặc Vùng thị trường điện để gửi cho đơn vị Mua bán điện.  
* **FR-08 (Cung cấp Biểu đồ Dự báo Phụ tải Ngắn hạn Day-Ahead / Week-Ahead):** Hệ thống sử dụng mô hình AI Time-Series để dự báo công suất/sản lượng điện theo từng khung 30 phút hoặc 1 giờ cho ngày tiếp theo (![][image7]) và tuần tiếp theo (![][image8]). Kết quả dự báo cung cấp thông tin phụ tải đỉnh (![][image9]), phụ tải đáy (![][image10]) và các khung giờ cao điểm/thấp điểm (![][image11]) để bên Mua bán điện chào giá và lập lịch huy động nhà máy điện (Thủy điện, Nhiệt điện, Điện gió, Mặt trời).  
* **FR-09 (Cảnh báo Lệch phụ tải & Biến động Nguồn cung):** Tự động phát hiện và cảnh báo độ lệch giữa sản lượng dự báo (![][image12]) và sản lượng tiêu thụ thực tế (![][image13]) nếu vượt ngưỡng sai số cho phép (![][image14]), giúp đơn vị Mua bán điện và Điều độ kịp thời kích hoạt các nguồn điện dự phòng quay (Spinning Reserve) hoặc điều chỉnh công suất lên lưới.

## **5\. BẢNG DANH SÁCH API (API INVENTORY & SUMMARY)**

Bảng tổng hợp chi tiết toàn bộ các API đối ngoại do Hệ thống Sản lượng Điện lực (EVN systems) cung cấp cho các bên liên quan:

| STT | Tên API & Endpoint | Mục đích Nghiệp vụ | Hệ thống Cung cấp API | Cấu trúc Request / Response | Performance (SLA/TPS) | Cơ chế Bảo mật (Security) |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **1** | **VNeID Power Consumption API** POST /api/v1/external/vneid/power-consumption | Trả về chỉ số công tơ, sản lượng tiêu thụ điện theo ngày/tháng cho công dân tra cứu trên App VNeID, cảnh báo tăng đột biến. | **API Gateway** ![][image15] **Identity Mapping Service** ![][image15] **AMR/MDMS DB** | **Req:** citizen\_id, customer\_code, period\_type, from\_date, to\_date **Res:** Chỉ số đầu/cuối, consumption\_kwh, is\_unusual\_spike, PII Masked. | Latency: ![][image1] (![][image16] reqs) Throughput: ![][image17] | • mTLS (Mutual TLS) • RSA SHA-256 Signature • PII Data Masking (NĐ 13/2023) • Token Exchange OAuth2 |
| **2** | **Government Analytics & Forecast API** GET /api/v1/external/gov/volume-forecast | Cung cấp tổng sản lượng điện tiêu thụ lịch sử và sản lượng dự báo AI (Tháng, Quý, Năm) theo cấp hành chính (tỉnh thành, xã phường, vùng, miền) và ngành kinh tế. | **API Gateway** ![][image15] **ClickHouse Aggregator** ![][image15] **AI Forecast Engine** | **Req (Query):** region\_level, province\_code, sector\_type, time\_horizon **Res:** predicted\_volume\_mwh, Tỷ lệ YoY %, Interval 95%. | Latency: ![][image18] Throughput: ![][image19] | • OAuth 2.0 Client Credentials • Data Scope Isolation (Phân quyền địa lý) • IP Whitelisting |
| **3** | **Power Trading Load Profile & Forecast API** POST /api/v1/external/power-trading/load-profile-forecast | Đồng bộ số liệu tổng sản lượng thực tế 24h và số liệu dự báo sản lượng Day-Ahead (![][image7]) / Week-Ahead phục vụ điều độ & chào giá thị trường điện. | **API Gateway** ![][image15] **Real-time Load Aggregator** ![][image15] **Short-Term AI Forecast** | **Req:** trading\_partner\_id, grid\_zone\_code, substation\_code, target\_date **Res:** ![][image20], Biểu đồ 24h (MW), % Variance, Reserve rec. | Latency: ![][image21] Throughput: ![][image17] | • mTLS Kênh riêng • HMAC SHA-256 Verification • API Key \+ IP Whitelisting • Payload Encryption AES-256 |

## **6\. THIẾT KẾ CHI TIẾT API VÀ WORKFLOW DÀNH CHO ĐỐI TÁC BÊN NGOÀI**

### **6.1. API External 1: Tra cứu Sản lượng Điện Khách hàng trên Hệ thống VNeID**

#### **6.1.1. Thông tin kỹ thuật API**

* **Endpoint:** POST /api/v1/external/vneid/power-consumption  
* **Protocol:** HTTPS / TLS 1.3 với Authentication mTLS (Mutual TLS) giữa VNeID Gateway và Điện lực.  
* **Header:**  
  * X-VNeID-Signature: Chữ ký số RSA SHA-256 từ VNeID Gateway.  
  * Authorization: Bearer Token (JWT được cấp sau bước Handshake).  
* **Request Body (JSON):**

{  
  "citizen\_id": "001095\*\*\*\*\*\*",  
  "vneid\_access\_token": "eyJhbGciOiJSUzI1NiIs...",  
  "period\_type": "DAILY",  
  "from\_date": "2026-07-01",  
  "to\_date": "2026-07-22",  
  "customer\_code": "PA01VD1234567"  
}

* **Response Body thành công (Status 200 OK):**

{  
  "success": true,  
  "data": {  
    "citizen\_id": "001095\*\*\*\*\*\*",  
    "customer\_code": "PA01VD1234567",  
    "customer\_name": "NGUYEN VAN A",  
    "address\_masked": "123 Đường \*\*\*, Phường \*\*\*, Quận Ba Đình, Hà Nội",  
    "meter\_id": "MET-8899201",  
    "total\_consumption\_kwh": 245.5,  
    "period\_type": "DAILY",  
    "daily\_records": \[  
      {  
        "date": "2026-07-21",  
        "start\_index": 12450.0,  
        "end\_index": 12462.5,  
        "consumption\_kwh": 12.5,  
        "is\_unusual\_spike": false  
      },  
      {  
        "date": "2026-07-22",  
        "start\_index": 12462.5,  
        "end\_index": 12481.0,  
        "consumption\_kwh": 18.5,  
        "is\_unusual\_spike": true,  
        "spike\_note": "Sản lượng tăng 54% so với trung bình 7 ngày"  
      }  
    \]  
  },  
  "timestamp": "2026-07-22T14:30:00Z"  
}

#### **6.1.2. Quy trình xử lý (Workflow Diagram \- VNeID Power Consumption API)**

\[Công dân trên VNeID App\]  
           │  
           │ (1) Chọn "Tra cứu Sản lượng Điện"  
           ▼  
\[VNeID Service Gateway\]  
           │  
           │ (2) mTLS \+ Signature \+ Encrypted Payload  
           ▼  
\[API Gateway Điện lực\] ───► \[Verify mTLS & Signature\] ──(Lỗi Signature/Token)──► \[Trả lỗi 401/403\]  
           │  
           ▼ (Hợp lệ)  
\[Identity Mapping Service\]  
           │  
           │ (3) Kiểm tra ánh xạ Citizen\_ID (CCCD) \<-\> Customer\_Code (PE/PB)  
           │  
           ├─► (Không tìm thấy mã KH / Chưa liên kết) ──► Trả mã lỗi 404 CUSTOMER\_NOT\_FOUND  
           │  
           ▼ (Khách hàng hợp lệ)  
\[Check Redis Cache\]  
           │  
           ├─► \[Cache Hit\] ──────────────────────────────────────────┐  
           │                                                         │  
           ▼ \[Cache Miss\]                                            │  
\[AMR / MDMS Metering Database\]                                       │  
           │                                                         │  
           │ (4) Query chỉ số công tơ & sản lượng tiêu thụ           │  
           ▼                                                         │  
\[Business Logic & Data Masking\]                                     │  
           │ (5) Áp dụng thuật toán phát hiện sản lượng bất thường   │  
           │ (6) Che giấu PII (Tên, Địa chỉ chi tiết)                │  
           │ (7) Ghi Cache (TTL \= 30 phút)                           │  
           │                                                         │  
           └─────────────────────────┬───────────────────────────────┘  
                                     ▼  
                      (8) Response 200 OK \-\> VNeID App

#### **6.1.3. Mô tả các bước xử lý nghiệp vụ VNeID**

1. **Xác thực Liên thông (mTLS & Signature Verification):** Request từ VNeID Gateway được truyền qua kết nối kênh riêng mTLS. API Gateway kiểm tra chứng thư số và Chữ ký số X-VNeID-Signature.  
2. **Xác thực Chủ thể Định danh (Identity Mapping):** Hệ thống gọi Identity Mapping Service để xác nhận số citizen\_id (CCCD) có quyền sở hữu/sử dụng customer\_code yêu cầu.  
3. **Kiểm tra Caching:** Tra cứu Redis Cache dựa trên Key vneid:vol:{customer\_code}:{period\_type}:{from\_date}:{to\_date}.  
4. **Truy vấn Dữ liệu Đo đếm (Metering Engine):** Trường hợp Cache Miss, gọi DB OLAP lấy chỉ số đầu/cuối của công tơ điện tử AMR theo từng ngày.  
5. **Phân tích Đột biến & Che giấu Thông tin PII:**  
   * Thuật toán so sánh sản lượng ngày ![][image22] với trung bình ![][image23]: Nếu ![][image24], đánh dấu is\_unusual\_spike \= true.  
   * Thực hiện Masking thông tin cá nhân theo quy định.  
6. **Phản hồi & Cập nhật Cache:** Trả kết quả JSON về VNeID Gateway và lưu Cache với TTL 30 phút.

### **6.2. API External 2: Tổng hợp & Dự báo Sản lượng Điện cho Cơ quan Nhà nước (Gov Portal API)**

#### **6.2.1. Thông tin kỹ thuật API**

* **Endpoint:** GET /api/v1/external/gov/volume-forecast  
* **Protocol:** HTTPS / TLS 1.3 \- OAuth 2.0 Client Credentials with Scope Level.  
* **Query Parameters:**  
  * region\_level (Required): NATIONWIDE | ECONOMIC\_REGION | PROVINCE | DISTRICT.  
  * province\_code (Optional): Mã tỉnh/thành phố (ví dụ: HAN \- Hà Nội, SGN \- TP.HCM).  
  * sector\_type (Optional): ALL | INDUSTRIAL | RESIDENTIAL | COMMERCIAL.  
  * data\_type (Required): HISTORICAL | FORECAST.  
  * time\_horizon (Required): NEXT\_MONTH | NEXT\_QUARTER | NEXT\_YEAR.  
* **Response Body thành công (Status 200 OK):**

{  
  "success": true,  
  "data": {  
    "region\_level": "PROVINCE",  
    "province\_code": "HAN",  
    "province\_name": "Thành phố Hà Nội",  
    "sector\_type": "INDUSTRIAL",  
    "data\_type": "FORECAST",  
    "time\_horizon": "NEXT\_QUARTER",  
    "forecast\_period": "Q4-2026",  
    "model\_info": {  
      "model\_name": "Ensemble Time-Series (LSTM \+ XGBoost)",  
      "mape\_accuracy": "2.15%",  
      "last\_updated": "2026-07-20"  
    },  
    "forecast\_summary": {  
      "predicted\_volume\_mwh": 1850000.0,  
      "growth\_rate\_compared\_to\_last\_year": 8.45,  
      "confidence\_interval\_95": {  
        "lower\_bound\_mwh": 1794500.0,  
        "upper\_bound\_mwh": 1905500.0  
      }  
    },  
    "monthly\_breakdown": \[  
      { "month": "2026-10", "predicted\_volume\_mwh": 610000.0 },  
      { "month": "2026-11", "predicted\_volume\_mwh": 600000.0 },  
      { "month": "2026-12", "predicted\_volume\_mwh": 640000.0 }  
    \]  
  },  
  "timestamp": "2026-07-22T14:35:00Z"  
}

#### **6.2.2. Quy trình xử lý (Workflow Diagram \- Gov Portal Forecast API)**

\[Cổng kết nối Cơ quan Nhà nước (Gov Portal)\]  
                   │  
                   │ (1) Request GET /volume-forecast kèm OAuth2 Token  
                   ▼  
         \[API Gateway System\]  
                   │  
                   │ (2) Validate OAuth2 & Phân quyền Khoanh vùng Dữ liệu (Data Scope Guard)  
                   │  
                   ├─► (Không đúng Scope địa lý) ──► Trả mã lỗi 403 PERMISSION\_DENIED  
                   │  
                   ▼ (Hợp lệ Scope)  
         \[Router Theo Data Type\]  
                   │  
        ┌──────────┴────────────────────────┐  
        ▼                                   ▼  
 \[data\_type \= HISTORICAL\]            \[data\_type \= FORECAST\]  
        │                                   │  
        │ (3a) Query ClickHouse             │ (3b) Fetch Model Inference Store  
        │ Aggregated Data Store             │ (Kết quả AI/ML đã huấn luyện)  
        │                                   │  
        └──────────┬────────────────────────┘  
                   │  
                   ▼  
     \[Merge Data & Compute Metrics\]  
     \- Tính Tỷ lệ Tăng trưởng YoY (%)  
     \- Đóng gói Khoảng tin cậy Confidence Interval (95%)  
                   │  
                   ▼  
     \[Return Response 200 OK \-\> Gov Portal\]

### **6.3. API External 3: Đồng bộ Sản lượng & Biểu đồ Dự báo Phụ tải cho Bên Mua bán điện (Power Trading API)**

#### **6.3.1. Thông tin kỹ thuật API**

* **Endpoint:** POST /api/v1/external/power-trading/load-profile-forecast  
* **Protocol:** HTTPS / TLS 1.3 \- mTLS \+ API Key Authentication với mã hóa HMAC SHA-256 Payload.  
* **Purpose:** Cung cấp thông tin phụ tải tiêu thụ điện thực tế theo từng giờ và biểu đồ dự báo nhu cầu phụ tải (Load Profile Curve) phục vụ lập lịch huy động nguồn phát, cân bằng lưới điện và chào giá thị trường điện.  
* **Request Body (JSON):**

{  
  "trading\_partner\_id": "EPTC\_MARKET\_01",  
  "grid\_zone\_code": "ZONE\_NORTH\_01",  
  "substation\_code": "ST\_220KV\_DONGANH",  
  "forecast\_horizon": "DAY\_AHEAD",  
  "target\_date": "2026-07-23",  
  "include\_actual\_historical\_24h": true  
}

* **Response Body thành công (Status 200 OK):**

{  
  "success": true,  
  "data": {  
    "trading\_partner\_id": "EPTC\_MARKET\_01",  
    "grid\_zone\_code": "ZONE\_NORTH\_01",  
    "substation\_code": "ST\_220KV\_DONGANH",  
    "target\_date": "2026-07-23",  
    "forecast\_horizon": "DAY\_AHEAD",  
    "unit": "MW",  
    "load\_summary": {  
      "p\_max\_peak\_mw": 485.2,  
      "p\_max\_time": "14:00",  
      "p\_min\_offpeak\_mw": 210.5,  
      "p\_min\_time": "03:00",  
      "p\_avg\_mw": 348.6,  
      "total\_energy\_mwh": 8366.4,  
      "grid\_balance\_reserve\_recommendation\_mw": 35.0  
    },  
    "hourly\_load\_profile": \[  
      {  
        "hour\_slot": "01:00",  
        "tariff\_time\_zone": "OFF\_PEAK",  
        "predicted\_load\_mw": 220.0,  
        "historical\_actual\_mw": 215.2,  
        "variance\_percentage": 2.23  
      },  
      {  
        "hour\_slot": "14:00",  
        "tariff\_time\_zone": "PEAK",  
        "predicted\_load\_mw": 485.2,  
        "historical\_actual\_mw": 472.0,  
        "variance\_percentage": 2.80  
      }  
    \],  
    "ai\_model\_metadata": {  
      "model\_type": "Short-Term Load Forecasting (STLF \- Hybrid Transformer)",  
      "mape\_score": "1.82%",  
      "weather\_factors\_integrated": true  
    }  
  },  
  "timestamp": "2026-07-22T14:40:00Z"  
}

#### **6.3.2. Quy trình xử lý (Workflow Diagram \- Power Trading & Load Dispatch API)**

\[Hệ thống Mua bán điện / Điều độ (EPTC / NSMO)\]  
                       │  
                       │ (1) Gửi Yêu cầu Biểu đồ Phụ tải & Dự báo Day-Ahead/Week-Ahead  
                       ▼  
             \[API Gateway Chuyên dụng\]  
                       │  
                       │ (2) mTLS Auth & Kiểm tra HMAC Signature & Trading Quota  
                       │  
                       ├─► (Mã HMAC / Auth không đúng) ──► Trả mã lỗi 401 UNAUTHORIZED  
                       │  
                       ▼ (Hợp lệ)  
      \[Power Trading Grid & Forecast Engine\]  
                       │  
        ┌──────────────┴─────────────────────────────────┐  
        ▼                                                ▼  
 \[Lấy Sản lượng Thực tế (Actual)\]            \[Lấy Biểu đồ Dự báo (Forecast)\]  
 \- Truy vấn dữ liệu AMR/MDMS theo Giờ        \- Gọi AI STLF (Short-Term Load Forecast)  
 \- Tổng hợp theo Nút/Trạm biến áp           \- Tích hợp dữ liệu thời tiết & lịch nghỉ lễ  
        │                                                │  
        └──────────────┬─────────────────────────────────┘  
                       │  
                       ▼  
         \[Cân bằng & Tính toán Lệch phụ tải\]  
         \- Tính $P\_{\\text{max}}$, $P\_{\\text{min}}$, $P\_{\\text{avg}}$  
         \- So sánh $V\_{\\text{actual}}$ vs $V\_{\\text{forecast}}$ cho 24h qua  
         \- Gắn cờ cảnh báo nếu lệch công suất $\\ge \\pm 5\\%$  
                       │  
                       ▼  
         \[Return Response 200 OK \-\> EPTC System\]  
         (Hệ thống Mua bán điện sử dụng để điều phối nguồn phát & chào giá)

#### **6.3.3. Chi tiết các bước xử lý nghiệp vụ Mua Bán Điện & Điều phối Nguồn**

1. **Tiếp nhận & Xác thực Giao dịch Mua bán điện:** Hệ thống nhận request từ đơn vị Mua bán điện (EPTC/NSMO). Xác thực tính chính xác của Chữ ký HMAC và chứng thư mTLS kênh riêng.  
2. **Tổng hợp Dữ liệu Tiêu thụ Thực tế (Real-time/Hourly Aggregation):** Lấy sản lượng điện thực tế theo từng chu kỳ giờ từ các hệ thống đo đếm trạm biến áp và công tơ ranh giới mua bán điện.  
3. **Chạy Mô hình Dự báo Phụ tải Ngắn hạn (Short-Term Load Forecasting \- STLF):**  
   * Sử dụng thuật toán AI (Hybrid Transformer / Prophet / XGBoost) kết hợp yếu tố nhiệt độ, độ ẩm, ngày làm việc/ngày nghỉ để dự báo đường cong phụ tải (Load Profile Curve) cho 24 giờ của ngày tới (![][image7]).  
   * Xác định phụ tải cực đại (![][image9]) rơi vào khung giờ cao điểm và phụ tải cực tiểu (![][image10]) rơi vào giờ thấp điểm.  
4. **Tính toán Cân bằng Nguồn & Cảnh báo Dự phòng:**  
   * Hệ thống tự động tính toán mức công suất dự phòng cần thiết lên lưới (grid\_balance\_reserve\_recommendation\_mw).  
   * Nếu phát hiện sản lượng tiêu thụ thực tế chênh lệch quá ![][image25] so with lịch điều độ trước đó, hệ thống sẽ tự động bật cảnh báo để bên Mua bán điện điều chỉnh lịch huy động các tổ máy phát điện (Thủy điện tích năng, Gas turbine, Thủy điện mở van).  
5. **Đồng bộ về Hệ thống Mua bán điện:** Trả về JSON chuẩn hóa theo định dạng của Thị trường điện Việt Nam.

## **7\. YÊU CẦU PHI CHỨC NĂNG BỔ SUNG (NFR FOR EXTERNAL PARTNERS)**

### **7.1. Bảo mật Quốc gia & Tuân thủ Pháp lý (Security & Compliance)**

* **mTLS Architecture:** Bắt buộc áp dụng mTLS cho tất cả kênh tích hợp VNeID, Cổng Chính phủ và Hệ thống Mua bán điện.  
* **Tuân thủ Nghị định 13/2023/NĐ-CP (Bảo vệ dữ liệu cá nhân):** Không trả về thông tin PII không cần thiết. Ghi nhật ký vết Audit Log cho ![][image26] các truy vấn dữ liệu.  
* **Mã hóa dữ liệu:** Toàn bộ dữ liệu truyền nhận bắt buộc dùng mã hóa TLS 1.3 và HMAC SHA-256 verification. Dữ liệu lưu trữ trong CSDL áp dụng mã hóa AES-256 đối với các trường thông tin nhạy cảm.

### **7.2. Hiệu năng & Khả năng chịu tải (Performance & Scalability)**

* **Response Time:**  
  * VNeID API: ![][image1] cho ![][image16] requests.  
  * Gov Portal API: ![][image18].  
  * Power Trading Load Profile API: ![][image21] cho các truy vấn chuỗi thời gian lớn.  
* **Throughput Chịu tải Peak Load:**  
  * VNeID Integration Gateway: Tối thiểu ![][image27].  
  * Gov Analytics API: Tối thiểu ![][image28].  
  * Power Trading API: Tối thiểu ![][image29].  
* **Availability SLA:** Cam kết ![][image30] Uptime đối với hệ thống API Gateway tích hợp đối tác.

## **8\. CƠ CHẾ XỬ LÝ LỖI DÀNH CHO ĐỐI TÁC BÊN NGOÀI (ERROR CODES)**

| HTTP Code | Error Code | Mô tả chi tiết | Biện pháp xử lý đề xuất cho Partner |
| :---- | :---- | :---- | :---- |
| **400 Bad Request** | INVALID\_REGION\_CODE | Mã tỉnh/thành phố hoặc mã vùng không nằm trong danh mục hệ thống. | Kiểm tra lại danh mục mã ISO/Hành chính chuẩn. |
| **401 Unauthorized** | INVALID\_MTLS\_SIGNATURE | Chữ ký số RSA, HMAC Signature hoặc chứng thư mTLS không hợp lệ. | Kiểm tra lại Chữ ký số và SSL Client Certificate. |
| **403 Forbidden** | GEO\_SCOPE\_VIOLATION | Tài khoản đối tác không có quyền truy cập dữ liệu của địa phương được yêu cầu. | Yêu cầu nâng quyền Data Scope trong API Key/OAuth2. |
| **404 Not Found** | CITIZEN\_NOT\_MAPPED | Số CCCD chưa được liên kết với bất kỳ mã khách hàng dùng điện nào. | Hướng dẫn người dùng đăng ký liên kết chủ thể hợp đồng điện trên VNeID. |
| **422 Unprocessable** | GRID\_LOAD\_DISPATCH\_MISMATCH | Yêu cầu biểu đồ phụ tải cho nút mạng/trạm biến áp không tồn tại. | Kiểm tra lại danh mục mã Nút/Trạm biến áp trên Thị trường điện. |
| **429 Too Many Requests** | PARTNER\_QUOTA\_EXCEEDED | Vượt quá ngưỡng Rate Limit dành cho cổng đối tác (ví dụ: ![][image31]). | Thực hiện Queueing request hoặc liên hệ tăng Quota. |
| **503 Service Unavailable** | FORECAST\_MODEL\_OFFLINE | Engine dự báo AI đang trong quá trình re-train hoặc bảo trì dữ liệu. | Hệ thống tự động chuyển sang trả dữ liệu dự báo cached gần nhất. |

## **9\. TIÊU CHÍ NGHIỆM THU BỔ SUNG (ACCEPTANCE CRITERIA \- AC)**

* **AC-05 (Kiểm thử Tích hợp VNeID):**  
  * Tra cứu từ ứng dụng VNeID thành công với ![][image26] các tài khoản đã được xác thực ánh xạ giữa CCCD và Mã khách hàng.  
  * Không để lộ thông tin địa chỉ chi tiết hoặc thông tin cá nhân chưa được Masking trong Response.  
* **AC-06 (Kiểm thử Phân quyền Địa lý Cơ quan Nhà nước):**  
  * Account thuộc Sở Công Thương Tỉnh A gọi API lấy dữ liệu Tỉnh B bắt buộc nhận mã lỗi 403 PERMISSION\_DENIED.  
  * Account thuộc Bộ Công Thương gọi API lấy dữ liệu Toàn quốc hoặc bất kỳ Tỉnh nào phải nhận kết quả thành công 200 OK.  
* **AC-07 (Độ chính xác Mô hình Dự báo AI):**  
  * Mô hình dự báo sản lượng tháng tiếp theo (![][image3]) cho các Tỉnh/Thành phố phải đạt chỉ số sai số tuyệt đối trung bình ![][image32].  
* **AC-08 (Kiểm thử Tích hợp Bên Mua Bán Điện & Điều Độ):**  
  * API trả về đầy đủ biểu đồ phụ tải 24 giờ (![][image33]) với độ trễ phản hồi ![][image21].  
  * Hệ thống cảnh báo chính xác khi độ lệch giữa phụ tải tiêu thụ thực tế và dự báo vượt quá ![][image25].

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE4AAAAZCAYAAACfIRhSAAADe0lEQVR4Xu1XS2xNURRVn4mBhAhJ9b37fpFUJUgHmhKfkBg0DDDxmxgRv4lIDAjqN1AhUQYqRIQQoomI+KWJkRIDEiNGitCEtoK22rTWet1bd3fvS99LK5q4K9m5Z639uefsd869940ZEyFChAgRLIq8kAuJROJBEAS9sBbYZu8nSkpKMvF4vFHiHnm/Ar4a2C/YF8Qv9P5RC0x4qSzusPeFgbHl5eUTOEYD10lus42Bvpi64XMtV0Brs/fFuB2xx23MqAN+3U2y6O3elwuIvYe8eqfdZZ1YLLbKaL1owFYXx131VDnqLPPNxC6d4rVRAyxorzRstfcNBeR0MxeLXqsaxmVS7zN5Op2eRs5rf2b/8VYujRzUJKm/0ev/DJjQGVgPFlDhffkCO2IGaly2Guotkca9IMei94c1BPpF1zjmdNkY1VHzlYw3YHwQ12vk+DFiGNfwtGh8cXHxVGgnqYOOV12B2F3wvYXd4iMDdsrHhAIJt2EdKJDyvpEA6t7nYnFUZwuvtw1SQKu1OsewbzbG6O0y3iec9lDXgHEzrAv8GBqxRTS+ZFj/z0sOvEXnRSC+EfHXlecEAnfITSu9b4QwTuo/VwETeyILGADop6lz15JL3lcfJ7pt8BVyrGWW0eZJ3IDdL3E7LceOnGhj8mqcAgV2y43WeN9wgHrtgRxRo121Czc6HxXUs8dJ5tPqwlTvUY5GXPL1wEup+ecoNTTmgOViDbAqE1oYkLxeCm3zvkKBGq8xyRtez/WMg3bB6jKPThtj9DeG1/l6/Eak5neT5B5Szs8m8B7Rs5ZKpWbanIKQTCYXsQgWedT78gFyb8KOOK2JV9RcwNp+NyQGv1WzC7ExqqPGOcPP+zg8t9LUhmpcJpOZpGOseU6uexYM+eU6YXXelwtY1J6E+0bDpKajRq1ymeCAzx3w74F5pmF8NmQRRdT0A5sIO6pD7Lhqy61fP5WsNixgcpNRsMHrHnH5aA0zNHOFieObttukZhuCmITRdHeVKYf7WeDetOB3GOe0Srlv0ul+t5KvVI76FUHIC+mvQ5sUZnCPdbEvYT+CvmPNBSy3fgI7NZD8x/C/w/WD9YO3wt7DmmCfYNWwDthH0Xj9GfQdZ/qpMb6N+VLzRCBz5A9j60cYaciXflU+xm3s8/9b8O8IGjI/H0PzSn1+hAgRIkSIEAH4DV+iW80aEicqAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADYAAAAZCAYAAAB6v90+AAACtklEQVR4Xu2Wz4uNYRTH7zXjZyRyF65773t/5dZdyRWalLKxoyZCkpoNNn5lSFhN2Mxq8iMpsSNbf4GxkLKxsFB2ipBm7gLDZHwO5zFnTi/e6V5S3m+dnnO+55znOed5nve5N5NJkaIjFAqFheVyeaPnPSTOc/8sSqXSVZp6hGyPomiy2WzO8zECfFMMvZ7vKlhkDBmq1WpFzF7ZbezHPk4Af1eKQp7m8/lFMX4pOOjnkDbyioZ3MO9W9Ieaf8bm/RHoQjOEIo7ZmFarNVf4arVaUqpHY1eGGHIOChdsAfawqllylyo3YUJmDy1m0PMe2shp5BZywPsFxIwiLxw3bBtBvxTT2KizX2Y6vYIUuSBhY5Oe85CCkSuO67ONsN4Ga9fr9flcwYvBxtcfdeMK5nK5xQkb++w5h3DtzlqyUqlEyvcHThubIzqN3s+Y04k6vYIBjUZjScLGJpDLyDg7fFuL7Qt+Clyj3HGbx4nkhCfnVOCKxWJNYx+Qdy3w2G8ynV7BgFk01pYXK9icxHopjt+Z5erfog0cns769rQv0yauW96DuffIN2zsQ5p33sbFgp1a54UJNpM84nkRn++hC7dF5zVbLTbzHbUxPPcrNG7I8h74Pxp9EHkrumwUc+6ejowBxW7zQtIukm96XsTmyutpbYEWHB6CrDb2Y9cF8runcXstb4HvHUOPsSV+rbWDnhhJriKN79PFZsS5xoId+ypG5rfMgo3Yj++E5SRevk1rW38iJGkM/4BMzve0yvFyQs+M/QX7iY1hU07+qjB872O4v9OYwE+Ofc9zNLHJc2IjI5YLgB9jyMbwU/Yb93MmQtLGzCMwruOHTExR4YQ4uTuMn5AbPkYAP1D+yT+Y8vc/x89FZ74jyE4f81skbazbYM3XnrOQ11A2iPGC96VIkSJFiv8OXwF3YN/Fw4i7lQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADoAAAAaCAYAAADmF08eAAABxElEQVR4Xu2WvUvDQBjGK7jpGjLU5gM3FRddnQUVN1cncROHTg7iJDjUP0IQBAVx8G+QLt0FcSi1imBF/EJUqM+rd/L62sRcog3C/eAhuffj7p5cAikULBbLv8HzvDXf9++htlJN1nCQr7Na6ivLmi7Qi3WfZTARbPNtmdOEYTiK/CrV4AGNyfxfg3WPk+wzFjSe+epkZU6DXAM6iqv5CfSuy5gpmOM21R7QNAUtQIdREyC+r67tqJokBEGwIWOmZDFaVddypwkcx+nHq7qkasjonqxJCnorMmZKFqPvTfTd0X2xWBwQ+Se64jQmKY+6IZ43Af2bMmZKFqNNdk9G5vUY98uu6/apXDXVAozcjNIpwcyiHtMEiG2x8edrSjmTBUql0rgU+rdlTEv2R5HKKJ2SGJOZhro/lzk8hF0eiwObn5XCQz2QMS3ZH0Vao18a9Klh4QlohMWnKY6NDvN6U/y8Xl00XIrxlTJ7IuI148k7kIfRHhSfQnUexHin0yTK/Le4Kb9k9DHRXlBUgW6gFnQHveocXs0ZaI7VPrBaepIv+E5XdN6ULEbVXi/8jz80UhO6xic2KGtzJ4tRi8VisVgs3eMNz1aufgu0NVYAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADQAAAAaCAYAAAD43n+tAAAB4klEQVR4Xu2WPUvDUBSGa7uIoC6iEmhvv6COgu4ibg7iIDj5A1T8Av+Au4ODgpOrSH+BqKuDgrsuDrr4gSgqlgrV99ATiK8mMW0MFvLA4SbnvPfe8+ampIlETMz/IZfLmUwms22M2SwWi11cbxmy2ewGTHzAzIzcw1gf7h8RFdZGCfZfRW+znPciKUYQZ1wQtFbj/F+C/XYRVd1bYo41ruiEZ87b4MSWRINxjGt+YN4r54ISyBCE1zKB805w3P26aJlrfkRqCI2OqPiIa4zqHjjvR6SGIHoXMS6TXHMC45Oiw7jHNT+iNiRCz9dNwG9nXw1Nc82PEA3Nc/4LhUKhV4VvXGN+YxxmB9Pp9DCHrM85iXw+381ruCF746EucJ5JaaOnXHCCRgdUt8M1J6iPo9EJDuQrnNOweA031NAi57+hjXp+X1TjeTpemJBeOTzYZc5/A8Inu1lMaNfmDxBbWj9sxowQoqEVzv+Imigjbh05+btzLoZx2+aQB6ZZQ5Zl9WiP61xzBeJ7fQonGGtyjXd2yq5j0Q6nPgiNGtKHfGfqH/4rHW8QVdb6AmMXmDhq35v6STVEo4ZCBU1cyikhXmTEaQ2xJgApTkQOTmhNDUkcc70lkY9fqVTq5HxMTExMS/AJxqWl9ePPIusAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADQAAAAaCAYAAAD43n+tAAABbElEQVR4Xu2WvUoEMRSF15/GTqxnJjNMY2FprZ2lpU/jIwhiZ6uFD7C+wDYLW6y1CHYiosuuv6CVeiIZCWdnhs1NFMF8cAmce3MnJ5PsTqcTiUS8UEqdIN4QH1ZcUNkc5V8o/yvkeb6KZ9+yXgsKe2axu5zTZFl2iNwp6z9NURRbeO7I2sw7rqklSZKVahLn0HQD+hnrLmB+nzVXnAxp6gwZo8+2JgE9Bqy5IjF0rCfhrB5Y2tQbk4A+Q9ZccTYEFu23hPEdwwLViFCeR1YjMfR97BD3ZVmmnJcS0NCI9Vbwa7avJ2Lc4dws4M4tpWm6zoGe56xVwT2aMIbGrLeid0BPZH1WcP+WschtDvS8ZK0K7tGEMTRhvRUzSWyoCRXuyD2w3sbXFwF2ucsJXwIaemS9ERTvGUObnPMloKFX1qeAgSMUPiEmiLHeBRX4e01qCHdszazrGnFl4kYF+LP3QmroLzPPQiQSiUT+PZ8DPYRsbmF6xgAAAABJRU5ErkJggg==>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHsAAAAaCAYAAACXbyOAAAAD6ElEQVR4Xu2ZS2gTURSGU1u1ouKzRNukk4ZKtQhSghYVNyIiCooF3frYiC6qorWID3AhLgR3rlyoCFURQSlFF+LOhe/HQhRRfFBwYdWi1aq1+p/m3nL75yZNxsm0I/PBYab/uTnnnnvuzGTSSCQkJCQkJCQkMDiO0w77k6/x5wuhuro6xfFyWW1t7XiOETTq6+vHcV25DGu0gmN4hkqwkTUxU0skEnNZKxR8vhN2jrRPtrg2LYigjjas721Tw1pelfoqKytnmrqqucTUPCMWi01HgnaSx6hmPyJdrsy3rBWCrYEql1VnLYjY6hiRmhG8I0I7CbtulyTFcZ2pq9vRCVMrhHg8vhAxN5GsN9ZD0otbuE+kUqmxuEBOs65q/m3TWfMMTGQna06W2yoaNbWmpibKer7gsytZQ8zdkgsbYS37oK9nLWighnkWrUE1O+PCQT+aWSsqaiIZzS4GyPPZr1yjBUd9IZaLh31+U6qafY8dxcDPjTVaGDU14zbSqiazhn0a+BqwK++w7oIylesuOwTkKHfUlQ87yv6gourpY10jX4JlDI4X2CfAtxy+Y6wXDAJ9kUSsa5DkMPwnvWj2cBsL+gPj/BdyXjT9QQQ1NKqaj7NPgN5jnL+AvTP9SpeN4EmzZSJZmy1gwke8aDbyfM2VS3wVFRWT5Bz59ucaGxTQpOtSRzKZnMI+QXyodZU6X8w14++XiHHln5tt/NKT83mdq9nQZyUyX6+s5LOxNBjXAftBcgm0g6QNAYuyYZg3CHn1a2HRBP4dqKmcdU0sFptge6uxUWDNLeZY5FgKm+ZJsxH4lARHYZvZZyLNdrI8Z41iStlngskm1dhX7LMhY6PR6ETSukRHrH2mbiCbQc/HCnx9quYhvylo0Mg5ecQY8GNTLWKfifmzKftsqLGDr244b5Oj62YjQBPsm5N+t5bFE5Pn9s9sk1LNvs+6AP0A7CnGbGGfoOLKly6dqxvWC+vnsRoZY7vtYXEXqBhv2KeRhYF/L+sa+JbAnrFuAv8tWBPrGvHBOlHzWfYJTnpDSZ0y1w9Ouv7vsP66urrJPF6A73E8Hp9v/q3PXTfbDarZg1+eGPhWY0wj625AnCc4lMk54l4i9wDQr7HmN9I0NKCVdTcgzhn96EFte9TxpmE9sNfZ1sNTVLMzfjfXwPecNTcgznnYVnmswLbjvJfHyB2kqqoqxrrfZLuqC0XqRLObVc3bbDXL+vpyZatd9dFJ3466I+qq0+DWswz6IVNzi6Oeb4Zdtox5z5rfYLPNwDxusO4GS81d5JfbvzwOpAcZr2W+ggnMZu1/53/4n3tISEhISEhISMgI8BcfZWzO8TpXLQAAAABJRU5ErkJggg==>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADUAAAAaCAYAAAAXHBSTAAABdklEQVR4Xu2WzUrDQBSFi4Irt3GVX+JCgksfQBe6FJe+ixvd+gA+QR9EfAhFXIqIFovFRREtei69g/GQ6cQxLQrzwaFw7r2TOUOSptcLBAJzI8uyU2gEfajG0JC8K55bJHmeb2APD+w7MQHYF+C/2mrzoiiKPVxzUDvYR+5xIoM4kQv2hSiKVrV+zDUXXRyGVyhs9lAHd7hmMCfGvgufGcYrFAYuXRf/j6GcG27T04TPDPObUOfsG/ShlZ4fvwU7DDVg34p5nvC7zTUD6jfSU5blGtcMUkuSZIslc+yJ0jSteA0bGuqJfStovnadpi46Yb9OHMfr2Ow+S0M1+daXEqPXH7JvRQesobKvl8gS19owa+226B6f2bciA7b/J/gnetKbXGtLh6FG7DeCxiMZwP29W/cR5kAWgd7qvg8dhhqz/w00nEETbTZ6z6afQ/Lt16+qaoXnfPANJXdHNv0GvYNuVffQC/cuHN9Qf51lNgKBQCAQmMEn+R+dXw9BxFYAAAAASUVORK5CYII=>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADoAAAAaCAYAAADmF08eAAABx0lEQVR4Xu2Wu0oDQRSGAwpWFrHRYJLdhYX1AbRTbCKIeOktVEhjZWGeQBS0FfIIWoidvoKWAQuxsJGggkIaBS8oXv6DZ3A47MhedNdiPjhk9v/nzJ5/NxELBYvF8t/pchznDvWhVYcM13WHsH7QPWjnqhHXV6Jv/vvY7MB9b6VmBJsfaVipEyqI1AkEX4bXkPpfg3ue6A9Z+kZUo9SJnw6D/iq1qFSr1YlKpTIi9TjgQR+YZgsFm/c5UEnXMcwCtPeww6A1y+Vyn9SjgiEnMw+KQJvUgBuP6Tq0Z9QxH9YtvDP9Oi6453TmQbG5Tg1oXNK03SAIevG5Q57neYHmnap1UhB0JvOgaBinBtSG0rBusbfG3hRdY8Ai1k21Lym5BMVvbZDf6B5dY91WHrRF8jDYCnsvyosKAg3LwjmrdLbUqWS/idhBCX5rLTQPoLaUjoCj/BC2sa7hc07viwKGn5WFM9dRDalTyX4TaYLeo96EXmLvUHppyOWrS3AY+orWTB7+IPVLLyl5Bw39/bF3IfU0/EZQzHREs/m+3yM9I/w2i1InYj+1CKQJinmeUDfO1//bl6hrVAdVl3tzJ01Qi8VisVgs2fEJjbGsG2rpYt4AAAAASUVORK5CYII=>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACkAAAAaCAYAAAAqjnX1AAABx0lEQVR4Xu1Wu0oDURBNLARBsJCouEluHt0WfoBgoY2WFhY2Ktr4DeIn2ChWVlYiamGjIGJvE7UICQq2IoIag0iSRj3jzsAwmsqwLrIHDjuvnT139uZuEokYMSKMXC637pyrgx/MBvisY6jZt/f9CUSQjefzecdCT20udLDIcxsntFtAqMCUZnlakzaXTqd7IiESAqrtRCB+yAuYtrlQ0W5SiI2zwA2bCx0iEnwBa2CT/bLnef22PnTIfgSXbC4ygLgbEmnjkYK8ahuPFFjkrY1ryEIKhUJfNps9gf0KjmKr7OG6C95LLY4sD34FuRVcG77vd3OPrxNEBiI2+m3LvT8CRatUiIbLNmfBTQ/YTcrDOHeNHlNk0xX+Gdn4Wo3Abkkd14rIuo5/Awo2XTAN+iXTd/oNfLd1GryYIe0r+woTmROf6hDbAe90HSGTyYxxLKnjHQE11seREVmCyHm2j8Aq2alUqteKRJ2PWBlc0/GOgB6G1zeofWVfYnoLqs6RDUEz5CO3xaVd8I+lDlMd5vjvgYYt8AmsFYvFARcc+uTT37qSC7ZMDWIW+VVT/QU4AT64YHvRPY9gk3vSdiO/YZ8XI0aM/4xPSWWmvpeo8zIAAAAASUVORK5CYII=>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACYAAAAaCAYAAADbhS54AAABlUlEQVR4Xu1WPUvEQBA9S7GwEIyQkM1H42+ws9LSThFUtPFH+CcUf4SohZUgNpZaHB544AfY23jmREQFUd+QWV2G3eLO3GqRB4/bmTeZfbvZJNdo1KjhEUmSbCqlHsFP5gv4YOZQsyev8wZtQubTNFVs7lhqXsDGTmWe4DI9cGA3FnhXZqQWRdHwnxnDpJeuiZE/YNNzUhs4XDuC3DSb2pKaF2hjYBcswFeO22EYjsl6L9DnC1yTWj9Av0n0epf5noEmN2RM5vtFEAQj6Hco8z2Dd6syY5WBjd3KvAltPsuy0TiOjzB+Aqdw23bxuwPeyVoaG8dkH2yBJ+DZT2cHULRBF6LButQk9AQcDunJWbtGj1kj/tawkGVR6747ELdVuWp6Aum7+Ax+yDoTvIAJMzbGLRhYcmiLZNymVQJqaL46xORN2hmbhsXMI27btEpADfFBD8zYGJ/DwIpNo51EfGXTfg00ewM7YJHn+bgqX8QU01+kpiqPQwFzq6o8IveqPB50G+m4dKBdcD1d15Vz1KhR4z/iC6sGnhIFOAqOAAAAAElFTkSuQmCC>

[image11]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE8AAAAaCAYAAAD2dwHCAAADMElEQVR4Xu2YSWgUQRSGJya4g6IMo7PVzDAHGRARt5NHxQUVDeTqchA8qUQUcQFBb+JNUDx6EFFPIXoS9KpxwYPgQUHBIOIW12jIxP9lqqD8p7p7qp3JJf3BI8X/Xr3X9aq7pzqpVEJCQsI0Qyk1AJto1Xi+D8VicRXnC7NqtTqLc/jA+SLsLM+PRCZiUX2sidlaqVRaxpovmP8WdpW0z668Ls0HrGk/cgxj2GU0rOGwXluvFSq16oVCYbWtRZLP5xdh4gDJM3SBp6TLBb1hzQdXQ3Qtp86aD5g/5tA+uvJCe8haJJg0mLJ2RsDuHJIC+LvD1mu12kzoF2zNB+zsGuTcTbLZqCekt6N5Lx1a0EaNsxYJ7qSDrKmAxwgLX1gulzOstwrmbmTNPEZo7Hb2Qd/JWqtUKpUFuVxuMclmox6TLrWOsBaLoN3pBKjzZQpr9Ust3DDb2NcuunXzhtjRCaZ4o0Y6Wgu7ckwvaCv7DPCtxOP2gPUY9Ohazpc1asxW+s6EnWO/L1EbBd892AfYbfa1BCZ+CyuA5p6G/2I7mhe1Ucp6N2E8hprXbb8noRuFa7kv16PHlxD3i2Mi0QUCmydgEWfa0TzU+R5WS3zpdHq+jFHveFhsFGZ+yEadgJ3X45vetfRRRAqEvu/Cmgd9San5OOJE12rpIhE3CPtNche0k6Q5QdwPj1oTOP8uZz0UTLoiE7H4PeyzkeapgNvfakg3+2zwaFR07Cv2uZDYTCYzj7TJAy9yHbV1B9Jkc12B4Ci1Fmu7jLi77HOCwF2wn6pxtpOLEZP33p+gYrp5j1gXVOP2f46YvewTdF75ETC15BdwFFbnWIPEyLmNdSx2hc7xmn2C5IR9hX1SjR8CqStrHUfDN3C8Af5eFbD2/0Y3r+mgaYBvC2LWsR4H5HmGPz0yRt4b5J4E+h3WfEGOIVhZxnhk5+jm/fPl1RZ085q+ew3wvWAtDshzDbZPXiOwAxiPcozc4fiSyLPuizQLuTbp8eaO3Hmq8eI1j8FISt8VBnxmrYd+ytbiIgsgu+WIecdaHOQ/Ksj1XjXe+/VsNjuXYzoOCi9lLSEhISEhYVrzF3Y3L7lvleLfAAAAAElFTkSuQmCC>

[image12]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAD4AAAAaCAYAAADv/O9kAAACnElEQVR4Xu2WOWhUURSGExERl0qGgVnemw0GB6xsLLVQsLATRIK4jRYqiKA2ajSd2FgKWggpRLAQm+CSaHAEOxECIiKmCFq4ZIwYxy3od96cK8frDIYY0BnfDz/3nP+cu5y5y5uenhgxYnQ7esMwfAu/Gb7xk9BmTHzGj3csgiC4KEXlcrm1fsxB4r7W8aDgHbqbx/yYgPg1fpzVvt7xyGazRS18yI+lUqkl6C98vWughU+10D/7WldBC//pHuPvhset1nVoU3jLB407n/O1joVfOPajUqmUsDmZTGYVj9xlYuPUfsjGOhYU03CF62N3r0VOyxPwL4C1XfG1WYGOw1JYoVAI2hXYTv/b4PStmXPhHOGTetwn4EY/rrGI5J7VPufwL8BBeEK1G5rXB2/DUdHl2mBPsshTtGNuXF30K9oB2veiVSqVRdjTjHWQtsEJTLl8/Jdh89F9Datm3RHz+XzZ5c4KdFqnnet+zEHixj4AB40/xCI2iU0RN/Fr5XJ5ufw50rjtex/2aYFWjz6dLD6J/VzlBS6HsbYwx1GTX5VWi5/bjoOFOkGvH3DwFik7v974++G02OjX4V4XM3+Q5DQJ6/ASPAM/uTyLZDK5lNh5+MyfVznqtD8t/LfwF8CEm41/GH4VWwp3Oy3QHfzlfdBjH/WxQDsC3xk/6ptIJJZJy9cljTYGa+IzXz/2VbX3uH7zBrt4JtiGP2xi4xS4Qe1bcJeLqfYlnU6vMP4+bX+MWSwWs/wYi0Wj3S6ae2zxt0pR2CtdPtoTaVUfUbvfxecFDPwhbD4odVNgFX6UmBxn0YLm4zapfOqNMQIn5A1wmuyi5j5A36myXLspWAubV+ghvCPXh5zT8C5+Q/LcOPiPhc6PESNGjBj/C74DqTTjcM/oT1gAAAAASUVORK5CYII=>

[image13]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADMAAAAaCAYAAAAaAmTUAAACHUlEQVR4Xu1WzStEURQfHxESG01NM/PmK1NjYSFLxdLGRilZUBYWko1ZUTb+Chv/gbKZUlMWPko2lCY2iHwsiBmhFON3xjm6TvPkoTx6v/p1z/mdc8+75913b8/n8+DBg1NUWJZ1AxYNXukkaE9G/EnHXYVwOLxAC41EIt06JqC41lwJNDHCb31axwiIL6HhDq27EqFQKM7NZHQsEAjUQz/TuqvBzeTL6I9acz24mXfnAv4oOGNqfwI2zfyNQ6+hm4GdSyQSLWbObwFrWXT0YpH8IBP4QljTOU7h9/sbcAu2af0rcNpMlibEYrGwo4kfAHUyv9IMHjrLn9oJ2KvjqVSqBvod8iZpF7F7ARWjuXPW6+1XRTmslUh5GPfY72f/Lcb+FupPYFwH50WXXNP/EEju4eLXOkaIRqN+xE7ZrVSLKAaDwTqysZjjeDweEl3vDLRDi5uRHMO+N3Wc2VrTF/szqOYJFTogoDOAnHnwQIonk8lGuwfZNLNv1wwBfxtj0DZIp0/eLu9bQLE0WDD8UnH+Qyj7IOjP2NF2srGwJtZ20eCAkfNuh6UBsjHXKpf3bVAxvLVhsuWSgD8oMWitRu4Uj6c4W13UsI933Ho9D0NmHbKR16cbA6NgWnyJ/QToM8yDq+A4uA2uSBD2DngEZkVDs83w77ETm6Jx7gW0ZYxDvOjSQqkeeA7m0FwnxgfWC+AleGvW8eDBg4f/hxdHNLseDK9tFAAAAABJRU5ErkJggg==>

[image14]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAZCAYAAABuKkPfAAACsUlEQVR4Xu2XTYhNYRiAzwz5KWXjkuvee879kdydSc1oxIYsFCWKlbLxU9iwkBRqxmIsLJSUjQWSjZKysBAWkoXV7CQrE2KiJqYpnnfmO+P1dmbOd+eeSXM7T73d7/37vvd7v/N3gyAnZ8FTKpWWR1HUZ+0WibO2jqBSqdygAa+RvWEYTjSbzSU2RsD3m5/F1v7foKBRa7MQswl5ivSgdrPZGhu9xu9DEyebi8cXkO/ICHH7id/F+JXEIOd13oyQuNMlnLC+LNGFzwS17HC1aPmkY9jkMTsX+lU37KrVaiud7ZcK8YMC+t2iQ9aXBbbwJNjgduIeI7eQy4VCYYWNwX7dzoX+wugfg3ZuAx4k66WLyF3rawdbeBIcxFYaccnaNfh79VyNRmMpeVdiHd++0Pc2SKNYLK5ism8s+tz65oJnE/rTmiC4ubpl7OqbPnU5wHicGSyyjInfI8Ooi6zfF58mELMFuSOxyD1kLOkQyuVy3cW8xH8ztqN/Dtq5DWajXq+vZoFRTuqJ9VmIe+QK9JUJlduDfDDzScw/93wSNOMQck7px13ugI5rmWq1uoFJxpnwtvW1ihRkbT6QN+KTS8xPNT6LfJExB3eK+g/+jfSEzW+ThZlg0Prmis9GkiDvmeRS0xrri8H/NVC3qrsC5FtjWo/Hqcgl5SbI/JvBpxC3tn39vRGbPJu0PQb7YfxntE3ieWsUtK79iXDiJyWQB84e68sKn0JcEx4Y2/hsufjGEmytNYFOXgzVpTNfpBYSTMa8ldek0tdKnpy2josJpz7FuxLscqCbta798w4F33cn6is/dD76sLNPXgHyKa39MfiOsNZRaxeiqT9W72RM/mnkgI3pCELzn8IibwXXxPSHO5/J6wje7SORx//3BYl8HrO5Xh+hERttfk5OTk5Oh/EHkBXjMlS1h9AAAAAASUVORK5CYII=>

[image15]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAWCAYAAADNX8xBAAAAdElEQVR4XmNgGAWjgDQgLy+/F12MLAA06B+6GFlATk7OBojL0MXJAkBXnVNQUDBHEZSVlTUhBwMNugU0cB+yQX7kYKAh10AYaAQLwlkkAqBrJgIN8UYXJwkADVAEGtSJLk4yABr0CV2MLAA06DC62CigIwAAJqAgbNKiql8AAAAASUVORK5CYII=>

[image16]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACYAAAAZCAYAAABdEVzWAAACWklEQVR4Xu2VO2vUQRTFg4gPREVUFvc1u+vi4opERLFIYSFWImJjYWOVTyAkYiFoJWIRkTTa+gFCSCWYwk6IhSYiigg+UZCAEGIIicnvkjvu3bvjZi0CFv8Dl5lz7pmZO4//bl9fhv8E5XL5vNc8KpXKMa9tGCjoSAhhQdulUql0wnsEFPWW/EWvd6DZbG7B/BTzCu1jn49gwSE8i+o77fOiU8xB9e6B/yKWiavws7QPxUN882M7gOm4mGu12m7hTHxSeMI3R0wZPkvccZ62cZbTP6DtQsvRBbqDFwlt2vBRv2i9Xt9vtXw+v897Evx66OUK4+TEA6vDn9tJ1ZM6RbnSa5b7vKGbQy9XKGDSKzr5iNXRJnstzC4W1t7fZenzps4QgzYX++uiWq0Gndyf2AfRC4XCXuXdCus4JeIR8cNot9j8BetbFzIRg156TUI+BOHkb/oCOI1LqcI85IvH8zVy+oeJeWLS+jogRy6TywTKh+HTuuCm6IPPyU+K4d+1sK5XRH4p9nnTu+JGcrncDvqvW84EuNKc/H5hfFUsFo/Svo8TWFD0DfSfxH3h4rHFepC7jedc5Iz/iDYeObl3sd8T9CR+e92CDWwXH5tq+JyAAraR/2w1nXfUeNo+ujaoOfWA//zexI+EiU5Fjd2P+XEW5JYTmi/sns23Qc3zhk8Rs9ZDEQPik2sW3mg0duq4qvVFoN+Vt+t1tGfkJiIP3a6S0+jXRT5J+7c3E1qP/Yv6Kt4j0CtMLsjj3ypjpa+Pf8Z7Ngws9sZrFvwnHwprP8RPfC5DhgwZ/hGroszQisMX2q0AAAAASUVORK5CYII=>

[image17]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGgAAAAWCAYAAAAsNNkQAAAA20lEQVR4Xu3YOwrCQBSF4YCNCxCLOJMHQcTWRkGwsxLs3YLgHqxsXIWNa7B2EbYuQVtt9ASsLghpZAb5P7iFOaTxcEeTJAEAAPgmy7J5nucvzdpmiIiKmn6K2tsMEXHO9VXSQ3O0GSKSpmlHJd2KojjbDBFRQW0VddVc9LFlc0SgqqquCrrrd+pkMwRUluVAxTy1RQebISAVM6v/0WljdjZDQNqUFc9EEdKmbOpivPdLmyEwbc1W5YzsdQD4A865no64RZPRcTix9+PH6lc6+uLHTUYlDe39ABCnN9zrLC1pqtPuAAAAAElFTkSuQmCC>

[image18]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE4AAAAZCAYAAACfIRhSAAADhUlEQVR4Xu1XXWjNYRg/8xFJNJmLnXP2nq9kKLQLa0TiQi03uJqPa/J148bFhAkXJoq5MJFEiiilfLUSYneUlI8bTFJsk9lmGr/fOc+rZ8/+287ZxMr/V0/v+/s9z/v83+f9v+d9/ycSCREiRIgQGkVWCIJzrq2srGwDrDiVSk1FuxZaq42LxWIZ+B7D9xN2x/o94KuHfYd9Qvxi6x+1wISXSXH7rS8IEtvHotFoTMckEoml1BWfr7kHtHb9XPQ7EXtIx4w6cNdI4VutbzBwDMYeRNuAdrn1E4zBAmw2GnfVI8851i4mduk0q40aoKBdsmCrrS8fDFVYOp2ewRi2Wsdzb+mxspD9csmLWW/1fwZM6DisFwVUWl8hCCpWA0XvDoqBfsYsHF9ej47xOub4VPrr0N+L9iI5XkYc/Xr+Wnx8aWnpdGhHqIOO87oHYnfA9wp2hUcG7KiNCQQGXIV1IUHK+oYDKewF2mewh7AfETVhPOcaY9SQLKCd0Dr7sC86Rumd0q8VTrvta0D/I6yHRwbmskk0XjLM//uSA2+Nx+NzPedlhfhLng8IBG6Th1ZZ33DBfJlMZoLiN2TCWWBi9zRX+jHqOMei5DKvzzZOdL3A58lRy2ylLZC4c14TnXHbNceOnKRj8lo4DyTYKQ9aY30jBSYyS3LXkqO9QG7jXO6ooJ7dnTKmzYR5vddzLMRZmw+8nJo9R6lhPns0F2uCVavQwoDBNZJoi/UVgLGGj5Gcz0kGOuOgnda6jOnWMUp/qXijzcdvRGp2N8nYfZ5XVFSMB+8VPWv49pypxxSEZDK5hElQ5AHrGwwud8jyrU70WklJyWSZ1H1y5FxEbndDov+tmi1Ex3gdOU4qfsrG4dxKUxtq4XCkTPF91DxvoGcWDHlz3bBG6wsCCnqD2A6tYUFWyoRqvCa8z+cO+FenzjT0GwKKKKLGneKFoJ/qEDuuTnPt959KWhsRMLliJGyyuoV8DrzWGniXk1vQA/luutxt65FdECxyQml+d83xHO5mZ25a8Ou2WPAqWaSk0e1uJV/lOfJXuoAL6a8AE9kok26R9oGNIaA/gXXALksBK2wMfj5OctyV3dyi/eBtsHewt7APsDqXe1HvRWP7zeV+zvRTY3w7x0vOwy73DL64Zp0/xJ8Gv5mw0tX5GLexHf/fgn9HsCAL8zEsXrkdHyJEiBAhQoQAfgF3B1YSlpRnOgAAAABJRU5ErkJggg==>

[image19]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFYAAAAWCAYAAABaDmubAAAA10lEQVR4Xu3XLwoCQRgF8AWLBxDDOH8ZRKwWBcFmEuxeQfAOJounsHgGs4ewegStWvQNmD4QlnHK4vvBF3YfWx4fw2xVERERNZG1du6ce2HWMqMCUPD0U/BeZlSA1rqPch+Yo8yoAKVUB+XevPdnmVEBKLaNgq+YCx5bMqcfxBi7KPaOc/gkM8oQQhig0Ce29iAzyoBCZ+mGgA3dyYwyYDNXvNMWhM3cpEKNMUuZUSZs6RaljuR7Ivor+IXt4ShY1BkcGxP5PX2Rfl1R2LjOoNyh/J6Imu4NKscsLXfFvygAAAAASUVORK5CYII=>

[image20]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHYAAAAXCAYAAADeD7vuAAAENUlEQVR4Xu1ZS2xMYRTWViQSIR7jMe3M32nHZgQhLLwibLARQmjizULC2kLsxI6FxoaF6EZQEiQk0thI0JB6JA3qVYJGPKptqtpqo74z9/zt6enc0ZlM79zK/ZLjnv875z//uef+98zfa8yYAAECBAgQIMCIo7S09KQxpg3Sx9IJ+SE5+FTreX5ANBr9wPna3H9CvvM1ycFnp56XL+Sl1jaw5mOxmOEFa7TNL+DcazWPnCvJhoe7TNvyCU9r7VYcglsifgDy2sbFWKNtKNR8zr1R2/IJz2qNolS4FaekpGR8ThfLMZBXg1tu4I+RDW/sNW3LFzytNQI9dwsG/ionskHb/IB0hRC2Im3LFzyttVtxwK3ihSq1zS/g3O+l4M+SLRKJLNK2fMLTWtvFIK2QFkgXj+uLi4unan+/AC12C+fZy3nT6ZJ04q5ofz+AcyMZ2Vrbng/Zq21+h0nz+5oNsFGqEO+u5nMFT2uNRV7msjhegouUs9zxYDdDtms+V/C01rkujpfg3N9p3q/wtNa82BvNS9iEysrKJmFH34LeDlmK1nIJ1wuQz9YXR/ZijJ/BdhjXzkQiMY5jJE+D9sasjnjn7FwL8Pvi8XhI8xKIv5vnp33DxDpzMKcaei/9luFaC7lBeWlf1pNtHvczF3MfQG+CHByIPADw6yi+5jU4ftpaE+DTAzkAaUbc5cwdtflBFkAO2THZcW8rZYAjZAC5v590AQe5zMMCG5BtDYixlnS6Ynyb9FgsNg96t/VjX1u4NslbiI8KaXc27F//5WPB8ZIPBafkFSr3PmyiiWwLK1sjpEn6Wl0gWQsXWz9MZrXuEHp/XGyyOMZ/7BgP/Y7Vk37455Rx3jo6ldG3yg45IRU4qZlyLPQnWGSHHZMfuPPG2eWDblgUtkDyEpj/ED7fNE/gfOn0S1c6WdL31k7tJyFzwPqLMe6RtvLy8gjpKNwU6Ys8XkEqpK/VJeBzHbb3mieYLGoNFMLnhEnxNy+PC+kh0wcNxWcOmiiP4zIQ9Dr7sd2I9hYKhSboBeGXAFcPOS55Ddh7NZctVK4LIV3Shp+YKOnIbbLyfQHZJH2trkGbUXPZAGssMeLB6zWR42nIRfAfJY/xWzkeNmgBtMkZciz0x7ixXcLPkE6nTBrDdoZdaSfetH7U+pgfAthfay5byFzpgwXGv6XN5qvfWCoWct8qfa2ukc6WCaiW4txRRHEhs5QPcYN+7+U9DRuY1A1phrSgbU03TgukMbXBOuO0mBYUYQ+3YfJ/BFkN+WKcdkRz6L/Ukm+LcdoTjYe0UeMcxlxbdSYwTuujdVq5DVNLpNx/GXFfxnmTre2pcd7WpC3qfAih9k9x2vUauOeacDg8TfPZAmt8gtxH3EqsXUU/Cco+6NxC0D6+hG2NowV402drzktgw67Hw96o+QCjH2NNis4X4D/BX8DX2PH0K9HYAAAAAElFTkSuQmCC>

[image21]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE4AAAAZCAYAAACfIRhSAAADfklEQVR4Xu1XS2xNURRtk4rEQELEoPp63y+kSJAONNX4hFljwqyYGRC/AYkYVFBBRIUEHaiQRghBhZFP08SIzogZA/Enoa2grUprrde9a3f3Pn3PE5q4K9l5Z629zz7n7HvuuecVFUWIECFCBItiL2RDPB6/HATBIKyvvLx8p/cTZWVlafgeSNxd71fA1wj7BvuA+BrvH7fAhJfJ4vZ7XxgYm06nJ1vOhdsYFHYJdcPnW66A1m3HRbsHsYdszLgDnu46WfRm78uGRCKxWPq0qYZ2JzX4ZhltEAXYqFw07qr7yjH+cl9M7NKpXhs3wIJ2yeJXeV8OKJG+h1VA+ys13YWpVGo6OX9/dsuMe9sWRQo5qkjUUNS1Xv9nwIROwAawgCrvKwRSyOECYNG7wwoC/awrHPv12xjVMcdH0l6D9l78XiTHw4ih3ci3ReNLS0unQTtKHbREdQVit8H3FHaVRwbsmI8JBTq0wnqRIOl9BYK77zFsoMh8WDDOdVsgBbSTVmcb9snGGL1H2vXCaXd0DWi/h/WDH0QhNojGjwzzD88FvDMWi81Vzo8V4i8pzwoEbpFBq72vECBfHXI34fcdJnLD+sDvyQJGAPpx6jjHZpDLvD76ONFtgc+TY7zZRlsgcS2qic64rZZjR06yMTkVToEEO2Sg1d5XKJDzrSw086TRvmAXrgiGjgrqmddJ5tPlwlTnLs4AhTjn84FXUPPnKDUUZo/lYu2wWhOaH9C5ThJt8r7fBXJtl5xvyLOdcdDOWF369NkYoz8xvNnn4x2Rmt9N0nef8srKygngA6JnLJlMzrR98oJeK7DIA973K6BPC+y71ZCjRiclfBHbfjfER39Vh/tYyLyaDD/t43BupaiNVTh738Sa52UbM2/Ik+uDNXtfGHRgFGGp0daL/srFjbjugH8OzJmG9qmQRRRT405RIexVHWPHNVhu/XpVslpBwOSmIGG71z0Q08rD32ksCO9xE1VDvlvByJ2ZKQj6xo2mu2uOcrg7AvelBb/pFwteLUVKON3vVvKVypG/Kgj5IP0VYOBrMuln8ttji2biHsK+wK7IAlb4GLw+geRog/95YHYtAd4Fewl7EQx9hBpgvbDXovGXF3C+zvRTY3w3+0vOI8HQGHxwHTZ/hD8N3plQ6dpcjNvY9/9vwb8jKMjCXAzFq/D9I0SIECFChAjAD2FZWu4oh8T3AAAAAElFTkSuQmCC>

[image22]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADEAAAAaCAYAAAAe97TpAAACGElEQVR4Xu1WO0sDQRBOQBDU9ogEcpdcUloIVjaihYqInZ0KglY+umCjQSwsBG21UBT9AYJNGoOVtVhZKSkEC1GiiE9M4jferpmMSQgawgXug2Fn5pt9fLt7D5/PgwcPEn7Lsh5geWb3sgi5LOOzkncFTNPcowWGw+FeyWkQL3OuAhY/qXZ5UXIE8EcQ2iXzrkIoFIoqEUnJBYPBFuRvZN6VUCIeS+Q/ZM61UCKK7j3iKdgSz7kaZUS4+2GWkCLgX8RiMYPX/BUY66AuG4JJXvVE6kE/lTV4S63JXLWol4gUTWTbtlluQuRzMlctyo1ZU+A7sKyu1DVsSPKK+zGdR78txNuWc2USrAv9DbzDdmBnvA/8OdiG4i5Zvmh8Fo/pmopAYZ/qkJGchh6cxbSYAxYnIWpE+Xk8U82M4yJOcDUHlX8If4VxadgM+bjWHfSd0lw1aFIT+SWhUUJEHovuZ/Es7NnnnMKvWh5HIpEeEk31sF2dh6B2XYv2ttCjRmCDD+sYIkYZH4d9GobRJhfNY6pBvwXlr9O/W6HSqQ0EAq1op3m+JmAiVqnF5BPwU4xPY4cHdC2/CkIE98+x+/uWc4I6l7D+8RKpCCx6HINnYHGdo92CvcFe6NXMyul6PsGOYZu0cL14jDNvOb/3d2rHcxDSyfp+i+NxwyAajYaopc2gKyn5hgCdKE7JRnsluYYCBHTLnAcPdcIXpL6yqpwlHg8AAAAASUVORK5CYII=>

[image23]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADEAAAAZCAYAAACYY8ZHAAACbUlEQVR4Xu2WPWhTURTHWz8HHUSNkSTmvpAsZnDJUnAQFUGU4qCTg9LJUd0qqLgKbk7VKi7uDi0U1EGLolQ7CFZEBcVGcBA/8KtUjb/Td288nL6IYjEvkD/8Oef8z8l759x3ubk9PQbFYrG/FW1takGzh1vR1nbRRQKcc29gQ/FdQs0nXVMulzfYmrYjiqKD0hz2lM0FSL5SqSy3empQKpWcX+WrNidguKPwiNVTBz/EZ6sL0L9bLZUI+z1Bf8ZXWGX1VCJpCOKN8JrWUo0WQ8z7MqkGDX/UTeMP80+93dQ00K7AS/gX4QXkRbqGrTfoF2SX1v8LaGzMv7xUq9WWYt/aGrT38A4cp9mb2FlbI0CfbMsQvPS4DMEwB1x8SvWakt5CobA6BNScFU3lmyB3ry1DsLJbZAj4Ev+0zWvkcrm11O3VGvGEbDXsUzgVhmDwPP5DnnkM+6VarS4T3X9Jed/cFsaO+vgJN4J12Fvy/+T+8nhfoh/6O1DzzcQTvPCQiqedHwJ9p/MnHH+qm/BnQh25AeJXKh4Ui/Yo8sc6C7Mj5P8IMoCsgtU18vn8Guq+as0OTvw8DCGgofXEl2E9oXYu1td9/M2iC/FP/qpeIPDg83DEaNJI85TyQ+z2/gicEj+TyaxMGOK2NIp9ETSGjrztQ5+1p+Q/g4f+cPHR2gQve6BXkvwHtD3eb8jdTHxq9klMbijUZrPZFaLpmzFxXflb4f4QLwhcfC0/YXUavIE+Dh+7+EvIVuj3W2kG3ofb4GsXn2xNSK2J6/z2LvY6nNS5tGGxpzQ9b1E6AjR+Bp6TL2hzHQU5cq3WRafiJ4tIxgUbwN/kAAAAAElFTkSuQmCC>

[image24]: <data:image/png;base64,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>

[image25]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACsAAAAZCAYAAACo79dmAAACD0lEQVR4Xu2Vu0sDQRDGE1FBUFAxinldQpBAECwCIjZWKpLaQjCNFnaCoIJgJ/gf2NgJFmoniGCnnaA2phDsgrVggiDi8xvZiZMhj70UQeR+MGTnm7ndudndi8/n8Q9wHCeTSqXatS6JxWITWms6KPQrHo9P4TcXjUa3dZxAbBN2rXUb/Jh0VosaTP4E20okEhG4rejMGPwbmQPtFrbHPhVubBdrTMKWWZPPWRMOhzvw8KrWNWLhkqGwlQo5GeE/8xjdHjDafiQSGWbdFYFAoBMTrGldY4rboM7BlnScMDmlswj/XISpMb1Og9v/QzKZ7LIs9l1rGipWHin4Hzoufde4KPZNaxp09Rh5d+xjnBexo4a3n3FR7CtsB1ZA9w6oS7DxCnmPiF+YLvpJw6Xsh3+lUmtjFrA2c/P52SK6M80+Lswo5dA5ZK0alCfcFvg52AONhV4f285WwrxUUesSOhrodIp9eoYuNY9/My2wLTadTrdpjXdA6wzOaBDxS/YxXpT5eIl57NAI+3WxKRaTZk1hZXn1itUxzHMmNXS9G9q6zKmJTbGIL9AiOJ8hpdN39V5qDGKnyB+SGnIPVWd76q1dhk2xhO4S/BOtMaFQKOyoPwQC2owqNlv1GFCiG5OdDAaDfUYvmN8Xn/k0aSiuNQaxT2qQGVfNawrY6jkUMah1CeJ5Mp/bT5eHh4fH3+UbAhC23iWu94EAAAAASUVORK5CYII=>

[image26]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAZCAYAAAB3oa15AAAClElEQVR4Xu2W24vNURTHz9Q8uOQScnRuv3OrE0rDJMkDRXkV5R/w4hJRlBeexhNNKZ6YlDIeqPEml1JT5JIizOR5FCJqGDFhfNac/Tsta/b+ORNF+X1rtff3u75777V+t3MymRT/MUql0rJ8Pl+wuka5XF5itX8CURS9JXqIXuKZzQvQK8SE1RPBgoN0vcvqMcgfIUaJT8QOmxcUCoU6V/eeHE7ctHm0bcQ3xd8Q45w7SGwmtsDfy3ru0EK91guMF2UDd6DEbusRoA8RNxR/StzWHg5fL3so3qW5AP6KeKD4oUajMUfmuVxuFkMnF2A5en9rUbsINVCv1+faQgSiUeR8w3+6g1Hz4tzVHuJWzPFvoOCNMY89mreNUANoj3ybOv9ZmddqtcXCZdQeCryu1zK/GqnnnvkxPDMUf9jWo+NDQgOihxqY1LmKR30e9HNap9g1mpucPHIXYj5t/GYDVwKe01an0JNR80UdrVQqK5R3yvppwRW0J6BP2VzrFDXo87hiJ/g65W1OA88TPAti7u6cvD8rtS8RchAL9/p0X3FaZ+wPeE45vdPmYhSLxdV4zsec+UsaP+zmIwwdLXMSXAP7fHqguJYeegfQ+ny6hsl3WD98QPMgZCGd7/foH+ymTpcGhmVOA+uE/+orZCHrq9XqPMXXWj98TPMgXAMHrE5x2+2mAtHIdWtObDWej8Q7rcVwxZ4x2lJ7VlsN8Cu4yBVwwuYErrmdih+3B9HMNbSvSpp8HFhXVloL5L5bTWD3jZIeIZKXoub/kRfEiBtfE+PaxxdipivmPuNj4nPG83K53BhxWfw0tcl6BOSeZ7PZ2VYXkLsTuc+51JTxnPPXQWE9VtMgP0B8IVbZXIoUKVKk+CP4ATmi8RO1PT43AAAAAElFTkSuQmCC>

[image27]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGAAAAAZCAYAAADOtSsxAAAESklEQVR4Xu1YWYiNYRgeg5Ab2aZm+86YEVGSuZDscSNR9gvkjlzYKVuUJW4ossYQTWS54IaIC65sJcqF3MgehrGMYazPc877He+85///c84wSv1Pvf3f+7zr933/XlAQI0aMGDH+YxQmEonFzrnlOHYhUVFR0cc6RQKBSyAnysvL+4neFwlrmdj6ZkNRUVFnxF1E/E/ITVBtrA8B2zZIE6QOdYdZO1FaWloF23XJdcnaA9BGfHMS5B7DIIxfG9sHyCtIo+e4JrYY+KOQJuTpT8H4FOQ+/a1vJBCwSRX3ctv6ZQMWrISxOHaiXlJS0k2aKdR+4N6xptIbMcGt2gf6SIn1+kCtBwF1e4tPetMRN4McjkeUK2u+xaItMlxy7poTvlZskzxXVlZWHOSLOnOC+EigkfUI2skmcVwLqq31yQWIbUCOE4a7BfnsdZ51tkEsXFfLUUeu+YbjFXNNcxpYlImIWaI51JvKXJBDmucZDds+zYnfd815iC3dI8aXbc8eYXwoELAWMtry+YKFManphlttGuciZjQosbM4rqys7EmdR+2TkFub5jRgX4n7r9Mc/KcwBlKjeQL1rmhd/L5qzkNs+oq8G9YL+HuWiwQC1rg/3ABMfAQbsvdzf0nyLKceNknynBTHyLEuaHLgDwfxHrBPsBz8J0vNg9YG/wVaD+uNEJvegHme49WkffMGEqxCos1M5icJbr/1i0Ii9SbAhgZpHvmmSb7B1MXnvfZRfCPHiDlDPcBndxAfBRexARbil7EB6P0kbbjFDdc8uM8Sk5aEuW3mBAQuxaQvGI4JN2ouCvDdwBhcCQMMP0lyzRSd4zfaR/HJxcUkrvqxBvgd5Pmwt7YwqPq5bgCljj1CGkT/UFxc3N36E7AdUHFeHli/vOGTWT4M2MC59MciDTR88iFY/vuVj3nrtY/if8j4WFBtcLuEb2dtYXAt2ADL5wrEjvc57DpkQ8a7OpJ8z6cZ/wyADNE8Fn42eX/Wis8X7aP45JkT9gwAVxPER8G10gYkzNuWQiFz2DtKJKRwXQCXUzNEVVVVBymc7S0oMK/E7uUYx6HU830LCoJrpQ2A30vLeTAHer1h+VBI4RUBXLNmoI/D4vTXnIbE7DTcOZ0H4z02b4F8wVZXV7f3hOSarJ2gf3QBz48ouNbbAH4pb7c8wRzYgJWWDwUCPuEM7uF1BI9iEvNPI/2Zr7hmcOZsF44xdiF5tqc3kmeLM29GvITBfVNUsj58E4rLCsQskx7OW5sG0nbMNj8Nl9oAzmOu5qEvzDVHM7jUkz/ZgCTuZX3Q5FnYHlpeA/bjLvX84JELlvE/iR9LUucy6jzC8an1IcDfcak3kdPS01jrEwaX+p/zAvIE8liOzyH1qF9kfPlr5JnyY1zGc0oD9le87aKnLexNSfqrv1WQ170txt8Hd9lyMf4RcPZfDPsgifEPwN+9losRI0aMGDFitAy/AG7nzbcmqXg+AAAAAElFTkSuQmCC>

[image28]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEwAAAAZCAYAAACb1MhvAAADVklEQVR4Xu1XW4hNYRQ+gxeK3NU5c84+Y46OS0lJxANK3rx44JGaiCeXMiWDB7yYFJN5UDQpKUbhze1JKePBpYQH1EQuMaNcDjGG75uzfq3W/veZPerUPPxfrf69vvWt9a+9zr6dTCYgICBgdKBYLG6OoqitUCjMpt/Y2Dg+m81OsLoYkLgLdgGJ88Wfi0LnsO60WvD7YZ9hFViLjRPYuIRaPYj/gd2ycR9Em9YOSc5H2KDiv8I+wL45rqmpaa3dC71tYQznt7S5uTmP9Tj8L6wF/SyrjwHCw2pTZw88uiewm8p/DLujNdh8JfOVv0j7SaCGv7Dz2bj00Wt092GXDUddbA8MZqvEjmletGM1B+0C8qkGBvFBiDtwcmextmVMMaJUKk3yNUUOeZONv91ofsLuak6DtZFzVXO4NabLyb7UPNAA7pkmRBfrjbCxqHqHJGl/pRoYhG2w1ZbXQPyhbyNp6DSPcXnPpM9VazCMG75cB8SXwTZpLpfLTZPaLzRPgBs0PnXe+i6Gi2KK+B2iHWOkjHWmHdi+aPiBeZvSPJo64NOA7/LxDmhyob5KCdyeU6X2c80T7Nf43t4IG8PxPMehrxVamxpodi8KHJEiXVzBndIau7GPR+6VBE2nj6+FWgOzSOoN/ewgj7VV8+B6XY6ydq2pCYh3o+h1w7HI0NtI+bGmNI8h3/ZpwJ8gjyHkbCwJvIWkduqBwfpg/VH1LU5/gFev1RNykbg8ZxWrSw1XJMn38VjPJ2hOCj/OxpLwPwOzfFogdzlsgDV4Vdq4Dw2WQPJv3URSU5pPeoaBO+Pja6FeA0PddZZzkDrfLR+DCPs8nB4YP+xiTYnuKY/5EKU/0rekD/UaGHq5CCtanojkg9fyMciGezzcv2ScwAZfMXKILdY+bL3R8Au8X3PDoV4Dg64btXssT8i5XLN8DBBW8PE4w/n4AVYxGQ/MstHx7blN+e22Ub48wA0oih+azCsqbljwR5BBvLMxi5EOTPRHNZ/P55ekrTGEqPqGGdqYVpA/pBr868IYzv0e1kewHxn/848xXt6XpNYaq0lCVP2P+h72GvZK7A3sE/bdaLTc463S8Zg9JQLxbpzHHKwt7E2Mz+vBcrk80eoDAgICAgICAgJGB/4CO6OHReV9Tm0AAAAASUVORK5CYII=>

[image29]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGAAAAAZCAYAAADOtSsxAAAEHUlEQVR4Xu1YS2xNURRtG4KYCKLJe23Pa1ohaSKNDkT8BkxESFq/AWJWQfzCoDEwEAkDJMQvoYQI8RkwIUQHjPwmJAZiIv5UKdW0SrHWe/u8bLv3vnf7qk0kdyU795y1197n7HPuv6goRowYMWL8xyhJpVKbnXPbcBxDorKycpIVRYYkWmv5qCgtLR2N+JvI8xv2AFSx1RDw7YP1wNoqKipmWj9RVlZWDd89yXXL+gNQLNpIhtxzGYT2R+PrgLXCujyHmibbwcCfgfUgTw0N7Uuwp9RbbU4g4DwT+cFg66wmCrBgScbjOIr9ZDI5TiZTonXgvsB2qX4XCtyjNejP0YWgX5uvMIw7UTTZTUfccnI4nlZSjvkZi7bJcOn6NSf8WfHVe668vDwRpMU4q4P4yJCBCtoAxHViAhcM9xDW7fs86+wEsXBjLce+vRJd5iS5qzkNLMoixGzRHMZbIjWd1DzPaPiOaU50vZrzEF92jmi32Dl7hPGRIAMVugG8rJcZbruZePpK0xrhGbuS7aqqqgns86g1Kbm1aU4D/ibcf53moF8sNTVrnsB4t3VfdD805yE+fUU+DpsL+CeWiwwZqN8bgMJnM9bez/0lybOc/bAiybMotpFjR1Bx4E8F8R7wL7Qc9A0y5gnrg36D7ofNjRCf3oA1nuPVpLUDgiTt9wakMm8CjJ2qeRS5VCY5jX3RfNUaxXexjZgr7AdoDgfxueBybICF6PpsAOZ+kT7c4mZpHly3xGQtNYAXmDQk0XrL5wNidjIWV8IUw9dLzhXSZ/uT1ig+vbgo4o5va4A/QJ4Pe+sLgxo/6gbQ2jhHWKf0OxKJxHirJ+A7ruK8PbO6yGACe2lGAWIaGYtFqjV8+iHIhy/7MsF2rVH8L2mfYz9Ac0j4YdYXBlfABlg+KhC7wOew6xAZDMZibbR8PvhnAGy65pFrFXl/1ormu9YoPn3mhD0DwDUH8bngBmkDUuZtS6GEOVDDDeuIBAbzfm75fKiurh4hA+d7CwosUmKPso3jDPb7+xYUBDdIGwDdB8t5MAfmet/ykSDBgbsL33wsTo3lPaSAg4a7potC+0hAkekv2Lq6uuGekFwNWoT+Nxfw/MgFN3gbwC/l/ZYnmANr2GT5vOCDRiax1/qK1Ge+dXg4c7YLxxi7kDzbsxvJs8WZNyNewuB+Kio9PrQpxeUFYrbKHK5bnwbSjsxXn4bLbADraNQ8b99Rc2ThMv8wmPAV7KUc38N6tA6TvAruueYsXOa3Rq8cA29n/FiiD9aCCb/A8bXVEOAfucybyGXqoZ1nNWFwmf8579zfNb2FtWP8UqPlr5E3Sse4Ps8pDfhbedvFnHZzbsqyX/2DgoLvbTH+DbjLlosxROBbSNgHSYwhAH/3Wi5GjBgxYsSIURj+AOH7vKGx+DomAAAAAElFTkSuQmCC>

[image30]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAAAZCAYAAACB6CjhAAADlUlEQVR4Xu1XTYiNURi+NP7z/3O5d2a+e2emrm5JTFggNVhIGhLKgpRsWNj4WSlSQkKyIJSwssFKs5AmG83IzxhKGqHRiCbGhBlmrucZ77ne+zr3uzPLqe+pt+97nvOc97znfOecuROLRYgwLJFKperxGGF1jSAI6qw27JHJZCZiYjnEAsQnxFbrIbBAdxBnrV4U2Wx2NDo0MjmeDbbdobKycj88veJbYdvDAP9tKf4laJltJ+DZgvYu+jDWNtsOvQeeHYozH73HEKvxflS0ft0vFDAvZKeqqqrJ5BUVFYvIPb5uRLPinYgT2uMDCp7CfHjOJcf7HHIuuvZBe4H4pngT4oHx5OLx+ATN3TvqTvCJcV4lEonxTi8JJkE89Wgtip/XgxE1NTUzreaD5CrwgT9H9DmOr7eJHjynGl+uurp6luamPZ+DQP9lQ9r6bhKIi1oHf6QH803C6RjwoNU1xPPKaKdM/tZi+TGpu5qzZs3du/CCBSkJFLZdCjyjdWj3TIE5O5jSO6zukE6nA/E80TomdUjGdcciLH9eR793gXwsbPMZeL+hvG1D2vqEKtDugLfUk8nkdOGDKtAHttsdAH5V+m4UzxtfHl9+8H7EzaDwCK1EnNa+QUMKfGY1Bi9EcrQftoVgwM2+Ai3EU7A1ZRLU9wmvI+dXdR6MOXsw+Qmdv7y8fBp4R2DutaLg6nEQdyuDHwBvkYFHOh94N4pqVPyjFNjrNB9QUJI+ToicFxX4fWpY4OXOB/4a8UXxH5I/dAF4LDDGOMe1H++/3HsocBTiKLABHVqRbB6ebb6B5ex+RZwjl4nlF6UYamtrRwV/t207Yi3iiuQv+D0AbQ/iA+K68FwQcrGhbQ3ipOLXEF2KX4qV+MXohQwc+mOCq04fFi9j20oBC/lQFiAU9MC7y+oOaP9t+E9Eq+P4OOtR32Lt+Q8yWXvRUNvguLsskXCJ01DYLdsP7WOh7dZa4NlNkqvg7zU1Hj/H0b7X9tNAWzv+JI4xGo9lfgE4B3ePFYVM9rvizYhO7UFhS+nj8SBXv8nT2icaJ5JV2sBZdhwTu6C5oEz67nSC8PxH0ED+dWg7YnXkPg6923G8X46VOgL4uvNlsPd8FjvTwb9Lj+eYvpT1QKpHW5PW8JUmSb/PiD5Em253YD/xDewYTHKV9TigvcdqDuwbk0kHJS7oYQnuoJj662QhC85/qB7btggRIkSIECFCHn8ADrtYCbOaca8AAAAASUVORK5CYII=>

[image31]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAI0AAAAWCAYAAAD9/x8lAAAAx0lEQVR4Xu3ZsQnCQBjF8cRCdADB4rjk4BoDtpYWbuEMrqFjiGPoAJY2DhB0A8FWQV/AwnzKYe//B69IXvtIjiTLAAAAAAA/CiFM7D0gqaqqblEUZ2Wvy9z2QEpHwzkqJ+dc35ZAkoazVa5lWQ5tByR57zcaz11PnrHtgCQNZ6U8dGie2g74SoNZNKPR62puO6BFQ1k2Y1FmtgNaNJK1clNGtgNadPDdaSiXGOPAdsC7XEM5KLXOLD1bAh9evxH4EgwAAIC/8QRUJB2pFymGPgAAAABJRU5ErkJggg==>

[image32]: <data:image/png;base64,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>

[image33]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE8AAAAaCAYAAAD2dwHCAAACjklEQVR4Xu2XO4gTURSGx8eKoOIqGwIhyU3SZDdgsQSxsBMbsRAsrEUrt1A70WJhi8ViwW4rC7sVWQQlBMTC3vejs1BEW1/L4nt9/IecK4c/k2weu06C94PD3PnPnXvPPWfmzkwUBQKBwH+Gc64G+92p8fVJUqlUtnB87Syfzx/kMfpCBz3GmpjVCoXCOGtJg3gWEPtdqyHOmxJnJpMZs7rGvsFqfZHNZndj0BrJGzV5T0iPEOhr1pIkrphxhfc6a32BAesRVQOVOysT4XjE6vqIXLJaklSr1REU8wrrmryfcTprfYHJz7CGST7ETYRkjhaLxTTrSYEYJ2K0SU1eU5Gx1tOsrTk6eVPyhgGnLz8pNPv+BZs0eQ/YMQwkWnjc2uc0gMPs88A3icreY30Q0NhXWBcQ81b4PmqfWfYL0A8gBxdZ7whcvNyuchh4Gv75QUweYtqniZljnwD9kWn/QP9r1q+6fLb1nDyZvGXyBEw6M4jJw6JvSeylUmkn+wTxpVKp7dJG/Od5nTh/gTFu9JQ887Xedr/rJnkY6wJrFoxzvNViBYmp07dkJ4X3oF8d9s2fY479sF09Jw+DXZbJZUHss0jy0O8+64zTTx7YFPsESdpqC/b+XC63l30W+5vGvjikXzqd3mbOF+TYVfJw0VHYZ9dY6Ds12fe+twpEk/eQdQZ/L3vQ7yXrFox1G31Osu6B7xDsMese+FZgSxr3W9d4IXyB/SqXyzu4vyD97d2O86e+3VXyekGT93fzHSYQ+zMcNksba1jU4x1jn2CvvG/N0eQ1/fcOOoj5KuyEbEuwU2h/jenzfN3uPK3Me9d4RJYireIw4HRPNHad/PK4y+Mv63tjfYFAIBAIBJLgD8ED68MifHS/AAAAAElFTkSuQmCC>