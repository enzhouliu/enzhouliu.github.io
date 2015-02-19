---
layout: post
title: Run setup and teardown for Unittest in instance level (not testcase level).
---
We all know that for unittest, JUnit for Java, we need to setup and teardown enviroment for testcases. Usually we use @Before/@After to decorate init and teardown method.
But sometimes, we do need setup or teardown for all the testcases in a test class.

For my current project, we clean/insert database for testings and rollback when test finishes. For my testcases all the tests can share the same setup since they are all queries.
And the insert rollback time is relatively long. If I run the same setup for each testcase, it would cost me several minutes. So an instance setup/teardown would be perfect for this situation.

we used spring for dependency injection, which injects database connection based on environment settings. I tried to put the init logic in the constructor, but unforturnately, the dependency objects are injected after construtor. So in the cosntructor I can not get the db session to clean and insert
data into database. The I looked up on internet, and I found this elegent solution:

So basically, the idea is extend the test runner and call the instace level init and tear down method in your customized runner at the right time.

Here is the code for SpringJUnit4ClassRunner.

1. Create an interface to define the init and tearup method:
{% highlight ruby linenos %}
public interface InstanceTestClassListener{
    void beforeClassSetup();
    void afterClassTearDown();
}
{% endhighlight %}

2. Inherit from the SpringUnit4ClassRunner, then override the createTest and run method to call setup and teardown method
{% highlight ruby linenos %}
public class SpringInstanceTestClassRunner extends SpringJUnit4ClassRunner{
    private InstanceTestClassListener instanceSetupListener = null;
    
    public SpringInstanceTestClassRunner(Class<?> clazz) throws InitializationError{
        super(clazz);
    }
    
    @Override
    public Object createTest() throws Exception{
        Object test = super.createTest();
        if(test instanceof InstanceTestClassListener && instanceTestClassListener == null){
            instanceSetupListener = (InstanceSetupListener)test;
            instanceSetupListener.beforeClasSetup();
        }
        return test;
    }
    
    @Override
    public void run(RunNotifier notifier){
        super.run(notifier);
        if(instnaceSetupListner != null){
            instanceSetupListener.afterClassTearDown();
            instanceSetupListener = null;
        }
    }
}
{% endhighlight %}

3. Run your test with your customerized runner and implements InstanceTestClassListener interface.
{% highlight ruby lineno %}
@RunWith(SpringInstanceTestClassRunner.class)
@ContextConfiguration(location={"/context.xml"})
public class ServiceTest implements InstanceTestClassListener{
    
    @Override
    public void beforeClassSetup(){
        //setup here
    }
    
    @Override
    public void afterClassTeardown(){
        //teardown here
    }
}
{% endhighlight %}




