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
}
```
As you can see each method/function is by max 3 to 4 lines.
- You can find the functional code for this application at : https://github.com/Scrappers-glitch/Vital-Watch/blob/254f827e8f566149250948f37e8ae84ede3fce06/app/src/main/java/com/scrappers/vitalwatch/core/RFCommSetup.java#L25

[C/C++] example : Operating on `CpuClock` to calculate an elapsed time of an operation (Building Chronograph) :
```cxx
/**
 * @file Chronometer.h
 * @author pavl_g.
 * @brief Calcualtes the elapsed time for a specific algorithm using cpu clocks.
 * @version 0.1
 * @date 2022-02-12
 * 
 * @copyright Copyright (c) Scrappers.
 * 
 */
#ifndef CHRONOGRAPH 
#define CHRONOGRAPH 
#include<ctime>
#define Clock clock_t
#define CPU_FREQUENCY CLOCKS_PER_SEC 
namespace Meters { 
	/**
	 * @brief Calcualtes the elapsed time using the cpu clocks and returns it in milliseconds.
	 * 
	 */
struct Chronometer {
	private:
		double* time;
		double* elapsedTime;
	public:
		Chronometer() noexcept;
		~Chronometer();
		/**
		 * @brief Get the current total cpu clock ticks since application start.
		 * 
		 * @return const Clock* a memory location pointing at the current cpu ticks.
		 */
		const Clock* getCPUClock();
		/**
		 * @brief Converts the cpu clocks to doubles.
		 * 
		 * @param cpuClock the cpuclock memory address.
		 * @return const double* a memory address to the clocks in milliseconds.
		 */
		const double* toDouble(const Clock* cpuClock);
		/**
		 * @brief Gets the Elapsed Time which is the difference between time1 and time0.
		 * 
		 * @param time0 the first time.
		 * @param time1 the second time.
		 * @return const double* a memory address to the elapsed time between time0 and time1.
		 */
		const double* getElapsedTime(double* time0, double* time1);
	};
}
#endif
```
```cxx
/**
* C-lib
* @author pavl_g.
*/
#include<Chronometer.h>

Meters::Chronometer::Chronometer() noexcept {
	this->time = new double();
	this->elapsedTime = new double();
}

const Clock* Meters::Chronometer::getCPUClock() {
	Clock* cpuClock = new Clock();
	*cpuClock = clock(); 
    return cpuClock;
}			

const double* Meters::Chronometer::toDouble(const Clock* cpuClock) {
	// the cpuClock is the number of cpu ticks or cycles and by convention : CPU_F = cycles/seconds
	// so, seconds = cycles / CPU_F.
	// and to convert it to millisecond then multiply the result by 1000. 
	*time = (double) (*cpuClock / CPU_FREQUENCY) * 1000;
	 delete cpuClock;
   return time;
}

const double* Meters::Chronometer::getElapsedTime(double* time0, double* time1) {
	*elapsedTime = *time0 - *time1;
    return elapsedTime;
}

Meters::Chronometer::~Chronometer() {
	delete (this->time);
	delete (this->elapsedTime);
}
```
Usage inside java (JNI) : 
```cxx
/**
* JNI code
* @author pavl_g.
*/
#include<utils_Chronograph.h>
#include<Chronometer.h>

extern "C" {
	Meters::Chronometer chronograph;
	JNIEXPORT jdouble JNICALL Java_utils_Chronograph_getCPUClock(JNIEnv* env, jobject object) {
		double cpuClockTime = *(chronograph.Meters::Chronometer::toDouble(chronograph.getCPUClock()));
		return cpuClockTime;
	}

	JNIEXPORT jdouble JNICALL Java_utils_Chronograph_getElapsedTime(JNIEnv* env, jobject object, jdouble time0, jdouble time1) {
		double elapsedTime = *(chronograph.Meters::Chronometer::getElapsedTime(&time0, &time1));
		return elapsedTime;
	}
}
```
[Java] CallBack :
```java
package utils;

import java.lang.IllegalAccessException;
import physics.Units;

/**
 * A utility used for calculating the elapsed time of an algorithm 
 * or various algos at the same time using the native clock speed.
 *
 * @author pavl_g.
 */
public final class Chronograph {
    
    private double[] records = new double[0];
    private int nOfRec = records.length - 1;
    private static Chronograph chronograph;
    private static final Object chronometer = new Object();
    
    static {
         System.loadLibrary("ArithmosNatives");
    }   
	
    private Chronograph() {
    }
    
    public static Chronograph getChronometer() {
        if (chronograph == null) {
             synchronized(chronometer) {
                  if (chronograph == null) {
                        chronograph = new Chronograph();
                  }
             }
        }
        return chronograph;
    }
    
    public void recordPoint() {
        double[] temp = new double[records.length];
        temp = records;
        records = new double[records.length + 1];
        for (int i = 0; i < temp.length; i++) {
            records[i] = temp[i];
        }    
        records[++nOfRec] = getCPUClock();
    }
    
    public void removePoint(final int index) throws IllegalAccessException {
        if (index < 0 || index > nOfRec) {
            throw new IllegalAccessException("Cannot find this record !");
        }
        final double[] temp = new double[records.length];
        // fill a new array without the specified index
        for (int i = 0; i < records.length; i++) {
            if (i == index) {
                continue;
            }
            temp[i] = records[i];
        }
        // remove the empty times (clamp times)
        records = new double[temp.length - 1];
        for (int i = 0; i < temp.length; i++) {
            if (temp[i] == 0.0d) {
                continue;
            }
            records[i] = temp[i];
        }
    }
    
