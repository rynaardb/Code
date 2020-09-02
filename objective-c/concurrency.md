# Concurrency

## Grand Central Dispatch \(GCD\)

With GCD you add blocks of code to queues, and GCD manages the thread pool behind the scenes. In other words, GCD used threads under the hood.

### Dispatch once

```objectivec
dispatch_once(&onceToken, ^{
    ...
});
```

### Dispatch with delay

```objectivec
double delayInSeconds = 2.0;

dispatch_time_t popTime = dispatch_time(DISPATCH_TIME_NOW, (int64_t) (delayInSeconds * NSEC_PER_SEC));

dispatch_after(popTime, dispatch_get_main_queue(), ^(void){
    ...
});
```

## Queues

Queues can be serial \(default\) or concurrent.

### Execute code on the main queue

```objectivec
dispatch_async(dispatch_get_main_queue(), ^{
    ...
});
```

### Getting the Global Concurrent Dispatch Queues

Concurrent dispatch queues are useful when you have multiple tasks that can run in parallel.

```objectivec
// Because they are global you don't need to create one, simply request one
dispatch_queue_t aQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
```

Priority can be `DISPATCH_QUEUE_PRIORITY_HIGH` or `DISPATCH_QUEUE_PRIORITY_LOW`

### Creating Serial Dispatch Queues

Serial queues are useful when you want your tasks to execute in a specific order. A serial queue executes only one task at a time and always pulls tasks from the head of the queue.

```objectivec
dispatch_queue_t myCustomQueue;
myCustomQueue = dispatch_queue_create("com.example.MyCustomQueue", NULL);

// Dispatch async task
dispatch_async(myCustomQueue, ^{
    ...
});

// Dispatch sync task
dispatch_sync(myCustomQueue, ^{
    ...
});
```

### Queue Groups

Useful for chaining asynchronous blocks together to perform a given task.

```objectivec
dispatch_group_t group = dispatch_group_create();

dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
dispatch_group_async(group, queue, ^(){
    ...
    dispatch_group_async(group, dispatch_get_main_queue(), ^(){
        ...
    });
});

dispatch_group_async(group, queue, ^(){
    ...
    dispatch_group_async(group, dispatch_get_main_queue(), ^(){
        ...
    });
});

// This block will run once everything above is done:
dispatch_group_notify(group, dispatch_get_main_queue(), ^(){
    ...
});
```

### Isolation Queues

```objectivec
self.isolationQueue = dispatch_queue_create([label UTF8String], DISPATCH_QUEUE_CONCURRENT);

dispatch_async(self.isolationQueue, ^(){
    ...
    // only one thread will access this block of code
});
```

## Operation Queues

### Creating a serial queue on the main thread

```objectivec
YourOperation *operation = [YourOperation new];
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
[queue  addOperation:operation];
```

### Adding a block to an operation queue

```objectivec
[[NSOperationQueue mainQueue] addOperationWithBlock:^{
    ...
}];
```

## Timers

### Creating a timer

```objectivec
// Creates a timer that fires every 5 seconds and allow for a leeway of 1/10 of a second
dispatch_source_t source = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 
  0, 0, DISPATCH_TARGET_QUEUE_DEFAULT);
dispatch_source_set_event_handler(source, ^(){
    NSLog(@"Time flies.");
});

dispatch_source_set_timer(source, DISPATCH_TIME_NOW, 5ull * NSEC_PER_SEC, 
  100ull * NSEC_PER_MSEC);
self.source = source;
dispatch_resume(self.source);
```

## Watching Files and Directories

### Watching a directory

```objectivec
NSURL *directoryURL; // assume this is set to a directory

int const fd = open([[directoryURL path] fileSystemRepresentation], O_EVTONLY);

if (fd < 0) {
    char buffer[80];
    strerror_r(errno, buffer, sizeof(buffer));
    NSLog(@"Unable to open \"%@\": %s (%d)", [directoryURL path], buffer, errno);
    return;
}

dispatch_source_t source = dispatch_source_create(DISPATCH_SOURCE_TYPE_VNODE, fd, 
  DISPATCH_VNODE_WRITE | DISPATCH_VNODE_DELETE, DISPATCH_TARGET_QUEUE_DEFAULT);

dispatch_source_set_event_handler(source, ^(){
    unsigned long const data = dispatch_source_get_data(source);
    if (data & DISPATCH_VNODE_WRITE) {
        NSLog(@"The directory changed.");
    }
    if (data & DISPATCH_VNODE_DELETE) {
        NSLog(@"The directory has been deleted.");
    }
});

dispatch_source_set_cancel_handler(source, ^(){
    close(fd);
});

self.source = source;

dispatch_resume(self.source);
```

