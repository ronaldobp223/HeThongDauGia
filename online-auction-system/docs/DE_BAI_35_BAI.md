# BỘ 35 ĐỀ BÀI THỰC HÀNH – ONLINE AUCTION SYSTEM

**Môn:** Lập trình nâng cao (LTNC 2026) · **Nhóm:** 4 người · **Công nghệ:** Java 25, Maven, JavaFX/FXML, TCP Socket + JSON, JUnit 5.  
**Mức độ:** BT01–BT32 là chuỗi bắt buộc do bộ đề này thiết kế; BT33–BT35 nâng cao nếu đủ thời gian.  
**Repository đã có:** `https://github.com/Ebi-27/online-auction-system.git` · Nhánh làm việc tích hợp: `dev`.

> **QUAN TRỌNG**: Đây là bộ bài tự biên soạn để *triển khai* bài tập lớn theo từng bước, **không phải** 35 bài riêng do giảng viên phát. Nội dung bám hai PDF `2026-Bài tập lớn` và `Hướng dẫn BTL LTNC-2026`. Một số quy tắc kỹ thuật cụ thể (VND nguyên, PBKDF2, cổng 8388, SQLite, JSON Lines, thời gian anti-sniping) được **nhóm/bộ đề này tự chuẩn hóa** để 4 người lập trình nhất quán. PDF không bắt buộc SQLite hay thuật toán hash/cổng mạng cụ thể.

## A. QUY TẮC ÁP DỤNG CHO TẤT CẢ BÀI

1. **Làm trên một dự án duy nhất:** dùng đúng cây thư mục này, không tạo 35 thư mục Maven mới. Mỗi bài sửa đúng các file trong mục “File phải sửa” và chỉ thêm file mới khi có giải thích trong PR.
2. **Không có sẵn lời giải:** mọi `.java` trong ZIP chỉ có `package`, khai báo class/interface/enum và `TODO`; FXML/CSS/SQL chỉ là chỗ trống. Người học tự viết thuộc tính, constructor, phương thức và test. Các enum có sẵn tên hằng số để cả nhóm thống nhất giao thức.
3. **Chuẩn dữ liệu:** ID chuỗi duy nhất (`U001`, `I001`, `A001`, `B001` trong ví dụ); thời điểm dùng `Instant` hoặc UTC thống nhất ở Server; tiền VND nguyên dương dùng `BigDecimal`, không dùng `double` để cộng/trừ tiền. Username không phân biệt hoa/thường khi kiểm tra trùng.
4. **Kiểm tra quyền ở Server:** mọi thao tác thay đổi tài khoản, sản phẩm, phiên, bid phải kiểm tra trạng thái đăng nhập/quyền sở hữu ở tầng nghiệp vụ phía Server, không tin vào nút đang hiện trên GUI.
5. **Quy tắc giá:** trước bid đầu tiên, `currentPrice = startingPrice`. Mỗi bid hợp lệ phải `amount > currentPrice`; chỉ Bidder tham gia khi trạng thái RUNNING và `now < endTime`; người bán không tự bid phiên mình.
6. **Trạng thái:** `OPEN → RUNNING → FINISHED → PAID / CANCELED`. Chỉ Server thay đổi trạng thái và xác định người thắng; hai nhánh PAID, CANCELED là cuối.
7. **Realtime:** các Client subscribe tới phiên qua Socket; Server gửi sự kiện sau khi bid đã được chấp nhận. **Không polling liên tục**, không gửi event bid giả khi giá không hợp lệ.
8. **Concurrency:** bảo vệ đồng bộ đọc-kiểm tra-ghi giá, cập nhật leader, chèn lịch sử bid; ghi dữ liệu phải nhất quán khi nhiều người thao tác cùng lúc.
9. **Code style:** lớp PascalCase, biến/phương thức camelCase, hằng UPPER_SNAKE_CASE; không để code nghiệp vụ trong FXML Controller; loại bỏ `System.out.println` debug trước khi nộp.
10. **Nộp bài:** nhánh riêng `feature/btXX-ten-ngan`, commit đúng phạm vi, test, gửi PR **vào dev**; ít nhất một bạn khác review; mọi người phải hiểu cả hệ thống. Không push thẳng `main`.
11. **Hoàn thành:** chỉ tính đạt khi đáp ứng toàn bộ yêu cầu, có minh chứng test chạy, có commit/PR, và giải thích được vì sao code đúng. `mvn test` thành công trong bộ khung mới nhưng chưa có `@Test` **không được tính** là đã kiểm thử.

## B. TIẾN ĐỘ VÀ PHÂN CHIA

| Chặng | Bài | Sản phẩm có thể trình diễn |
|---|---|---|
| Dự án 01 | BT01–BT04 | Repository sạch, Maven, menu console, JUnit đầu tiên |
| Dự án 02 | BT05–BT08 | Tài khoản, kế thừa User, login, phân quyền |
| Dự án 03 | BT09–BT12 | Item phân loại, factory và CRUD sản phẩm |
| Dự án 04 | BT13–BT16 | Một phiên đấu giá được tạo, bid, kết thúc |
| Dự án 05 | BT17–BT20 | Ngoại lệ, Thread-safe, Observer, Singleton |
| Dự án 06 | BT21–BT24 | Lưu/khôi phục, Socket đa Client, JSON và broadcast |
| Dự án 07 | BT25–BT28 | JavaFX login, danh sách, bid realtime, Seller |
| Dự án 08 | BT29–BT32 | Client-Server-MVC tích hợp, test, CI, demo |
| Nâng cao | BT33–BT35 | Auto-bidding, anti-sniping, price line chart |

### Chia vai nhóm (người phụ trách chính, KHÔNG độc quyền code)

- **TV1 – OOP/model:** BT05, BT06, BT09, BT10, BT13, BT14.  
- **TV2 – Business/user/product:** BT07, BT08, BT11, BT12, BT15, BT16, BT17.  
- **TV3 – Concurrent/network/data:** BT18–BT24.  
- **TV4 – JavaFX/MVC:** BT25–BT29.  
- **Cả nhóm:** BT01–BT04, BT30–BT32 và BT33–BT35 (nếu thực hiện).  
Mỗi PR phải có người **khác** đọc thử. Cả 4 luân phiên demo và giải thích luồng `GUI → Socket → Server → Service → Repository`.

### Hướng dẫn tự chấm từng bài (thang 10 nội bộ)

- 2 điểm: thiết kế đúng class, package, kiểu dữ liệu, phạm vi trách nhiệm.
- 4 điểm: nghiệp vụ chính chạy đúng trong điều kiện bình thường.
- 2 điểm: kiểm thử đủ các ca bắt buộc + lỗi/biên.
- 2 điểm: code sạch, có commit/PR, giải thích được code.

> Đây là **thang tự luyện nội bộ**, không phải điểm chính thức của giảng viên. Rubric chính thức theo PDF chú trọng OOP, design patterns, user/product, bidding, exceptions, concurrency, realtime, Client-Server/MVC, Maven/JUnit và CI/CD.

## C. CHUẨN HÓA API ĐỂ 4 NGƯỜI DỄ GHÉP

Các tên dưới đây là **hợp đồng nhóm đề xuất**, học viên tự quyết định bổ sung kiểu DTO tương ứng nếu cần; không phải code được viết sẵn. Trước khi làm một module, cả nhóm chốt chính xác kiểu tham số/kiểu trả về trong PR thiết kế.

```java
// Minh họa mục tiêu thiết kế, KHÔNG phải code có sẵn trong skeleton:
MoneyValidator.isPositiveWholeVnd(BigDecimal value);
UserService.register(String username, String rawPassword, Role role);
UserService.login(String username, String rawPassword);
AuthorizationService.requireRole(User actor, Role requiredRole);
ProductService.findById(String itemId);
AuctionService.placeBid(String auctionId, String bidderId, BigDecimal amount);
AuctionLifecycleService.closeExpiredAuctions(Instant now);
AuctionManager.getInstance();
```

