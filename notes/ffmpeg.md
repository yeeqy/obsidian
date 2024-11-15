好的！既然你已经安装好了FFmpeg，那我们可以用它做很多音视频相关的处理。以下是一些基本的用法：

### 1. **查看文件信息**
   使用`ffprobe`命令，可以获取音视频文件的详细信息，例如分辨率、时长、编码格式等。
   ```bash
   ffprobe -i 文件路径
   ```
   例如：`ffprobe -i input.mp4`

### 2. **格式转换**
   可以将不同格式的文件相互转换，比如把MP4文件转换为AVI格式。
   ```bash
   ffmpeg -i input.mp4 output.avi
   ```

### 3. **提取音频**
   从视频中提取音频并保存为MP3文件。
   ```bash
   ffmpeg -i input.mp4 -q:a 0 -map a output.mp3
   ```

### 4. **视频裁剪**
   可以剪切视频的一部分，比如提取从30秒到60秒的片段。
   ```bash
   ffmpeg -i input.mp4 -ss 00:00:30 -to 00:01:00 -c copy output.mp4
   ```

### 5. **调整分辨率**
   可以改变视频的分辨率，比如将视频转换成720p。
   ```bash
   ffmpeg -i input.mp4 -vf scale=1280:720 output_720p.mp4
   ```

### 6. **压缩视频**
   减少视频的比特率来降低文件大小。
   ```bash
   ffmpeg -i input.mp4 -b:v 1M output_compressed.mp4
   ```

### 7. **合并视频**
   多个视频文件可以合并成一个文件。
   首先创建一个文本文件（如`filelist.txt`），文件内容类似如下：
   ```
   file 'input1.mp4'
   file 'input2.mp4'
   ```
   然后执行：
   ```bash
   ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
   ```

