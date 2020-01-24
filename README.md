# Sonoff Refirmware


## MQTT Inf

### Host + Port

```
    Host: ""
    Port: 1883 (MQTT), 8080 (MQTT Over Websocket)
    Username: ""
    Password: ""
```
Can use AWS IoT, etc

### Topic MQTT

####  LWT:

```
    Topic: "tele/A82A95/LWT"
    Online Payload: "Online"
    Offline Payload: "Offline"
```

#### Prefix

```
    cmnd/<DeviceId>/ : Các topic gửi lệnh
    stat/<DeviceId>/ : Các topic confirm trạng thái hoặc thông báo về cấu hình thiết bị
    tele/<DeviceId>/ : Các topic gửi thông tin theo chu kì định sẵn (thông tin thiết bị, cảm biến,...)
```

#### Chi tiết các topic Publish từ thiết bị:

```
    tele/A82A95/LWT: Gói tin LWT (Last Will Message)
    tele/A82A95/INFO1: Thông tin về thiết bị, chỉ gửi khi khởi động thiết bị.
    tele/A82A95/INFO2: Thông tin mạng của thiết bị, chỉ gửi khi khởi động thiết bị.
    tele/A82A95/INFO3: Thông tin trạng thái khởi động, reset của thiết bị
    tele/A82A95/STATE:  Toàn bộ thông tin về trạng thái hệ thống, Publish 60s 1 lần
    stat/A82A95/RESULT: Xác thực và thông báo lệnh điều khiển
    stat/A82A95/POWER: Trạng thái của Relay khi điều khiển
```

#### Chi tiết các topic Subscribe từ thiết bị:

```
    cmnd/A82A95/POWER: Điều khiển On/Off/Toggle Relay
    cmnd/A82A95/TIMERx: Hẹn giờ cho thiết bị
``` 


## Điều khiển thiết bị

### Điều khiển qua MQTT

```
    Topic Puslish lệnh điều khiển: cmnd/A82A95/POWER
    Lệnh On Relay: "ON"
    Lệnh Off Relay: "OFF"
    Lệnh Toggle Relay: "TOGGLE"
```

Sau khi gửi lệnh tại topic cmnd/A82A95/power
Thiết bị gửi trả lại thông tin lệnh tại topic: stat/A82A95/RESULT và trạng thái Relay tại Topic stat/A82A95/POWER

### Điều khiển qua nút nhấn

Cứ ấn nút thôi :)))

### Điều khiển thiết bị qua mạng local:

Điện thoại phải kết nối chung mạng với thiết bị:
Protocol: HTTP

```
    method: GET
    url: http://<ip>/cm?cmnd=<Query params>
    Query params:
        "POWER ON": On Relay
        "POWER OFF": Off Relay
        "POWER TOGGLE": Toggle Relay
```

## Hẹn giờ

Tối đa 16 Timer

```
    Topic: cmnd/A82A95/TIMERx (x từ 1 -> 16)
    Payload Example: {"Arm":1,"Time":"02:23","Window":0,"Days":"--TW--S","Repeat":1,"Output":1,"Action":1}
```

Arm:

```
    1: enbale Timer x
    0: disable Timer x
```

Mode:

```
    0 = sử dụng clock nội trên thiết bị
    1 = Sử dụng giờ sunrise địa phương dựa trên kinh độ, vĩ độ và Time offset
    2 = Sử dụng giờ sunset địa phương dựa trên kinh độ, vĩ độ và Time offset
```

Time:

```
    hh:mm = giờ từ 0 - 24 và phút từ 00 - 59
    -hh:mm = giờ hiệu chỉnh từ -11 .. 12 và phút 0 .. 59 (sử dụng với mode 1 và mode 2)
```

Days

```
    SMTWTFS = thiết lập ngày trong tuần kích hoạt hẹn giờ, 0 = Off và ký tự bất kỳ khác = On vào ngày đó
```

Repeat

```
    0 = Timer kích hoạt 1 lần duy nhất
    1 = Timer được lặp lại
```
Output
```
    1..16 = chọn ngõ ra relay
```
Action
```
    0 = turn output OFF
    1 = turn output ON
    2 = TOGGLE output
```

## Docs
Chi tiết xem thêm tại
https://tasmota.github.io/docs/#/Home

