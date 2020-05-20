light_ble(ios)库使用说明
====================

### 集成方式
1. 在Linked Frameworks and Libraries中添加BLECommAPI.framework
2. 导入头文件 `#import <BLECommAPI/BLECommAPI.h>`
3. 实现IBeaconListener的协议 `@interface ViewController : UIViewController <IBeaconListener>`
4. 绑定listener `IBeaconManager.sharedInstance.listener = self;`
5. 开始编写业务逻辑

### 方法说明
#### IBeaconManager
* 开始扫描

  `- (void)startScan;`

  ```
  [IBeaconManager.sharedInstance startScan];
  ```

* 指定扫描
  
  `- (void)startScanWith:(NSString *)name;`

  ```
  [IBeaconManager.sharedInstance startScanWith:name];
  ```

* 停止扫描
  
  `- (void)stopScan;`

  ```
  [IBeaconManager.sharedInstance stopScan];
  ```

* 连接指定设备
  
  `- (void)connect:(Beacon*) beacon token: (NSString*) token timeout: (int) timeout;`

  ```
  NSString *tokenHex = @"010203040506";
  [IBeaconManager.sharedInstance connect:self.beacon token:tokenHex timeout:10];
  ```

* 断开连接
  
  `- (void)disconnect;`

  ```
  [IBeaconManager.sharedInstance disconnect];
  ```

* 向设备写数据
  
  `- (void)write:(NSData *)data;`

  ```
  NSString *msg = @"hello";
  [IBeaconManager.sharedInstance write:[msg dataUsingEncoding:NSUTF8StringEncoding]];
  ```

#### IBeaconListener
* 扫描开始回调
  ```
    /**
    * start scan
    * @param result: 扫描结果
    * @param error: 错误值
    */
    @required
    -(void) onScanStart:(bool) result error:(BLEError) error;
  ```

* 扫描成功回调
  ```
    /**
    * scan success
    * @param beacon: 扫描到的设备
    */
    @required
    -(void) onScanSuccess:(Beacon*) beacon;
  ```

* 操作执行成功回调
  ```
    /**
    * ble comm success
    * @param Op :
    *             OpAdv : start advertisement
    *             OpConnReq : connection request
    *             OpConnResp : connection resp
    *             OpDisconnReq : disconnect
    *             OpTran : write data
    */
  @required
  -(void) onOperationStartSuccess:(int) op;
  ```

* 操作执行失败回调
  ```
    /**
    * ble comm failed
    * @param Op :
    *             OpAdv : start advertisement
    *             OpConnReq : connection request
    *             OpConnResp : connection resp
    *             OpDisconnReq : disconnect
    *             OpTran : write data
    * @param errorCode :  BLE_ERROR_INVALID_PARAMETER or BLE_ERROR_INVALID_OPERATION
    */
  ```

* 开始连接回调
  ```
    /**
    * connect start
    * @param beacon: target beacon
    * @param error: error
    */
    @required
    -(void) onConnectStart:(Beacon*) beacon error:(BLEError) error;
  ```

* 连接成功回调
  ```
    /**
    * connect success
    * @param beacon: target beacon
    */
    @required
    -(void) onConnectSuccess:(Beacon*) beacon;
  ```

* 断开连接回调
  ```
    /**
    * disconnect
    * @param beacon: target beacon
    * @param error: error
    */
    @required
    -(void) onDisconnect:(Beacon*) beacon error:(BLEError) error;
  ```

* 本机蓝牙状态更新回调
  ```
    /**
    * ble status update
    * @param beacon: target beacon
    * @param status: new device status
    */
    @required
    -(void) onBLEStatusUpdate:(BLEStatus) status;
  ```
* 数据接收回调
  ```
    /**
    * received data
    * @param data: received data
    */
    @required
    -(void) onReceive:(NSData*) data;
  ```

* 发送成功回调
  ```
    /**
    * send success with index
    * @param data: received data
    * @param index: index of sended data
    */
    @required
    -(void) onSendSuccess:(int) index;
  ```

