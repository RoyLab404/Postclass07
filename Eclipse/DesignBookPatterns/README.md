# Composite và Decorator trong DesignBook

Nguồn: https://github.com/prmr/DesignBook
Commit: ea45e0d72cbbfdc9bd7120e14d87d01f0bd3d1da
Thư mục nguồn: code-e3/e3/chapter6

11 file Java trong src được giữ nguyên từng byte từ nguồn, gồm cả thông tin tác giả và giấy phép CC BY-NC-ND 4.0. Chỉ chọn các lớp minh họa và phụ thuộc cần để biên dịch; không thêm lớp thiết kế. Command là phụ thuộc của Deck; LoggingDeck dùng để đối chiếu với LoggingDecorator.

## Mở bằng Eclipse

1. Dùng Eclipse hỗ trợ Java 21 trở lên và cài JDK 21 hoặc mới hơn.
2. File > Import > General > Existing Projects into Workspace.
3. Chọn thư mục DesignBookPatterns rồi Finish.
4. Nếu thiếu JavaSE-21, vào Window > Preferences > Java > Installed JREs > Execution Environments, gán JDK đã cài cho JavaSE-21.
5. Project > Build Project. Không cần JavaFX, Maven hay thư viện ngoài.

## Quan sát hai mẫu

Mở CompositeCardSource.java, LoggingDecorator.java và MemorizingDecorator.java. File kiem-tra.jsh chỉ tạo và gọi các đối tượng mẫu để kiểm tra, không định nghĩa thêm lớp. Có thể chạy bằng Terminal trong Eclipse, tại thư mục project:

```bash
javac --release 21 -d bin src/e3/chapter6/*.java
jshell --class-path bin kiem-tra.jsh
```

Kết quả mong đợi là ba dòng PASS. Script ném AssertionError nếu sai. Thứ tự lá bài thay đổi do Deck.shuffle(). Script kiểm tra thứ tự chuyển giữa các nguồn, không cố định thứ tự xáo bài.

Để xem trực tiếp lá bài trên JShell:

```java
import e3.chapter6.*;
CardSource source = new LoggingDecorator(new MemorizingDecorator(new CompositeCardSource(new Deck(), new Deck())));
source.draw();
source.isEmpty();
```

Deck.main() trong mẫu gốc minh họa Command và không in kết quả. Dùng các lệnh trên để quan sát Composite và Decorator. Khi debug Java, bật VM argument -ea để thực thi assert; luôn kiểm tra !isEmpty() trước draw(). MemorizingDecorator không có getter lịch sử, có thể quan sát aDrawnCards bằng debugger mà không sửa lớp gốc.

## Xác minh đã thực hiện

Đã biên dịch bằng javac 25 với --release 21 và chạy kiem-tra.jsh: cả ba nhóm kiểm tra PASS. Chưa thao tác trực tiếp trên giao diện Eclipse. Phần khảo sát Solitaire và JetUML trong báo cáo cộng điểm dựa trên đọc mã nguồn, không khẳng định đã chạy toàn bộ hai ứng dụng.