**Điểm phải thống nhất trước khi code:** ID đối tượng, cách ném exception, tên JSON `type/requestId/payload`, biểu diễn tiền, vòng đời phiên, cách lưu mật khẩu, đầu vào/đầu ra của Repository. Tránh việc 2 người tự định nghĩa 2 kiểu `Auction` khác nhau.

## D. ĐỀ BÀI TỪ CƠ BẢN ĐẾN HOÀN CHỈNH


---

## DỰ ÁN 01 – KHỞI TẠO DỰ ÁN & GIT

### BÀI 01/35 – Quy trình làm việc Git cho nhóm 4 người

**Dự án:** DỰ ÁN 01 – KHỞI TẠO DỰ ÁN & GIT  
**Bài trước cần hoàn thành:** Repository đã clone, origin/dev tồn tại  
**Kiến thức luyện tập:** Git, branch, PR, commit  
**Tên nhánh:** `feature/bt01-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **quy trình làm việc git cho nhóm 4 người** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Từ repository hiện có, kiểm tra main và origin/dev; không commit trực tiếp lên main.
2. Tạo nhánh feature/bt01-git-intro xuất phát từ dev mới nhất.
3. Viết CONTRIBUTING: tên nhánh, quy tắc commit, quy trình PR, xử lý conflict và trách nhiệm review.
4. Push nhánh, tạo PR vào dev; bạn khác review, sau đó merge.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `README.md`
- `.gitignore`
- `docs/CONTRIBUTING.md`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. git status sau commit cho thấy working tree clean.
2. git log --oneline thể hiện commit riêng cho BT01.
3. PR có đúng base dev, không phải main.
4. Ít nhất một review từ bạn khác và có link PR.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `docs: add collaboration guidelines`

### BÀI 02/35 – Tổ chức dự án Java 25 bằng Maven

**Dự án:** DỰ ÁN 01 – KHỞI TẠO DỰ ÁN & GIT  
**Bài trước cần hoàn thành:** BT01  
**Kiến thức luyện tập:** Maven, JDK 25, package  
**Tên nhánh:** `feature/bt02-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **tổ chức dự án java 25 bằng maven** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Khảo sát cấu trúc src/main/java, src/test/java và resources trong bộ khung.
2. Kiểm tra Maven Compiler đặt release=25; biên dịch mọi lớp trống trong skeleton.
3. Giữ quy ước tên package vn.edu.uet.auction.*, không để các lớp ở default package.
4. Ghi hướng dẫn chạy mvn clean compile và mvn test vào README.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `pom.xml`
- `src/main/java/vn/edu/uet/auction/app/AuctionConsoleApp.java`
- `README.md`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. java -version cho thấy JDK 25 trên máy.
2. mvn -version xác nhận Maven đang dùng Java 25.
3. mvn clean compile thành công.
4. Không có file .class, target hoặc .idea bị commit.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `build: initialize java 25 maven project`

### BÀI 03/35 – Menu console đấu giá lần đầu

**Dự án:** DỰ ÁN 01 – KHỞI TẠO DỰ ÁN & GIT  
**Bài trước cần hoàn thành:** BT02  
**Kiến thức luyện tập:** switch, while, Scanner, input  
**Tên nhánh:** `feature/bt03-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **menu console đấu giá lần đầu** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Viết public static void main(String[] args) hiển thị các lựa chọn 1-Người dùng, 2-Sản phẩm, 3-Đấu giá, 0-Thoát.
2. Nhận số nguyên từ terminal; vòng lặp chạy đến khi chọn 0.
3. Ở giai đoạn này từng lựa chọn chỉ cần in ra tên mục; chưa viết nghiệp vụ thật.
4. Xử lý ký tự không phải số, số không tồn tại mà không làm chương trình dừng đột ngột.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/app/AuctionConsoleApp.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Nhập 1 => in mục Người dùng.
2. Nhập abc => báo không hợp lệ, menu tiếp tục.
3. Nhập 9 => không có lựa chọn; nhập 0 => kết thúc.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add console navigation`

### BÀI 04/35 – Kiểm thử giá tiền với JUnit

