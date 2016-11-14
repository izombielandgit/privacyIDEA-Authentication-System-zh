# 14. Code Documentation|代码文档

The code roughly has three levels.

代码大致有三个级别。

## 14.1. API level

The API level is used to access the system. For some calls you need to be authenticated as administrator, for some calls you can be authenticated as normal user. These are the token and the audit endpoint. For calls to the validate API you do not need to be authenticated at all.

API级别用于访问系统。对于某些调用，您需要以管理员身份进行认证，对于某些调用，您可以认证为普通用户。这些是token和audit endpoint。对validate API的调用，您不需要进行认证。

At this level Authentication is performed. In the lower levels there is no authentication anymore.

在该级别执行Authentication。在较低级别中，不再有认证。

The object g.logged_in_user is used to pass the authenticated user. The client gets a JSON Web Token to authenticate every request.

对象g.logged_in_user用于传递经过认证的用户。客户端获得一个JSON Web令牌来认证每个请求。

API functions are decorated with the decorators admin_required and user_required to define access rules.

API函数由装饰器admin_required和user_required来定义访问规则。

## 14.2. LIB level

At the LIB level all library functions are defined. There is no authentication on this level. Also there is no flask/Web/request code on this level.

在LIB级别定义所有库函数。此级别上没有认证。此外，此级别上没有flask/Web/request代码。

Request information and the logged_in_user need to be passed to the functions as parameters, if they are needed.

请求信息和logged_in_user需要作为参数传递给函数（如果需要）。

If possible, policies are checked with policy decorators.

如果可能，使用策略装饰器检查策略。

## 14.3. DB level

On the DB level you can simply modify all objects.

在DB级别上，您可以简单地修改所有对象。

14.3.1. The database model