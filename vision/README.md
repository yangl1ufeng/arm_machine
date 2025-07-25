# 机器臂智能喂食系统

## 功能特性

### 1. 基础功能
- **自动初始化**: 程序启动时自动将所有舵机设置到初始位置
- **嘴部检测**: 使用MediaPipe进行实时嘴部位置检测
- **串口控制**: 通过串口发送舵机控制命令

### 2. 控制模式

#### 单次喂食模式 (`start`)
- 截取一帧图像进行嘴部检测
- 根据嘴部位置智能调整舵机角度
- 执行完整的喂食动作序列

#### 动态跟踪模式 (`dynamic`)
- 实时检测嘴部位置
- 连续调整舵机角度以跟踪嘴部移动
- 提供实时视觉反馈

#### 停止模式 (`stop`)
- 停止所有喂食动作
- 将所有舵机复位到初始位置

### 3. 智能角度调整

#### 舵机1 (水平控制, 90-150度)
- 嘴部向左偏移 → 增大角度
- 嘴部向右偏移 → 减小角度
- 中心位置 → 120度

#### 舵机2 (垂直控制, 20-60度)
- 嘴部向上偏移 → 减小角度
- 嘴部向下偏移 → 增大角度
- 中心位置 → 45度

#### 舵机3
- 保持在120度（不变）

#### 舵机4
- 喂食时设置为0度

## 使用说明

### 1. 硬件准备
- 确保摄像头连接到设备1
- 确保串口设备连接到COM5
- 确保机器臂4个舵机正常工作

### 2. 软件依赖
```bash
pip install opencv-python mediapipe pyserial numpy
```

### 3. 运行程序
```bash
python calculate_angle.py
# 或
python test_feeding.py
```

### 4. 可用命令
- `start`: 开始单次喂食（检测嘴部位置并执行一次喂食动作）
- `dynamic`: 进入动态跟踪模式（实时跟踪嘴部并调整舵机）
- `stop`: 停止喂食并复位所有舵机
- `init`: 重新初始化舵机到初始位置
- `exit`: 退出程序

### 5. 动态模式操作
- 进入动态模式后，程序会显示实时视频
- 红点表示检测到的嘴部中心
- 蓝点表示图像中心
- 绿线连接两点显示偏移方向
- 按 'q' 键退出动态模式

## 技术参数

### 舵机初始位置
- 舵机1: 90度 (水平面旋转)
- 舵机2: 130度 (竖直面旋转)
- 舵机3: 120度
- 舵机4: 115度

### 喂食位置范围
- 舵机1: 90-150度 (水平跟踪范围)
- 舵机2: 20-60度 (垂直跟踪范围)
- 舵机3: 120度 (固定)
- 舵机4: 0度 (喂食位置)

### 检测精度
- 最小角度调整: 2度 (避免频繁微调)
- 有效偏移范围: 图像尺寸的1/3
- 检测置信度: 0.5

## 安全注意事项

1. **设备检查**: 运行前确保所有硬件连接正常
2. **舵机范围**: 程序已限制舵机角度范围，防止超限
3. **紧急停止**: 可随时输入 `stop` 命令停止动作
4. **视觉反馈**: 动态模式提供实时视觉确认

## 故障排除

### 常见问题
1. **串口连接失败**: 检查COM端口号和波特率设置
2. **摄像头无法打开**: 确认摄像头设备号（默认为1）
3. **嘴部检测失败**: 确保光线充足，面部清晰可见
4. **舵机无响应**: 检查串口连接和舵机供电

### 调试模式
程序会显示详细的检测信息和舵机反馈，便于问题诊断。

## 扩展功能

程序设计为模块化结构，可以轻松扩展：
- 添加更多舵机控制
- 集成语音控制
- 添加安全检测功能
- 支持多目标跟踪