**Dự án:** DỰ ÁN 01 – KHỞI TẠO DỰ ÁN & GIT  
**Bài trước cần hoàn thành:** BT03  
**Kiến thức luyện tập:** BigDecimal, JUnit 5, assert  
**Tên nhánh:** `feature/bt04-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **kiểm thử giá tiền với junit** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Thiết kế phương thức kiểm tra giá khởi điểm > 0 dùng BigDecimal; null là không hợp lệ.
2. Định nghĩa rõ độ chính xác tiền: hệ thống demo dùng VND nguyên (scale=0).
3. Tạo ít nhất 5 @Test: null, âm, 0, số dương, số thập phân.
4. Không dùng double/float để cộng so sánh số tiền đấu giá.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/util/MoneyValidator.java`
- `src/test/java/vn/edu/uet/auction/util/MoneyValidatorTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. null, -10, 0 => không hợp lệ.
2. 1, 5000000 => hợp lệ.
3. 1000.5 => không hợp lệ theo quy ước VND nguyên.
4. mvn test thực sự chạy >=5 test mới, không chỉ BUILD SUCCESS vì không có test.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `test: add money validation tests`


---

## DỰ ÁN 02 – QUẢN LÝ NGƯỜI DÙNG (OOP)

### BÀI 05/35 – Lớp cơ sở trừu tượng Entity

**Dự án:** DỰ ÁN 02 – QUẢN LÝ NGƯỜI DÙNG (OOP)  
**Bài trước cần hoàn thành:** BT04  
**Kiến thức luyện tập:** abstract, encapsulation, UUID, Instant  
**Tên nhánh:** `feature/bt05-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **lớp cơ sở trừu tượng entity** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Entity là abstract class, quản lý id (không rỗng) và createdAt (không null).
2. Giữ thuộc tính private, cung cấp getter cần thiết; không public setter tùy tiện cho id.
3. Khai báo abstract String displayInfo() để lớp con ghi đè.
4. Bảo đảm id không thay đổi sau khi tạo đối tượng.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/model/Entity.java`
- `src/test/java/vn/edu/uet/auction/model/UserModelTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Không thể new Entity trực tiếp.
2. ID và createdAt truy cập được qua getter.
3. Tạo id rỗng / createdAt null bị từ chối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add abstract entity model`

### BÀI 06/35 – Đa hình với Bidder, Seller, Admin

**Dự án:** DỰ ÁN 02 – QUẢN LÝ NGƯỜI DÙNG (OOP)  
**Bài trước cần hoàn thành:** BT05  
**Kiến thức luyện tập:** inheritance, override, enum Role  
**Tên nhánh:** `feature/bt06-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **đa hình với bidder, seller, admin** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. User là abstract class extends Entity, có username, passwordHash, role.
2. Ba lớp Bidder/Seller/Admin extends User và override displayInfo(); khác biệt role phải tường minh.
3. Áp dụng List<User> chứa ít nhất mỗi loại 1 đối tượng và gọi displayInfo() đa hình.
4. Không công khai passwordHash trong displayInfo hoặc toString.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/model/User.java`
- `src/main/java/vn/edu/uet/auction/model/Bidder.java`
- `src/main/java/vn/edu/uet/auction/model/Seller.java`
- `src/main/java/vn/edu/uet/auction/model/Admin.java`
- `src/main/java/vn/edu/uet/auction/model/Role.java`
- `src/test/java/vn/edu/uet/auction/model/UserModelTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. 3 loại user có role chính xác.
2. List<User> gọi đúng override tương ứng.
3. Username trống bị từ chối.
4. Không in password hoặc hash trong log và console.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: create user inheritance hierarchy`

### BÀI 07/35 – Đăng ký, đăng nhập và lưu tạm tài khoản

**Dự án:** DỰ ÁN 02 – QUẢN LÝ NGƯỜI DÙNG (OOP)  
**Bài trước cần hoàn thành:** BT06  
**Kiến thức luyện tập:** HashMap, password hashing, service  
**Tên nhánh:** `feature/bt07-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **đăng ký, đăng nhập và lưu tạm tài khoản** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Đăng ký tài khoản Bidder/Seller với username duy nhất sau khi chuẩn hóa trim và không phân biệt hoa-thường; Admin do hệ thống tạo sẵn.
2. Mật khẩu tối thiểu 8 ký tự; không lưu plaintext, dùng PBKDF2-HMAC-SHA256 với salt ngẫu nhiên mỗi tài khoản.
3. Lưu tạm qua InMemoryUserRepository; UserService không thao tác trực tiếp với HashMap của repository.
4. Đăng nhập kiểm tra username/password; không tiết lộ tài khoản có tồn tại hay không qua thông báo lỗi.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/service/UserService.java`
- `src/main/java/vn/edu/uet/auction/util/PasswordHasher.java`
- `src/main/java/vn/edu/uet/auction/repository/UserRepository.java`
- `src/main/java/vn/edu/uet/auction/repository/memory/InMemoryUserRepository.java`
- `src/main/java/vn/edu/uet/auction/exception/DuplicateUsernameException.java`
- `src/main/java/vn/edu/uet/auction/exception/AuthenticationException.java`
- `src/test/java/vn/edu/uet/auction/service/UserServiceTest.java`
- `src/test/java/vn/edu/uet/auction/util/PasswordHasherTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Đăng ký tan rồi đăng ký TAN => từ chối trùng.
2. Sai password => AuthenticationException.
3. 2 người cùng mật khẩu có salt/hash lưu khác nhau.
4. Tài khoản hợp lệ đăng nhập, trả về dữ liệu an toàn không bao gồm password.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: implement in-memory authentication`

### BÀI 08/35 – Phân quyền theo vai trò

**Dự án:** DỰ ÁN 02 – QUẢN LÝ NGƯỜI DÙNG (OOP)  
**Bài trước cần hoàn thành:** BT07  
**Kiến thức luyện tập:** authorization, OOP  
**Tên nhánh:** `feature/bt08-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **phân quyền theo vai trò** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Định nghĩa 3 hành động: PLACE_BID, MANAGE_OWN_ITEMS, ADMIN_MANAGE_USERS.
2. Chỉ BIDDER đặt giá, chỉ SELLER quản lý sản phẩm của mình, chỉ ADMIN dùng chức năng quản trị.
3. Phân quyền phải kiểm tra tại tầng service phía Server; không phụ thuộc nút bị ẩn ở GUI.
4. Nếu người dùng chưa xác thực hoặc role sai thì ném ngoại lệ rõ nghĩa.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/service/AuthorizationService.java`
- `src/main/java/vn/edu/uet/auction/exception/UnauthorizedActionException.java`
- `src/test/java/vn/edu/uet/auction/service/AuthorizationServiceTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Seller gọi PLACE_BID => từ chối.
2. Bidder gọi MANAGE_OWN_ITEMS => từ chối.
3. Admin gọi ADMIN_MANAGE_USERS => hợp lệ.
4. User null => từ chối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: enforce role permissions`


---

## DỰ ÁN 03 – QUẢN LÝ SẢN PHẨM (CRUD & FACTORY)

### BÀI 09/35 – Mô hình sản phẩm đấu giá Item

**Dự án:** DỰ ÁN 03 – QUẢN LÝ SẢN PHẨM (CRUD & FACTORY)  
**Bài trước cần hoàn thành:** BT08  
**Kiến thức luyện tập:** abstract class, BigDecimal, validation  
**Tên nhánh:** `feature/bt09-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **mô hình sản phẩm đấu giá item** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Item abstract extends Entity, thuộc tính sellerId, name, description, startingPrice, itemType.
2. Tên sản phẩm 3–100 ký tự; mô tả không rỗng; startingPrice là VND nguyên >0.
3. Cung cấp getter, chỉ sửa tên/mô tả trước khi phiên RUNNING; logic kiểm tra trạng thái thực hiện ở service.
4. Khai báo các loại ELECTRONICS, ART, VEHICLE trong enum ItemType.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/model/Item.java`
- `src/main/java/vn/edu/uet/auction/model/ItemType.java`
- `src/test/java/vn/edu/uet/auction/model/ItemModelTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Giá -1/0 bị từ chối; giá 5000000 hợp lệ.
2. Tên 2 ký tự bị từ chối.
3. sellerId rỗng bị từ chối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: define auction item abstraction`

### BÀI 10/35 – Ba loại sản phẩm kế thừa Item

**Dự án:** DỰ ÁN 03 – QUẢN LÝ SẢN PHẨM (CRUD & FACTORY)  
**Bài trước cần hoàn thành:** BT09  
**Kiến thức luyện tập:** override, polymorphism  
**Tên nhánh:** `feature/bt10-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **ba loại sản phẩm kế thừa item** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Electronics có brand và warrantyMonths >=0.
2. Art có artist và material; Vehicle có manufacturer và manufactureYear hợp lệ.
3. Cả 3 override displayInfo() khác nhau, không lặp code thuộc tính chung.
4. Tạo List<Item> chứa cả 3 loại, in danh sách theo đa hình.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/model/Electronics.java`
- `src/main/java/vn/edu/uet/auction/model/Art.java`
- `src/main/java/vn/edu/uet/auction/model/Vehicle.java`
- `src/test/java/vn/edu/uet/auction/model/ItemModelTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Electronics warrantyMonths=-1 => lỗi.
2. Vehicle manufactureYear ngoài khoảng hợp lý => lỗi.
3. Đa hình xuất đúng thuộc tính từng loại.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add specialized item types`

### BÀI 11/35 – Nhà máy sản phẩm ItemFactory

**Dự án:** DỰ ÁN 03 – QUẢN LÝ SẢN PHẨM (CRUD & FACTORY)  
**Bài trước cần hoàn thành:** BT10  
**Kiến thức luyện tập:** Factory Method / simple factory  
**Tên nhánh:** `feature/bt11-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **nhà máy sản phẩm itemfactory** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Tạo factory hỗ trợ ba ItemType, dùng dữ liệu chung và cấu hình loại sản phẩm.
2. Tách việc khởi tạo cụ thể Electronics/Art/Vehicle khỏi tầng GUI/Controller.
3. Báo InvalidDataException khi thiếu thuộc tính bắt buộc theo từng loại.
4. Viết ví dụ tạo ba sản phẩm qua factory, tránh if/else tạo Item rải rác.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/factory/ItemFactory.java`
- `src/test/java/vn/edu/uet/auction/factory/ItemFactoryTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. ELECTRONICS => instanceof Electronics.
2. ART => instanceof Art.
3. VEHICLE thiếu manufacturer => lỗi.
4. ItemType null => lỗi.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: implement item factory`

### BÀI 12/35 – CRUD và tìm kiếm sản phẩm

**Dự án:** DỰ ÁN 03 – QUẢN LÝ SẢN PHẨM (CRUD & FACTORY)  
**Bài trước cần hoàn thành:** BT11  
**Kiến thức luyện tập:** collections, CRUD, ownership  
**Tên nhánh:** `feature/bt12-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **crud và tìm kiếm sản phẩm** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Seller thêm, tìm ID, lọc theo tên không phân biệt hoa-thường và liệt kê sản phẩm của mình.
2. Chỉ chủ sở hữu được sửa/xóa; item có phiên RUNNING hoặc đã có bid không được tùy tiện xóa hay thay đổi giá khởi điểm.
3. Sản phẩm không có thật phải trả kết quả rõ ràng/ngoại lệ, không được NullPointerException.
4. Sử dụng repository để cất dữ liệu; service không truy cập file hay JDBC trực tiếp.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/service/ProductService.java`
- `src/main/java/vn/edu/uet/auction/repository/ItemRepository.java`
- `src/main/java/vn/edu/uet/auction/repository/memory/InMemoryItemRepository.java`
- `src/test/java/vn/edu/uet/auction/service/ProductServiceTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Seller A sửa item của Seller B => từ chối.
2. Tìm laptop tìm thấy Laptop Dell.
3. Xóa item chưa có phiên => thành công.
4. Xóa item đang RUNNING => từ chối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: implement item CRUD service`


---

## DỰ ÁN 04 – ĐẤU GIÁ CƠ BẢN

### BÀI 13/35 – Mô hình Auction và trạng thái

**Dự án:** DỰ ÁN 04 – ĐẤU GIÁ CƠ BẢN  
**Bài trước cần hoàn thành:** BT12  
**Kiến thức luyện tập:** association, state, time  
**Tên nhánh:** `feature/bt13-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **mô hình auction và trạng thái** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Auction có auctionId, itemId, sellerId, startTime, endTime, startingPrice, currentPrice, leaderId, status.
2. Mỗi Item có tối đa một phiên RUNNING; startTime phải trước endTime.
3. Trạng thái theo đề: OPEN, RUNNING, FINISHED, PAID, CANCELED.
4. Giá hiện tại bắt đầu bằng giá khởi điểm, leaderId ban đầu chưa có; chỉ đổi qua phương thức nghiệp vụ.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/model/Auction.java`
- `src/main/java/vn/edu/uet/auction/model/AuctionStatus.java`
- `src/test/java/vn/edu/uet/auction/model/AuctionModelTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. endTime <= startTime => từ chối.
2. Auction mới: OPEN, leader null, giá hiện tại = giá khởi điểm.
3. ID đấu giá duy nhất.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: model auction lifecycle`

### BÀI 14/35 – Mỗi lần trả giá là một BidTransaction

**Dự án:** DỰ ÁN 04 – ĐẤU GIÁ CƠ BẢN  
**Bài trước cần hoàn thành:** BT13  
**Kiến thức luyện tập:** immutable data, timestamp, list  
**Tên nhánh:** `feature/bt14-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **mỗi lần trả giá là một bidtransaction** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. BidTransaction chứa id, auctionId, bidderId, amount, createdAt.
2. Giao dịch sau khi tạo không thay đổi amount hay bidderId.
3. Một phiên lưu lịch sử bid theo thứ tự thời gian chấp nhận, dùng thêm số thứ tự sequence khi cần phá hòa timestamp.
4. Không lưu bid không hợp lệ vào danh sách bid thành công.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/model/BidTransaction.java`
- `src/test/java/vn/edu/uet/auction/model/BidTransactionTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Giao dịch tạo ra có timestamp.
2. Không có public setter để đổi amount.
3. 3 bid thành công => lịch sử 3 bản ghi theo thứ tự.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add immutable bid records`

### BÀI 15/35 – Xử lý placeBid() và xác định người dẫn đầu

**Dự án:** DỰ ÁN 04 – ĐẤU GIÁ CƠ BẢN  
**Bài trước cần hoàn thành:** BT14  
**Kiến thức luyện tập:** business validation, BigDecimal  
**Tên nhánh:** `feature/bt15-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **xử lý placebid() và xác định người dẫn đầu** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Chỉ Bidder xác thực được đặt bid trong phiên RUNNING và trước endTime.
2. Bid amount là VND nguyên và phải LỚN HƠN currentPrice (không chấp nhận bằng); Seller không tự bid phiên mình.
3. Sau bid thành công phải cập nhật nguyên tử currentPrice, leaderId và bid history (về sau khóa đồng bộ ở BT18).
4. Trả về kết quả đủ auctionId, giá hiện tại, người dẫn đầu, thời gian chấp nhận.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/service/AuctionService.java`
- `src/main/java/vn/edu/uet/auction/exception/InvalidBidException.java`
- `src/main/java/vn/edu/uet/auction/repository/BidRepository.java`
- `src/main/java/vn/edu/uet/auction/repository/memory/InMemoryBidRepository.java`
- `src/test/java/vn/edu/uet/auction/service/AuctionServiceTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. 5.000.000 -> bid 6.000.000: chấp nhận, leader thay đổi.
2. Giá 6.000.000/5.500.000 tiếp theo => từ chối.
3. Seller tự bid => từ chối.
4. Bid khi OPEN/FINISHED => từ chối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: implement manual bidding`

### BÀI 16/35 – Kết thúc phiên và tìm người thắng

**Dự án:** DỰ ÁN 04 – ĐẤU GIÁ CƠ BẢN  
**Bài trước cần hoàn thành:** BT15  
**Kiến thức luyện tập:** max, state transitions  
**Tên nhánh:** `feature/bt16-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **kết thúc phiên và tìm người thắng** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Đóng phiên RUNNING khi now >= endTime, chuyển FINISHED một lần duy nhất.
2. Nếu có bid hợp lệ: winner = leaderId cuối cùng; nếu không có bid: winner = null.
3. Khi FINISHED không nhận thêm bid.
4. Viết công việc tự động quét phiên hết hạn (ScheduledExecutorService) hoặc cơ chế kích hoạt tương đương phía Server.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/service/AuctionLifecycleService.java`
- `src/test/java/vn/edu/uet/auction/service/AuctionLifecycleServiceTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Không bid -> không có winner.
2. Bid 6 triệu rồi 7 triệu -> người đặt 7 triệu thắng.
3. Chạy đóng hai lần không tạo 2 winner, không làm sai dữ liệu.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: finalize auctions and resolve winners`


---

## DỰ ÁN 05 – ĐA LUỒNG, NGOẠI LỆ, OBSERVER

### BÀI 17/35 – Ngoại lệ riêng và luật chuyển trạng thái

**Dự án:** DỰ ÁN 05 – ĐA LUỒNG, NGOẠI LỆ, OBSERVER  
**Bài trước cần hoàn thành:** BT16  
**Kiến thức luyện tập:** custom exception, finite state machine  
**Tên nhánh:** `feature/bt17-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **ngoại lệ riêng và luật chuyển trạng thái** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Xây dựng phương thức chuyển OPEN->RUNNING->FINISHED->PAID/CANCELED theo bảng chuyển hợp lệ.
2. FINISHED->PAID chỉ sau thao tác thanh toán mô phỏng, FINISHED->CANCELED theo quy tắc rõ ràng.
3. Không cho PAID->RUNNING, CANCELED->PAID, RUNNING->OPEN, và không bid khi CLOSED.
4. Map lỗi thành mã ổn định: INVALID_BID, AUCTION_CLOSED, UNAUTHORIZED, INVALID_STATE, DATA_ERROR.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/exception/InvalidAuctionStateException.java`
- `src/main/java/vn/edu/uet/auction/exception/AuctionClosedException.java`
- `src/main/java/vn/edu/uet/auction/exception/InvalidDataException.java`
- `src/main/java/vn/edu/uet/auction/service/AuctionLifecycleService.java`
- `src/test/java/vn/edu/uet/auction/service/AuctionLifecycleServiceTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. OPEN->RUNNING hợp lệ.
2. FINISHED->PAID hợp lệ.
3. PAID->RUNNING bị từ chối.
4. Bid khi FINISHED ném AuctionClosedException.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add auction state guards and exceptions`

### BÀI 18/35 – 100 luồng đặt giá cùng lúc

**Dự án:** DỰ ÁN 05 – ĐA LUỒNG, NGOẠI LỆ, OBSERVER  
**Bài trước cần hoàn thành:** BT17  
**Kiến thức luyện tập:** ExecutorService, Lock, race condition  
**Tên nhánh:** `feature/bt18-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **100 luồng đặt giá cùng lúc** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Mô phỏng 100 bidder/requests gửi cùng lúc vào một phiên RUNNING.
2. Dùng synchronized hoặc ReentrantLock theo từng Auction để bảo vệ: xác minh giá, cập nhật giá, leader, lưu bid trong một vùng critical section.
3. Sau vùng khóa mới phát event/notify để tránh observer gây deadlock.
4. Đảm bảo cuối cùng currentPrice bằng giá trị bid thành công cao nhất; số bid lưu bằng số lệnh chấp nhận.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/service/AuctionService.java`
- `src/test/java/vn/edu/uet/auction/service/ConcurrentBiddingTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Đồng thời 6 triệu và 7 triệu từ mốc 5 triệu -> cuối cùng 7 triệu.
2. 100 task không sinh lost update, không có 2 winner.
3. Bất biến: lịch sử chấp nhận tăng giá nghiêm ngặt.
4. Lặp test nhiều lần, không dùng Thread.sleep ngẫu nhiên để kiểm tra chính.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: make bidding thread safe`

### BÀI 19/35 – Thông báo thay đổi bằng Observer Pattern

**Dự án:** DỰ ÁN 05 – ĐA LUỒNG, NGOẠI LỆ, OBSERVER  
**Bài trước cần hoàn thành:** BT18  
**Kiến thức luyện tập:** observer, event-based, thread safety  
**Tên nhánh:** `feature/bt19-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **thông báo thay đổi bằng observer pattern** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Định nghĩa cơ chế subscribe/unsubscribe theo auctionId.
2. Mỗi bid thành công phát một AuctionEvent gồm auctionId, giá mới, người dẫn đầu, thời gian.
3. Một Observer lỗi không làm mất dữ liệu đấu giá hay ngăn Observer khác nhận event.
4. Chỉ phát event cho bid đã chấp nhận, không phát khi bid sai.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/observer/AuctionObserver.java`
- `src/main/java/vn/edu/uet/auction/observer/AuctionEvent.java`
- `src/main/java/vn/edu/uet/auction/observer/AuctionEventPublisher.java`
- `src/test/java/vn/edu/uet/auction/observer/AuctionEventPublisherTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. 2 observer cùng theo dõi => đều nhận 1 thông báo sau bid hợp lệ.
2. Unsubscribe => không còn nhận.
3. Bid thất bại => 0 event.
4. Một observer ném lỗi không làm hỏng bid.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: publish bid events using observer`

### BÀI 20/35 – Singleton AuctionManager quản lý nhiều phiên

**Dự án:** DỰ ÁN 05 – ĐA LUỒNG, NGOẠI LỆ, OBSERVER  
**Bài trước cần hoàn thành:** BT19  
**Kiến thức luyện tập:** singleton, collection, concurrency  
**Tên nhánh:** `feature/bt20-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **singleton auctionmanager quản lý nhiều phiên** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Thiết lập đúng một AuctionManager hoặc instance theo application-context với Singleton; mô tả lý do dùng.
2. Đăng ký/tìm/lấy danh sách auction, không trả về collection cho caller sửa tùy ý.
3. Tránh duplicate auctionId và bảo đảm thread safety khi nhiều request tạo phiên.
4. Tích hợp AuctionManager với service qua constructor hoặc API thích hợp; không biến Singleton thành nơi chứa mọi logic.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/pattern/AuctionManager.java`
- `src/test/java/vn/edu/uet/auction/pattern/AuctionManagerTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Gọi getInstance 2 lần cho cùng tham chiếu.
2. Trùng auctionId => từ chối.
3. Thêm 100 phiên khác id đồng thời không mất dữ liệu.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: create singleton auction manager`


---

## DỰ ÁN 06 – LƯU TRỮ VÀ LẬP TRÌNH MẠNG

### BÀI 21/35 – Lưu và khôi phục bằng File I/O

**Dự án:** DỰ ÁN 06 – LƯU TRỮ VÀ LẬP TRÌNH MẠNG  
**Bài trước cần hoàn thành:** BT20  
**Kiến thức luyện tập:** Serialization, try-with-resources  
**Tên nhánh:** `feature/bt21-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **lưu và khôi phục bằng file i/o** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Lưu danh sách User/Item/Auction/BidTransaction vào file dữ liệu local (chỉ Server truy cập).
2. Khởi động lại và khôi phục đúng id, trạng thái, leader và lịch sử bid.
3. Dùng try-with-resources, ghi qua file tạm rồi thay thế để giảm nguy cơ file hỏng.
4. Không deserialze ObjectInputStream trực tiếp từ dữ liệu Socket/nguồn không tin cậy; JSON cho giao tiếp mạng.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/io/FileDataStore.java`
- `src/test/java/vn/edu/uet/auction/io/FileDataStoreTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Tạo 2 user, 1 auction và 2 bid -> save/load đầy đủ.
2. File không tồn tại lần đầu => khởi động rỗng, không crash.
3. Lỗi file bị hỏng => thông báo lỗi có kiểm soát.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: persist auction data to files`

### BÀI 22/35 – Repository/DAO và cơ sở dữ liệu SQLite

**Dự án:** DỰ ÁN 06 – LƯU TRỮ VÀ LẬP TRÌNH MẠNG  
**Bài trước cần hoàn thành:** BT21  
**Kiến thức luyện tập:** JDBC, DAO, SQL transactions  
**Tên nhánh:** `feature/bt22-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **repository/dao và cơ sở dữ liệu sqlite** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Thiết kế các bảng users, items, auctions, bids, ràng buộc FK, UNIQUE username và ID.
2. Viết Repository/DAO CRUD qua JDBC; service không được phụ thuộc vào câu SQL cụ thể.
3. Khi bid, cập nhật giá hiện tại + leader + chèn bid trong 1 transaction để tránh dữ liệu nửa chừng.
4. Cho phép chọn backend SQLite hoặc FileDataStore bằng cấu hình; SQLite là lựa chọn mở rộng của lộ trình, không phải yêu cầu công nghệ bắt buộc của PDF.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/repository/jdbc/DatabaseManager.java`
- `src/main/java/vn/edu/uet/auction/repository/jdbc/SqliteUserRepository.java`
- `src/main/java/vn/edu/uet/auction/repository/jdbc/SqliteItemRepository.java`
- `src/main/java/vn/edu/uet/auction/repository/jdbc/SqliteAuctionRepository.java`
- `src/main/java/vn/edu/uet/auction/repository/jdbc/SqliteBidRepository.java`
- `src/main/resources/db/schema.sql`
- `src/test/java/vn/edu/uet/auction/repository/jdbc/SqliteRepositoryTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Insert/find user thành công; duplicate username bị chặn.
2. Restart DB -> dữ liệu vẫn còn.
3. Rollback transaction nếu insert bid lỗi.
4. Không có lớp Client kết nối SQLite.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add sqlite persistence layer`

### BÀI 23/35 – TCP Echo Server và Client

**Dự án:** DỰ ÁN 06 – LƯU TRỮ VÀ LẬP TRÌNH MẠNG  
**Bài trước cần hoàn thành:** BT22  
**Kiến thức luyện tập:** ServerSocket, Socket, streams  
**Tên nhánh:** `feature/bt23-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **tcp echo server và client** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Tạo ServerSocket trên localhost:8388 (cấu hình đổi được).
2. Client TCP gửi chuỗi PING; Server trả PONG và có thể trả lại một chuỗi ECHO.
3. Dùng UTF-8, try-with-resources/close phù hợp; phát hiện Server không chạy và báo lỗi.
4. Server có thể kết nối ít nhất 2 Client lần lượt; đây là bài thực hành mạng tách khỏi giao diện.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/app/ServerMain.java`
- `src/main/java/vn/edu/uet/auction/network/server/AuctionServer.java`
- `src/main/java/vn/edu/uet/auction/network/client/AuctionClient.java`
- `src/main/java/vn/edu/uet/auction/app/ClientMain.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Client gửi PING => nhận PONG.
2. Không có server => Client báo kết nối thất bại, không crash khó hiểu.
3. Hai client kết nối lần lượt đều có response.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add tcp echo client server`

### BÀI 24/35 – Giao thức JSON và broadcast bid mới

**Dự án:** DỰ ÁN 06 – LƯU TRỮ VÀ LẬP TRÌNH MẠNG  
**Bài trước cần hoàn thành:** BT23  
**Kiến thức luyện tập:** JSON, multi-client, event-driven  
**Tên nhánh:** `feature/bt24-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **giao thức json và broadcast bid mới** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Định nghĩa JSON Lines (1 đối tượng JSON/1 dòng UTF-8), requestId và message type: REGISTER, LOGIN, LIST_AUCTIONS, PLACE_BID, SUBSCRIBE_AUCTION, UNSUBSCRIBE_AUCTION.
2. Server dùng ExecutorService nhận đồng thời nhiều Client; xử lý logic duy nhất ở Server; Client không gửi role/bidderId tự khai để vượt xác thực.
3. Sau LOGIN, liên kết session với userId; sự kiện BID_UPDATED phát tới mọi Client subscribe auctionId, không polling.
4. Có response success/error chuẩn và xử lý Client ngắt bất ngờ, đóng stream, cleanup subscriptions.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/network/protocol/RequestMessage.java`
- `src/main/java/vn/edu/uet/auction/network/protocol/ResponseMessage.java`
- `src/main/java/vn/edu/uet/auction/network/protocol/EventMessage.java`
- `src/main/java/vn/edu/uet/auction/network/protocol/MessageType.java`
- `src/main/java/vn/edu/uet/auction/network/protocol/JsonCodec.java`
- `src/main/java/vn/edu/uet/auction/network/server/ClientHandler.java`
- `src/main/java/vn/edu/uet/auction/network/server/ClientSession.java`
- `src/main/java/vn/edu/uet/auction/network/server/EventBroadcaster.java`
- `src/main/java/vn/edu/uet/auction/network/server/RequestDispatcher.java`
- `src/main/java/vn/edu/uet/auction/network/client/ServerConnection.java`
- `src/main/java/vn/edu/uet/auction/network/client/ServerEventListener.java`
- `src/test/java/vn/edu/uet/auction/network/AuctionNetworkIntegrationTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. 2 Client subscribe A001; Client A bid -> cả A và B nhận BID_UPDATED.
2. Bid 5 triệu khi giá 6 triệu -> response INVALID_BID, không broadcast.
3. Client gửi JSON hỏng -> trả BAD_REQUEST, Server tiếp tục.
4. Server chạy >=3 Client đồng thời; requestId khớp response.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: implement multi client json auction protocol`


---

## DỰ ÁN 07 – GIAO DIỆN JAVA FX & MVC

### BÀI 25/35 – Màn hình đăng nhập và đăng ký

**Dự án:** DỰ ÁN 07 – GIAO DIỆN JAVA FX & MVC  
**Bài trước cần hoàn thành:** BT24  
**Kiến thức luyện tập:** FXML, Controller, CSS, JavaFX  
**Tên nhánh:** `feature/bt25-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **màn hình đăng nhập và đăng ký** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Viết JavaFX Application, load login.fxml, tạo cửa sổ hoạt động và điều hướng sang register.fxml.
2. Form có username, password, nút Submit và thông báo lỗi; không hiển thị mật khẩu nguyên dạng.
3. Controller gọi ClientApi/Service phía client, tuyệt đối không query DB hoặc viết SQL trong Controller.
4. Đăng nhập thành công lưu thông tin session client tối thiểu và điều hướng về danh sách phiên.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/app/ClientMain.java`
- `src/main/java/vn/edu/uet/auction/ui/controller/LoginController.java`
- `src/main/java/vn/edu/uet/auction/ui/controller/RegisterController.java`
- `src/main/java/vn/edu/uet/auction/ui/SceneNavigator.java`
- `src/main/resources/fxml/login.fxml`
- `src/main/resources/fxml/register.fxml`
- `src/main/resources/css/app.css`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Login sai -> hiển thị thông báo, không crash.
2. Thiếu username -> kiểm tra đầu vào.
3. Có thể đổi màn login/register.
4. Chạy được mvn javafx:run.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: build javafx authentication screens`

