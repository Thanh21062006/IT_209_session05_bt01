# Bài 1: Khôi phục commit đã mất bằng Git Reflog

## 1. Mục tiêu
Thực hành sử dụng Git Reflog để tìm và khôi phục một commit đã bị mất khỏi lịch sử của nhánh sau khi sử dụng lệnh:

```bash
git reset --hard HEAD~1
```

Mục tiêu là khôi phục lại commit cũ mà không cần viết lại mã nguồn thủ công.
---

## 2. Khởi tạo Repository
Khởi tạo Git Repository:

```bash
git init
```

Tạo file:

```text
feature.txt
```

Nội dung ban đầu:

```text
Day la tinh nang quan trong
```

Kiểm tra trạng thái:

```bash
git status
```

---

## 3. Tạo commit đầu tiên

Đưa file vào Staging Area:

```bash
git add .
```

Tạo commit:

```bash
git commit -m "Them tinh nang quan trong"
```

Commit đầu tiên được tạo thành công.

Ví dụ:

```text
f19df5f Them tinh nang quan trong
```

---

## 4. Tạo commit thứ hai

Thêm nội dung mới vào file `feature.txt`:

```text
Dong code thu 2
```

Sau đó thực hiện:

```bash
git add .
git commit -m "Cap nhat feature"
```

Kiểm tra lịch sử:

```bash
git log --oneline
```

Kết quả ví dụ:

```text
a25b8c1 Cap nhat feature
f19df5f Them tinh nang quan trong
```

---

## 5. Giả lập sự cố mất commit

Thực hiện lệnh:

```bash
git reset --hard HEAD~1
```

Lệnh này đưa HEAD về commit trước đó và loại bỏ commit mới nhất khỏi lịch sử của nhánh hiện tại.

Kiểm tra lại:

```bash
git log --oneline
```

Kết quả:

```text
f19df5f Them tinh nang quan trong
```

Commit:

```text
a25b8c1 Cap nhat feature
```

không còn xuất hiện trong `git log`.

---

## 6. Sử dụng Git Reflog để tìm commit đã mất

Thực hiện:

```bash
git reflog
```

Ví dụ kết quả:

```text
f19df5f HEAD@{0}: reset: moving to HEAD~1
a25b8c1 HEAD@{1}: commit: Cap nhat feature
f19df5f HEAD@{2}: commit: Them tinh nang quan trong
```

Từ Reflog có thể xác định commit đã bị mất:

```text
a25b8c1
```

Commit này có tiêu đề:

```text
Cap nhat feature
```

Reflog vẫn lưu lại lịch sử di chuyển của HEAD mặc dù commit không còn xuất hiện trong `git log`.

---

## 7. Khôi phục commit

Sử dụng mã hash lấy được từ Reflog:

```bash
git reset --hard a25b8c1
```

Sau khi khôi phục, kiểm tra:

```bash
git log --oneline
```

Kết quả:

```text
a25b8c1 Cap nhat feature
f19df5f Them tinh nang quan trong
```

Commit đã bị mất đã được khôi phục thành công.

---

## 8. Kiểm tra nội dung file

Kiểm tra nội dung:

```bash
type feature.txt
```

Kết quả:

```text
Day la tinh nang quan trong
Dong code thu 2
```

Như vậy nội dung của commit trước khi thực hiện `git reset --hard HEAD~1` đã được khôi phục.

---