    public double getPoint(final int index) throws IllegalAccessException  {
        if (index < 0 || index > nOfRec) {
            throw new IllegalAccessException("Cannot find this record : " + index);
        }
        return records[index];
    }
    
    public void reset() {
        records = new double[0];
        nOfRec = records.length - 1;
    }
    
    public double getElapsedTime(int index0, int index1) throws IllegalAccessException  {
        return getElapsedTime(getPoint(index1), getPoint(index0));
    }
    
    public double toSeconds(final double time) {
        return Units.convertInto(time, Units.IndexNotation.MILLI);
    }
    
    private native double getCPUClock();
    private native double getElapsedTime(final double time0, final double time1);
}
```
As you can see some functions are large, but still not exceeding 20-25 lines which is still good, but if you want to improve, we had better restructure this api to be 10 lines at max per function.
You can find the full functioning code at : https://github.com/Scrappers-glitch/Arithmos

## Functions arguments (parameters)
- Functions should be of zero arguments as far as possible.
- Arguments above 2 arguments make using a function a bit harder.
- Arguments above 2 arguments make a function harder to test.
- 3 or more arguments need to be placed inside a data model class (class holding only some data and passing them as parameters to other places).
- Arguments should be only input and the output would be via a return statement.
- Output arguments are very confusing.
- Examples : 
```java
public void add(int a, int b) {
    a = a + b;
}
```
As you can see, this example adds two numbers a and b and store the result in the first number, this is dumb, because how we would really get that result and print it to our screen ? Are we supposed to build that using variables and pass variables only to this function so we can have a visible pointer to store the result inside ?

## Functions malicious activity (side effects)
- A function doing something that reflects to be something else not as documented or not from what can be anticipated from its name.
- A function doing something not documented in addition to its primary functionality.
- Example :
```java
public class UserValidator {
	private Cryptographer cryptographer;
	public boolean checkPassword(String userName, String password) {
		User user = UserGateway.findByName(userName);
		if (user != User.NULL) {
			String codedPhrase = user.getPhraseEncodedByPassword();
			String phrase = cryptographer.decrypt(codedPhrase, password);
			if ("Valid Password".equals(phrase)) {
				Session.initialize();
				return true;
			}
		}
		return false;
	}
}
```
As you can see this funtion should only check the password for this username and return a boolean, but it should never initialize the session if it was valid.
The refractory of this function is the removal of `Session.initialize();`.
If you for some reasons want to build a listener for the validity check, you can either inject an action or use an abstract class with some asbtract methods :
```java
public interface ValidatorListener {
       void onValidationSuccess(InitializationPolicy initPolicy);
       void onValidationFailure(InitializationPolicy initPolicy);
}
public enum InitializationPolicy {
	INIT_ON_VALIDATION, INIT_LATER
};
public class LoginScreen extends Screen implements ValidatorListener {
	@Override
	public void onScreenInstantiation(Object[] args) {
		final UserValidator validator = new UserValidator(args);
		validator.setValidatorListener(this);
		validator.setInitPolicy(InitializationPolicy.INIT_ON_VALIDATION);
		final boolean checkResult = checkPassword("Clean-Code", "Java");
	}
	@Override
	public void onValidationSuccess(InitializationPolicy initPolicy) {
	    if (initPolicy == InitializationPolicy.INIT_ON_VALIDATION) {
		Session.initialize();
	    }
	}
	@Override
	public void onValidationFailure(InitializationPolicy initPolicy) {
		//TODO-Something on validation failure
	}
}
public class UserValidator {
	private InitializationPolicy initPolicy = InitializationPolicy.INIT_LATER; 
	private Cryptographer cryptographer;
	private ValidatorListener validatorListener;
	
	public boolean checkPassword(String userName, String password) {
		User user = UserGateway.findByName(userName);
		if (user == User.NULL) {
			checkValidity(validatorListener).onValidationFailure(initPolicy);
			return false;
		}
		String codedPhrase = user.getPhraseEncodedByPassword();
		String phrase = cryptographer.decrypt(codedPhrase, password);
		if ("Valid Password".equals(phrase)) {
			checkValidity(validatorListener).onValidationSuccess(initPolicy);
			return true;
		}
		checkValidity(validatorListener).onValidationFailure(initPolicy);
		return false;
	}
	public void setInitPolicy(final InitializationPolicy initPolicy) {
		this.initPolicy = initPolicy;
	}
	public void setValidatorListener(final ValidatorListener validatorListener) {
		this.validatorListener = validatorListener;
	}
	private ValidatorListener checkValidity(final ValidatorListener arg) {
		if (arg == null) {
			return new ValidatorListener() {
				@Override public void onValidationSuccess(InitializationPolicy initPolicy) {}
				@Override public void onValidationFailure(InitializationPolicy initPolicy) {}
			};
		}
		return arg;
	}
	
}
```
As you can see, we have introduced an even better code with an intialization policy and a validation listener in which we can listen and optionally inject some actions when a validation comes to true but this should never be a mandatory procedure !! It's all the way an optional code for improvements.
