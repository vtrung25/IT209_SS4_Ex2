# BÁO CÁO BÀI TẬP 2: QUẢN LÝ NHÁNH VÀ GIẢI QUYẾT XUNG ĐỘT (MERGE CONFLICT)

## 1. Mục tiêu
- **Quản lý nhánh:** Tạo mới và chuyển đổi linh hoạt giữa các nhánh cục bộ (`main` và `feature-update`).
- **Tạo xung đột có chủ đích:** Thực hiện chỉnh sửa trên cùng một dòng code tại file `README.md` ở cả hai nhánh để tạo ra xung đột gộp (Merge Conflict).
- **Xử lý xung đột thủ công:** Nhận diện và xử lý thủ công các ký hiệu đánh dấu xung đột (`<<<<<<<`, `=======`, `>>>>>>>`), loại bỏ xung đột và bảo toàn dữ liệu.
- **Hiểu cơ chế 3-Way Merge:** Hoàn thành commit gộp nhánh (Merge Commit) và nắm vững nguyên lý hoạt động của thuật toán gộp 3 vùng (3-Way Merge).

---

## 2. Các bước thực hiện và xử lý xung đột thủ công

### Bước 1: Khởi tạo repository và tạo commit ban đầu trên nhánh `main`
- **Lệnh thực hiện:**
  ```bash
  git init -b main
  git add .
  git commit -m "feat: khoi tao du an va tep README.md ban dau"
  ```
- **Nội dung ban đầu của file `README.md`:**
  ```markdown
  # Dự Án Ứng Dụng Web - Session 04

  ## 1. Giới thiệu
  Dự án được xây dựng phục vụ môn học DevOps Fundamentals.

  ## 2. Trạng thái phát triển
  - Phiên bản: 1.0.0
  - Cập nhật: Khởi tạo dự án ban đầu trên nhánh main.
  ```

---

### Bước 2: Tạo nhánh `feature-update`, cập nhật nội dung và commit
- **Lệnh tạo và chuyển nhánh:**
  ```bash
  git checkout -b feature-update
  ```
- **Chỉnh sửa file `README.md` (thay đổi dòng trạng thái phát triển):**
  ```markdown
  - Cập nhật: Phát triển tính năng mới thanh toán trực tuyến qua cổng VNPay (nhánh feature-update).
  ```
- **Commit thay đổi trên nhánh `feature-update`:**
  ```bash
  git add README.md
  git commit -m "feat: cap nhat tinh nang thanh toan VNPay tren branch feature-update"
  ```

---

### Bước 3: Chuyển về nhánh `main`, cập nhật cùng dòng code và commit
- **Lệnh chuyển về nhánh `main`:**
  ```bash
  git checkout main
  ```
- **Chỉnh sửa file `README.md` trên cùng dòng tại nhánh `main`:**
  ```markdown
  - Cập nhật: Triển khai bản vá lỗi bảo mật khẩn cấp và tối ưu hóa hệ thống (nhánh main).
  ```
- **Commit thay đổi trên nhánh `main`:**
  ```bash
  git add README.md
  git commit -m "fix: cap nhat ban va loi bao mat tren branch main"
  ```

---

### Bước 4: Thực hiện gộp nhánh và phát sinh xung đột (Merge Conflict)
- **Lệnh thực hiện gộp nhánh `feature-update` vào `main`:**
  ```bash
  git merge feature-update
  ```
- **Thông báo xung đột từ Git Terminal:**
  ```text
  Auto-merging README.md
  CONFLICT (content): Merge conflict in README.md
  Automatic merge failed; fix conflicts and then commit the result.
  ```

---

### Bước 5: Phân tích và xử lý thủ công xung đột trong file `README.md`

#### a. Ý nghĩa các ký hiệu đánh dấu xung đột của Git:
- `<<<<<<< HEAD`: Đánh dấu điểm bắt đầu của đoạn code thuộc nhánh hiện tại đang đứng (`main`).
- `=======`: Đường ranh giới ngăn cách giữa hai phiên bản thay đổi xung đột từ hai nhánh.
- `>>>>>>> feature-update`: Đánh dấu điểm kết thúc của đoạn code thuộc nhánh đang được gộp vào (`feature-update`).

#### b. Nội dung file `README.md` khi xuất hiện xung đột:
```markdown
<<<<<<< HEAD
- Cập nhật: Triển khai bản vá lỗi bảo mật khẩn cấp và tối ưu hóa hệ thống (nhánh main).
=======
- Cập nhật: Phát triển tính năng mới thanh toán trực tuyến qua cổng VNPay (nhánh feature-update).
>>>>>>> feature-update
```

