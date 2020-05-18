Salesforce Apex is pretty finicky with inheritance in comparison to Java.

Even though it is Java based, there are many caveats, especially in inheritance and OO related principles.



In Visualforce, you have the option of denoting either a StandardController or Controller, in addition to controller extensions.  The extensions may or may not have code in common with the controller.  If they do, then you'll want to take advantage of inheritance and creating an abstract controller.



Below is a simple example of a Visualforce page utilizing an abstract controller.



Test.page:
```
<apex:page controller="Test" extensions="Test1,Test2,Test3">
  <apex:form>
    <apex:commandButton value="Go" action="{!go}" rerender="out" />
    <apex:outputPanel id="out">{!output}</apex:outputPanel>
  </apex:form>
</apex:page>
```


Test.cls:
```
public abstract class Test {
  public String output {get; set;}
  private Test controller = null;

  public Test(){}

  public Test(Test controller){
      this.controller = controller;
  }

  public virtual void go(){
      output = 'Test';
  }
}



Test1.cls:
```
public class Test1 extends Test{
  public override void go(){
      this.output = 'Test1';
  }

  //Visual force needs this constructor to perform the controlling logic
  public Test1(Test test){ super(test); }
}
```

Test2.cls:
```
public class Test2 extends Test{
  public override void go(){
      this.output = 'Test2';
  }

  public Test2(Test test){super(test);}
}
```

Test3.cls:
```
public class Test3 extends Test{
  public override void go(){
      this.output = 'Test3';
  }

  public Test3(Test test){super(test);}
}
```

FYI, if you're reading through the Advanced developer study guide, 'Test1' is printed after clicking  the go command button.
