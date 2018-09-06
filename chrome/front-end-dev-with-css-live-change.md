Thay đổi css local thì trang web tự cập nhật các thay đổi chứ không cần phải reload lại trang:

DevTools > Source > Filesystem, sau đó add new workspace tới dev-source của mình đang dev

--> Các thay đổi này là 2 chiều, thay đổi trong Chrome sẽ được overwrite lên local space. Thay đổi ở local space (via IDE, e.g) sẽ được live update về Chrome.
