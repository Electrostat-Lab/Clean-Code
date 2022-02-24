# Functions 
In this chapter you are expected to learn how to write functions efficiently with an easy to read and maintaible format.

## Small functions
- Writing small functions is the key to writing good functions.
- A function should have no more than 10 - 15 lines of code.
- A function shouldn't ever exceed 20 lines of code.
- A function should do only one thing perse.
- A function shouldn't delegate work to other functions or classes, and if it would that should be visible from the function name and/or documentations.
- Practically speaking, programs with functions of just 4 - 5 lines of code are always the best.

## Avoid Nested block control of flow structures
- Avoid writing `nested statements` (conditional or loop statements) inside the functions block as it increases the size of the function.
- Writing `nested statements` inside a function block adds a burden documentary bear to the situation (you should never document the mess you write, rather clean it first).

## Do one thing
- Again, a function should do only one thing and it should state that thing in its self-descriptive name.
- If you ever thought of dividing a function into sections, it's far better to take your time to separate the tasks inside this fucntion into 
functions if they are dependant upon each other.
- If there are some tasks that need to be called together inside a function, they must be first distributed among several functions and then collect their call 
in an `initialize()` or `startTransaction()` functions if they are a beginning of operation, or put the call in a `Util` (which is a static final class) for quick call.

## One level of abstraction per function
- Avoid mixing between low level and high level computing instructions which indeed is confusing.
example : mixing between a `getter` of an abstract tree and a `String` concatenation procedure.
- If you have to say create a `url` before parsing data to another high level of abstraction function, you better include a function in the higher level
state that does so.

## The Stepdown Rule
- Inside your classes, as you read your code from top to bottom, you should write your functions in the order you will call them for instructions.
- A function should have a code that continues after the function before it and introduces the next function.
- Overriden functions should be in the beginning in the same order from the super class.
- Embedded interfaces and enums should be at the beginning and it would be better to be in their own respective files.

## Switch statements 
- Switch statements are large by nature.
- It's hard to make a program with small switch statements that do one thing, because they are multi-functional by nature.
- Example : 
```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
  switch (e.type) {
    case COMMISSIONED:
        return calculateCommissionedPay(e);
    case HOURLY:
        return calculateHourlyPay(e);
    case SALARIED:
        return calculateSalariedPay(e);
    default:
        throw new InvalidEmployeeType(e.type);
  }
}
```
This function violates the following rules : 
- Single Responsibility per principle (it must do the same thing differently for different situations -polymorphism-).
- Open Closed Principle.
- It must adapt with any new addtitions.
### A better version of this function is to refractor the program to use the ABSTRACT FACTORY PATTERN :
1) The Employee hierarchy : 
- Employee.java
```java
public abstract class Employee {
  public abstract boolean isPayday();
  public abstract Money calculatePay();
  public abstract void deliverPay(Money pay);
}
```
- Children of employee : `HourlyEmployee`, `CommisionedEmployee`, `SalariedEmployee`.
```java
public class HourlyEmployee extends Employee {
   @Override
   public boolean isPayday() {
        ....
   }
   @Override
   public Money calculatePay() {
        ....
   }
   @Override
   public void deliverPay(Money pay) {
        ....
   }
   .....
}
```
2) The Factory pattern that creates a new employee object :
- The factory class : EmployeeFactory.java
```java
public abstract class EmployeeFactory {
    public abstract Employee getEmployeeByRecord(final EmployeeRecord record) throws InvalidEmployeeType;
}
```
- The factory implementation : EmployeeFactoryImpl.java
```java
public class EmployeeFactoryImpl extends EmployeeFactory {
    @Override
    public Employee getEmployeeByRecord(final EmployeeRecord record) throws InvalidEmployeeType {
        switch(record.getType()) {
            case COMMISIONED :
                  return new CommisionedEmployee();
            case HOURLY :
                  return new HourlyEmployee();
            ....
            default:
                  throw new InvalidEmployeeType("Employee Type not found !");
        }
    }
}
```
In this version of code, whenever there is a new type of employee, we will add it here in the factory implementation without touching the other employee classes and building chaos.
[C/C++] Version :
- Employee
```cxx
/**
* Employee.h
* @author pavl_g.
*/
#ifndef ABSTRACT_EMPLOYEE
#define ABSTRACT_EMPLOYEE
class Employee {
   public:
        Employee();
        ~Employee();
        virtual bool isPayday();
        virtual Money& calculatePay();
        virtual void deliverday();
        ....
};
#endif
```
```cxx
/**
* Employees.h
* @author pavl_g.
*/
#ifndef EMPLOYEES
#define EMPLOYEES
#include<Employee.h>
namespace EmployeeType {
  class HourlyEmployee: public Employee {
        public:
            HourlyEmployee();
            ~HourlyEmployee();
  };
  class InvalidEmployeeType() {
        public: 
            InvalidEmployeeType();
  };
}
#endif
```
```cxx
/**
* Employees.cxx
* @author pavl_g.
*/
#include<Employees.h>
EmployeeType::HourlyEmployee() {
    ...
}
EmployeeType::~HourlyEmployee() {
    ...
}
bool EmployeeType::isPayday() {
    ...
}
Money& EmployeeType::calculatePay() {
    ...
}
void EmployeeType::deliverday() {
    ...
}
```
```cxx
/**
* InvalidEmployeeType.cxx
* @author pavl_g.
*/
#include<Employees.h>
EmployeeType::InvalidEmployeeType() {
    #error Employee not found !
}
```
- Factory pattern
```cxx
/**
* EmployeeFactory.h
* @author pavl_g.
*/
#ifndef EMPLOYEE_FACTORY
#define EMPLOYEE_FACTORY
#include<Employee.h>
class EmployeeFactory {
    public: 
        EmployeeFactory();
        ~EmployeeFactory();
        virtual Employlee& getEmployeeByRecord(Record&);
};
#endif
```
```cxx
/**
* EmployeeFactoryImpl.h
* @author pavl_g.
*/
#ifndef EMPLOYEE_FACTORY_IMPL
#define EMPLOYEE_FACTORY_IMPL

#include<EmployeeFactory.h>
class EmployeeFactoryImpl: public EmployeeFactory {
    public:
        EmployeeFactoryImpl();
        ~EmployeeFactoryImpl();
};

#endif
```
```cxx
/**
* EmployeeFactoryImpl.cxx
* @author pavl_g.
*/
#include<EmployeeFactoryImpl.h>
#include<Employees.h>

EmployeeFacotryImpl() {
    ...
}

~EmployeeFacotryImpl() {
    ...
}

Employee& EmployeeFacotryImpl::getEmployeeByRecord(Record& record) {
    switch(record.getType()) {
        case COMISSIONED:
            return EmployeeType::ComissionedEmployee();
        case HOURLY:
            return EmployeeType::HourlyEmployee();
        ....
        default:
            return EmployeeType::InvalidEmployeeType();
    }
}
```

