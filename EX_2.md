# Bài 2: Tối ưu Prompt

## 1. Nội dung Prompt sau khi tối ưu

```text
Hãy đóng vai trò là một Java Debugger chuyên phân tích Stack Trace và gỡ lỗi chương trình Java.

Bối cảnh:
Tôi đang chạy thử lớp UserManager để quản lý danh sách người dùng, nhưng chương trình bị lỗi NullPointerException khi gọi phương thức addUser.

Mã nguồn hiện tại:

import java.util.List;

public class UserManager {

    private List<String> users; // Danh sách chưa được khởi tạo

    public void addUser(String user) {
        users.add(user); // Dòng gây lỗi NullPointerException
    }
}

Stack Trace thực tế:

Exception in thread "main" java.lang.NullPointerException: Cannot invoke "java.util.List.add(Object)" because "this.users" is null
    at UserManager.addUser(UserManager.java:7)
    at Main.main(Main.java:6)

Yêu cầu:
1. Giải thích nguyên nhân gốc rễ (Root Cause) của lỗi NullPointerException.
2. Chỉ ra chính xác dòng code gây lỗi và vì sao biến users đang là null.
3. Đề xuất cách sửa an toàn nhất bằng cách khởi tạo danh sách bằng ArrayList.
4. Không phá vỡ tính bao đóng (Encapsulation) của OOP: trường users vẫn phải là private, không được public trực tiếp danh sách.
5. Cung cấp mã nguồn Java hoàn chỉnh sau khi sửa trong khối code markdown.
6. Giải thích ngắn gọn bằng tiếng Việt.
```

## 2. Mã nguồn Java hoàn chỉnh và an toàn

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class UserManager {

    private final List<String> users;

    public UserManager() {
        this.users = new ArrayList<>();
    }

    public void addUser(String user) {
        if (user == null || user.trim().isEmpty()) {
            throw new IllegalArgumentException("User name must not be null or empty");
        }

        users.add(user);
    }

    public List<String> getUsers() {
        return Collections.unmodifiableList(users);
    }
}
```

## 3. Giải thích ngắn

Lỗi xảy ra vì biến `users` chỉ được khai báo là `List<String>` nhưng chưa được khởi tạo bằng một đối tượng cụ thể như `ArrayList`. Do đó, khi gọi `users.add(user)`, chương trình đang gọi phương thức `add` trên giá trị `null`, dẫn đến `NullPointerException`.

Cách sửa an toàn là khởi tạo `users` trong constructor bằng `new ArrayList<>()`. Trường `users` vẫn để `private final` để bảo vệ tính bao đóng. Phương thức `getUsers()` trả về danh sách chỉ đọc thông qua `Collections.unmodifiableList(users)`, tránh việc code bên ngoài sửa trực tiếp danh sách nội bộ.