### BÀI 26/35 – Danh sách phiên và chi tiết sản phẩm

**Dự án:** DỰ ÁN 07 – GIAO DIỆN JAVA FX & MVC  
**Bài trước cần hoàn thành:** BT25  
**Kiến thức luyện tập:** TableView/ListView, observable  
**Tên nhánh:** `feature/bt26-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **danh sách phiên và chi tiết sản phẩm** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Danh sách phiên lấy từ Server gồm tên sản phẩm, giá hiện tại, trạng thái, thời gian kết thúc.
2. Tìm theo tên sản phẩm (không phân biệt hoa-thường), lọc theo trạng thái.
3. Click 1 dòng mở chi tiết đúng auctionId, hiện mô tả, seller, thời gian, giá, leader.
4. Có trạng thái Loading/Empty/Error, không chặn JavaFX Application Thread bằng I/O mạng.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/ui/controller/AuctionListController.java`
- `src/main/java/vn/edu/uet/auction/ui/controller/AuctionDetailController.java`
- `src/main/resources/fxml/auction-list.fxml`
- `src/main/resources/fxml/auction-detail.fxml`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Server có 3 phiên -> list 3 dòng.
2. Tìm tên không tồn tại -> empty state.
3. Click A002 -> chi tiết A002 chính xác.
4. Mất mạng -> hiện lỗi dễ hiểu, UI không treo.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: build auction list and details`

### BÀI 27/35 – Màn hình đấu giá trực tiếp

**Dự án:** DỰ ÁN 07 – GIAO DIỆN JAVA FX & MVC  
**Bài trước cần hoàn thành:** BT26  
**Kiến thức luyện tập:** JavaFX binding, realtime, Platform.runLater  
**Tên nhánh:** `feature/bt27-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **màn hình đấu giá trực tiếp** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Form hiển thị currentPrice, leadingBidder, countdown, bid history, input giá đấu và nút Bid.
2. Nhấn Bid gửi PLACE_BID; chỉ xóa ô nhập nếu server trả success; hiển thị lỗi server nếu không hợp lệ.
3. Khi nhận BID_UPDATED qua Socket, JavaFX cập nhật trên UI thread bằng Platform.runLater.
4. Khi Auction FINISHED, nút Bid bị vô hiệu và kết quả hiển thị rõ.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/ui/controller/LiveAuctionController.java`
- `src/main/resources/fxml/live-auction.fxml`
- `src/main/java/vn/edu/uet/auction/ui/service/ClientApi.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Hai cửa sổ A/B xem cùng A001; A đặt 6 triệu -> B đổi giá không refresh.
2. Nhập giá nhỏ hơn giá hiện tại -> UI báo lỗi.
3. Kết thúc phiên -> nút Bid vô hiệu.
4. Không throw Not on FX application thread.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: implement realtime bidding screen`

### BÀI 28/35 – Trang quản lý sản phẩm của Seller

**Dự án:** DỰ ÁN 07 – GIAO DIỆN JAVA FX & MVC  
**Bài trước cần hoàn thành:** BT27  
**Kiến thức luyện tập:** FXML, validation, permissions  
**Tên nhánh:** `feature/bt28-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **trang quản lý sản phẩm của seller** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Seller xem danh sách sản phẩm của chính mình và trạng thái từng phiên.
2. Có form thêm/sửa/xóa Item với xác nhận xóa; chọn Electronics/Art/Vehicle.
3. Tạo phiên từ sản phẩm, nhập startTime/endTime/startingPrice có validate rõ.
4. Ẩn/khóa hành động không được phép nhưng Server vẫn là nơi kiểm tra quyền cuối cùng.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/ui/controller/SellerDashboardController.java`
- `src/main/resources/fxml/seller-dashboard.fxml`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Seller A không nhìn thấy nút thao tác sửa Item Seller B.
2. Tên sản phẩm rỗng -> báo lỗi rõ.
3. Tạo phiên end<=start -> bị từ chối.
4. Xóa Item có Auction RUNNING => bị từ chối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: build seller item management ui`


