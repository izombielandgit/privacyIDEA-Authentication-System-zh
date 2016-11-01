# 7. Userview|用户视图

The administrator can see all users in realms he is allowed to manage.

> Note:
> 
> Users are only visible, if the useridresolver is located within a realm. If you only define a useridresolver but no realm, you will not be able to see the users!

You can select one of the realms in the left drop down box. The administrator will only see the realms in the drop down box, that he is allowed to manage. **(TODO)** No migrated, yet.

![user-view](../Contents/user-view.png)

User View. List all users in a realm.

The list shows the users from the select realm. The username, surname, given name, email and phone are filled according to the definition of the useridresolver.

Even if a realm contains several useridresolvers all users from all resolvers within this realm are displayed.

* [7.1. User Details](7.1. User Details 用户细节.md)
* [7.2. Manage Users](7.2. Manage Users 管理用户.md)