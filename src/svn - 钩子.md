# SVN HOOK(钩子)

### 钩子类型

| 类型 |	触发条件|	用途|	备注|
|   :-    |  :-  |  :-  |  :- |
|start-commit |	提交事务生成之前|	一般可根据用户名对提交进行权限控制,一般多用于对svn版本功能check的检查|	参数为2个,svn1.5为3个|
|pre-commit	|事务已生成,但是尚未提交|	可根据提交的内容或者comment进行控制,比如提交日志不能为空	||
|post-commit|	提交之后|	数据进行备份,以及修改提示的mail操作,或者驱动自动构成	||
|pre-revprop-change|	|属性变更前,尚未变化|	可以进行属性变更权限的控制	|
|post-revprop-change|	|属性变更前	进行版本属性的备份以及mail通知操作等	||
|pre-lock	|加锁之前	|可进行是否能加锁的控制	||
|post-lock|	加锁之后	|可进行mail通知,使开发成员了解此文件已经加锁|	往往有时结合特定流程,比如有提交冻结期的结束,可结合这个属性进行使用|
|pre-unlock|	解锁前	|可进行是否能解锁的控制	||
|post-unlock|	解锁之后|	可进行mail通知,使开发成员了解此文件已经解锁	往往有时结合特定流程,比如有提交冻结期的结束,可结合这个属性进行使用||

### 钩子类型参数

|类型|	参数1|	参数2|	参数3|	参数4|	参数5|
|   :-    |  :-  |  :-  |  :- |:- |:- |
|start-commit|	代码库路径|	用户名|	-	|-	|-|
|pre-commit	|代码库路径	|事务名|	-	|-	|-|
|post-commit|	代码库路径|	版本号|	事务名	|-	|-|
|pre-revprop-change|	代码库路径|	版本号|	用户名	|属性名|	ACTION|
|post-revprop-change|	代码库路径|	版本号|	用户名|	属性名|	ACTION|
|pre-lock	|代码库路径	|路径|	用户名	|Comment STEAL(1
|post-lock	|代码库路径|	用户名|	-	|-	|-|
|pre-unlock	|代码库路径	|路径	|用户名|	TOKEN|	BREAK_UNLOCK(1|
|post-unlock|	代码库路径|	用户名|	-|	-|	-|