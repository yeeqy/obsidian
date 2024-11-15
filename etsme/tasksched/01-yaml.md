```
# 角色和设备终端优先级定义  
rolePriority:  
  user:  
    {s0: 0, master: 2, member: 3, visitor: 4, passer: 5, s3: 7}  
  device:  
    {sys: 0, phone: 1, pc: 3, etsme-myself: 5, etsme-others: 7}  
  
# 注册的功能任务  
tasks:  
################################################ 测试 ################################################  - name: "B"           #  池化任务测试，JOBID固定  
    priority: 32  
    mode: 1             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1        # 只能创建一个，ready信息从任务执行者获取  
    opThr: 5  
    concurrencyThr: 1   # 每个分组里每个用户的并发上限  
    storeType: 2        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "tasksched.test"  
  - name: "BGoTest"     #  池化任务测试  
    priority: 64  
    mode: 1             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1        # 只能创建一个，ready信息从任务执行者获取  
    opThr: 5  
    concurrencyThr: 1   # 每个分组里每个用户的并发上限  
    storeType: 2        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "tasksched.test"  
  
  - name: "CGoTest"     #  并发任务测试  
    priority: 32  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 10000    # 只能创建一个，ready信息从任务执行者获取  
    opThr: 10000  
    concurrencyThr: 1   # 每个分组里每个用户的并发上限  
    storeType: 2        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "tasksched.test"  
#################################################################### T0 任务 0 - 15 #################################################################################################################### 系统S0任务 ################################################  - name: "S0SystemReboot"     # 系统S0级任务 - 重启  
    priority: 0  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "devmon"  
  
  - name: "S0SystemUpgrade"     # 系统S0级任务 - 升级  
    priority: 0  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1000  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "updpro"  
    policy:  
      performance:  
        cpuspeed : 2    # High  
        alive : true  
  
  - name: "S0CPUTempCriticalHigh"     # 系统S0级任务 - CPU温度严重高  
    priority: 1  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "devmon"  
  
  - name: "S0SystemLoadHigh"     # 系统S0级任务 - CPU高负载  
    priority: 1  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "devmon"  
  
################################################ 数据迁移 ################################################  - name: "PhdDataBackup"     # 数据迁移  
    priority: 2  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 10  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "etsopsdm"  
  
  - name: "EstorDataBackup"     # estor数据迁移  
    priority: 2  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  ################################################ 跨版本升级 ################################################  - name: "PhdUpgradeFrom2To3"     # 跨版本升级  
    priority: 2  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 10  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "etsopsdm"  
  
  - name: "PhdUpgradeFrom2To3Usermgmt"     # 跨版本升级  
    priority: 2  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 10  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "usermgmt"  
  
  - name: "PhdUpgradeFrom2To3Filemgmt"     # 跨版本升级  
    priority: 2  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 10  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "filemgmt"  
  
  - name: "PhdUpgradeFrom2To3Ebox"     # 跨版本升级  
    priority: 2  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 10  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "ebox"  
  
#################################################################### T1 任务 32 - 47 ####################################################################  
#################################################################### T2 任务 64 - 79 #################################################################################################################### 手机文件传输 ################################################  - name: "DeviceUploadToPhd"   # 设备上传数据到PHD  
    priority: 64  
    mode: 1             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 50  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "estor"  
    policy:  
      performance:  
        cpuspeed : 2    # High  
        alive : true    # 系统需要保活，不能休眠  
  
  - name: "DeviceDownloadFromPhd"   # 设备从PHD下载数据  
    priority: 64  
    mode: 1             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 50  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "estor"  
    policy:  
      performance:  
        cpuspeed : 2    # High  
        alive : true  
  
  - name: "PlayVideo"   # 播放视频  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 50  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "estor"  
    policy:  
      performance:  
        alive : true  
  
  - name: "PlayAudio"   # 播放音频  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 50  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "estor"  
    policy:  
      performance:  
        alive : true  
  
  - name: "DownloadFromPhd"       # 从PHD下载文件到本PHD   
priority: 64          # 静态优先级  
    mode: 0               # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 10000      # 实例化作业上限  
    opThr: 10000          # 操作项上限  
    concurrencyThr: 1     # 每个分组里每个用户的并发上限  
    storeType: 2          # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true     # 持久化  
    proactiveStop : true  # 只能主动停止  
    visible: true         # 需要呈现，老无线果，后续可去掉  
    executor: "estor"     # 任务执行者  
    policy:               # 策略配置  
      performance:        # 性能相关  
        cpuspeed : 2      # 0 低主频； 1 中间主频； 2 最高主频  
        alive : true      # 系统需要保活，不能休眠  
  
  - name: "DownloadFromChat"       # 从Chat下载文件到本PHD  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 10000  
    opThr: 10000  
    concurrencyThr: 1   # 每个分组里每个用户的并发上限  
    storeType: 2        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    proactiveStop : true  # 只能主动停止  
    visible: true  
    executor: "estor"  
    policy:  
      performance:  
        alive : true  
  
  - name: "DownloadFromThirdParty"       # 从第三方下载文件到本PHD   
priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1000  
    opThr: 5000  
    concurrencyThr: 1   # 每个分组里每个用户的并发上限  
    storeType: 2        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    visible: true  
    executor: "etsget"  
    policy:  
      performance:  
        alive : true  
  
  - name: "FastExg"       # 闪传任务  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 10000  
    opThr: 10000  
    concurrencyThr: 1   # 每个分组里每个用户的并发上限  
    storeType: 2        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    proactiveStop : true  # 只能主动停止  
    visible: true  
    executor: "estor"  
    policy:  
      performance:  
        cpuspeed : 2    # High  
        alive : true  
  
  - name: "FileImportToEstor"          # 文件导入到Estor  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 100  
    opThr: 100000  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "filemgmt"  
  
  - name: "FileExportFromEstor"          # 从Estor导出文件  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 100  
    opThr: 100000  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "filemgmt"  
  
  - name: "FsGatewayCopyAndMove" # fs gateway  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 100  
    opThr: 100000  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "FileBatchProcess" # 文件批量操作  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 100  
    opThr: 100000  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "FileBatchOperation" # 文件批量操作git diff  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 100  
    opThr: 100000  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "filemgmt"  
  
  - name: "GatewayKeepalive"          # 网关保活  
    priority: 64  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    executor: "gateway"  
    policy:  
      performance:  
        cpuspeed : 2    # High  
        alive : true  
  
#################################################################### T3 任务 96 - 111 #################################################################################################################### 能力任务 ################################################  - name: "GenThumb"       # 产生缩略图  
    priority: 92  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "EstorGC"       # estor垃圾回收  
    priority: 91  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "EstorDupCheck"       # estor 副本可靠性检测  
    priority: 96  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "EstorMd5Calc"       # estor Md5后计算任务  
    priority: 94  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "EstorFileEncrypt"       # estor 文件加密任务  
    priority: 94  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "FileStarLevelAutoChange"       # 文件星级自动改变  
    priority: 96  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "FileStarLevelManualChange"       # 文件星级手动改变  
    priority: 96  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1000  
    opThr: 1000  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "AI"     # AI  
    priority: 98  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "smartalbum"  
    policy:  
      performance:  
        cpuspeed : 2    # High  
  
  - name: "IDLE"     # 空闲任务，永久存在  
    priority: 111  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "tasksched"  
  
  - name: "PhdMetadataBackup"       # 元数据备份统一服务  
    priority: 90  
    mode: 2             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "etsopsdm"  
  
  - name: "EstorMetadataBackup"       # estor元数据备份  
    priority: 90  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "estor"  
  
  - name: "FilemgmtMetadataBackup"       # filemgmt元数据备份  
    priority: 90  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "filemgmt"  
  
  - name: "UsermgmtMetadataBackup"       # usermgmt元数据备份  
    priority: 90  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "usermgmt"  
  
  - name: "EboxMetadataBackup"       # ebox元数据备份  
    priority: 90  
    mode: 0             # 0 并发任务；1 池化任务；2 例程任务  
    createThr: 1  
    opThr: 1  
    concurrencyThr: 1  
    storeType: 0        # 0 不保存详细信息；1 只保存失败项；2 保存全部完成项  
    persistence: true  
    executor: "ebox"

```