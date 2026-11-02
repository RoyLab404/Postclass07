# Postclass07 Composite và Decorator

## Thông tin sinh viên

| Thông tin | Nội dung |
|---|---|
| Họ và tên | Huỳnh Ngọc Khang |
| MSSV | 4651050116 |
| Lớp | CNTT46D |
| Bài thực hành | Tìm hiểu hai mẫu thiết kế Composite và Decorator |
| Phần cộng điểm | Phân tích Composite và Decorator trong Solitaire và JetUML |

## Nội dung bài làm

Bài làm gồm báo cáo kiến thức về hai mẫu, project Java dùng code mẫu DesignBook và báo cáo cộng điểm riêng. Các lớp Java được giữ nguyên từ chương 6, bản 3 của sách *Introduction to Software Design with Java* của Martin P. Robillard. Bài không bổ sung lớp thiết kế mới; `kiem-tra.jsh` chỉ tạo và gọi các đối tượng có sẵn để kiểm tra hành vi.

## Cấu trúc thư mục

```text
postclass07/
├── .project
├── .classpath
├── .gitignore
├── .settings/
│   └── org.eclipse.jdt.core.prefs
├── README.md
├── kiem-tra.jsh
├── Bao_cao_Composite_Decorator_4651050116.docx
├── Bao_cao_cong_diem_Solitaire_JetUML_4651050116.docx
└── src/e3/chapter6/
    ├── Card.java
    ├── CardSource.java
    ├── CardStack.java
    ├── Command.java
    ├── CompositeCardSource.java
    ├── Deck.java
    ├── LoggingDeck.java
    ├── LoggingDecorator.java
    ├── MemorizingDecorator.java
    ├── Rank.java
    └── Suit.java
```

## Mở bằng Eclipse

Cần JDK 21 trở lên và Eclipse có hỗ trợ Java 21. Mẫu DesignBook sử dụng các API như `List.reversed()`, `getLast()` và `removeLast()`.

1. Tải repository về máy bằng Git hoặc **Code > Download ZIP** rồi giải nén.
2. Trong Eclipse, chọn **File > Import > General > Existing Projects into Workspace**.
3. Chọn **Select root directory**, nhấn **Browse** và chọn thư mục vừa tải về, nơi chứa `.project`, `.classpath` và `src`.
4. Chọn project **Postclass07** rồi nhấn **Finish**.
5. Chọn **Project > Build Project**. Nếu bật **Build Automatically**, Eclipse tự biên dịch khi thay đổi file.

Nếu project báo thiếu JavaSE-21, vào **Window > Preferences > Java > Installed JREs**, thêm JDK đã cài. Trong **Execution Environments > JavaSE-21**, chọn JDK tương ứng.

Nếu workspace còn project **DesignBookPatterns** từ cách bố trí cũ, hãy xóa mục project cũ khỏi workspace và **không chọn xóa nội dung trên ổ đĩa**, rồi import project **Postclass07** ở thư mục gốc. Không cần tạo Java Project mới hoặc import riêng thư mục `src` hay `bin`.

## Chạy và kiểm tra

Các lớp Composite và Decorator trong mẫu không có `main()` để chạy riêng bằng **Run As > Java Application**. `Deck.main()` có sẵn minh họa Command và không in kết quả. Để kiểm tra hai mẫu, mở Terminal ở thư mục gốc project, có thể dùng Terminal tích hợp trong Eclipse, rồi chạy:

```bash
javac --release 21 -d bin src/e3/chapter6/*.java
jshell --class-path bin kiem-tra.jsh
```

Kết quả mong đợi:

```text
PASS Composite: empty, nested, order, 104 cards
PASS Decorator: 52 log lines, shared empty state
PASS copy: same order, independent source state
```

Script kiểm tra composite rỗng, nguồn lồng nhau, thứ tự chuyển nguồn, số lần ghi log và trạng thái bản sao. Khi một điều kiện sai, script ném `AssertionError`. Lịch sử trong `MemorizingDecorator` được phân tích theo mã nguồn; script chưa kiểm tra trực tiếp trường riêng này.

Để xem một lần rút bài, chạy `jshell --class-path bin` rồi nhập:

```java
import e3.chapter6.*;
CardSource source = new LoggingDecorator(
    new MemorizingDecorator(
        new CompositeCardSource(new Deck(), new Deck())));
source.draw();
source.isEmpty();
/exit
```

`draw()` in lá bài và trả lại lá vừa rút. Giá trị lá bài thay đổi vì mỗi `Deck` được xáo ngẫu nhiên. Cần kiểm tra `!source.isEmpty()` trước khi rút tiếp. Khi chạy Java bằng debugger, có thể bật VM argument `-ea` để thực thi các câu lệnh `assert` của mẫu.

## Giải thích Composite

