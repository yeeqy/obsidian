是的，可以通过 Python 将返回体包装成 JSON 格式并打印日志。以下是实现方法和示例：

---

### 方法 1：使用 `json.dumps`

Python 的 `json` 模块可以将字典或列表等对象序列化为 JSON 格式。

```python
import json

def log_response_as_json(response, logger):
    try:
        # 假设 response 是字典或可以转换为 JSON 的对象
        json_str = json.dumps(response, indent=4, ensure_ascii=False)
        logger.info(f"Response in JSON format:\n{json_str}")
    except (TypeError, ValueError) as e:
        logger.error(f"Failed to convert response to JSON: {e}")
```

#### 示例使用：

```python
response = {
    "status": "success",
    "payload": {
        "jobs": [
            {"job_id": "12345", "state": "running"},
            {"job_id": "67890", "state": "completed"}
        ]
    }
}

log_response_as_json(response, logger)
```

#### 输出日志：

```plaintext
INFO: Response in JSON format:
{
    "status": "success",
    "payload": {
        "jobs": [
            {
                "job_id": "12345",
                "state": "running"
            },
            {
                "job_id": "67890",
                "state": "completed"
            }
        ]
    }
}
```

---

### 方法 2：捕获不能直接转换为 JSON 的对象

有些返回体（如包含自定义对象、元组）可能不能直接被 JSON 序列化，可以通过自定义处理逻辑解决。

#### 示例：

```python
def serialize_to_json(obj):
    """
    自定义序列化逻辑，处理不能直接序列化为 JSON 的对象。
    """
    if isinstance(obj, (dict, list, str, int, float, bool, type(None))):
        return obj  # 基本类型直接返回
    elif isinstance(obj, tuple):
        return list(obj)  # 将元组转换为列表
    elif hasattr(obj, "__dict__"):
        return obj.__dict__  # 将对象转换为字典
    else:
        return str(obj)  # 无法处理时，转为字符串

def log_response_as_json_safe(response, logger):
    try:
        json_str = json.dumps(response, indent=4, ensure_ascii=False, default=serialize_to_json)
        logger.info(f"Response in JSON format:\n{json_str}")
    except Exception as e:
        logger.error(f"Failed to serialize response to JSON: {e}")
```

#### 示例使用：

```python
class CustomObject:
    def __init__(self, id, state):
        self.id = id
        self.state = state

response = {
    "status": "success",
    "payload": {
        "jobs": [
            CustomObject("12345", "running"),
            CustomObject("67890", "completed")
        ]
    }
}

log_response_as_json_safe(response, logger)
```

#### 输出日志：

```plaintext
INFO: Response in JSON format:
{
    "status": "success",
    "payload": {
        "jobs": [
            {
                "id": "12345",
                "state": "running"
            },
            {
                "id": "67890",
                "state": "completed"
            }
        ]
    }
}
```

---

### 方法 3：格式化打印 JSON 字符串（适用于接口响应是字符串）

如果返回体本身是 JSON 格式的字符串，可以直接格式化打印。

```python
def log_json_string(json_string, logger):
    try:
        parsed = json.loads(json_string)
        pretty_json = json.dumps(parsed, indent=4, ensure_ascii=False)
        logger.info(f"Formatted JSON response:\n{pretty_json}")
    except json.JSONDecodeError as e:
        logger.error(f"Invalid JSON string: {e}")
```

#### 示例使用：

```python
json_response = '{"status":"success","payload":{"jobs":[{"job_id":"12345","state":"running"},{"job_id":"67890","state":"completed"}]}}'
log_json_string(json_response, logger)
```

#### 输出日志：

```plaintext
INFO: Formatted JSON response:
{
    "status": "success",
    "payload": {
        "jobs": [
            {
                "job_id": "12345",
                "state": "running"
            },
            {
                "job_id": "67890",
                "state": "completed"
            }
        ]
    }
}
```

---

### 总结

- 使用 `json.dumps` 是最直接的方式。
- 对于复杂对象，需要自定义序列化逻辑。
- 如果响应体是字符串，可以通过 `json.loads` 和 `json.dumps` 格式化打印。

选择合适的方法可以让你的日志输出更加美观且易于调试。