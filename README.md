# RouterOS-规则
1、导入中国区IP段(每条命令单独执行):

/tool fetch url=http://www.iwik.org/ipcountry/mikrotik/CN

/import file-name=CN

这里的CN指的是中国, 如果需要导入其他国家或地区的IP可以根据直接的需求修改代码
CN: 中国
US: 美国
HK: 香港
TW: 台湾
JP: 日本
CH: 瑞士

2、RouterOS端口转发, 或者叫端口映射.
简单一点说就是把本地的局域网的端口转发到公网上.
回流指的是本地与局域网内的设备也可以通过这个地址访问.

先设置端口回流配置
IP-Firewall-NAT-添加
Chain: srcnat
Src Address: 你本地的IP地址段, 如192.168.88.0/24
Out.Interface: 出口接口, 一般是LAN或者bridge（默认是把4个LAN口绑定为一个桥接了）
然后再进Action页面选择masquerade
<img width="627" height="455" alt="image" src="https://github.com/user-attachments/assets/deaf2f0e-da18-4a78-8484-c449372e3862" />
<img width="629" height="455" alt="image" src="https://github.com/user-attachments/assets/70b36bb4-a22a-4eef-b9ee-f65102cff854" />
<img width="652" height="455" alt="image" src="https://github.com/user-attachments/assets/ff0dc7fe-0f03-4e59-8105-fbdedd0029cd" />


再来设置端口转发.
还是IP-Firewall-NAT-添加
Chain: dstnat
Protocol: 选择协议, 一般是TCP或者UDP
Dst. Port: 外网端口 (也就是说通过外网打开的端口, 可以和内网端口不一样)
Action: dst-nat
To Addresses: 内网设备的IP地址.
To Ports: 内网设备的端口
<img width="652" height="455" alt="image" src="https://github.com/user-attachments/assets/7a83fed1-6fdd-4ec0-96a7-8666f7f2a735" />
<img width="653" height="458" alt="image" src="https://github.com/user-attachments/assets/4e48531e-0399-4c8e-bf4e-0e3dac097b72" />

然后点击Apply
当然，为了方便管理，你也可以在Comment里面添加备注.
<img width="651" height="454" alt="image" src="https://github.com/user-attachments/assets/e853998c-db6a-4e83-ad01-35a16bf4dc2e" />

一些不需要对所有IP都开放的服务，为了安全, 我一般会加个限制.
比如只允许部分IP能访问.
先在IP列表里面把IP地址添加进去, 然后在端口转发设置Advanced的Src.Address List里面选择IP列表.

<img width="654" height="456" alt="image" src="https://github.com/user-attachments/assets/5f9f6293-50be-4675-840e-225dcb0701cc" />
<img width="649" height="455" alt="image" src="https://github.com/user-attachments/assets/140d2763-c8b7-4ecc-8ee2-9df676b203dc" />

