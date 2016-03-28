# JPLogRecorder
For iOS development - a replacement for NSLog() that persistently records your logs so that they remain intact post-crash, continuous across sessions, and retrievable.

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
`[JPLogRecorder logArrayAsString]` returns a string of the entire log with `\n\n` separating log statements. 


### The recorded log is capped
The number of retained log statements is capped at 50 -- this prevents the recorded log eating up potentially endless memory. You can change this by editing the static property in JPLogRecorder.m, or by calling [JPLogRecorder setMaxLogsRetained:100];

### Enable Production logging
Say you're about to drop a release to TestFlight, but you want logging kept on so your users can use a function in your app that sends you the log when an error occurs. No problem, just call `[JPLogRecorder setShouldLogInProduction:YES]`. The default behavior for prod is that logging is disabled.

### Javascript? 
If you are looking for a javascript equivalent to this logging class, check out [debugout](https://github.com/inorganik/debugout.js).