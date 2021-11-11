---
title: For vs Code
sidebar_position: 1
---

# Các Extensions hữu ích cho vs code (Javascript)

## Visual Studio Intellicode
Đây là 1 extensions hữu ích khi cài đặt, extensions này giúp giảm thời gian code với việc gợi ý các code đã được nhiều người sử dụng tương ứng với code đang lập trình.
Nó sẽ gợi ý các hàm, biểu thức, biến chưa sử dụng.


## ESLint
Là một extension giúp chỉ ra các lỗi trong lập trình để người dùng có thể nhận biết và khắc phục.

## Material Icon Theme
Là extensions format lại các icon file hoặc folder giúp người dùng dễ nhìn và nhận biết hơn, đối với các ngôn ngữ khác nhau.

## Javascript (ES6) Code snippet
Extensions này sẽ gợi ý các code về Javascript giúp người sử dụng tiết kiệm thời gian hơn

## Bracket Pair Colors
Đây là extensions giúp format các cặp ngoặc trong code cùng màu để người dùng dễ nhận biết ta đang thao tác trên phạm vi hoặc function nào.

## Prettier - Code Formatter
Là extensions giúp format code chuẩn chỉ hơn với cách tự động lưu code thành 1 dạng nhất định và chuẩn theo form giúp code nhìn dễ chịu hơn và đẹp hơn.
Vì trong vs code cũng đã có formatter code riêng, để chạy được Prettier ta cần config lại 1 chút trong file settings.json của hệ thống:
Trong phần settings, tìm kiếm default formatter và chọn edit settings.json:
- Edit lại 1 chút trong file:
```js
"editor.defaultFormatter": "esbenp.prettier-vscode",
"[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
},
```
Ở đây sẽ set lưu file được định dạng theo Prettier, ngôn ngữ đc chỉ định là Javascript, nó cũng đc set default cho toàn ngôn ngữ khác.

## Typescript React code Snippet
Đây là extensions gợi ý code cho Typescript, sẽ được sử dụng trong các project sử dụng Typescript giúp check code dễ dàng hơn

## Import Cost
Là extensions giúp check khi ta import module hoặc thư viện vào nó sẽ tốn mất bao nhiêu kb hoặc thêm bao nhiêu kb, có thông tin khi nén file import đó.