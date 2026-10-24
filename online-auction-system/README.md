# Online Auction System – bộ khung 35 bài tập LTNC 2026

**Đây là KHUNG CHƯA CODE**: không chứa nghiệp vụ, không chứa lời giải, không có test thật. Tất cả các lớp Java chỉ là chỗ trống có tên và package; FXML/CSS/SQL chưa thiết kế. Bộ đề nằm ở `docs/DE_BAI_35_BAI.md`.

## XEM FILE NÀO TRƯỚC?

1. **`docs/DE_BAI_35_BAI.md`** – toàn bộ 35 đề: yêu cầu cụ thể, đường dẫn file, dữ liệu test, điều kiện đạt, commit.
2. **`docs/bai-tap/BT01.md` → `BT35.md`** – từng đề tách riêng cho mỗi buổi học/PR.
3. **`docs/MA_TRAN_BAI_TAP_FILE.csv`** – ma trận 35 bài → file cần sửa (mở bằng Excel vẫn có dấu tiếng Việt).
4. **`docs/HUONG_DAN_SU_DUNG.md`** – hướng dẫn mở dự án, giữ nhánh, nhận bài và không ghi đè repo cũ.
5. **`pom.xml`** – Maven Java 25, JavaFX, Gson, SQLite, JUnit; JaCoCo & Checkstyle để học viên thêm ở BT31/32.

## YÊU CẦU MÁY

- Windows 11 / Linux / macOS; **JDK 25**, Maven 3.9+ và Git.
- VS Code hoặc IntelliJ IDEA; JavaFX bắt đầu BT25, Socket từ BT23.

## MỞ DỰ ÁN TRONG POWER SHELL (WINDOWS)

```powershell
cd <thu-muc-vua-giai-nen>\online-auction-system
java -version
mvn -version
mvn clean compile
```

Đây mới là kiểm tra **biên dịch khung rỗng**. Lệnh `mvn test` lúc đầu không có testcase JUnit thực nên **không chứng minh các chức năng đã hoạt động**.

Sau khi làm **BT03**, chương trình console mới có `main()` và chạy được theo cách nhóm cấu hình. Sau **BT25**, `mvn javafx:run` mới chạy giao diện JavaFX. Trước hai mốc này, không kỳ vọng UI hay console hoạt động.

## GITHUB CỦA NHÓM

Repository: https://github.com/Ebi-27/online-auction-system.git

Khuyến nghị: mở PR riêng đưa scaffold vào nhánh `dev`; mỗi bài tiếp theo một feature branch từ `dev`. Repo trong file ZIP **không** mang thư mục `.git`, tránh can thiệp lịch sử Git hiện có.

## CẤU TRÚC MỤC TIÊU

```
online-auction-system/
├── pom.xml
├── README.md
├── .gitignore
├── .github/workflows/ci.yml     # TODO BT32
├── docs/
│   ├── DE_BAI_35_BAI.md         # 35 đề + mapping file
│   ├── MA_TRAN_BAI_TAP_FILE.csv # 35 bài ↔ file
│   ├── HUONG_DAN_SU_DUNG.md
│   └── bai-tap/BT01.md ... BT35.md
└── src/
    ├── main/java/vn/edu/uet/auction/
    │   ├── app/ model/ service/ factory/ pattern/
    │   ├── exception/ util/ repository/ io/ observer/
    │   ├── network/ ui/ advanced/
    ├── main/resources/fxml/ css/ db/
    └── test/java/vn/edu/uet/auction/
```

## GHI CHÚ QUAN TRỌNG

- Các enum đã có tên hằng để nhóm dùng thống nhất; đó **không phải** code triển khai nghiệp vụ.
- Tầng `repository/jdbc`/SQLite là phương án tự thiết kế thêm. Hai PDF không yêu cầu bắt buộc dùng SQLite; phần bắt buộc là lưu trữ và bảo đảm chỉ Server truy cập dữ liệu.
- Phải tự viết test và chịu trách nhiệm giải thích **mọi phần code** trước giảng viên.
