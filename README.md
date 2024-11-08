
前段时间在搬迁项目的时候，遇到一个问题，就是用sqlsugar调用oracle的存储过程的时候调用不了；


当时卡了一整天，现在有空了把这个问题记录分享一下。


先去nuget上安装一下sqlsugar的包：


![](https://img2024.cnblogs.com/blog/2064545/202411/2064545-20241108101949650-1382148116.png)


再安装一个oracle的驱动：


![](https://img2024.cnblogs.com/blog/2064545/202411/2064545-20241108102105481-296588224.png)


添加一下Json包：


![](https://img2024.cnblogs.com/blog/2064545/202411/2064545-20241108102805222-1710393278.png)


再去创建一下连接


![](https://img2024.cnblogs.com/blog/2064545/202411/2064545-20241108102304253-1713610700.png)


 再创建一个测试用的存储过程




```
create or replace procedure pr_test(i_name   in varchar2,
                                    i_age    in varchar2,
                                    o_result out sys_refcursor) as

begin

  open o_result for
    select * from dual;

end pr_test;
```


创建一个类来接受存储过程返回的数据




```
    public class People
    {
        public string Dummy { get; set; }
    }
```


单独把存储过程里面的那句sql拿出来执行，会得到下面的结果：


![](https://img2024.cnblogs.com/blog/2064545/202411/2064545-20241108105112170-619734170.png)


dual这个表是oracle提供的一个表，里面就一个X，一般可以用这个来测试数据库连接是不是正常。


调用的方式如下：


![](https://img2024.cnblogs.com/blog/2064545/202411/2064545-20241108103630126-367139416.png)


里面那个游标的入参必须是个空字符，我之前尝试过object，null，就是没想到过会是一个空字符。


当时也是没想到一个空字符，就把我卡了一个下午，这个坑应该是不会再踩了。


 本博客参考[wgetCloud机场](https://tabijibiyori.org)。转载请注明出处！