Composite biểu diễn một nhóm đối tượng qua cùng giao diện với phần tử đơn. Trong mẫu này, client dùng `CardSource` để rút bài từ một `Deck` hoặc một nhóm nguồn bài.

| Vai trò | Thành phần |
|---|---|
| Component | `CardSource`, khai báo `draw()`, `isEmpty()` và `copy()` |
| Leaf | `Deck`, quản lý một bộ bài cụ thể |
| Composite | `CompositeCardSource`, giữ danh sách các `CardSource` |
| Client | Đoạn mã gọi các phương thức thông qua biến kiểu `CardSource` |

`CompositeCardSource.draw()` duyệt danh sách theo thứ tự và rút từ nguồn đầu tiên còn bài. Khi nguồn thứ nhất hết, lần gọi tiếp theo chuyển sang nguồn còn bài phía sau. Nguồn con có thể là một `CompositeCardSource` khác, nên cách xử lý này áp dụng cho cấu trúc lồng nhau.

`isEmpty()` trả `true` khi mọi nguồn con đều rỗng, kể cả nhóm không có nguồn con. `copy()` gọi `copy()` trên từng nguồn để tạo bản sao cấu trúc.

Hai `Deck` mới được ghép lại cho tổng cộng 104 lá. Composite không trộn bài giữa hai nguồn; nó rút hết nguồn đầu trước khi dùng nguồn sau. `CardStack` trong phiên bản này chỉ triển khai `Iterable<Card>`, nên không phải Leaf thuộc giao diện `CardSource`.

## Giải thích Decorator

Decorator giữ một đối tượng có cùng giao diện với nó, chuyển tiếp lời gọi và bổ sung hành vi. Cả `LoggingDecorator` lẫn `MemorizingDecorator` đều triển khai `CardSource` và nhận một `CardSource` qua constructor.

| Lớp | Hành vi của `draw()` |
|---|---|
| `LoggingDecorator` | Rút từ nguồn được bọc, in lá bài rồi trả lại kết quả |
| `MemorizingDecorator` | Rút từ nguồn được bọc, thêm lá bài vào lịch sử rồi trả lại kết quả |

Với cấu trúc trong đoạn JShell, lời gọi đi qua `LoggingDecorator`, `MemorizingDecorator`, `CompositeCardSource`, rồi tới `Deck` còn bài. Khi kết quả trả về, lớp ghi nhớ lưu lá bài trước, sau đó lớp ghi log in lá bài. Một lần gọi từ ngoài chỉ rút một lá.

Đặt decorator ngoài composite áp dụng hành vi cho toàn bộ nhóm nguồn. Nếu chỉ bọc một `Deck` bên trong composite, hành vi bổ sung chỉ áp dụng khi rút từ `Deck` đó.

Mẫu không có lớp `BaseDecorator` riêng; hai decorator triển khai giao diện trực tiếp. `LoggingDeck` là trường hợp đối chiếu: lớp tự quản lý bộ bài thay vì nhận một nguồn bất kỳ để bọc, nên tác giả xác định nó không phải Decorator. `Command` được giữ vì `Deck` cần lớp này để biên dịch.

## Phần cộng điểm

Báo cáo riêng phân tích những vị trí sau trong mã nguồn:

| Ứng dụng | Composite | Decorator |
|---|---|---|
| Solitaire | `CompositeMove` gộp các `Move`, được tạo trong `GameModel.getCardMove()` | `CardStack.iterator()` sử dụng `Collections.unmodifiableList(aCards)` |
| JetUML | `CompoundOperation` gộp các `DiagramOperation` | `Diagram.rootNodes()` và `edges()` sử dụng `Collections.unmodifiableList(...)` |

Trong hai trường hợp Decorator, ứng dụng sử dụng lớp bọc của thư viện Java Collections. Lớp bọc giữ giao diện `List`, cho phép đọc và chặn sửa qua lớp bọc. Danh sách gốc vẫn có thể thay đổi; lớp bọc không tạo bản sao sâu hoặc làm các phần tử bất biến.

Các liên kết ghim phiên bản và phân tích chi tiết nằm trong báo cáo cộng điểm. Phần này dựa trên đọc mã nguồn, chưa chạy toàn bộ hai ứng dụng JavaFX.

## Tài liệu và kết quả xác minh

- [Báo cáo Composite và Decorator](Bao_cao_Composite_Decorator_4651050116.docx).
- [Báo cáo cộng điểm về Solitaire và JetUML](Bao_cao_cong_diem_Solitaire_JetUML_4651050116.docx).
- [Refactoring Guru về Composite](https://refactoring.guru/design-patterns/composite).
- [Refactoring Guru về Decorator](https://refactoring.guru/design-patterns/decorator).
- [Code mẫu DesignBook bản 3, chương 6](https://github.com/prmr/DesignBook/tree/ea45e0d72cbbfdc9bd7120e14d87d01f0bd3d1da/code-e3/e3/chapter6).

