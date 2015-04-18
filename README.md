# GeneralMacro
usefully macro in iOS

////////////////////////////////////////////////////////////

#ifdef DEBUG
#define DLog(...) NSLog(__VA_ARGS__)
#define DFLog(fmt, ...) NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__)
#else
#define DLog(...)
#define DFLog(...)
#endif

// Debug log
#define NEED_OUTPUT_LOG 1
#if NEED_OUTPUT_LOG

#define DLogRect(rect) \
DFLog(@"%s x=%f, y=%f, w=%f, h=%f", #rect, rect.origin.x, rect.origin.y, \
rect.size.width, rect.size.height)

#define DLogPoint(pt) \
DFLog(@"%s x=%f, y=%f", #pt, pt.x, pt.y)

#define DLogSize(size) \
DFLog(@"%s w=%f, h=%f", #size, size.width, size.height)

#define DLogColor(_COLOR) \
DFLog(@"%s h=%f, s=%f, v=%f", #_COLOR, _COLOR.hue, _COLOR.saturation, _COLOR.value)

#define DLogSuperViews(_VIEW) \
{ for (UIView* view = _VIEW; view; view = view.superview) { DFLog(@"%@", view); } }

#define DLogSubViews(_VIEW) \
{ for (UIView* view in [_VIEW subviews]) { DFLog(@"%@", view); } }

#else
#define DLogRect(rect)          ((void)0)
#define DLogPoint(pt)           ((void)0)
#define DLogSize(size)          ((void)0)
#define DLogColor(_COLOR)       ((void)0)
#define DLogSuperViews(_VIEW)   ((void)0)
#define DLogSubViews(_VIEW)     ((void)0)
#endif

// Singleton
#define SINGLETON_GCD(classname)                        \
\
+ (classname *)shared##classname {                      \
\
static dispatch_once_t pred;                        \
__strong static classname * shared##classname = nil;\
dispatch_once( &pred, ^{                            \
shared##classname = [[self alloc] init]; });    \
return shared##classname;                           \
}

// GCD
#define dispatch_main_sync_safe(block)\
if ([NSThread isMainThread]) {\
block();\
} else {\
dispatch_sync(dispatch_get_main_queue(), block);\
}

#define dispatch_main_async_safe(block)\
if ([NSThread isMainThread]) {\
block();\
} else {\
dispatch_async(dispatch_get_main_queue(), block);\
}

// PATH
#define PATH_OF_APP_HOME    NSHomeDirectory()
#define PATH_OF_TEMP        NSTemporaryDirectory()
#define PATH_OF_DOCUMENT    [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0]
#define PATH_OF_CACHE       [NSSearchPathForDirectoriesInDomains (NSCachesDirectory, NSUserDomainMask, YES) objectAtIndex:0]

// COLOR AND FRAME
#define RGBCOLOR(r,g,b)             [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:1]
#define RGBACOLOR(r,g,b,a)          [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:(a)]

#define SCREEN_WIDTH            [UIScreen mainScreen].bounds.size.width
#define SCREEN_HEIGHT           [UIScreen mainScreen].bounds.size.height

// NSUserDefault
#define defaults()                          [NSUserDefaults standardUserDefaults]
#define defaults_init(dictionary)			[defaults() registerDefaults:dictionary]
#define defaults_save()                     [defaults() synchronize]
#define defaults_object(key)                [defaults() objectForKey:key]
#define defaults_bool(key)                  [defaults_object(key) boolValue]
#define defaults_set_bool(key, object)      [defaults() setBool:object forKey:key]; defaults_save();
#define defaults_set_object(key, object)    [defaults() setObject:object forKey:key]; defaults_save(); defaults_post_notification(key)
#define defaults_remove(key)				[defaults() removeObjectForKey:key];defaults_save();
#define defaults_object_from_notification(n) [n.userInfo objectForKey:@"value"]
#define defaults_observe_object(key, block) [[NSNotificationCenter defaultCenter] addObserverForName:key object:nil queue:[NSOperationQueue mainQueue] usingBlock:^(NSNotification *n){ block( defaults_object_from_notification(n) ); }]
#define defaults_post_notification(defaults_key) [[NSNotificationCenter defaultCenter] postNotificationName:defaults_key object:nil userInfo:@{ @"value" : defaults_object(defaults_key) }];
#define NOTIFICATION_CENTER         [NSNotificationCenter defaultCenter]


// Reference
#define kAppDelegate (MSAppDelegate *)[UIApplication sharedApplication].delegate
#define IS_INSTANCE_OF(instance, className) ((instance) && [instance isKindOfClass:[className class]])
#define WEAKSELF typeof(self) __weak weakSelf = self;

// System version
#define SYSTEM_VERSION_EQUAL_TO(v)                  ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] == NSOrderedSame)
#define SYSTEM_VERSION_GREATER_THAN(v)              ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] == NSOrderedDescending)
#define SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(v)  ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] != NSOrderedAscending)
#define SYSTEM_VERSION_LESS_THAN(v)                 ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] == NSOrderedAscending)
#define SYSTEM_VERSION_LESS_THAN_OR_EQUAL_TO(v)     ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] != NSOrderedDescending)





