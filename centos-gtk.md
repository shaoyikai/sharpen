### 环境安装

必须

`yum install gtk2 gtk2-devel gtk2-devel-docs`

选装

`yum install gnome-devel gnome-devel-docs`

测试环境是否成功

```c

#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
        GtkWidget *windows;
        gtk_init(&argc,&argv);
        windows = gtk_window_new(GTK_WINDOW_TOPLEVEL);

        gtk_widget_show(windows);
        gtk_main();
        return 0;
}

```

运行

- gcc main.c-o main `pkg-config --libs --cflags gtk+-2.0`
- ./main



