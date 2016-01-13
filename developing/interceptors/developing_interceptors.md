# Developing Interceptors

Interceptors are a powerful event-driven model inside CommandBox that allows you to listen to broadcasts from within the core code to respond to events and even influence execution.  Interceptors are packaged within CommandBox Modules and can be for your own use, just debugging, or to share with the world.

## Creating An Interceptor

An interceptor is a CFC that has one or more methods whose names match the name of an interception point that is broadcast.  Interceptors are packaged within CommandBox modules.  When the module is loaded, the InterceptorService will register your interceptor and take care of calling it when necessary.
