#Implementation of lifecycle aware beans
See [issue #843] (https://github.com/excilys/androidannotations/issues/843) for more information.

#Status
  - Fragment: working (waiting preliminary review).
  - Activity: not implemented
  - Service: not implemented

#Fragments

##TODO
  - handle bundle savedBeanInstance
  - implementing onPause, onResume
  - emit warning if method signatures are not the expected one
  - fix warning about rootContext not used. Is it mu changes?
````

The value of the field ACompleteBean_.context_ is not used	ACompleteBean_.java	/test-AA-2/.apt_generated/com/example/test_aa_2/beans	line 14
The value of the field AHalfbean_.context_ is not used	AHalfbean_.java	/test-AA-2/.apt_generated/com/example/test_aa_2/beans	line 14	
The value of the field Abean_.context_ is not used	Abean_.java	/test-AA-2/.apt_generated/com/example/test_aa_2/beans	line 14
The value of the field NakedBean_.context_ is not used	NakedBean_.java	/test-AA-2/.apt_generated/com/example/test_aa_2/beans	line 14

````

##Sample

```` java
@EFragment
public class MainFragment extends Fragment {

	@Bean 
	Abean aBean ; 

	@Bean 
	Abean anotherBean ; 
	
	@Bean 
	AHalfbean aHalfBean ; 
	
	@Bean 
	NakedBean nakedBean ; 
	
	@Bean 
	ACompleteBean completeBean ; 
	
	NakedBean simpleNakedBean ; 
	
	DirectBean directBean ; 
}

````

Beans

````java
@EBean
public class NakedBean {

}

@EBean
public class AHalfbean {
	
	@OnCreate
	public void testOnCreate() 
	{
		
	}

	public void testOnDestory() 
	{
		
	}
}

@EBean
public class ACompleteBean {
	
	@OnCreate
	public void testOnCreate(Context context) 
	{
		
	}

	@OnDestroy
	public void testOnDestory(Fragment fragment) 
	{
		
	}
}

@EBean
public class Abean {
	
	@OnCreate
	public void testOnCreate() 
	{
		
	}

	@OnDestroy
	public void testOnDestory() 
	{
		
	}
}

````

Generated

```` java
public final class MainFragment_ ...
{
   ...

    @Override
    public void onCreate(Bundle savedInstanceState) {
        OnViewChangedNotifier previousNotifier = OnViewChangedNotifier.replaceNotifier(onViewChangedNotifier_);
        init_(savedInstanceState);
        super.onCreate(savedInstanceState);
        aBean.testOnCreate();
        aHalfBean.testOnCreate();
        anotherBean.testOnCreate();
        completeBean.testOnCreate(getActivity());
        OnViewChangedNotifier.replaceNotifier(previousNotifier);
    }

...

    @Override
    public void onDestroy() {
        aBean.testOnDestory();
        anotherBean.testOnDestory();
        completeBean.testOnDestory(this);
        super.onDestroy();
    }
}

````
