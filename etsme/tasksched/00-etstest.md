1. 有更新记得先提交并推送git
2. 在测试主机上git pull origin yqy
3. cd到/etstest/go目录下，执行 sh build.sh
4. cd到相应目录看到运行脚本生成
5. 执行 python3 -m http.server 9999
6. 从浏览器打开该测试主机ip:port
7. 复制相应目录链接
8. 到自己的盒子内wget 该链接
9. 在自己盒子执行脚本

```bash
chmod +x tasksched.test
./tasksched.test

./tasksched.test -test.v -test.run TestHighConcurrenceJobCreation
./tasksched.test -test.v -test.run TestConcurrenceJobSimple
./tasksched.test -test.v -test.run TestConcurrenceJob  

```

其他：
```bash
./tasksched.test -test.v -test.run TestPoolJob1  
./tasksched.test -test.v -test.run TestPoolJob2  
./tasksched.test -test.v -test.run TestRegisterJobChangeState  
./tasksched.test -test.v -test.run TestSvr
```




---
# 任务中心

网盘任务：FsGatewayCopyAndMove
后台任务：FileBatchProcess
// 上传下载：DeviceDownloadFromPhd / DeviceUploadToPhd
闪传任务：FastExg
# testsched_test.go

### TestPoolJob1
---
- **验证任务创建**：确保调用 `CreateJob` 时任务可以成功创建，并返回有效的 `JobID`。
- **验证任务追加参数的功能**：测试中逐步追加新的任务参数，确保在同一个 `JobID` 下可以追加参数。
- **验证查询功能**：通过 `QueryJobs` 和 `QueryJobInfo`，验证查询接口能够正确返回任务列表和详细信息。
- **验证任务参数池**：测试参数池 `ArgsPool` 的更新，确保符合预期的参数数目。
- **综合任务的创建和更新流程**：通过多次调用 `CreateJob` 和查询接口，确保任务调度系统在多次参数更新、查询的情况下能够正常工作。
> 问题：为什么需要在同一个JobID下追加参数？



---
## tasksched conf

在这个 `conf` 包中，`TaskDesc` 作为全局变量初始化，并且由配置文件中的数据填充其内容。这里的 `TaskDesc` 是 `model.TaskDescription` 类型的实例，而配置的内容则存储在 `TaskConf` 中。

### `TaskDesc` 变量初始化过程
1. **从配置文件加载**：  
   程序从 `yamlFile`（即 `taskschedConfig.yaml`）读取 YAML 格式的配置数据，并解析为 `TaskYamlConfig` 类型的 `TaskConf`。
   
2. **填充 `TaskDesc` 结构**：  
   解析后的 `TaskConf.Tasks` 是一个任务配置列表。代码遍历该列表，将每个 `TaskCfg` 配置转化为 `model.Task`，并存入 `TaskDesc.Tasks` 中。每个任务配置的关键信息包括 `Name`、`Priority`、`Mode`、`ConcurrencyThr`、`Executor` 等。

3. **初始化 `Jobs` 和 `GroupJobs`**：  
   每个 `model.Task` 的 `Jobs` 和 `GroupJobs` 字段在初始化时被设为空映射或空切片，待后续逻辑（例如调度）动态填充任务实例。

### `TaskDesc` 的内容结构
根据这段初始化代码，`TaskDesc` 的内容结构大致如下：
```go
type TaskDescription struct {
    Tasks        map[string]*model.Task // 任务名称到任务详情的映射
    PriorityJobs map[int][]*model.Job   // 按优先级管理的作业
    RolePrio     model.RolePriority     // 角色优先级
}
```

### `TaskDesc` 的使用
- **任务调度**：  
  在 `SchedReadyJobs()` 中，代码遍历 `TaskDesc.Tasks`，并根据配置的 `GroupJobs` 和 `Jobs` 进行调度。
  
- **管理优先级**：  
  `TaskDesc.PriorityJobs` 用于按照任务的优先级对 `Job` 进行分类，便于根据策略安排任务执行。