---

## DỰ ÁN 08 – TÍCH HỢP, KIỂM THỬ, TRIỂN KHAI

### BÀI 29/35 – Kết nối GUI tới Server theo MVC

**Dự án:** DỰ ÁN 08 – TÍCH HỢP, KIỂM THỬ, TRIỂN KHAI  
**Bài trước cần hoàn thành:** BT28  
**Kiến thức luyện tập:** MVC, services, async IO  
**Tên nhánh:** `feature/bt29-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **kết nối gui tới server theo mvc** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Sắp xếp các Controller chỉ thực hiện xử lý sự kiện UI và gọi ClientApi; không chứa validate nghiệp vụ thay cho Server.
2. ClientApi ánh xạ request/response thành thao tác login, list, detail, bid, item CRUD.
3. Dùng CompletableFuture/Task cho network; hiển thị loading/error và hủy thao tác khi đóng cửa sổ.
4. Toàn hệ thống có luồng JavaFX -> ClientApi -> Socket -> RequestDispatcher -> Service -> Repository.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/ui/service/ClientApi.java`
- `src/main/java/vn/edu/uet/auction/ui/SceneNavigator.java`
- `src/main/java/vn/edu/uet/auction/network/client/ServerConnection.java`
- `src/test/java/vn/edu/uet/auction/ui/MvcArchitectureTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Mỗi tính năng từ GUI gọi đúng Server, không dùng mock còn sót lại.
2. Tắt Server -> GUI không treo.
3. Không import java.sql trong package ui.
4. Đóng Client -> giải phóng kết nối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `refactor: integrate ui and server mvc`

### BÀI 30/35 – Admin, thanh toán mô phỏng, phục hồi hệ thống

**Dự án:** DỰ ÁN 08 – TÍCH HỢP, KIỂM THỬ, TRIỂN KHAI  
**Bài trước cần hoàn thành:** BT29  
**Kiến thức luyện tập:** end-to-end, permissions, error recovery  
**Tên nhánh:** `feature/bt30-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **admin, thanh toán mô phỏng, phục hồi hệ thống** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Admin xem user/auction, khóa hoặc hủy phiên theo quyền thiết kế; ghi lại lý do thao tác.
2. Mô phỏng thanh toán: FINISHED -> PAID chỉ khi đủ điều kiện; không tích hợp cổng thanh toán thật.
3. Server restart khôi phục dữ liệu và xử lý phiên đã hết hạn khi offline.
4. Xử lý ngắt kết nối Client: UI báo mất mạng và cho thử kết nối lại, không giả vờ bid thành công.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/ui/controller/AdminDashboardController.java`
- `src/main/resources/fxml/admin-dashboard.fxml`
- `src/main/java/vn/edu/uet/auction/service/PaymentService.java`
- `src/main/java/vn/edu/uet/auction/service/AuctionLifecycleService.java`
- `src/test/java/vn/edu/uet/auction/service/PaymentServiceTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Bidder không gọi chức năng Admin.
2. FINISHED -> PAID hợp lệ; RUNNING -> PAID bị từ chối.
3. Server restart: giá/bid cũ không mất.
4. Mất mạng lúc bid => trạng thái chờ/xác minh, không tự ghi nhận thành công.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: finalize admin and payment flow`

### BÀI 31/35 – Kiểm thử tích hợp và độ phủ tối thiểu

**Dự án:** DỰ ÁN 08 – TÍCH HỢP, KIỂM THỬ, TRIỂN KHAI  
**Bài trước cần hoàn thành:** BT30  
**Kiến thức luyện tập:** JUnit, integration, refactor  
**Tên nhánh:** `feature/bt31-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **kiểm thử tích hợp và độ phủ tối thiểu** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Viết test table-driven hoặc parameterized cho nghiệp vụ giá/role/trạng thái/thời gian.
2. Viết integration test: 1 Server, ít nhất 3 client giả lập, gửi bid đồng thời và nhận broadcast.
3. Đo line coverage bằng JaCoCo, mục tiêu >=60% cho code nghiệp vụ (theo hướng dẫn tuần 10).
4. Refactor code trùng lặp, không chỉnh sửa test chỉ để né lỗi; giải thích các test quan trọng trong TEST_CASES.md.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/test/java/vn/edu/uet/auction/service/AuctionServiceTest.java`
- `src/test/java/vn/edu/uet/auction/service/ConcurrentBiddingTest.java`
- `src/test/java/vn/edu/uet/auction/network/AuctionNetworkIntegrationTest.java`
- `pom.xml`
- `docs/TEST_CASES.md`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. mvn clean verify chạy test không có lỗi.
2. 100 concurrent bids: currentPrice hợp lệ, một leader.
3. Mất kết nối giữa chừng không làm Server dừng.
4. Báo cáo JaCoCo >=60% theo cách đo thống nhất.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `test: add auction integration coverage`

### BÀI 32/35 – CI, README, demo và bản nộp

**Dự án:** DỰ ÁN 08 – TÍCH HỢP, KIỂM THỬ, TRIỂN KHAI  
**Bài trước cần hoàn thành:** BT31  
**Kiến thức luyện tập:** GitHub Actions, checkstyle, docs  
**Tên nhánh:** `feature/bt32-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **ci, readme, demo và bản nộp** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Bật GitHub Actions chạy trên push/PR: checkout, setup Java 25, mvn clean verify.
2. Thêm Checkstyle/Google Java Style hoặc chuẩn nhóm thống nhất, fix vi phạm thay vì tắt rule.
3. README mô tả Java/Maven, build, run Server/Client, data, tài khoản demo không chứa mật khẩu thật, cấu trúc và troubleshooting.
4. Chuẩn bị class diagram, sơ đồ Client-Server-MVC và demo 3 Client gồm hai bid đồng thời, update realtime, đóng phiên.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `.github/workflows/ci.yml`
- `pom.xml`
- `README.md`
- `docs/CLASS_DIAGRAM.md`
- `docs/ARCHITECTURE.md`
- `docs/DEMO_SCRIPT.md`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. GitHub Actions thành công với PR vào dev.
2. Người mới clone theo README chạy Server + 2 Client được.
3. Cả 4 người giải thích được User/Item/Auction/Bid/Thread/Observer/Socket/MVC.
4. Không commit mật khẩu thật/SQLite DB chứa dữ liệu cá nhân.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `ci: automate build and document deployment`


