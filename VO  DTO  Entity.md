#### VO | DTO | Entity

1、entity 里的每一个字段，与数据库相对应，

2、vo 里的每一个字段，是和你前台 html 页面相对应，

3、dto 这是用来转换从 entity 到 vo，或者从 vo 到 entity 的中间的东西 。

举个例子：

你的html页面上有三个字段，name，pass，age

你的数据库表里，有两个字段，name，pass ， 注意没有 age。

而你的 vo 里，就应该有下面三个成员变量 ，因为对应 html 页面上三个字段 。

private string name；

private string pass; 

private string age;
这个时候，你的 entity 里，就应该有两个成员变量 ，因为对应数据库表中的 2 个字段 。

private string name；

private string pass

PS： dto 和 entity 里面的字段应该是一样的，dto 只是 entity 到 vo，或者 vo 到 entity 的中间过程，如果没有这个过程，你仍然可以做到增删改查，这是根据具体公司规范来的 。