# I.MX6ull-test-board
自制的i.mx6ull测试板，用于对电路功能进行验证，目前已经打样焊接测试功能准确无误。

# 目前具有的功能
1. 网口ETH2正常，可以正常使用TFTP下载内核镜像和设备树，使用nfs挂载根文件系统
2. UART1连接CH340N，使用USBMini-A连接PC终端软件

# 制作过程中遇到的问题

## BTB附近的磁珠电路无效

在实际焊接过程中发现L2磁珠靠近CORE_5V端电压一直在波动，最后为了给核心板供电，选择将L2短路，VCC_5V直接接入核心板。

## 能识别PHY芯片但是无法ping通

我发现在uboot阶段无法通过TFTP下载内核镜像和设备树，后面对I.MX6u底板BTB接口重新加锡焊接，解决了这个问题，如果再次遇到还是要先检查硬件焊接。SMT可以跳过此步骤。

## 原有模块无法加载

```shell
/lib/modules/4.1.15 # modprobe gpioled.ko
gpioled: disagrees about version of symbol device_create
gpioled: Unknown symbol device_create (err -22)
gpioled: disagrees about version of symbol device_destroy
gpioled: Unknown symbol device_destroy (err -22)
gpioled: disagrees about version of symbol device_create
gpioled: Unknown symbol device_create (err -22)
gpioled: disagrees about version of symbol device_destroy
gpioled: Unknown symbol device_destroy (err -22)
modprobe: can't load module gpioled.ko (gpioled.ko): Invalid argument
```

**解决核心思想**：`内核版本与模块版本不一致造成，更改版本一致即可解决`。

如上所示的gpioled.ko模块，在Ubuntu中重新编译发送到开发板根文件系统后可以正常加载。

[解决disagrees about version of symbol device_create_CinzWS的博客-CSDN博客](https://blog.csdn.net/qq_37619128/article/details/124346470)

# 制作参考

> 正点原子IMX6ULL_ALPHA_V2.2(底板原理图).pdf

> 正点原子IMX6ULL_ALPHA_V2.4(底板原理图).pdf
