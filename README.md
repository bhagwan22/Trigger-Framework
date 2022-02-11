# Trigger-Framework
Triggers should (IMO) be logicless. Putting logic into your triggers creates un-testable, difficult-to-maintain code. It's widely accepted that a best-practice is to move trigger logic into a handler class.

This trigger framework bundles a single TriggerHandler base class that you can inherit from in all of your trigger handlers. The base class includes context-specific methods that are automatically called when a trigger is executed.

The base class also provides a secondary role as a supervisor for Trigger execution. It acts like a watchdog, monitoring trigger activity and providing an api for controlling certain aspects of execution and control flow.

But the most important part of this framework is that it's minimal and simple to use.


# Usage
To create a trigger handler, you simply need to create a class that inherits from TriggerHandler.cls. Here is an example for creating an Opportunity trigger handler.

public class OpportunityTriggerHandler extends TriggerHandler {

In your trigger handler, to add logic to any of the trigger contexts, you only need to override them in your trigger handler. Here is how we would add logic to a beforeUpdate trigger.
public class OpportunityTriggerHandler extends TriggerHandler {
  
  public override void beforeUpdate() {
    for(Opportunity o : (List<Opportunity>) Trigger.new) {
      // do something
    }
  }

  // add overrides for other contexts

}

**Note:** When referencing the Trigger statics within a class, SObjects are returned versus SObject subclasses like Opportunity, Account, etc. This means that you must cast when you reference them in your trigger handler. You could do this in your constructor if you wanted.
  
    public class OpportunityTriggerHandler extends TriggerHandler {

      private Map<Id, Opportunity> newOppMap;

      public OpportunityTriggerHandler() {
        this.newOppMap = (Map<Id, Opportunity>) Trigger.newMap;
      }

      public override void afterUpdate() {
        //
      }

    }

To use the trigger handler, you only need to construct an instance of your trigger handler within the trigger handler itself and call the _**run()**_ method. Here is an example of the Opportunity trigger.  

    trigger OpportunityTrigger on Opportunity (before insert, before update) {
      new OpportunityTriggerHandler().run();
    }  

# Cool stuf
 ## Max Loop Count
  
  
  
