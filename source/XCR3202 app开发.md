XCR3202 app开发
======================

## IBeaconDemo Light_ble oc源码工程(ios)  
[zozo825117/ibeacon-tunnel_source_code](https://github.com/zozo825117/ibeacon-tunnel_source_code)

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



## 可与模拟蓝牙设备实现基本的收发通讯(android)  
[zozo825117/ble_comm_api](https://github.com/zozo825117/ble_comm_api)  
    `此工程是在github上发布的只带blecomm-api.jar的版本`

### 描述
`blecomm-api.jar`可实现与模拟蓝牙设备实现基本的收发通讯的基本方法,本样例就是一种基于api的实现方法.

![](http://www.zozo825117.cn:10080/fake_ble/ble_comm_api/app_1.png)


### 要求
* android 最低支持的版本:   
  Android 5.1 (LOLLIPOP_MR1  API22)
* ble peripheral 支持
  目前同时用到ble的Central角色和peripheral角色,所以在android版本满足的前提下,还需要检测手机是否支持peripheral角色,参考如下:
    MainActivity.java
    ```
     private void ensureBleFeaturesAvailable() {
        BluetoothManager mBluetoothManager = (BluetoothManager) getSystemService(Context.BLUETOOTH_SERVICE);
        BluetoothAdapter mBluetoothAdapter = mBluetoothManager.getAdapter();
        if (mBluetoothAdapter == null) {
            Toast.makeText(this, "Bluetooth not supported", Toast.LENGTH_LONG).show();
            Log.e(TAG, "Bluetooth not supported");
            finish();
        }else{
            Log.i(TAG, "Bluetooth supported");
        }
        BluetoothLeAdvertiser mBluetoothLeAdvertiser = mBluetoothAdapter.getBluetoothLeAdvertiser();
        if(mBluetoothLeAdvertiser == null){
            Toast.makeText(this, "the device not support peripheral", Toast.LENGTH_SHORT).show();
            Log.e(TAG, "the device not support peripheral");
            finish();
        } else{
            Log.i(TAG, "the device support peripheral");
        }
    }
    ```

### 特性
- [x] bleStartAdvertisementSimulate
- [x] bleStopAdvertisement
- [x] bleStartConnect
- [x] bleDisConnect wear
- [x] bleSendMessage
- [x] onReceive
- [x] onStartSuccess
- [x] onStartFailure

### TODO
- [ ] onSendSta

### Project Organization
![](http://www.zozo825117.cn:10080/fake_ble/ble_comm_api/app_structure.png)

### Useage
1. 在bulid.gradle中加入`blecomm-api.jar`的依赖
2. 需要在manifest中加入蓝牙相关权限
3. 需要在manifest中注册com.wearlink.blecomm.BleService服务
4. 在activity用如下方法开启服务:  
  `((BleService.MyBinder)service).getService()`
5. 开启服务同时,用如下方法设置以实现的接口`onServiceProgressListener`:
  `bleService.setOnBleCommProgressListener(onServiceProgressListener);`
6. 在此接口的`onServiceOpen`方法中通过如下方法获取主要方法接口:
  `bleService.getBleCommMethod();`

### 主要方法
由BleCommMethod接口提供:
```
/**
 * ble communication method
 */

public interface BleCommMethod {
    /**
     * ble communication session open
     * @param device_name : device name
     */
    void bleOpen(String device_name);
    /**
     * ble communication session restart
     * @param device_name : device name
     */
    void bleRestart(String device_name);
    /**
     * ble communication session close
     */
    void bleClose();
    /**
     * get now ble communication operator
     * @return byte : operation mode
     */
    byte bleGetOperator();
    /**
     * ble advertisement start by special device name
     * only simulate method because ble communication session
     * support device need equal  LE General Discoverable Mode and
     * BR/EDR Not Supported.
     * @param device_name : device name
     */
    void bleStartAdvertisementSimulate(String device_name);
    /**
     * ble advertisement stop
     */
    void bleStopAdvertisement();
    /**
     * ble communication connect requirement
     * @param device_addr : device mac address
     * @param conn_pass : ble communication pass word
     * @param time_out :  ble connection timeout
     */
    void bleStartConnect(String device_addr, byte[] conn_pass, int time_out);
    /**
     * ble communication disconnect requirement
     */
    void bleDisConnect();
    /**
     * ble communication send message requirement
     * @param dat : ble send message
     * @param len :  length of send message maximum is 15 bytes.
     */
    boolean bleSendMessage(byte[] dat, byte len);

}
```
### 主要事件回调
由OnBleCommProgressListener接口提供,客户可根据需求重写相应方法:
```
/**
 * ble communication progress listener interface.
 */

public interface OnBleCommProgressListener {
    /**
     * scan device return
     * @param device_address: device mac address
     * @param device_name : device name
     * @param adv_flag : advertisement flag
     */
    void onScanDevice(String device_address, String device_name, byte adv_flag);
    /**
     * scan device return
     * @param device_address: device mac address
     * @param errorCode :  BLE_ERROR_OK or BLE_ERROR_CONNECTION_TIMEOUT
     */
    void onConnection(String device_address,int errorCode);
    /**
     * scan device return
     * @param device_address : device mac address
     * @param errorCode :
     *                  BLE_ERROR_OK : response for OPER_DISCON_REQ
     *                  BLE_ERROR_CONNECTION_TIMEOUT
     */
    void onDisConnection(String device_address, int errorCode);
    /**
     * ble communication send message requirement
     * @param dat : ble receive message
     * @param len :  length of receive message maximum is 15 bytes.
     */
    void onReceive(byte[] dat,int len);
    void onSendSta(int code);
    /**
     * service open success callback
     * ble communication core is a service named BleService
     * user need start it in user's Activity like this:
     * ((BleService.MyBinder)service).getService()
     * and register service in manifest like this
     * <service android:name="com.wearlink.blecomm.BleService"/>
     */
    void onServiceOpen();
    /**
     * ble communication requirement success
     * @param Oper :
     *             OPER_ADV : bleStartAdvertisementSimulate
     *             OPER_CON_REQ : bleStartConnect
     *             OPER_DISCON_REQ : bleDisConnect
     *             OPER_TRAN : bleSendMessage
     */
    void onStartSuccess(byte Oper);
    /**
     * ble communication requirement failed
     * @param Oper :
     *             OPER_ADV : bleStartAdvertisementSimulate
     *             OPER_CON_REQ : bleStartConnect
     *             OPER_DISCON_REQ : bleDisConnect
     *             OPER_TRAN : bleSendMessage
     * @param errorCode :  BLE_ERROR_INVALID_PARAMETER or BLE_ERROR_INVALID_OPERATION
     */
    void onStartFailure(byte Oper, int errorCode);
}
```

### 操作模式(状态机)
```
    /**
     * operation mode: advertisement.
     */
    static final byte OPER_ADV       =   0x00;
    /**
     * operation mode: connection requirement.
     */
    static final byte OPER_CON_REQ   =   0x01;
    /**
     * operation mode: connection response.
     */
    static final byte OPER_CON_RSP   =   0x02;
    /**
     * operation mode: disconnection requirement.
     */
    static final byte OPER_DISCON_REQ =   0x03;
    /**
     * operation mode: send message requirement.
     */
    static final byte OPER_TRAN  =   0x04;
    /**
     * operation mode: ble communication session open
     */
    static final byte OPER_OPEN = 0x05;
    /**
     * operation mode: ble communication session clos
     */
    static final byte OPER_CLOSE = 0x06;
```
### 回调函数返回状态
```
    /**
     * No Error occurred
     */
    static final byte  BLE_ERROR_OK = 0x00;

    /**
     * Error: At least one of the input parameters is invalid
     */
    static final byte BLE_ERROR_INVALID_PARAMETER = 0x01;

    /**
     * Error: Operation is not permitted
     */
    static final byte BLE_ERROR_INVALID_OPERATION = 0x02;
    /**
     * Error: connection requirement failed
     * time out
     */
    static final byte BLE_ERROR_CONNECTION_TIMEOUT = 0x03;
    /**
     * Error: connection has disconnected
     * time out
     */
    static final byte BLE_ERROR_CONNECTION_DISCONNECTED = 0x04;
```

### 更新
#### V02
1.  增加新的方法
```
    /**
     * ble advertisement start by special device name
     * only simulate method because ble communication session
     * support device need equal  LE General Discoverable Mode and
     * BR/EDR Not Supported.
     * @param device_addr : scan device address
     * @param response_data : response data
     */
    void bleScanResponse(String device_addr, byte[] response_data);
```
2. 加速收发流程
> 会影响V01的模块，请更新模块固件