# 9. Event Handler|事件处理程序

Added in version 2.12.

What is the difference between Policies and event handlers?

Policies are used to define the behaviour of the system. With policies you can change the way the system reacts.

With event handlers you do not change the way the system reacts. But on certain events you can trigger a new action in addition to the behaviour defined in the policies.

## 9.1. Events|事件

Each **API call** is an **event** and you can bind arbitrary actions to each event as you like.

Internally events are marked by a decorator “event” with an event identifier. At the moment not all events might be tagged. Please drop us a note to tag all further API calls.

![event-list](../Contents/event-list.png)

An action is bound to the event token_init.

## 9.2. Handler Modules and Actions|处理程序模块和操作

The actions are defined in handler modules. So you bind a handler module and the action, defined in the handler module, to the events.

The handler module can define several actions and each action in the handler module can require additional options.

![event-details](../Contents/event-details.png)

The event sendmail requires the option emailconfig.

## 9.3. Conditions|条件

Added in version 2.14

And event handler module may also contain conditions. Only if all conditions are fullfilled, the action is triggered. Conditions are defined in the class property conditions and checked in the method check_condition. The UserNotification Event Handler defines such conditions.

### 9.3.1. Available Handler Modules

* [9.3.1.1. User Notification Handler Module](9.3.1.1. User Notification Handler Module 用户通知处理程序模块.md)

