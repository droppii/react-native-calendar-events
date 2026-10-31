# Ngữ cảnh repository — react-native-calendar-events

Bridge React Native truy cập lịch thiết bị.

## 1. Trách nhiệm và luồng xử lý

API JavaScript gọi Android/iOS để xin quyền và đọc/ghi calendar/event; example là môi trường host để kiểm tra.

Giữ permission, timezone, recurring event và khác biệt hai hệ điều hành. Test không được ghi vào lịch cá nhân thật. Script test gốc chỉ in thông báo, không chứng minh native behavior được kiểm thử.

## 2. Điểm bắt đầu trong mã

- [android](../android).
- [ios](../ios).
- [example](../example).
- [package.json](../package.json).

Đọc hướng dẫn tại thư mục con trước khi sửa module tương ứng. Đối chiếu hợp đồng và nơi gọi khi thay đổi API, dữ liệu hoặc thư viện dùng chung.

## 3. Toolchain và kiểm chứng

Phiên bản và lệnh được quản lý trong manifest của từng package; giữ lockfile đang được đường build sử dụng. Với monorepo, chạy tại đúng package hoặc qua workspace. Không nâng toolchain chỉ để làm một lệnh cũ chạy được.

| Manifest | Toolchain được khai báo |
| --- | --- |
| [example/package.json](../example/package.json) | react ^16.13.0; react-native ^0.63.0; jest ^25.1.0 |
| [package.json](../package.json) | Theo workspace và lockfile; package không khai báo toolchain riêng. |

| Mục đích | Thư mục chạy | Lệnh | Điều kiện và tác động |
| --- | --- | --- | --- |
| lint | `example` | `yarn run lint` | Kiểm tra tĩnh; không thay thế kiểm thử hành vi. Script: `eslint .`. |
| test | `example` | `yarn run test` | Kiểm tra fixture, hook và môi trường trước khi chạy; không mặc định là test cô lập. Script: `jest`. |
| test | `.` | `yarn run test` | Placeholder, không phải bộ kiểm thử; không dùng làm bằng chứng đạt. Script: `echo "Everything is awesome!"`. |

Điểm vào bộ kiểm thử: [example/__tests__/App-test.js](../example/__tests__/App-test.js). Đọc setup/fixture trước khi chạy rộng hơn; những đường dẫn này không biểu thị mức bao phủ của toàn hệ thống.

Ghi kết quả và phạm vi thực sự đã chạy trong PR. Không ghi build, lint hoặc test thành công khi mới đọc cấu hình hay khi script không thực thi assertion.

## 4. Cấu hình và phát hành

Không có workflow GitHub Actions thực thi build/deploy được khai báo trong cây nguồn này. Cách phát hành phải được xác định từ cấu hình hosting, pipeline ngoài GitHub hoặc ứng dụng sử dụng package; không suy ra là tự triển khai khi push.

Dùng credential được cấp cho đúng môi trường. Không lưu secret hoặc dữ liệu cá nhân vào mã, tài liệu, ảnh chụp hay log. Migration, seed, publish, gửi thông báo và rollout là thao tác có tác động; chỉ thực hiện trong phạm vi công việc và môi trường đã được giao. Phiên bản đang phục vụ cần được xác định từ artifact/deployment, không chỉ từ default branch.

## 5. Tiêu chuẩn chung và duy trì

Áp dụng [Engineering Standards](https://github.com/droppii/engineering-knowledge/tree/docs/repository-context-pilot/handbook/standards/standards.md), [Principles](https://github.com/droppii/engineering-knowledge/tree/docs/repository-context-pilot/handbook/principles/principles.md) và [AI Context](https://github.com/droppii/engineering-knowledge/tree/docs/repository-context-pilot/handbook/ai-context/README.md). Cấu trúc đang có giải thích cách tìm mã; những phần chưa theo tiêu chuẩn cần được cải thiện khi sửa, không trở thành ngoại lệ mặc định.

Cập nhật tài liệu này trong cùng thay đổi khi module, toolchain, public contract, pipeline hoặc điều kiện kiểm thử thay đổi. Quyết định đặc thù cần được ghi ở repository; kiến thức và tiêu chuẩn dùng chung được duy trì tại Engineering Knowledge.