#### c. Thao tác xử lý thủ công:
1. Mở file `README.md` bằng trình soạn thảo mã nguồn.
2. Xóa bỏ hoàn toàn các dòng chứa thẻ đánh dấu: `<<<<<<< HEAD`, `=======`, và `>>>>>>> feature-update`.
3. Giữ lại và hợp nhất đầy đủ nội dung hợp lệ từ cả hai nhánh:
   ```markdown
   - Cập nhật: Triển khai bản vá lỗi bảo mật khẩn cấp, tối ưu hóa hệ thống (nhánh main) và tích hợp tính năng thanh toán trực tuyến qua cổng VNPay (nhánh feature-update).
   ```

---

### Bước 6: Hoàn tất commit gộp nhánh (Merge Commit)
- **Lệnh đánh dấu file đã giải quyết xung đột:**
  ```bash
  git add README.md
  ```
- **Lệnh tạo commit gộp:**
  ```bash
  git commit -m "Merge branch 'feature-update' into main - Resolved merge conflict"
  ```

---

## 3. Giải thích cơ chế gộp 3 vùng (3-Way Merge)

### 3.1. Khái niệm và nguyên lý hoạt động
Cơ chế **3-Way Merge** được Git áp dụng khi hai nhánh đã rẽ nhánh (diverged) và không thể áp dụng Fast-Forward. Quá trình này dựa trên 3 điểm commit:
1. **Base Commit (Common Ancestor - Tổ tiên chung):** Commit chung gần nhất trước khi hai nhánh rẽ hướng.
2. **Current Branch Commit (HEAD / Ours):** Commit mới nhất trên nhánh đích đang đứng (`main`).
3. **Incoming Branch Commit (Theirs):** Commit mới nhất trên nhánh nguồn được gộp vào (`feature-update`).

### 3.2. Quy tắc so sánh và nguyên nhân phát sinh Conflict
- **Tự động gộp:** Nếu một vùng code chỉ thay đổi ở một nhánh so với Base commit, Git sẽ tự động áp dụng thay đổi của nhánh đó vào kết quả gộp.
- **Phát sinh xung đột:** Nếu cùng một vùng dòng code bị thay đổi ở cả hai nhánh so với Base commit nhưng theo hai cách khác nhau, Git không thể tự đưa ra quyết định mà dừng tiến trình merge, chèn các ký hiệu conflict markers và yêu cầu lập trình viên giải quyết thủ công.

### 3.3. So sánh 3-Way Merge với Fast-Forward Merge
| Tiêu chí | Fast-Forward Merge | 3-Way Merge |
| :--- | :--- | :--- |
| **Điều kiện** | Nhánh chính không có commit mới sau khi nhánh phụ tách ra. | Cả hai nhánh đều có các commit mới riêng biệt. |
| **Commit mới** | Không tạo commit mới, chỉ dịch chuyển con trỏ nhánh chính tiến tới. | Luôn tạo một **Merge Commit** mới có **2 nhánh tổ tiên (2 parent commits)**. |
| **Xung đột (Conflict)** | Không bao giờ xảy ra xung đột. | Có thể xảy ra xung đột nếu cả hai nhánh cùng sửa một dòng code. |

---

## 4. Kết quả kiểm tra lịch sử nhánh (`git log --graph --oneline`)

### 4.1. Lệnh kiểm tra
```bash
git log --graph --oneline
```

### 4.2. Kết quả hiển thị từ Terminal
```text
*   2999441 (HEAD -> main) Merge branch 'feature-update' into main - Resolved merge conflict
|\  
| * 7f46589 (feature-update) feat: cap nhat tinh nang thanh toan VNPay tren branch feature-update
* | 06a779f fix: cap nhat ban va loi bao mat tren branch main
|/  
* 5ba31b1 feat: khoi tao du an va tep README.md ban dau
```

### 4.3. Phân tích kết quả mong đợi
- Đồ thị hiển thị trực quan nhánh `feature-update` được rẽ ra từ commit gốc trên `main` (`5ba31b1`).
- Cả hai nhánh phát triển song song với các commit riêng biệt (`7f46589` và `06a779f`).
- Hai nhánh được hợp nhất lại tại một **Merge Commit** duy nhất có 2 nhánh tổ tiên (`|\` và `|/`), đáp ứng chính xác yêu cầu của bài thực hành.
