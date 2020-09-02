# Blocks

## Block literal

```objectivec
^{
    NSLog(@"This is a block");
}
```

## As a local variable

```objectivec
returnType (^blockName)(parameterTypes) = ^returnType(parameters) {...};

// Declare
void (^simpleBlock)(void);

// Assign
simpleBlock = ^{
    ...
};

// Invoke
simpleBlock();
```

## As a property

You should specify copy as the property attribute because a block needs to be copied to keep track of its captured state outside of the original scope. This isn’t something you need to worry about when using Automatic Reference Counting, as it will happen automatically, but it’s best practice for the property attribute to show the resultant behavior.

```objectivec
@property (nonatomic, copy, nullability) returnType (^blockName)(parameterTypes);

// Declare
@property (nonatomic, copy) void (^blockProperty)(void);

// Assign
self.blockProperty = ^{
    ...
};
```

## As a method parameter and argument

```objectivec
- (void)someMethodThatTakesABlock:(returnType (^nullability)(parameterTypes))blockName;

// Declare
- (void)beginTaskWithCallbackBlock:(void (^)(void))callbackBlock;

// Invoke
[task beginTaskWithCallbackBlock:^{
    ...
}];
```

## As a typedef

```objectivec
typedef returnType (^TypeName)(parameterTypes);
TypeName blockName = ^returnType(parameters) {...};

// Declare
typedef void (^APISuccessHandler)(id responseObject);
typedef void (^APIFailureHandler)(NSError *error);
```

## \_\_block

Use \_\_block to change the value of a captured variable from within a block.

```objectivec
__block int anInteger = 42;

void (^testBlock)(void) = ^{
    ...
    anInteger = 100;
};
```

## Avoid Strong Reference Cycles when Capturing self

Blocks maintain strong references to any captured objects, including self, which means that it’s easy to end up with a strong reference cycle.

To avoid this problem, it’s best practice to capture a weak reference to `self`, like this:

```objectivec
- (void)configureBlock {
    MyClass * __weak weakSelf = self;

    self.block = ^{
        [weakSelf doSomething];   // capture the weak reference
                                  // to avoid the reference cycle
    }
}
```