---

## BÀI NÂNG CAO TỰ CHỌN – ĐIỂM CỘNG

### BÀI 33/35 – Auto-Bidding dùng PriorityQueue

**Dự án:** BÀI NÂNG CAO TỰ CHỌN – ĐIỂM CỘNG  
**Bài trước cần hoàn thành:** BT32  
**Kiến thức luyện tập:** PriorityQueue, rule engine, ordering  
**Tên nhánh:** `feature/bt33-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **auto-bidding dùng priorityqueue** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Người dùng đăng ký maxBid và increment >0; server lưu mỗi quy tắc cùng thời gian đăng ký.
2. Nếu có bid đối thủ, hệ thống có thể thay người dùng tăng giá từng bước nhưng không vượt maxBid.
3. Với hai quy tắc maxBid bằng nhau, ưu tiên đăng ký sớm; không để vòng lặp auto-bid vô hạn.
4. Tích hợp engine với vùng khóa AuctionService, Observer và lưu lịch sử minh bạch.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/advanced/AutoBidRule.java`
- `src/main/java/vn/edu/uet/auction/advanced/AutoBidEngine.java`
- `src/test/java/vn/edu/uet/auction/advanced/AutoBidEngineTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. maxBid=10 triệu không bao giờ chấp nhận tự đặt 10.000.001.
2. Hai maxBid cùng nhau -> người đăng ký sớm dẫn đầu theo rule.
3. Đấu giá đã FINISHED -> auto-bid không chạy.
4. Cạnh tranh auto và manual bid đồng thời không làm rollback giá.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add automatic bidding`

