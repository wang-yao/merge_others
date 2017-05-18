1.aidl应用于同进程及不同进程中的服务绑定。 <br>
2.服务中的IBinder返回的是aidl接口中的Stub的实例。<code>new Stub() > onBind return stub</code> <br>
3.不同进程的应用中使用Stub需要定义完全相同的aidl文件，包括包名，文件名及方法名。<br>
4.同进程使用仍可以使用Stub代理。<br>
5.绑定服务一般采用隐式启动方式，除需要意图外还要提供将要访问的进程的包名.<br>
6.Stup通过代理系统中描述相同的IBinder的实例进行进程间的通信。<br>
7.Service连接实例也需要通过代理获取IBinder的实例。<code> <<Stub.asInterface(IBinder) </code>
