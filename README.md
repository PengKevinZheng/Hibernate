# Hibernate

Hibernate Configuration: 

Following is the list of important properties you would require to configure for a databases:

                1. hibernate.dialect 

                This property makes Hibernate generate the appropriate SQL for the chosen database.

                2. hibernate.connection.driver_class

                The JDBC driver class.

                3.hibernate.connection.url

                The JDBC URL to the database instance.

                4.hibernate.connection.username

                The database username.

                5.hibernate.connection.password

                The database password.

                6.hibernate.connection.pool_size

                Limits the number of connections waiting in the Hibernate database connection pool.

                7.hibernate.connection.autocommit

                Allows autocommit mode to be used for the JDBC connection.

                8.hibernate.show_sql:

                show the sql in console for testing purpose.

                9.hibernate.format_sql: 

                the sql shown in the console is easy to read if this parameter is true;

                10.hibernate.hbm2ddl.auto:

                hibernate.hbm2ddl.auto Automatically validates or exports schema DDL to the database when the SessionFactory                 is created. 

The list of possible options are:

                validate: validate the schema, makes no changes to the database.

                update: update the schema.

                create: creates the schema, destroying previous data.

                create-drop: drop the schema at the end of the session.

Note: 

表空间(tablespace)是一个逻辑容器，它和数据文件关联起来，一个表空间至少有一个数据文件与之关联。一个表空间可以有多个段，一个段只能属于一个表空间。

    方案（schema）又叫模式，是比表空间小一级的逻辑概念，它也是一个逻辑容器。多个用户可能共用一个表空间，那如何区分开每一个用户？那么在表空间中对每个用户都有一个对应的方案，用于保存单个用户的信息。
    
    oracle中存储的层次结构总结如下：
    
                一、数据库由一个或多个表空间组成

                二、表空间由一个或多个数据文件组成，一个表空间包含多个段(segment)

                三、段由一个或多个区(extent)组成

                四、区是数据文件中一个连续的分配空间，由一个或多个块(block)组成

                五、块是数据库中最小、最基本的单位，是数据库使用的最小的I/O单元

                六、每个用户都有一个对应的方案(schema)



Hibernate Util: 

Now a helper class is needed to get Hibernate up and running. This class creates a SessionFactory object which in turn can open up new Session's. A session is a single-threaded unit of work, the SessionFactory is a thread-safe global object instantiated once. For our application the HibernateUtil class is implemented below:

  

          import org.hibernate.SessionFactory;
          import org.hibernate.cfg.Configuration;

          public class HibernateUtil {
    
                private static final SessionFactory sessionFactory;

                static {
                    try {
                        // Create the SessionFactory from hibernate.cfg.xml
                        sessionFactory = new Configuration().configure().buildSessionFactory();
                    } catch (Throwable ex) {
                        // Make sure you log the exception, as it might be swallowed
                        System.err.println("Initial SessionFactory creation failed." + ex);
                        throw new ExceptionInInitializerError(ex);
                    }
                }

                public static SessionFactory getSessionFactory() {
                    return sessionFactory;
                }

          }
          
Place HibernateUtil.java in a util package next to your main class package(s).


Session:

to create a session, we have two methods: 

                1.sessionFactory.openSession();

                2.sessionFacotry.getCurrentSession();

Difference:

                1. using the getCurrentSession () requires in hibernate.cfg.xml file add the following configuration: 
 
                * If you are using a local transaction (jdbc Affairs) 

                <property name="hibernate.current_session_context_class"> thread </ property> 

                * If you are using the global transaction (JTA transaction) 

                <property name="hibernate.current_session_context_class"> jta </ property>

                2.  Session.openSession():it creates a new session(for new session) ;
                Session.getCurrentSession(): return the existing session; 
                采用getCurrentSession()创建的session会绑定到当前线程中，而采用openSession()
                创建的session则不会

                3. 采用getCurrentSession()创建的session在commit或rollback时会自动关闭，而采用openSession()
                创建的session必须手动关闭
 
  