### BÀI 34/35 – Anti-sniping gia hạn khi sắp hết giờ

**Dự án:** BÀI NÂNG CAO TỰ CHỌN – ĐIỂM CỘNG  
**Bài trước cần hoàn thành:** BT33  
**Kiến thức luyện tập:** clock, timestamps, policies  
**Tên nhánh:** `feature/bt34-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **anti-sniping gia hạn khi sắp hết giờ** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. Cấu hình X=30 giây cuối, Y=60 giây gia hạn cho demo; X,Y có thể thay đổi.
2. Nếu bid thành công vào thời điểm endTime - now <= X và now < endTime, gia hạn endTime cũ thêm Y giây (quy tắc cụ thể cho bộ đề này).
3. Sau gia hạn, mọi Client nhận event endTime mới; bộ lập lịch kết thúc phải tôn trọng endTime cập nhật.
4. Kiểm thử chính xác ranh giới với TimeProvider/Clock, không dùng sleep.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/advanced/AntiSnipingPolicy.java`
- `src/test/java/vn/edu/uet/auction/advanced/AntiSnipingPolicyTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Còn 31 giây -> không gia hạn.
2. Còn đúng 30 giây -> gia hạn 60 giây.
3. Bid sai không gia hạn.
4. Bid tại/sau endTime -> từ chối.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: extend last second auctions`

