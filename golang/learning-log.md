# Golang learning log

Các bài nên đọc trước [Tour of Go][1], [How to Write Go Code][2]

$GOPATH: Biến môi trường để set path tới chỗ để mọi thứ về Go. Mặc định là $HOME/go.

$GOBIN: Biến môi trường để các executable binary sau khi được compiled ra từ src. Mặc định là $GOPATH/bin

## Go command

Build & install

```bash
go install
```

Cái này nó giống với

```
go install .

// hoặc

go install github.com/u/repo
              ^
              |
          Module path
```

Test

```
go test
```

## Go module

Cách go quản lý module.

```
Package0
   │
   ├── source file 0
   ├── source file 1
   └── ...
Package1
   │
   ├── source file 0
   ├── source file 1
   └── ...
```

Mọi fn, types, var, const khai báo trong src file này đều visible với các src file khác (giống java), tức là package scope.

```
Repository
   │
   ├── module
   └── go.mod

// hoặc

Repository
   │
   ├── module0
   ├── module1
   ├── ...
   └── go.mod
```

Typically, 1 repo chỉ chứa 1 module. Trong root dir của repo, có file `go.mod` sẽ khai báo _module path_. Module path là prefix cho các packages của module. Nó còn là địa chỉ để `go` cmd biết đường mà load về module.

[1]: https://tour.golang.org/
[2]: https://golang.org/doc/code.html
