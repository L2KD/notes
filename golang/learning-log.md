# Golang learning log

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

```
Repository
   │
   └── module

// hoặc

Repository
   │
   ├── module0
   ├── module1
   └── ...
```

Mọi fn, types, var, const khai báo trong src file này đều visible với các src file khác (giống java), tức là package scope.
