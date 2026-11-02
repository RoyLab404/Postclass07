# Postclass07

Bài thực hành Composite và Decorator, dùng code mẫu DesignBook bản 3.

## Mở project bằng Eclipse

Project Java nằm trong **Eclipse/DesignBookPatterns**, không phải thư mục gốc Postclass07.

1. Chọn **File > Import > General > Existing Projects into Workspace**.
2. Chọn **Select root directory**, nhấn **Browse** rồi chọn thư mục `Eclipse/DesignBookPatterns` (thư mục có file `.project`).
3. Chọn project **DesignBookPatterns** trong danh sách rồi nhấn **Finish**.

Nếu mở từ file `Project_Eclipse_DesignBook.zip`, tại bước 2 chọn **Select archive file** và chọn file ZIP, không dùng Select root directory để chọn ZIP.

Nếu Eclipse báo project đã tồn tại hoặc không cho chọn lại, tìm **DesignBookPatterns** trong Package Explorer. Nếu project đang đóng, nhấn chuột phải > **Open Project**. Có thể dùng **Window > Show View > Package Explorer** để hiện danh sách project.

Nếu chọn thư mục `Postclass07` hoặc `Eclipse` làm thư mục gốc để dò, bật **Search for nested projects** nếu phiên bản Eclipse có tùy chọn này; cách chắc chắn hơn là chọn trực tiếp `Eclipse/DesignBookPatterns`.

## Java và cách chạy

Dùng JDK 21 trở lên. Nếu project có dấu X đỏ do thiếu JavaSE-21, gán JDK trong **Window > Preferences > Java > Installed JREs > Execution Environments > JavaSE-21**.

Các lớp Composite và Decorator là code mẫu thư viện. Để xem kết quả kiểm tra, mở Terminal tại thư mục `Eclipse/DesignBookPatterns` rồi chạy:

```bash
javac --release 21 -d bin src/e3/chapter6/*.java
jshell --class-path bin kiem-tra.jsh
```

Kết quả đúng gồm ba dòng PASS. Hướng dẫn chi tiết nằm trong `Eclipse/DesignBookPatterns/README.md`.

## Sản phẩm

- `Bao_cao_Composite_Decorator_4651050116.docx`: báo cáo chính.
- `Bao_cao_cong_diem_Solitaire_JetUML_4651050116.docx`: báo cáo cộng điểm.
- `Eclipse/DesignBookPatterns`: project Java để import.
- `Project_Eclipse_DesignBook.zip`: bản đóng gói project.

11 file Java giữ nguyên từ [DesignBook](https://github.com/prmr/DesignBook/tree/ea45e0d72cbbfdc9bd7120e14d87d01f0bd3d1da/code-e3/e3/chapter6), gồm thông tin tác giả và giấy phép trong từng file. Cấu hình Eclipse, hướng dẫn và script JShell đi kèm phục vụ mở và kiểm tra mẫu.