### 配置文件的内容
通过配置文件 `taskschedConfig.yaml`，可以自定义任务的属性和调度策略，包括：
- `name`: 任务名称
- `priority`: 优先级
- `mode`: 任务模式
- `createThr` 和 `opThr`: 任务门限
- `executor`: 指定执行者

这样，在调度系统中，`TaskDesc` 作为核心数据结构，结合具体任务和调度逻辑，实现任务的管理和调度。



---

## tasksched model

从 `model` 包的定义来看，`Task` 和 `Job` 是任务调度系统的核心结构，它们分别表示任务和任务下的具体作业。这些结构定义了调度系统中的任务组织方式以及调度逻辑的实现基础。

### 关键结构和字段分析

#### **`TaskDescription`**
`TaskDescription` 是任务调度系统的总体描述，包含以下内容：
- `Tasks`: 所有任务的集合，每个任务以名称为键。
- `PriorityJobs`: 任务按优先级分类的作业列表，便于调度时快速访问。
- `RolePrio`: 用户和设备的优先级配置，用于确定调度策略。

#### **`Task`**
`Task` 表示一个任务，定义了任务的静态配置和动态作业：
- **静态配置字段**：
  - `Priority`: 任务的优先级。
  - `Mode`: 任务模式（如并发任务、池化任务）。
  - `CreateThr` 和 `ConcurrencyThr`: 限制任务创建或同时运行的作业数。
  - `Policy`: 调度和性能相关策略。
  - `Executor`: 执行该任务的服务名称。

- **动态作业字段**：
  - `Jobs`: 一个映射，存储属于该任务的所有作业（以 `JobID` 为键）。
  - `GroupJobs`: 按组存储作业，用于支持任务的分组调度。

#### **`Job`**
`Job` 是任务的具体实例，描述作业的运行状态和属性：
- **关键字段**：
  - `JobID`: 作业的唯一标识。
  - `GroupID` 和 `UserID`: 作业的分组和所属用户。
  - `State`: 作业当前状态，可能是 `JobStateReady`、`JobStateRunning` 等。
  - `Priority`: 作业的动态优先级，用于细化调度策略。
  - `FatherTask`: 作业所属的任务。
  - `Executor`: 作业的执行者信息，包括进度和服务名称。

#### **调度策略**
`TaskPolicy` 定义了调度策略，其中包括：
- `Sched`: 调度相关限制，例如是否限制作业调度。
- `Performance`: 性能相关策略，例如 CPU 调频或保活设置。

---

### 结构之间的关系

1. **`Task` 管理多个 `Job`**：
   - 每个任务可以包含多个作业，作业通过 `Jobs` 和 `GroupJobs` 组织。
   - `Jobs` 提供全局访问，`GroupJobs` 支持按分组的调度逻辑。

2. **调度过程中动态更新 `Job`**：
   - 作业的 `State` 和 `Priority` 是动态调整的，用于实现不同的调度策略。
   - 调度逻辑可能修改 `Job` 的 `GroupID` 或 `UserID`，以优化资源利用。

3. **优先级影响调度**：
   - `PriorityJobs` 提供了按优先级组织的作业集合。
   - 调度策略根据优先级对作业进行排序和分配。

---

### `TaskDesc.Tasks` 的实际意义
在调度过程中，`TaskDesc.Tasks` 是任务调度的入口：
- 它是一个映射，存储了所有任务的静态和动态数据。
- 每次调度时，调度器会遍历 `TaskDesc.Tasks` 中的所有任务，并调用相应逻辑（如 `SchedReadyJobs`）处理每个任务下的作业。

---

### 下一步调试建议

1. **查看 `TaskDesc.Tasks` 的内容**：
   - 在关键代码处（例如 `SchedReadyJobs`）打印 `TaskDesc.Tasks` 的内容，确认任务是否初始化正确。

2. **关注作业的动态变化**：
   - 作业的 `State` 和 `Priority` 是否被调度逻辑正确更新。
   - 核查 `Jobs` 和 `GroupJobs` 的一致性，确保没有作业被误操作。

3. **从配置文件入手**：
   - 检查 `taskschedConfig.yaml` 中任务和调度策略的定义，确保与预期一致。