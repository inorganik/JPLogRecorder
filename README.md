# JPLogRecorder
A replacement for NSLog() that records your logs so you can retrieve them at runtime. You can optionally [make it persistent](#persistence), so that log statements from _before a crash_ can be reported.

### Usage

Simply replace all instances of `NSLog` with `JPLog`.

### Magic

JPLogRecorder does magical things like adding the file and line number where your log statement is. For example, given:
```objc
JPLog(@"retrieved feed. number of items: %@", items.count);
```

Your Xcode console will show:
```
2016-02-15 13:11:42.121 myApp [16664:1168045] <FeedViewController.m:46> retrieved feed. number of items: 26
```

The log, retrieved with `[JPLogRecorder logArrayAsString]` will show:
```
[2/15/16 1:11:42 PM] <FeedViewController.m:46> retrieved feed. number of items: 26
```

### Get the log

`[JPLogRecorder logArray]` returns an array of log statements in reverse chronological order (newest first).
`[JPLogRecorder logArrayAsString]` returns a string of the entire log. 

### Persistently recorded logs <a name="persistence"></a>
Optionally, you can make the retained log statements persistent. This means that even if your app crashes or is force quit by the user, log statements _before the crash_ are retained. All you need is **this one weird trick** in AppDelegate.m:
```objc
- (void)applicationWillTerminate:(UIApplication *)application
{
  [JPLogRecorder saveLogArray];
}
```
If you have background-tasks enabled in your app, also save the log here:
```objc
- (void)applicationDidEnterBackground:(UIApplication *)application
{
  [JPLogRecorder saveLogArray];
}
```
When the app re-launches, the log array will initialize with the saved log array.

### The recorded log is capped
The number of retained log statements is capped at 50 -- this prevents the recorded log eating up potentially endless memory. You can change this by editing the static property in JPLogRecorder.m, or by calling [JPLogRecorder setMaxLogsRetained:100];

### Enable Production logging
Say you're about to drop a release to TestFlight, but you want logging kept on so your users can use a function in your app that sends you the log when an error occurs. No problem, just call `[JPLogRecorder setShouldLogInProduction:YES]`. The default behavior for prod is that logging is disabled.
