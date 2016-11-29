# Objective-C Coding Standards

## Overview

I Finally got round to digging up an old template I wrote and used across various teams I lead (Projects at Deloitte Digital & miPic to name a few). It acts as a framework I normally use for the internal coding standards for the teams im in. 

If you happened to land here on accident actually wanting to learn Objective-C coding standards (why...you do know there is something called Swift...) I would highly recommend taking a look at my colleague and super dev [Alberto De Bortoli's](https://github.com/albertodebortoli) book [Zen and the Art of the Objective-C Craftsmanship](https://github.com/objc-zen/objc-zen-book) which he co-authored.

Would also like to thank [Pete Cornish](https://github.com/outofcoffee) for providing the idea and first draft for this...also another super dev, check him out.

Finally a nod towards [NY Times Objective C style guide](https://github.com/NYTimes/objective-c-style-guide) for help setting the tone of this document

### Languages ###

The guide only covers Objective-C

### Contributions ###

If you feel there is something you disagree with, or missing, then do something about it!!!

This is intended to be a living document and therefore we welcome feedback and contributions via [Pull Requests](https://github.com/ssh88/Objective-C-Coding-Standards/pulls).

If you intend to create a pull request please consider the following

 * This document is created using the markdown language, follow suite and use the correct syntax!
 * All PR's must include @ssh88 as the reviewer.
 * Only reviewers can approve and merge the PR.

### Table Of Contents ###

[TOC]

## Naming Conventions ##

Consistent and good naming conventions are vital to creating code that is easy to use, understand and maintain. Taking time to think about the right name is important, especially if it is part of the API that will be used other developers.

### Abbreviations ###

Following Apple's naming conventions, Objective-C is a verbose language and therefore abbreviations should be avoided.

**Do this:**

```objc
updateProfileDetails
```

**Not this:**

```objc
updteProfDetls
```

Being clear is more important than reducing code. 

Acronyms on the other hand are promoted for some object such as MOC (Managed Object Context). An acronym is always written in uppercase (e.g., ```MOC```). Don't abuse the use of acronyms i.e do not use ```VC``` for view controllers!!!

### Word boundaries ###

Use camel casing to indicate the start/end of words within the same identifier

**For Example**:

```obj
sharedInstance
currentStatus
```

### Type-specifying names ###

If incorporating the type into the object name, make it the last word. 

**Do this:**

```objc
UIButton *continueButton;
UILabel *firstNameLabel;
UITextField *firstNameTextField;
```

**Not this:**

```objc
UIButton *buttonContinue;
UILabel *labelFirstName;
UITextField *textFieldFirstName;
```

Do not use old-school prefixes for object

**Not this:**

```objc
UIButton *btnContinue;
UILabel *lblFirstName;
UITextField *txtFirstName;
```

### Class prefix ###

All classes must be prefixed with 3 uppercase characters.

**For example:**

 ```objc
 MIP
 DMK
 GRZ
 ```

### Image files ###

Camel casing should be used for image names, however if an image relates to a state, then the image name should be suffixed with an underscore followed by the state name.

**For example:**

```objc
filterButton_selected.png
filterButton_unselected.png

alertSwitch_on.png
alertSwitch_off.png
```

Asset Catalogs must be used with no exceptions. In addition to using asset catalogs, when referencing image files in code, do not use the file extension.

**Do this:**

```objc
[UIImage imagedNamed:@"logo"];
```

**Not this:**

```objc
[UIImage imagedNamed:@"logo.png"];
```

### Objects ###

Always suffix the class name by its parent object type. 

**For example:**

| Object Type          | Suffix          |
|:-----------          |:--------------- |
| UIViewController     | ViewController  |
| UIView               | View            |
| UIImageView          | ImageView       |
| UILabel              | Label           |
| UIButton             | Button          |

### Constants & Enums ###

Follow the Apple convention of using camel casing, prefixing your Constant and Enum names with kClassName, this is to avoid name space collisions.

**For example:**

```objc
kDMKFooProgressStatus
```

If a the value of a string constant is not needed, then the constant string value must match the words in the constant name, minus the k prefix.

**For example:**

```objc
NSString * const kDMKFooProgressStatus = @"DMKFooProgressStatus";
```

Do not use ```#define``` to declare constants....do not use ```#define``` period! More on that later :)

### iVars & Properties ###

Use camel casing for iVar and property names.

**For example:**

```objc
UIButton *loginButton;
```

All global instance variables must use the ```@property``` declarations without exception. 

**For example:**

```objc
@property (nonatomic, strong) UIButton *loginButton;
```

This automatically creates backing iVars (using the property above this would be ```_loginButton```) as well as the the setter/getter methods.

Property attributes let the complier/ARC know how to treat the properties memory retention.

### Method names ###

Method names should follow the camel casing syntax.

**For example:**

```objc
- (void) checkForValidToken;
```

Methods without parameters should not be named ```getFooBar``` or ```setFooBar```, these should be reserved for the names of getter/setters. Instead use names such as ```fetchFoo```, ```calculateFoo```, ```checkForFoo``` etc to describe is actions.

### Protocols ###

#### Protocol Name ####

Protocols should follow the Apple naming convention by using the class name suffixed with the protocol name. 

**For example:**

Take the class ```DMKProfileViewController``` which has a protocol named ```Delegate``` it should be declared as:

`DMKProfileViewControllerDelegate`

The same class with a protocol named ```Data Source``` should be declared as:

`DMKProfileViewControllerDataSource`

#### Protocol Method Names ####

In addition to following the same syntax as explained in **Method names**, protocol method names should start with the class name, without the prefix, and also pass the class back as an argument. 

**No arguments:**

**Do this:**

```objc
- (void) profileViewControllerDidDissmissView:(DMKProfileViewController *)profileViewController;
```

**Not this:**

```objc
- (void) DMKProfileViewControllerDidDissmissView;
```

**With arguments:**

**Do this:**

```objc
- (void) profileViewController:(DMKProfileViewController *)profileViewController didUpdateProfileData:(NSDictionary *)profileData;
```

**Not this:**

```objc
- (void) DMKProfileViewControllerDidUpdateProfileData:(NSDictionary *)profileData;
```

## Language Usage ##

### Singleton pattern ###

Generally the singleton pattern is used for shared manger classes. The method name for returning a singleton of a class should be named ```sharedInstance``` 

**For example:**

```objc
[UserSessionManager sharedInstance];
```

The method implementation should follows Apple's recommendation by using the ```dispatch_once_t``` queue.

**For example:**

```objc
+ (instancetype)sharedInstance
{
    static dispatch_once_t once;
    static id sharedInstance;
    dispatch_once(&once, ^{
        sharedInstance = [[self alloc] init];
    });
    return sharedInstance;
}
```

### Blocks ###

In-line blocks should be short to aid readability.

**For Example:**

```objc
^(BOOL completed){
    self.status = kPhotoUploadStatusCompleted;
    self.progressBar.hidden = YES;
    self.selectedPhoto = nil;
}
```

If a large implementation is required within a block, call a method from inside the block instead which handles the implementation.

**For Example:**

```objc
^(BOOL completed){
    [self handleUploadCompletion];
};
```
    
To avoid retain cycles, self should not be referred to directly but via a weak reference variable. This is not strictly necessary in all cases, but it's best to follow this pattern to avoid 'hard-to-debug' issues.

**For Example:**

```objc
__weak typeof(self) weakSelf = self;

^{
    UIViewController *viewController = weakself.viewController
});
```

If the block will be reused often as a variable, it is best practice to ```typedef``` the block.

**For Example:**

```objc
typedef void (^kDMKURLSessionManagerCompletionBlock)(NSDictionary *response, NSError *error);
```

This allows it to be used as a variable or property. When declaring a block property, ensure to use the ```copy``` attribute.

**For Example:**

```objc
@property(nonatomic, copy) kDMKURLSessionManagerCompletionBlock completionBlock;
```

### Expressions ###

#### Brackets ####

Use brackets to group logic to improve readability of code, however do not overuse.

**Do this:**

```objc
NSInteger result = (a * b) / (c + d);
```

**Not this:**

```objc
NSInteger result = a * b / (c + d);
```

**Do this:**

```objc
BOOL result = (a && b || c == d);
```

**Not this:**

```objc
BOOL result = ((a && b) || (c == d));
```

#### Coercion ####

Don't compare a Boolean value to true or false.

**Do this:**

```objc
if (flag)
```

**Not this:**

```objc
if (flag == true)
```

**Do this:**

```objc
BOOL flag = a && b;
```

**Not this:**

```objc
BOOL flag = (a && b) != false;
```

#### Nil checking ####

TODO: Also worth noting optionals api in iOS 9

**Do this:**

```objc
if (!view) 
{
    
} 
else 
{
  
}
```

**Not this:**

```obj
if (view == nil) 
{
   
}
else
{
   
}
```

#### Comparison ####

Write comparisons in the order that they read most naturally.

**Do this:**

"if n is 3"

```objc
if ( n == 3 )
```

**Not this:**

"if 3 is n"

```objc
if ( 3 == n ) 
```

#### Increment/decrement operators ####

**Do this:**

```objc
for (NSInteger index = 0; index < array.count; index++)
```

**Not this:**

```objc
for (NSInteger index = 0; index < array.count; ++index)
```

#### Ternary operator ####

The ternary operator is promoted for simple logic, as long as it does not affect the readability. For more complex logic use an if/else statement.

**Do this:**

```objc
return (a < b ? a : nil);
```

**Not this:**

```objc
return a < b ? -1 : (a > b ? 1 : 0);
```

### Statements ###

#### Switch statements ####

Logic within case statements should not be longer than a few lines. If a large implementation is required, call a method within the case instead.

**For example:**

```objc
switch (status) 
{
    NSString *alertTitle = nil;

    case kStatusPending:
        alertTitle = @"Pending";
        break;
    case kStatusInProgress:
        alertTitle = @"In Progress";
        break;
    case kStatusCompleted:
        // large implementation
        [self loadNewObject];
        break; 
    default:
        alertTitle = @"Error";
        break;

    if(alertTitle)
    {
        [self showAlertWithTitle:alertTitle];
    }
}
 
-(void) loadNewObject 
{
    // large implementation goes here
}
```

#### If statements ####

Always encapsulate if statement logic within parenthesis, even if the logic is a single line.

**Do this:**

```objc
if (flag) 
{
    view.hidden = NO;
}
```

**Not this:**

```objc
if (flag)
    view.hidden = NO;
```

**Do this:**

```objc
if (flag) 
{
    view.hidden = NO;
} 
else 
{
    view.hidden = YES;
}
```

**Not this:**

```objc
if ( flag )
    view.hidden = NO;
else
    view.hidden = YES;
```

#### For Loop ####

As with If Statements, always encapsulate the loops (```for```,```while```,```do``` etc) logic within parenthesis.

**Do this:**

```objc
for (NSInteger index = 0; index < subviews.count; index++) 
{
   [self updateSubviewAtIndex:index];
}
```

**Not this:**

```objc
for (NSInteger index = 0; index < subviews.count; index++) 
   [self updateSubviewAtIndex:index];
```

Declare the loop variable inside the parentheses of the for statement, unless it is reused elsewhere.

**Do this:**

```objc
for (NSInteger index = 0; index < 3; index++)
{

}
```

**Not this:**

```objc
NSInteger index;
for (index = 0; index < 3; index++)
{

}
```

#### Fast Enumeration ####

Commonly known as a for-in/for-each loop. If a counter is not needed, use this loop over a the classical ```for``` loop above. However consider using the ```enumerateObjectsUsingBlock:``` if available.

Always encapsulate the loops logic with parenthesis.

```objc
for(NSString *string stringsArray)
{
    ...
}
```

#### return statements ####

Enclose a return value in brackets if the statement is evaluating something.

**Do this:**

```objc
return (n + 1);
```

**Not this:**

```objc
return n + 1;
```

Returning from the middle of a method is OK, but consider if using if-else statement would make the code more readable. 

**For example:** 

```objc
if(n<10)
{
    return 10;
} 
else
{
    return n;
}
```

is more explicit in it's intent than:

```objc
if(n<10)
{
    return 10;
}

return n;

```

### Dot Syntax vs Message sending ###

#### Properties Vs iVars ###

The dot syntax should always be used when modifying the setter and getter methods of a property. Since Xcode 4 properties are automatically synthesized meaning their default setters and getters are created.

Calling the dot syntax on a property will automatically update the iVar that is backing it.

**For Example:**

Setter and Getter Methods for Property ```@property (nonatomic, strong) NSString *foo```

```
- (void) setFoo:(NSString *)foo
{
    _foo = foo;
}

- (NSString *) foo
{
    return _foo;
}

```

### Declarations ###

#### Method visibility ####

Only declare methods in the header file if they need to be called from outside the class.

___

Note:

If writing a test for a private method that is not declared in the header file, in the unit test class create a class extension and declare the method there. This allows the test class to have visibility of the private method, while at the same time keeping the method private in live code.

This pattern can also be used for properties, constants etc.

___

#### Property Visibility ####

As with method visibility above, think about whether a property really needs to be declared in the header file as opposed to the class extension in the implementation file. In most cases IBOutlet properties should be private.

If it does need to be exposed outside the class, then think whether it needs to be edited from outside the class, if not declare it as ```readonly``` in the header file and ```readwrite``` in the implementation file.

**For example:**

Header file (.h):

```objc
@property (nonatomic, readonly, strong) NSString *fooString;
```

Implementation file (.m):

```objc
@property (nonatomic, readwrite, strong) NSString *fooString;
```

#### Local variables ####

Declare local variables at or just before the point of first use. Don't declare them all at the top of a method.

#### Constants ####

When declaring constants it is important the order in which you declare the pointer. Constants should start with the data type followed by the pointer then by the ```const``` key.

**Do this:**

```objc
NSString * const kFooCellIdentifier = @"FooCellIdentifier"; 
```

**Not this:**

```objc
NSString  const * kFooCellIdentifier = @"FooCellIdentifier";
```

Placing the pointer in a different location will cause the constant not to be recognised as its data type.

Also think about the visibility of the constant, if it needs to be exposed outside of the class, it must be declared in the header field with a prefix of ```extern``` and then again in the implementation file without the ```extern``` prefix.

**For example:**

Header file (.h):

```objc
extern NSString * const kFooCellIdentifier; 
```

Implementation file (.m):

```objc
NSString * const kFooCellIdentifier = @"FooCellIdentifier"; 
```

#### Enums ####

There are 3 ways to declare enums in Objective C, most of which are legacy implementations which have changed as the language has matured. 

Only the following enum declaration should be used:

```objc
typedef NS_ENUM('DATA TYPE', 'ENUM NAME')
```

As explained in **Naming Conventions**, each enum value must be prefixed by the enum name followed by the value name.

**For example:**

```objc
typedef NS_ENUM(NSInteger, kFooStatus) {
    kFooStatusUnknown,
    kFooStatusInProgress,
    kFooStatusCancelled,
    kFooStatusCompleted,
};
```

Assigning a value to one or more values is optional.

#### Import Vs Forward Declarations ####

Imports of libraries and classes should be done in the implementation file. If a method or property in the header file takes an argument whose data type is in a given library/class, consider using the ```@class``` statement to declare the library/class to safely use the data type.

**For example:**

```
@class fooClass;
```

There are some occasions where a library/class needs to be imported in the header file, for example if sub classing or to use a non-object type such as an enum from the library/class. However keep in mind the goal is to keep the header file light as possible.

#### Comments ####

Comments for methods, classes etc should follow Apple's recommended conventions, this is important so that technical API documentation can be correctly auto generated. 

**Header File Comments**

TODO: revise

Comment for classes:
  
```
```
  
Comment for methods:
  
```
```
  
Comment for header file separators:
  
```

```
 
Other comments in header file:
  
```

```
  
**Implementation File Comments**
  
The following link explains how to use the correct Apple style comments plus a few more fancy comments: 
  
https://github.com/tomaz/appledoc/blob/master/CommentsFormattingStyle.markdown

### Literals ###

In addition to strings, iOS 5 extended literals in Objective C to more objects. The ```@``` symbol can now be used as a short-hand to create and assign instances of objects making code more concise and readable.

#### Numbers ####

**Do this:**

```objc
NSNumber *number = @(5);
```

**Not this:**

```objc
NSNumber * number = [NSNumber numberWithInt:5];
```

#### Arrays ####

**Do this:**

```objc
NSArray *myArray = @[@”One”, @”Two”];
NSString *secondString = myArray[1];
```

**Not this:**

```objc
NSArray *myArray = [NSArray arrayWithObjects:@”One”,@”Two”,nil];
NSString *secondString = [myArray objectAtIndex:1];
```

#### Dictionaries ###

**Do this:**

```objc
NSDictionary *myDictionary = @{
                                @”key1” : @”value1”,
                                @”key2” : @”value2”
                            };

NSString *value = myDictionary[@”key1”];
```

**Not this:**

```objc
NSDictionary *myDictionary = [NSDictionary 
dictionaryWithObjectsAndKeys:@”value1”, @”key1”, @”value2”, @”key2];
NSString *value = [myDictionary objectForKey:@”key1”];
```

## File Organization ##

### Pragma Marks ###

TODO: examples of common prgame marks i.e view life cycle, setup, delegates etc

Each header and implementation file should be grouped using pragma marks, both having the same order and pragma mark structure.

### Class File Size ###

Classes should be lean as possible. Follow the MVVM (MVC if you must :( ) pattern and move different types of logic to the correct classes i.e business logic to service layers, networking to networking layers, view logic to the view layer.

If your class is over 400 lines you should really revise your architecture.

### White Spacing ###

White spaces should be kept to a minimum.

No more than a single blank line should be used as a vertical separator between methods, implementations, variables etc. 

In an implementation, group common logic together, again only using a single blank line to separate groups.

### Service Classes ###

TODO: Explain service class uses i.e business logic, stateless etc

### Protocol Managers ###

Objects such as table views, collection views and map views usually require objects that conform to their protocols to implement many lines of code. For example a view controller conforming to the ```dataSource``` and ```delegate``` protocols of a table view will normally implement the following methods:

```objc
- tableView:cellForRowAtIndexPath:
- tableView:didSelectRowAtIndexPath:
- tableView:numberOfRowsInSection:
- tableView:heightForRowAtIndexPath:
```

Another way to further optimise class sizes is to move these protocol method implementations to a separate class known as a protocol manager.

The following example illustrates how a protocol manager is implemented and used by a view controller to manage a table view.

**Protocol Manager header file:**

```objc
@protocol FooTableViewManagerDelegate <NSObject>

@end

@interface FooTableViewManager : NSObject

- (instancetype)initWithDelegate:(id<FooTableViewManagerDelegate>)delegate tableView:(UITableView *)tableView;

@end
```

Notice how the table view manager is instantiated with the view controller as its delegate. This is so the manager can communicate any actions performed back to view controller, such as a row being selected or to reload the table view when it updates its datasource. 

At the view controller, the call site is simple and clean, and now all logic is shifted away from the view controller.

```objc
self.tableViewManager = [FooTableViewManager alloc] initWithDelegate:self tableView:self.tableView];

```

It is important to note the table view manager **DOES NOT** hold a reference to the table view, any actions that need to be performed on the table view outside of the ```UITableViewDataSource``` or ```UITableViewDelegate``` protocol methods, are delegated back to the table view managers delegate, in this case, ```FooViewcContoller```

**For example:**

Table view manager protocol method to reload a table view.

```objc
- (void) fooTableViewManagerReloadData:(FooTableViewManager *)fooTableViewManager;
```

Also, the table view manager should not manipulate the ```dataSource```, and therefore does not hold a reference to the ```datasource```. Any time the data is needed, it is requested via a protocol method

```
- (NSArray *) fooTableViewManagerTableData:(FooTableViewManager *)fooTableViewManager;

...
...

#pragma mark - UITableViewDataSource

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return self.delegate.fooTableViewManagerTableData.count
}
```

However, as you can see the table view is passed as an argument in the table view manager's ```initWithDelegate:tableView:``` method, this allows the manager to setup additional attributes on the table view, for example:

**Protocol Manager implementation file:**
```
@interface FooTableViewManager()
<
UITableViewDataSource,
UITableViewDelegate
>
@property (nonatomic, weak) id <FooTableViewManagerDelegate> delegate;

@end

@implementation FooTableViewManager

- (instancetype)initWithDelegate:(id <FooTableViewManagerDelegate>)delegate tableView:(UITableView *)tableView
{
    self = [super init];
    if (self) {
        self.delegate = delegate;
        [self setupTableView:tableView];
    }
    return self;
}

#pragma mark - setup

- (void) setupTableView:(UITableView *)tableView
{
    tableView.dataSource = self;
    tableView.delegate = self;
}
```

Above, the implementation file conforms to the ```UITableViewDataSource``` or ```UITableViewDelegate``` protocol methods, this then allows the table view manager to set the ```delegate``` and ```dataSource``` properties of the table view.

Other cases where the manager may need to interact with the table view outside of its protocol methods, is registering cells or setting a collection view layout

**For example:**

```
#pragma mark - setup

- (void) setupTableView:(UITableView *)tableView
{
    ...
    ...
    tableView.rowHeight = UITableViewAutomaticDimension;
    [self registerCellsForTableView:tableView];
}
```

Using this pattern, the view controller remains light and only needs to worry about UI logic such as the view aspects of the table view, shifting all the data and cell logic to the table view manager. 

This pattern should be used for collection views, map views and any other object which requires a large implementation of its protocols.

## Best practices ##

### Platform Independence ###

TODO: finish this i.e NSInteger over int,

Integers

Booleans

Floats

Doubles

### Base Classes & Categories ###

It is recommended to create base classes of common objects early on in development. Chances are the project will contain many UI elements such as buttons which will look the same, labels and text fields using the same font etc. Sub classing these elements and implementing common behavior on initialization makes the reuse and maintenance of these elements easier.

Other objects such as view controllers, table view cells etc are likely to contain common methods that would be reusable throughout the code base. For example a common behavior of a view controller is to style its sub views, apply localisation and cancel the editing mode of its sub views when the background is tapped. It makes sense to implement these behaviors in a base view controller that all view controller can inherit from. Again this makes the maintenance of these objects easier as the code base scales.

Another preferred practice is to create categories of default libraries in iOS to extend functionality. A good example is to create a category of the ```UIColor``` class, adding custom colours which are unique to the project.

Though base classes and categories effectively achieve the same goal, use base classes to create new shared functionality and categories to extend existing functionality.

TODO: add section on protocols for abstract classes

### Date Formatters ###

Frequently used formatters should be created as static values whenever possible, as creation is expensive.

**For Example:**

```objc
+ (NSDateFormatter *) dateFormatter
{
    static NSDateFormatter *dateFormatter = nil;
    if (!dateFormatter)
    {
        dateFormatter = [[NSDateFormatter alloc] init];
        /* 1988-03-19 */
        [dateFormatter setDateFormat:@"yyyy-MM-dd"];
    }
    return dateFormatter;
}
```

### Threading ###

  *   Leverage GCD/NSOperation for any long running operations e.g. database, network or storage access
  *   No long running operations on main / UI thread
  *   No updates to UI from background thread

### Anti-Patterns and Gotchas ###

There are a number of iOS specific things to look for:

  *   No hard coded strings or numbers. For configuration; use an external files such as a plist or JSON file.
  *   Unsuppressed compiler warnings; these should be resolved, and if not, suppressed with a valid comment explaining why it's safe.

### Auto Layout ###

Use Auto Layout for all UIView subclasses

...ALWAYS
...SERIOUSLY
...NO EXCEPTION
...DONT KNOW IT? 
...LEARN
....ASK! 

Auto layout future proofs projects to support future devices.

When applying auto layout on views, test the layout on different devices sizes and orientations. 

If using Interface Builder to apply auto layout, the view size and orientation can be toggled in the attributes inspector.

If applying auto layout in code, use the ```updateViewConstraints``` method in UIViewControllers or the ```updateConstraints``` in UIViews.

### Macros ###

Writing macros is discouraged, as they can be difficult to get debug/maintain by developers that did not write it, and thus have unforeseen consequences. Do not use the ```#define``` statement!

### Targets ###

To build multiple versions of an app for different environments i.e development, UAT, Release; consider leveraging build configurations and schemes, rather than multiple targets. 

Targets require more maintenance i.e multiple plists, ensuring all files are added to the correct targets etc. They should only be used if the target is actually another app (watch, TV) rather than pointing to a different environment,

## Frameworks & Tools ##

### IDE ###

XCode / AppCode no exceptions.

### Dependency management ###

CocoaPods no exceptions.

For internal development libraries submodules may be an option

### Test distribution ###

Only developers should run apps directly on devices via Xcode. All other users, such as Testers, should install apps via the test distribution service Hockey App.

All test builds are distributed to Hockey App via the Jenkins CI (see **Continuous integration**). This way all versions are tracked and managed correctly. Following internal release management processes is important in order to limit risks, deployment issues and to avoid the overall fuss around 'getting a build out'.

### Continuous Integration ###

Use Jenkins for continuous integration and delivery.
TODO: Talk fast lanes

### Xcode plug-ins ###

To manage Xcode plugins, consider using Alcatraz which can be installed via:

```
curl -fsSL https://raw.github.com/supermarin/Alcatraz/master/Scripts/install.sh | sh
```

See this article for some good suggestions: http://nshipster.com/xcode-plugins/

### Mogenerator ###

Use the Mogenerator framework in conjunction with Core Data. Mogenerator creates an additional model class per entity which is auto generated every time the model is updated, leaving the default model class to contain custom implementation without ever being overwritten.  

Mogenerator can be installed via homebrew:

```
$ brew install mogenerator
```

More information can be found here:

```
http://rentzsch.github.io/mogenerator/
```

### Logging ###

TODO: Update once logging tools are installed. Talk about diff levels of logging i.e debug, info

### Testing ###

#### Unit Testing ####

Use the ```XCTest``` framework for unit tests and the ```OCMock``` framework for mocking. For code coverage reports use gcovr (installed via gem package manager).

Method names for unit tests should be the name of the method being tested (prefixed by ```test``` as required by ```XCTest```) followed by an underscore then the test scenario.

**For Example:**

For the method ```fetchUserDetailsWithToken:``` test methods should be named along the lines of:

```objc
-(void) testFetchUserDetailsWithToken_unauthenticaedUser;
-(void) testFetchUserDetailsWithToken_authenticaedUser;
-(void) testFetchUserDetailsWithToken_unvalidSession;
```

This allows developers to easily understand not only what method is being tested, but also what the test scenario is.

#### UI Testing ####

TODO: XCT vs Appium for UI Tests

### Crash Tools 

TODO: Talk Crashlytics

### Analytic tools ###

TODO: Talk flurry/fabric
