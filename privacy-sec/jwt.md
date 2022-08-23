## JWT

JWT là một dạng token dùng trao đổi thông tin một cách 'securely' giữa các bên với nhau. Mục tiêu là gọn nhẹ. JWT là một standard. Tức nó sẽ có các implementations. Tức (2), tất cả JWT đều là tokens.

Thường nó gọn nhẹ, nên nó sẽ được truyền đi bằng (1) HTTP header hoặc (2) POST param.

JWT được sử dụng vào các case sau:

- Authentication (Xác thực).
- Authorization (Phân quyền).
- Info exchange (trao đổi dữ liệu).

JWT có 3 phần, được encode bằng base64, ngăn cách bằng dấu chấm (`.`).

0. Header: Chứa thông tin: Loại token và Giải thuật dùng để mã hóa nội dung token.
0. Payload (1 loạt các Claims): Chứa các thứ liên quan đến sec, ví dụ như định danh user và các quyền mà user có.

## OIDC (OpenID Connect) spec