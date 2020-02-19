# Golang learning log

Các bài nên đọc trước [Tour of Go][1], [How to Write Go Code][2]

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
