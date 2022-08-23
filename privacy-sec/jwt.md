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
0. Signature: Để biết được token nhận được có valid hay không (có bị thay đổi nội dung không). Trước khi dùng (và lưu) jwt, cần validate sign của token.

### Claims

Claim là những thông tin về đối tượng đang được nhắc đến. Ví dụ trong jwt lúc nào cũng có claim `name`, thường để phân biệt các đối tượng. Một claim trong jwt có dạng key/value pair, trong đó key lúc nào cũng là string và value là JSON value. Ví dụ token sau có 3 claims

```json
{
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true
    }
```

Có 2 loại claims:

0. Có sẵn.
0. Custom.

## OIDC (OpenID Connect) spec