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