## Use descriptive names :
- Use a self-descriptive name to the functions that do only one thing.
- Names could be long.
- A long descriptive name is better than a long descriptive comment.
- Choosing self-descriptive names would clarify the intentions of the module in your mind and would help you improve it.
- It's common to refractor some code, just to give new namings with meaningful keywords.
- favorable restructuring of the code.
- Use the same phrases, nouns, and verbs in the function names you choose for your modules.
- Examples : 
  - A Teacher-Student Platform would have a page to add some students to a teacher's class, so at first the names were : `AddStudentsScreen`, `StudentsDataReader` which can be refractored to better names as : `StudentsPortal` and `StudentsPortalReader` (which fetches the data and fills the screen with students' data), the naming now is consistent with the students portal which is the entry gate to the virtual class of a teacher.
  - A Spacecraft driving simulator game was having a class named `Player` and another class named `NPC` (non-player charachter), the refractored names were : `Spacecraft` and its subclasses (`StarwarsShip`, `MarsRover`, `DarkFuryRover` ,....) inside `ship` package and `Opponent` and its subclasses (`Turrent`, `Rover`, `Submarine`) inside the `opponent` package which also applies the rule of contexting.
  - An idea of a device that measures the heart rate and sends back the result on bluetooth (RFComm) to android was suggested, the application has a class that prepares the android device to attach to the bluetooth module of that device through bluetooth serial profile (which is a bluetooth socket indeed), anyway, a good structure with good names for `RFCommSetup` class that prepares the android device would be : 
```java
/**
 * Setups an RFComm communication service.
 *
 * @author pavl_g.
 */
public class RFCommSetup {
    private final ComponentActivity context;
    private BluetoothSPP bluetoothSPP;
    private RFCommTracker rfCommTracker;

    public RFCommSetup(final ComponentActivity context) {
        this.context = context;
    }

    public RFCommSetup createNewSSPInstance() {
        bluetoothSPP = new BluetoothSPP(context);
        return this;
    }

    /**
     * Turns the bluetooth adapter on if it's off.
     * @return radio frequency instance.
     */
    public RFCommSetup prepare() {
        final BluetoothAdapter bluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
        if(!bluetoothAdapter.isEnabled()){
            context.startActivity(new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE));
        } else {
            if (rfCommTracker != null) {
                rfCommTracker.onPrepare();
            }
        }
        return this;
    }
    /**
     * Initializes the bluetooth serial communication.
     * Must be called after {@link #prepare()} ()}.
     *
     * @return radio frequency instance.
     */
    public RFCommSetup startBluetoothService() {
        if (bluetoothSPP.isBluetoothEnabled()) {
            // setup service protocol -- bluetooth socket
            bluetoothSPP.setupService();
            bluetoothSPP.startService(BluetoothState.DEVICE_OTHER);
            /* setup the data listener, the data listener should then fill a data model */
            if (rfCommTracker != null) {
                rfCommTracker.onInitialize();
            }
        }
        return this;
    }
    public void setupRFCommTracker(final UiModel uiModel) throws InterruptedException, IOException, JSONException {
        /* set the state tracker */
        final BluetoothStateTracker bluetoothStateTracker = new BluetoothStateTracker(context, uiModel).setupCache().prepare();
        bluetoothSPP.setBluetoothConnectionListener(bluetoothStateTracker);
    }
    /**
     * Connects to a device after decoupling the data intent.
     *
     * @param data an intent holding the device's mac address.
     */
    public void connect(final Intent data) {
        bluetoothSPP.connect(data);
        if (rfCommTracker != null) {
            rfCommTracker.onConnectionPassed();
        }
    }

    /**
     * Disconnects the device.
     */
    public void disconnect() {
        if (bluetoothSPP != null) {
            bluetoothSPP.disconnect();
        }
    }
     /**
     * Sets up the rfcomm tracker.
     *
     * @param rfCommTracker the tracker instance.
     */
    public void setRfCommTracker(RFCommTracker rfCommTracker) {
        this.rfCommTracker = rfCommTracker;
    }

    public void launchDevicesScreen(final ActivityResultLauncher<Intent> activityResultLauncher) {
        if (activityResultLauncher == null) {
            throw new IllegalStateException("Must register a callback inside onCreate()");
        }
        final Intent intent = new Intent(context, DevicesScreen.class);
        activityResultLauncher.launch(intent);
    }

    public BluetoothSPP getBluetoothSPP() {
        return bluetoothSPP;
    }
```
As you can see each method/function is by max 3 to 4 lines.