### BÀI 35/35 – Đường giá realtime bằng JavaFX LineChart

**Dự án:** BÀI NÂNG CAO TỰ CHỌN – ĐIỂM CỘNG  
**Bài trước cần hoàn thành:** BT34  
**Kiến thức luyện tập:** chart, event update, history  
**Tên nhánh:** `feature/bt35-<ten-ngan>`  

**Đề bài:**

> Hãy tự xây dựng phần **đường giá realtime bằng javafx linechart** cho cùng một hệ thống đấu giá. Phần đã làm ở bài trước phải tiếp tục dùng lại, không chép thành dự án khác. Không được viết hộ logic kiểm tra/ghi dữ liệu ở lớp giao diện.

**Yêu cầu chi tiết (phải làm đủ):**

1. LineChart trục X thời gian bid, trục Y giá cao nhất; lấy dữ liệu cũ từ Server khi mở phiên.
2. Mỗi BID_UPDATED chèn ngay một điểm mới, không reload toàn trang và không polling.
3. Bảo đảm các điểm theo thứ tự thời gian/sequence, không thêm điểm khi bid bị từ chối.
4. Không thao tác chart từ worker thread; giới hạn hoặc phân trang khi lịch sử quá lớn.

**File phải tạo/sửa (đúng đường dẫn từ thư mục gốc repo):**

- `src/main/java/vn/edu/uet/auction/service/BidHistoryService.java`
- `src/main/java/vn/edu/uet/auction/ui/controller/LiveAuctionController.java`
- `src/main/resources/fxml/live-auction.fxml`
- `src/test/java/vn/edu/uet/auction/advanced/BidHistoryServiceTest.java`

**Dữ liệu/ca kiểm thử bắt buộc:**

1. Lịch sử 3 bid -> 3 điểm đúng thứ tự.
2. Bid thứ 4 thành công -> 4 điểm không refresh.
3. Bid sai -> vẫn 3 điểm.
4. Nhiều Client update -> không lỗi thread JavaFX.

**Điều kiện được chấm ĐẠT:** (1) Chạy đúng tất cả ca trên; (2) Không có lỗi biên chưa được xử lý; (3) Phần mới được tích hợp vào bài cũ; (4) Có ít nhất một test tự động khi bài có `src/test/...`, hoặc bản ghi màn hình/console + checklist kiểm thử khi làm UI/mạng; (5) Code và file liên quan được commit, tạo PR vào `dev`; (6) Tác giả giải thích được code.

**Commit gợi ý:** `feat: add realtime bid price chart`


---

## E. KỊCH BẢN DEMO BẮT BUỘC KHI HOÀN THÀNH BT32

1. Chạy một Server duy nhất; dùng hai hoặc ba tiến trình JavaFX Client riêng biệt.
2. Đăng nhập Seller `seller01`, tạo item (điện tử), tạo phiên RUNNING giá 5.000.000đ.
3. Đăng nhập Bidder A và Bidder B ở hai Client khác nhau; cả hai subscribe phiên.
4. A đặt 6.000.000đ: Server báo thành công; cả A/B cùng thấy giá mới ngay lập tức.
5. B đặt 5.500.000đ: từ chối, không thêm lịch sử, không phát BID_UPDATED.
6. B đặt 7.000.000đ: thành công; cả A/B thấy giá mới, leader thay đổi.
7. Cho hai Client gửi bid gần đồng thời; chứng minh không có lost update, không rollback giá.
8. Server tự đóng phiên khi hết giờ, chọn đúng winner; mọi Client nhận trạng thái FINISHED.
9. Khởi động lại Server và chứng minh dữ liệu lưu trữ còn nguyên.
10. Cho xem GitHub Actions, JUnit test, sơ đồ lớp và phân công đóng góp của cả bốn thành viên.

## F. KIỂM TRA CÁC MỤC CHẤM ĐIỂM TỪ PDF

| Mục đánh giá của đề chính thức | Bài liên quan |
|---|---|
| Thiết kế lớp, cây kế thừa | BT05, BT06, BT09, BT10, BT13, BT14 |
| OOP đóng gói, kế thừa, đa hình, trừu tượng | BT05–BT11 |
| Design Patterns Singleton / Factory / Observer | BT11, BT19, BT20 |
| Người dùng và sản phẩm | BT07, BT08, BT12, BT28, BT30 |
| Đặt giá và kết thúc đấu giá | BT13–BT17 |
| Xử lý ngoại lệ | BT07, BT15, BT17, BT23–BT24 |
| Concurrency, tránh lost update | BT18, BT24, BT31 |
| Realtime Observer / Socket | BT19, BT24, BT27 |
| Client–Server | BT23, BT24, BT29 |
| MVC JavaFX/FXML + Server DAO | BT22, BT25–BT29 |
| Maven/Style/JUnit/CI | BT02, BT04, BT31–BT32 |
| Auto bid / anti-sniping / line chart (nâng cao) | BT33–BT35 |

## G. LƯU Ý KHI GHÉP BỘ KHUNG VÀO REPO CÓ SẴN

- Đây là **bản ZIP mẫu độc lập**, không tự sửa repo của nhóm. Không giải nén ghi đè trực tiếp lên code đã có nếu repo đã phát triển.
- Sau khi giải nén, kiểm tra `git status`, sao lưu các file tự làm, và **chỉ sao chép từng file cần thiết** vào nhánh feature tương ứng; không gộp 100 file trống lên `main`.
- Nếu nhóm thống nhất sẽ khởi tạo toàn bộ skeleton trên `dev`, nên tạo PR riêng `chore: scaffold project structure` và mỗi bạn review để tránh những PR đầu vô tình xóa file người khác.
- Bài tập 01 được làm bằng nội dung git/docs; bài tập 02 mới kiểm tra Maven và layout; từ bài tập 03 mới có phương thức main chạy trong console; đến BT25 JavaFX mới mở cửa sổ.
- Nếu phát hiện thư viện Maven không tải được do proxy/network, kiểm tra kết nối Maven Central và JDK 25 thay vì sửa code nghiệp vụ ngay.

## H. PHÂN BIỆT TÀI LIỆU GỐC VÀ PHẦN TỰ THIẾT KẾ

**Theo hai tài liệu BTL gốc:** 3–4 thành viên; Java OOP, User Bidder/Seller/Admin, Item Electronics/Art/Vehicle, Auction/BidTransaction, GUI JavaFX/Swing (JavaFX khuyến nghị), Client–Server dùng REST API hoặc Socket + JSON, chỉ Server vào DB, MVC, Observer/Factory/Singleton, đồng thời an toàn, JUnit, CI/CD, Git commit thường xuyên; có tùy chọn AutoBid/Anti-sniping/LineChart. Hướng dẫn môn học đưa lịch làm và nhấn mạnh Socket, JavaFX, Serialization là tự học.

**Quy ước do bộ 35 bài này chọn thêm:** dùng Java 25 theo môi trường nhóm, JavaFX, TCP Socket thay REST, Gson, JUnit 5, VND nguyên với BigDecimal, PBKDF2, cổng TCP 8388, định dạng JSON Lines, SQLite/JDBC bên cạnh File I/O, quy tắc anti-sniping X=30s/Y=60s, Git branch/PR và tên lớp/file cụ thể. Có thể điều chỉnh khi cả nhóm thống nhất và vẫn đáp ứng rubric gốc.
