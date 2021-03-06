# The Whisper Objective-C style guide.

This style guide outlines the coding conventions for Whisper.

## Introduction

There are a good deal of rules in here that have not been strictly adhered to thus far in the codebase. If working on a part of the app that contains such errors, feel free to change them, but any large-scale refactoring should be discussed by the iOS team beforehand.

## Credits

The creation of this style guide was a collaborative effort from various raywenderlich.com team members under the direction of Nicholas Waynik. The team includes: [Soheil Moayedi Azarpour](https://github.com/moayes), [Ricardo Rendon Cepeda](https://github.com/ricardo-rendoncepeda), [Tony Dahbura](https://github.com/tdahbura), [Colin Eberhardt](https://github.com/ColinEberhardt), [Matt Galloway](https://github.com/mattjgalloway), [Greg Heo](https://github.com/gregheo), [Matthijs Hollemans](https://github.com/hollance), [Christopher LaPollo](https://github.com/elephantronic), [Saul Mora](https://github.com/casademora), [Andy Pereira](https://github.com/macandyp), [Mic Pringle](https://github.com/micpringle), [Pietro Rea](https://github.com/pietrorea), [Cesare Rocchi](https://github.com/funkyboy), [Marin Todorov](https://github.com/icanzilb), [Nicholas Waynik](https://github.com/ndubbs), and [Ray Wenderlich](https://github.com/raywenderlich)

We would like to thank the creators of the [New York Times](https://github.com/NYTimes/objective-c-style-guide) and [Robots & Pencils'](https://github.com/RobotsAndPencils/objective-c-style-guide) Objective-C Style Guides.  These two style guides provided a solid starting point for this guide to be created and based upon.

The Whisper specific changes that have been made by [Paul Fleiner](mailto:paul@whisper.sh)

## Background

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Language](#language)
* [User Interface Programming](#user-interface-programming)
* [Code Structure](#code-structure)
* [Spacing](#spacing)
* [Comments](#comments)
* [Naming](#naming)
  * [Underscores](#underscores)
* [Methods](#methods)
* [Variables](#variables)
* [Property Attributes](#property-attributes)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Case Statements](#case-statements)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Init Methods](#init-methods)
* [Class Constructor Methods](#class-constructor-methods)
* [CGRect Functions](#cgrect-functions)
* [Golden Path](#golden-path)
* [Error handling](#error-handling)
* [Singletons](#singletons)
* [Line Breaks](#line-breaks)
* [Xcode Project](#xcode-project)


## Language

US English should be used.

**Preferred:**
```objc
UIColor *myColor = [UIColor whiteColor];
```

**Not Preferred:**
```objc
UIColor *myColour = [UIColor whiteColor];
```


## User Interface Programming

In the interest of sanity, we have chosen to avoid the use of Interface Builder. Between the countless implicit decisions it makes for the developer, the masochistic merge conflicts, and the annual XCode update gridlock, we have found programmatic autolayout to be much much simpler (although it may take a little getting used to).

We use [our fork](https://github.com:whisper1/UIView-Autolayout) of smileyborg's [UIView-Autolayout](https://github.com/smileyborg/UIView-AutoLayout). This has since been deprecated in favor of a new project, [PureLayout](https://github.com/smileyborg/PureLayout), which we will soon fork and begin using.


## Code Structure

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```objc

@interface WWhateverViewController () <FirstProtocol, SecondProtocol>

@property (nonatomic, strong) id customProperty;

@end

#pragma mark - Class

+(instancetype)whatever {}

#pragma mark - Initialization

-(instancetype)init {}

// Subclass Specific Methods

#pragma mark - UIViewController

-(void)viewDidLoad {}
-(void)viewWillAppear:(BOOL)animated {}
-(void)didReceiveMemoryWarning {}

#pragma mark - Public

-(void)publicMethod {}

#pragma mark - Private

-(void)privateMethod {}

// Protocols

#pragma mark - <FirstProtocol>
#pragma mark - <SecondProtocol>

#pragma mark - Accessors

-(void)setCustomProperty:(id)customProperty {}
-(id)customProperty {}

#pragma mark - Dealloc

- (void)dealloc {}
```


## Spacing

* Indent using 4 spaces. Make sure this preference is set in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line, with any subsequent conditionals placed on a new line. Never omit brackets in any kind of conditional/loop statement.

**Preferred:**
```objc
if (user.isHappy) {
  //Do something
} 
else {
  //Do something else
}
```

**Not Preferred:**
```objc
if (user.isHappy)
{
    //Do something
} else {
    //Do something else
}

if (venice.isAwesome)
    [self goToTheBeach];
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* Prefer using auto-synthesis. But if necessary, `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.
* Colon-aligning method invocation should often be avoided.  There are cases where a method signature may have >= 3 colons and colon-aligning makes the code more readable. Please do **NOT** however colon align methods containing blocks because Xcode's indenting makes it illegible.

**Preferred:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Not Preferred:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. *Exception: This does not apply to those comments used to generate documentation.*

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good. They should typically be suffixed by the most important suffixes of the class to which they belong. *Exception: In the interest of brevity/sanity, any view controller properties should omit the term `View`.*

**Preferred:**

```objc
UIButton *settingsButton;
WSettingsViewController *settingsController;
```

**Not Preferred:**

```objc
UIButton *setBut;
```

The prefix `W` should always be used for class names and constants, however may be omitted for Core Data entity names.

Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

Double values should always have a decimal and should end in `0`. Float values should always have a decimal and end in `0f`.

**Preferred:**

```objc
static NSTimeInterval const WInboxViewControllerAnimationTransitionDuration = 0.30;
static CGFloat const WInboxViewControllerDefaultTabBarOffset = 60.0f;
```

**Not Preferred:**

```objc
static NSTimeInterval const fadetime = 1.7;
static CGFloat const inbox_height = 100;
```

Properties should be camel-case with the leading word being lowercase. Use auto-synthesis for properties rather than manual @synthesize statements unless you have good reason.

**Preferred:**

```objc
@property (nonatomic, strong) NSString *descriptiveVariableName;
```

**Not Preferred:**

```objc
id varnm;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using the autosynthesized, underscore-prefixed name. The use of accessors is reserved for instances in which either setting or getting the variable produces any additional effects.

Local variables should not contain underscores, nor should they obscure previous definitions in the case of nested scopes.

## Methods

In method signatures, there should **NOT** be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style). Always include a keyword and be descriptive with the word before the argument which describes the argument.

The usage of the word "and" is reserved. It should not be used for multiple parameters as illustrated in the `initWithWidth:height:` example below.

**Preferred:**
```objc
-(void)setExampleText:(NSString *)text image:(UIImage *)image;
-(void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
-(id)viewWithTag:(NSInteger)tag;
-(instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**

```objc
-(void)setT:(NSString*)text i:(UIImage *)image;
-(void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
-(id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables at all times. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent.

The only time a property should be publicly declared in the interface file is if another class absolutely must have access to it, and should only be `readwrite` if absolutely necessary, as well.

**Preferred:**

```objc
@interface RWTTutorial : NSObject

@property (nonatomic, strong) NSString *tutorialName;

@end
```

**Not Preferred:**

```objc
@interface RWTTutorial : NSObject {
  NSString *tutorialName;
}
```


## Property Attributes

Property attributes should be in the following order:
* Atomicity (nonatomic/atomic, but probably nonatomic)
* Access Permission (readonly/readwrite)
* Memory Management (strong/weak/assign)

The access permission attribute should always be included in the interface file of a class. The only time it should appear in the private interface of the implementation file is to change the publicly declared permission.

**Preferred:**

```objc
@property (nonatomic, readonly, strong) NSString *tutorialName;
@property (nonatomic, assign) NSUInteger pageNumber;
```

**Not Preferred:**

```objc
@property (weak, nonatomic) UIView *containerView;
@property (nonatomic) NSString* tutorialName;
```

Properties with mutable counterparts (e.g. NSString) should prefer `copy` instead of `strong`. 
Why? Even if you declared a property as `NSString` somebody might pass in an instance of an `NSMutableString` and then change it without you noticing that. *Exception: Pretty much all of our code. Don't set mutable counterparts when the property calls for the nonmutable version.*

Always use the `weak` attribute type when possible, particularly when dealing with UI elements. The only time a child view controller property should be `strong` is if it will be removed from its parent controller and added again later, with the expectation that it maintains its state. The same goes for `UIView` subclasses.

## Dot-Notation Syntax

Dot syntax is purely a convenient wrapper around accessor method calls. When you use dot syntax, the property is still accessed or changed using getter and setter methods.  Read more [here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)

Within a classes implementation file, dot-notation should only be used for accessing and mutating properties when the automatically generated accessors have been overridden. In other contexts, dot syntax is preferred whenever accessing or mutating properties, unless doing so would interrupt a series of brackets. In some instances, dot syntax is appropriate when calling a method of an instance variable (e.g. `arrayOfThings.count`). This is purely at the discretion of Sr. Manuel Gomez, and he has yielded his power of objectivec to Mr. Paul Fleiner.

**Preferred:**
```objc
NSInteger arrayCount = _array.count;
view.backgroundColor = [UIColor orangeColor];
WAppDelegate *appDelegate = (WAppDelegate *)[[UIApplication sharedApplication] delegate];
```

**Not Preferred:**
```objc
NSInteger arrayCount = [self.array count];
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
[UIApplication sharedApplication].delegate;
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash. Collection literals should only contain spaces after commas that separate elements.

**Preferred:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *projectManagers = @{@"iPhone":@"Kate", @"iPad":@"Kamal", @"Mobile Web":@"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;

NSMutableDictionary *productManagers = [NSMutableDictionary dictionaryWithDictionary:productManagers];

NSString *mendy = [WMendyManager isGoingOutOfTown] ? @"Mendy" : nil;
if (mendy) {	
    properties[@"Mixpanel"] = mendy;
}
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro. If a constant must be declared in a class's interface file, use `extern` and define the constant in the implementation file.

Use the processor-agnostic data type typedefs whenever appropriate (i.e. almost always). In addition to ensuring equivalent performance on 32- and 64-bit architectures, they indicate what the constant will be used for:
* `NSInteger` instead of `int`
* `NSUInteger` instead of `unsigned_int`
* `CGFloat` instead of `float`
* `NSTimeInterval` or `CLLocationDistance` instead of `double`
* etc.

**Note, this applies to [Variables](#variables) as well.**

When implementing mocks, any UI constant values (dimensions, color values, animation durations) should not be hardcoded, but declared as constants in the implementation file. *Exception: In the case of different RGB values, those may be hardcoded until we devise some sort of tuple scheme.*

**Preferred:**

```objc
static NSString *const WWhisperWebsiteName = @"whisper.sh";

static CGFloat const WWhisperSearchBarStyle7BDefaultHeight = 50.0f;

extern NSString *const WUserSettingsUpdatedNotificationName;
```

**Not Preferred:**

```objc
#define CompanyName @"WhisperText, Inc."

#define thumbnailHeight 2
```

## Enumerated Types

Enumerations are highly recommended, as they increase the readability of the code substantially, while providing an efficient way to cycle through values.

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`.

Enumerations should be of type `NSUInteger` unless a negative value is necessary. The name of the enumeration should be prefixed with `W`, followed by a thorough description of the values being enumerated. The values themselves should be be the name of the enumeration, prefixed by `k`, and suffixed with `_(value specific descriptor)`. **Exception: If the enumeration will be used in a `for()` loop, it may behoove you to include a final value for the count. This should be simply the enumeration name suffixed with `Count`.**

**For Example:**

```objc
typedef NS_ENUM(NSUInteger, WViewControllerTopLayoutStyle) {
    kWViewControllerTopLayoutStyle_Purple,
    kWViewControllerTopLayoutStyle_White,
    kWViewControllerTopLayoutStyle_Custom,
    kWViewControllerTopLayoutStyle_None,
    WViewControllerTopLayoutStyleCount
};
```

**Not Preferred:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```


## Case Statements

Braces are not required for case statements, unless enforced by the complier. When a case contains more than one line, braces should be added. Otherwise, the like should be written on the same line as the `case` statement. The only time it is appropriate to omit the `break` statement is when the code will always return a value. **Note: Only include the `default` case when all enumeration values have not been handled. This allows changes to the enumeration to flag  

```objc
+(NSString *)nameForSource:(WWhisperImageSource)source
{
    switch (source) {
        case kWWhisperImageSource_Unknown: return @"unknown";
        case kWWhisperImageSource_Camera: return @"camera";
        case kWWhisperImageSource_Photos: return @"photos";
        case kWWhisperImageSource_Search: return @"search";
        case kWWhisperImageSource_Suggest: return @"suggest";
        case kWWhisperImageSource_SearchEngine: return @"search_engine";
        case kWWhisperImageSource_SocialNetwork: return @"social_network";
        default: return nil;
    }
}
```

A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value.  A fall-through should be commented for coding clarity. **Note: Honestly, fall-throughs may reduce the number of lines of code, but they're really difficult to catch when something goes wrong. There are a number of them currently in the code, but let's not add any more, and eventually get to removing the ones that are there.**

## Booleans

Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code.  Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**Preferred:**

```objc
if (someObject) {}
if (!anotherObject.boolValue) {}
```

**Not Preferred:**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property should omit the “is” prefix, but specify the conventional name for the get accessor (unless the property is private and there is no custom getter implemented), for example:

```objc
@property (nonatomic, readonly, assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**Preferred:**
```objc
if (!error) {
  return success;
}
```

**Not Preferred:**
```objc
if (!error)
  return success;
```

or

```objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something, and parentheses are added for improved readability.  If the variable being compared is a boolean type, then no parentheses are needed.

**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**Not Preferred:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

Note that if you want to ensure something exists and use another value if it doesn't, you can simplify this:
```objc
NSString *message = responseDictionary[@"error"] ? responseDictionary[@"error"] : defaultError;
```
into this:
```objc
NSString *message = responseDictionary[@"error"] ?: defaultError;
```

## Init Methods

A return type of 'instancetype' should also be used instead of 'id'.

```objc
-(instancetype)init 
{
  if (self = [super init]) {
    // ...
  }
  return self;
}
```

See [Class Constructor Methods](#class-constructor-methods) for link to article on instancetype.

## Class Constructor Methods

Where class constructor methods are used, these should always return type of 'instancetype' and never 'id'. This ensures the compiler correctly infers the result type. 

```objc
@interface WFeed

+(instancetype)feedWithFeedSource:(WFeedSource *)feedSource;

@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat xCoordinate = CGRectGetMinX(frame);
CGFloat yCoordinate = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0f, 0.0f, width, height);
```

**Not Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.  That is, don't nest `if` statements.  Multiple return statements are OK.

**Preferred:**

```objc
-(void)someMethod 
{
  if (!someOther.boolValue) {
	return;
  }

  //Do something important
}
```

**Not Preferred:**

```objc
-(void)someMethod 
{
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).


## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+(instancetype)sharedInstance
{
  static id sharedInstance = nil;
  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [self new];
  });

  return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## Line Breaks

Line breaks are an important topic since this style guide is focused for print and online readability.

For example:
```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Other Objective-C Style Guides

If ours doesn't fit your tastes, have a look at some other style guides:

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
