Device manager that replaced devfs in the first iterations of the Linux kernel.

Triggers in multiple ways, here it gets triggered when a usb device is plugged in OR when booting the system, which we use for our persistence.

However, udev is not made for "long-running" programs, and simply cannot trigger them.

We have to detach the process using `at`.
