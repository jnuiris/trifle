#idea数据库折腾小记  
<p>同学邀请试一下idea连接数据库，上次用idea连数据库已经是一年多前了，现在基本上用的webstorm，不过既然webstorm和idea是一家的，想必连接方式差不多，简单试一下。</p>
<p><strong>首先必须说明，我使用的是idea教育版，用学校邮箱白嫖的。</strong></p>
<p>由于本篇博客需要上传到github，因此就不加图片了，因为github要显示图片需要上传到图床很麻烦。</p>
<p>首先打开idea,点击右侧栏的database,点击+,选择data source，就可以选择需要连接的数据库类型，我们首先测试mysql,在弹出的框中填入mysql的种种信息，点击test connection,然后报错，在提示的框中点击serverTimezone中设置时区，改成Asia/Shanghai，再次test发现就成功了，这时候我们可以在弹出的console.sql中编写sql代码并且测试。</p>
<p>idea已经可以连上mysql了，接下来是代码连接mysql，我们需要下载mysql驱动，地址<blockquote>链接：https://pan.baidu.com/s/1LTTpxwSHX9fchsz0oQQkHg 
提取码：qwer</blockquote></p>
<p>
然后用以下的示例代码就可以连接数据库。
<pre>
<code>
import java.sql.*;
public class MySQLDemo {
    static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://localhost:3306/manage";
    //static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
    //static final String DB_URL = "jdbc:mysql://localhost:3306/RUNOOB?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC";
    static final String USER = "root";
    static final String PASS = "342401zgaw!!!";
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try{
            // 注册 JDBC 驱动
            Class.forName(JDBC_DRIVER);
            // 打开链接
            System.out.println("连接数据库...");
            conn = DriverManager.getConnection(DB_URL,USER,PASS);
            // 执行查询
            System.out.println(" 实例化Statement对象...");
            stmt = conn.createStatement();
            String sql;
            sql = "SELECT * FROM t_book";
            ResultSet rs = stmt.executeQuery(sql);
            while(rs.next()){
                int num = rs.getInt("price");
                System.out.println(num);
//                System.out.println(rs.toString());
                // 通过字段检索
//                int id  = rs.getInt("id");
//                String name = rs.getString("name");
//                String url = rs.getString("url");
//
//                // 输出数据
//                System.out.print("ID: " + id);
//                System.out.print(", 站点名称: " + name);
//                System.out.print(", 站点 URL: " + url);
//                System.out.print("\n");
            }
            rs.close();
            stmt.close();
            conn.close();
        }catch(SQLException se){
            se.printStackTrace();
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            try{
                if(stmt!=null) stmt.close();
            }catch(SQLException se2){
            }// 什么都不做
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
        System.out.println("Goodbye!");
    }
}
</code>
</pre>
</p>
<p>
sqlserver基本跟mysql一样，不过这里有个插曲，我忘了sqlserver用户的密码，其实也很简单，首先windows登录进去然后进管理页面修改密码就可以了。sqlserver的默认端口是1433,然后驱动会有点区别，sqlserver驱动下载地址:<blockquote>
链接：https://pan.baidu.com/s/1GmaW9O-MoTwMFzypPbMGUw 
提取码：qwer </blockquote>
</p>
<p>
示例代码如下:
<pre>
<code>
import java.sql.*;
public class SQLServerDemo {
    //驱动路径
    private static final String DBDRIVER = "com.microsoft.sqlserver.jdbc.SQLServerDriver";
    //数据库地址
    private static final String DBURL = "jdbc:sqlserver://localhost:1433;DataBaseName=jxgl";
    //数据库登录用户名
    private static final String DBUSER = "sa";
    //数据库用户密码
    private static final String DBPASSWORD = "342401zgaw!!!";
    //数据库连接
    public static Connection conn = null;
    //数据库操作
    public static Statement stmt = null;
    //数据库查询结果集
    public static ResultSet rs = null;
    //程序入口函数
    public static void main(String[] args) {
        try {
            //加载驱动程序
            Class.forName(DBDRIVER);
            //连接数据库
            conn = DriverManager.getConnection(DBURL, DBUSER, DBPASSWORD);
            //实例化Statement对象
            stmt = conn.createStatement();
            rs = stmt.executeQuery("select * from Course");
            while(rs.next()){
                String Cname = rs.getString("Cname");
//                String Id = rs.getString(1);
//                String Name = rs.getString(2);
//                String Age = rs.getString(3);
//                String Sex = rs.getString(4);
//                System.out.print("学号:"+Id);
//                System.out.print(" 姓名:"+Name);
//                System.out.print(" 年龄:"+Age);
//                System.out.println(" 性别:"+Sex);
//                System.out.println("------------------------------------");
                System.out.println(Cname);
            }
            stmt.close();
            conn.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
</code>
</pre>
然后就可以用java代码操控本地数据库了。
</p>
<p>
注意事项
<ol>
<li>以上驱动仅适用于JDK8，其他版本并没有测试过。</li>
<li>添加驱动的方式是,file->project_structure->dependencies</li>
<li>这篇文章中有数据库端口设置，不过这次我并没有用到:<blockquote>https://cloud.tencent.com/developer/news/203568</blockquote></li>，也许有机会用到。
</ol>
</p>
<ol>
<p><strong>参考文章太多，不再一一列出。</strong></p>
<p>
心得:
<br/>
在经历了前端一系列复杂环境的搭建磨砺后，现在觉得这些问题也比较简单，悟已往之不谏，知来者之可追，加油。
</p>

