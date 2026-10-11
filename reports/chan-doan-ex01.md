# BÁO CÁO CHẨN ĐOÁN & QUẢN TRỊ KHO CHUNG (SESSION 02 - EX01)

- **Lớp:** AI-SDLC-PTHB260930
- **Dự án:** LibraryApp
- **Người thực hiện:** Lê Văn C (Developer) / Vũ Ngọc Minh Phương

---

## 1. Bảng chẩn đoán nguyên nhân gốc và giải pháp

| STT | Triệu chứng quan sát | Nguyên nhân gốc | Thao tác khắc phục | Kết quả kiểm chứng |
| :---: | :--- | :--- | :--- | :--- |
| **1** | Commit "update" trên `main` làm mất mục FR-05 trong `docs/dac-ta-yeu-cau.md`; không có Pull Request nào để xem lại. | Nhánh `main` chưa được thiết lập quy tắc bảo vệ (Branch protection rules), cho phép push trực tiếp mà không cần qua Pull Request và review. | Bật Branch protection cho `main`: Bắt buộc "Require a pull request before merging" (tối thiểu 1 approval) và bật "Do not allow bypassing the above settings" (áp dụng cho cả Lead/Admin). | Một Developer thử push trực tiếp lên `main` bằng lệnh `git push origin main` và bị từ chối với lỗi `GH006: Protected branch hook declined`. |
| **2** | Mọi người đều tạo nhánh làm việc từ `main` vì kho chỉ có một nhánh duy nhất. | Kho thiếu nhánh phát triển tích hợp `develop` theo quy trình chuẩn Git Flow / GitHub Flow, và `main` đang là nhánh mặc định. | Tạo nhánh `develop` từ `main`, đẩy lên GitHub và vào **Settings -> Branches** đặt `develop` làm **Default branch**. | Lệnh `git branch -a` hiển thị cả `main` và `develop`. Trên GitHub, `develop` hiển thị là Default branch. Các PR mới tự động trỏ vào `develop`. |
| **3** | Tab Issues trên GitHub trống trơn (0 mở, 0 đóng); không ai biết mình phụ trách FR nào. | Nhóm chưa tạo và phân bổ công việc thông qua GitHub Issues tương ứng với các chức năng đặc tả (FR-01 đến FR-10). | Tạo lần lượt 10 issues theo đúng thứ tự từ FR-01 đến FR-10 với tiêu đề `FR-0X: <tên chức năng>`, gán Assignee đúng theo phân công: FR-10 (Lead), FR-09 (B), FR-08 (C), FR-07 -> FR-01 (Dev 4 -> Dev 10). | Tab Issues hiển thị đủ 10 issues mở, số Issue #1 đến #10 trùng khớp số FR, mỗi issue có đúng 1 người phụ trách. |
| **4** | Developer 9 và Developer 10 không nhìn thấy kho `library-app`. | Hai thành viên này chưa chấp nhận lời mời tham gia Organization/Repository (vẫn đang ở trạng thái "Pending invitations"). | Nhắc Developer 9 và 10 kiểm tra email hoặc truy cập thông báo trên GitHub để bấm **Accept Invitation**. | Danh sách thành viên (Members) hiển thị đủ 10 thành viên chính thức; danh sách "Pending invitations" về 0. |
| **5** | Quy tắc bảo vệ nhánh không có hiệu lực trên kho Private thuộc Organization gói GitHub Free (*Ngoại lệ 4.3*). | Chính sách GitHub: Gói GitHub Free chỉ hỗ trợ thực thi Branch Protection Rules trên kho Public. Kho Private không kích hoạt được tính năng này trừ khi nâng cấp GitHub Team/Enterprise. | Chuyển chế độ hiển thị của repository từ **Private sang Public** trong phần **Settings -> General -> Danger Zone -> Change visibility -> Make public** (hoặc nâng cấp gói nếu tổ chức hỗ trợ). | Quy tắc bảo vệ nhánh kích hoạt thực tế ngay lập tức, chặn push trực tiếp từ Developer thành công. |
| **6** | Pull Request chứa cú pháp `Closes #N` cần tự động đóng issue khi merge vào `develop` (*Ngoại lệ 4.3*). | GitHub chỉ tự động đóng issue bằng từ khóa `Closes`/`Fixes` khi Pull Request được merge vào **nhánh mặc định (Default branch)** của repository. | Đặt `develop` làm Default branch của repository. Khi tạo PR, ghi rõ từ khóa liên kết (ví dụ `Closes #8`). | Khi merge Pull Request vào `develop`, Issue liên kết tự động chuyển sang trạng thái Closed ngay lập tức. |

---

## 2. Trạng thái kiểm chứng của kho

1. **Nhánh mặc định (Default branch):** Đã chuyển sang `develop`.
2. **Quy tắc bảo vệ (Branch Protection):** Đã áp dụng cho cả `main` và `develop`, yêu cầu 1 approval và không cho phép bypass (kể cả Lead/Admin).
3. **Danh sách Issues:** 10/10 issues được tạo đúng thứ tự `#1` đến `#10` tương ứng `FR-01` đến `FR-10` và gán đúng người nhận.
4. **Thành viên:** 10/10 thành viên đã chấp nhận lời mời tham gia tổ chức.
5. **Chặn push trực tiếp:** Developer khi chạy `git push` trực tiếp lên `main` hoặc `develop` bị chặn với mã lỗi `GH006`.
