# HƯỚNG DẪN SỬ DỤNG BỘ KHUNG

## 1. Mục đích

Một ZIP chứa toàn bộ đề bài + code placeholder. Bạn phải **tự cài** thuộc tính, constructor, methods, logic, UI, SQL và tests. Không có lời giải bí mật trong class rỗng.

## 2. Nếu bạn đã có repository online-auction-system

**Không `git init` lại** bên trong repo hiện có. Nếu muốn mang skeleton vào repo, thực hiện ở **nhánh mới**:

```powershell
cd D:\01.LTNC\online-auction-system
git fetch origin
git switch dev             # Nếu máy chưa có dev: git switch --track origin/dev
git pull --ff-only origin dev
git switch -c chore/scaffold-35-bai
```

Sau đó mở ZIP ở **một thư mục ngoài repo**, kiểm tra file rồi sao chép những file mới vào repo (không ghi đè file đã code). Cuối cùng:

```powershell
git status
git add .
git commit -m "chore: add empty lt nc exercise scaffold"
git push -u origin chore/scaffold-35-bai
```

Lên GitHub tạo Pull Request **vào dev**; để ít nhất một người review. Nếu repo đang có các file người khác viết, ưu tiên cherry-pick/thêm từng phần thay vì `git add .` mù quáng.

## 3. Quy trình thực hành từng bài

1. Đọc `docs/bai-tap/BTXX.md`.
2. Từ dev đã cập nhật, tạo `feature/btXX-ten-ngan`.
3. Chỉ chỉnh phần file liệt kê ở đề; nếu cần file mới, ghi lý do ở PR.
4. Tự viết JUnit test/kiểm tra console/UI theo đề và chụp minh chứng.
5. `mvn clean test`, kiểm tra `git diff`, commit, push, PR -> dev.
6. Thành viên khác review, tác giả giải thích đường đi dữ liệu và edge cases.
7. Merge thành công mới nhận bài tiếp theo; sau khi ghép, làm smoke test chức năng cũ.

## 4. Cách nhận biết file nào là TODO

- Java: `public class ... { // TODO }` = chưa có thuộc tính, constructor, phương thức.
- JUnit: class test trống = chưa kiểm thử, Maven có thể báo build thành công nhưng không có test chạy.
- FXML: chỉ có VBox rỗng; chưa có GUI hay Controller liên kết.
- SQL: chỉ comment, **chưa có bảng**.
- CI: file `.github/workflows/ci.yml` mới có comment, **chưa có workflow**.

## 5. Thứ tự triển khai kỹ thuật

BT01–BT04: Git/Maven/test; BT05–BT12: model + CRUD; BT13–BT20: đấu giá + an toàn đồng thời; BT21–BT24: lưu trữ + mạng; BT25–BT29: GUI + MVC; BT30–BT32: E2E/test/CI; BT33–BT35: điểm cộng.

## 6. Phần không nằm trong yêu cầu gốc

SQLite (BT22), PBKDF2/salt, JSON Lines, cổng TCP 8388, VND nguyên/BigDecimal là phương án rèn luyện do bộ bài tập đề xuất. Nhóm có thể thay đổi khi đã thống nhất giao diện module, nhưng phải bảo đảm các yêu cầu trong hai PDF và thang chấm của giảng viên.
