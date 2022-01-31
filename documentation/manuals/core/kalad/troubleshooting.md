# Kalad Troubleshooting

## **How to solve the error "Failed to lock access to wallet directory; is another `kalad` running"?**

Since alacli may auto-launch an instance of `kalad`, it is possible to end up with multiple instances of `kalad` running. That can cause unexpected behavior or the error message above.

To fix this issue, you can terminate all running `kalad` instances and restart `kalad`. The following command will find and terminate all instances of `kalad` running on the system:

```
pkill kalad
```