# ZoneMinder

ZoneMinder is running in Docker. The following folders are mounted to ~/apps/zoneminder:

| Host   | Container                    |
| ------ | ---------------------------- |
| events | /var/cache/zoneminder/events |
| images | /var/cache/zoneminder/images |
| logs   | /var/log/zm                  |
| mysql  | /var/lib/mysql               |

## Activate Hikvision Camera

- Set PC to manual IP of 192.168.1.60
- Navigate to web interface at http://192.168.1.64 and set admin password
- Configure for DHCP and enter reserved address in router
- Add `zm` user (creds in LastPass)

## Add Monitor to ZoneMinder

| Setting                 | Value                                                                    |
| ----------------------- | ------------------------------------------------------------------------ |
| Source Path             | rtsp://zm:\<password\>@\<camera_ip\>/cam/realmonitor?channel=1&subtype=0 |
| Remote Method           | TCP                                                                      |
| Options                 | Empty                                                                    |
| Target Colorspace       | 24-bit color                                                             |
| Capture Width (pixels)  | 1920                                                                     |
| Capture Height (pixels) | 1080                                                                     |
| Preserve Aspect Ratio   | Not checked                                                              |
| Orientation             | Normal                                                                   |
| Deinterlacing           | Disabled                                                                 |
