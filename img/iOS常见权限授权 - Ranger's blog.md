# iOS常见权限授权 - Ranger's blog
权限状态枚举, API 比较统一, 都是相关框架前缀 + 固定描述拼接, 大致如下:

-   `AuthorizationStatusNotDetermined`代表还未向用户进行过询问授权, 一般为应用新装或第一次请求对应权限. 此时可以主动向用户请求授权.
-   `AuthorizationStatusRestricted`代表对应权限被拒绝, 并且用户无权改变权限状态, 例如文档中举例家长控制这种情况. 因此当做未授权的情况处理处理.
-   `AuthorizationStatusDenied`代表被用户拒绝. 此时可以提醒用户该权限无法访问, 需要去设置中打开.
-   `AuthorizationStatusAuthorized`代表已经获取对应权限. 此时可以访问相关数据和使用相关功能.

## 相册权限[](#相册权限)

| \`\`\`
 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 

````

 | ```
// 导入框架
#import <Photos/Photos.h>

// 当前授权状态 
// status就对应上面提到的枚举以及对应含义
PHAuthorizationStatus status = \[PHPhotoLibrary authorizationStatus\];

// 主动请求授权
if (status == PHAuthorizationStatusNotDetermined) {
        \[PHPhotoLibrary requestAuthorization:^(PHAuthorizationStatus status) {
            if (status == PHAuthorizationStatusAuthorized) {
                // 已授权 可以访问相册等操作
            }
            else {
                // 未授权 提示用户
            }
        }\];
    }
````

 \|

## 相机 麦克风权限[](#相机-麦克风权限)

| \`\`\`
 1 2 3 4 5 6 7 8 9 10 11 12 13 14 

````

 | ```
#import <AVFoundation/AVFoundation.h>

// AVMediaTypeVideo 相机权限
// AVMediaTypeAudio 麦克风权限

// 当前授权状态
AVAuthorizationStatus status = \[AVCaptureDevice authorizationStatusForMediaType:AVMediaTypeVideo\];

// 主动请求授权
if (status == AVAuthorizationStatusNotDetermined) {
        \[AVCaptureDevice requestAccessForMediaType:AVMediaTypeVideo completionHandler:^(BOOL granted) {
            // granted == YES 已授权
        }\];
    }
````

 \|

## 推送[](#推送)

| \`\`\`
 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 

````

 | ```
#import <UserNotifications/UserNotifications.h>

// 获取当前授权状态
\[\[UNUserNotificationCenter currentNotificationCenter\] getNotificationSettingsWithCompletionHandler:^(UNNotificationSettings \* \_Nonnull settings) {
        // 当前授权状态
        UNAuthorizationStatus unStatus = settings.authorizationStatus;
        
        if (unStatus == UNAuthorizationStatusAuthorized) {
            // 已授权
        }
        else {
        // 请求授权
            \[\[UNUserNotificationCenter currentNotificationCenter\] requestAuthorizationWithOptions:UNAuthorizationOptionAlert | UNAuthorizationOptionSound |UNAuthorizationOptionBadge completionHandler:^(BOOL granted, NSError \* \_Nullable error) {
                // granted == YES 已授权
            }\];
        }
    }\];
````

 \|

## 日历[](#日历)

| \`\`\`
 1 2 3 4 5 6 7 8 9 10 11 12 13 

````

 | ```
#import <EventKit/EventKit.h>

// 当前授权状态 
//EKEntityTypeEvent日历 EKEntityTypeReminder提醒事项
EKAuthorizationStatus status = \[EKEventStore authorizationStatusForEntityType:EKEntityTypeEvent\];
    
// 未询问
    if (status == EKAuthorizationStatusNotDetermined) {
    // 请求授权
        \[\[\[EKEventStore alloc\] init\] requestAccessToEntityType:EKEntityTypeEvent completion:^(BOOL granted, NSError \* \_Nullable error) {
            // granted == YES 已授权
        }\];
    }
````

 \|

## 联系人[](#联系人)

| \`\`\`
 1 2 3 4 5 6 7 8 9 10 11 

````

 | ```
#import <Contacts/Contacts.h>

// 当前授权状态
CNAuthorizationStatus status = \[CNContactStore authorizationStatusForEntityType:CNEntityTypeContacts\];
// 未询问
    if (status == CNAuthorizationStatusNotDetermined) {
    // 请求授权
        \[\[\[CNContactStore alloc\] init\] requestAccessForEntityType:CNEntityTypeContacts completionHandler:^(BOOL granted, NSError \* \_Nullable error) {
            // granted == YES 已授权
        }\];
    }
````

 \|

## 定位[](#定位)

| \`\`\`
 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 

````

 | ```
#import <CoreLocation/CoreLocation.h>

// 需要引用 CLLocationManager
@property (nonatomic, strong) CLLocationManager \*locationManager;

// 获取授权状态
CLAuthorizationStatus status = \[CLLocationManager authorizationStatus\];
    // 未授权
    if (status == kCLAuthorizationStatusNotDetermined) {
    // 请求一直定位
        \[self.locationManager requestAlwaysAuthorization\];
    // 请求使用时定位
        \[self.locationManager requestWhenInUseAuthorization\];
    }

// CLLocationManagerDelegate
// 定位权限变化的回调方法
- (void)locationManager:(CLLocationManager \*)manager didChangeAuthorizationStatus:(CLAuthorizationStatus)status {
    
}
````

 \|

## ZRAutorizationManager[](#zrautorizationmanager)

封装了以上所有权限的状态和授权, 统一 API, 一行代码获取状态和请求授权. 并且系统 API 的授权请求结果回调线程一般不是主线程, 使用**ZRAutorizationManager**, 可以在系统 API 线程或在**主线程回调授权结果**.

| \`\`\`
 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 

````

 | ```
// 一个枚举统一处理授权状态
typedef NS\_ENUM(NSUInteger, ZRAutorizationStatus) {
    ZRAutorizationStatusNotDetermined,// 用户还未选择是否允许授权 可以代表第一次询问
    ZRAutorizationStatusDenied,// 拒绝授权 包括 Restricted & Denied
    ZRAutorizationStatusAuthorized,// 已授权
    
    ZRAutorizationStatusUnknow,// 未知状态
};

// 获取授权状态
+ (ZRAutorizationStatus)autorizationStatusForType:(ZRAutorizationType)type;

// 请求授权后结果的回调是系统API回调的线程,一般不是主线程
+ (void)requestAuthorization:(ZRAutorizationType)type grantedBlock:(void (^)(BOOL granted))grantedBlock;

// 请求授权后结果的回调是主线程
+ (void)requestAuthorization:(ZRAutorizationType)type grantedBlock:(void (^)(BOOL granted))grantedBlock;
````

 \|

[下载查看 github](https://github.com/RangerZz/ZRAutorizationManager)

文章作者 Ranger

上次更新 2018-12-19

许可协议 [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/) 
 [https://rangerzz.github.io/post/4/](https://rangerzz.github.io/post/4/)
