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
  - fix warning about rootContext not used?

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
