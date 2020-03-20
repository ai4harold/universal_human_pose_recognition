# Universal human pose analysis system
## Brief
  This method proposes a human pose observation and analysis method based on edge computing devices, which not only meets the requirements of implementation in edge computing (the test results on Jetson nano are achieved by installing TensoRT tf-pose to achieve an effect of 8fts) And through the second channel of data transmission, artificial intelligence workstations are used to further analyze the live video using openpose, and the analysis data is visually analyzed using Microsoft PowerBI.

## Product Design and Technology Implementation
### aim of design
Observe the body posture of the target crowd, analyze the normal posture and gait or abnormal posture and gait judgment of the target population, and use it to compare or adjust their posture or gait during exercise; it is used in social security, Autonomous driving, driver attitude monitoring, sports, fitness, weight loss, and disease rehabilitation in the medical field have important roles. Traditional human pose recognition: Usually based on body-bound sensors and motion capture devices, 3D coordinates of 18 key points of the human body are collected, and a human body motion model is generated by computer 3D modeling software. Need to analyze the target population and scientific research. The limitation of this method is that the motion capture device worn by the sensor itself needs to be tied to the body of the person to be analyzed, plus the winding and length of the data cable is limited, and it depends on a computer. When performing analysis, the bias movement creates restraints and effects, and it is more difficult to track and analyze the target to be analyzed continuously and in multiple spaces.

### Design goals
The camera is used as a posture capture device, and the posture of the human body is digitized and data analyzed using artificial intelligence image processing methods. We use multiple algorithms of artificial intelligence, use the visual recognition method of artificial intelligence to locate the key points of the human body, and identify the posture of the human body by establishing "line segments between human key points" technology, and use big data technology to record the data and use the collection The data is analyzed visually. Realization of human body pose recognition algorithm: The target to be analyzed is vectorized based on 18 points and 18 key points through the camera, and the target to be analyzed is digitized and quantified. The human pose recognition system supports being mounted on a robot or establishing a slide rail, so artificial intelligence computing devices can run on edge devices for human pose recognition.

### Improved edge computing
In order to observe the calculation results in real time at the observation site, the following series of methods based on image processing technology and artificial intelligence technology are used:

1. Camera captures video;
2. The video is stored to the tf card of the edge computing device;
3. Automatically trim the video, including: automatically converting the video to a 800 * 600 resolution video, regardless of the use of any Galaxy camera;
4. Video correction rules:
4-1. Ensure that the subject in the screen is centered, including the object to be identified / analyzed;
4-2. When multiple human bodies are recognized in the picture, the subject closest to the lens is taken as the closest subject to the lens (bottom edge of the picture);
4-3. The resolution is corrected to 800 * 600 pixels;
4-4. The video format is modified to avi or mp4 format;
5. Convert the trimmed video to black and white pixels;
6. Sampling in the original video at x frames per second (x is the number of frames per second taken by the camera) in the video at a frequency of 8 times per second;
7. Use open-pose TensoRT Pose algorithm, which is the gait_pose_analysis algorithm in this git repository for real-time calculation;
7-1. Use the semantic segmentation algorithm on the image to identify the human body;
7-2. Make a reverse selection in the image after semantic segmentation to remove the character background;
7-3. Use OpenCV to identify 18 key points of the human body such as glasses, ears, mouth, nose, left shoulder, right shoulder, shoulder center point, left arm joint, right arm joint, left leg joint, right leg joint, etc .;
7-4. Use SVM algorithm to optimize the recognition accuracy of OpenCV by making a model locally (50 samples are taken for every 1000 images for manual verification) to ensure that the recall rate is below 3%;
7-5. Use the random forest algorithm in visual processing to identify the line segments between key points of the human body, and use film, television or medical materials to perform deep learning on the graphics workstation to establish "the line segments between the key points of the human body with maximum probability are bone line segments "Model;
7-6. Download the established model to the edge computing device for offline field judgment;
8. Combine the calculated video at 8 frames per second into a new video;
9. Real-time display on the display of the new synthetic video with the established "maximum probability human key points are skeletal line segments".

### Edge Computing Implementation
Build the foundation: replace the artificial intelligence algorithm engine (DNN / ANN Engine) with the inference engine and share the computing power of the GPU. The Inference Engine supports the acceleration of deep learning models at the hardware instruction set level. At the same time, the traditional OpenCV image processing library has also been optimized for instruction sets, with significant performance and speed improvements.
The key points of the human body are identified first, followed by the connection between the key points, which we call "skeletal line segments based on human key points". Finally, it is determined through deep learning that the line segments between points can be defined as bones (that is, 2D model established) correct maximum probability, and then connect the points.
a) input image;
b) detecting part of the bounding box from the input image;
c) limb detection;
d) Differentiate everyone in the picture.
Based on Convolutional Neural Networks (CNN), expands computer vision (OpenCV) workloads to maximize performance. The innovation point is to deploy the toolkit (DLDT) on AI workstations, not on edge computing devices.


### Product design ideas
Human pose recognition is an important scenario for the application of artificial intelligence. Posture recognition technology can not only understand the posture of the target object, the movement track of the body, the movement track of the limb, the coordination and angle between the limbs, the target object through the gesture recognition of a single person. The positional relationship with the background scene, etc., can also be used to obtain the crowd's distribution, movement status, abnormal individual discovery and tracking, etc. The calculation result will be output by the edge computing host to the display, wearable device, remote terminal, etc.

### Technical implementation ideas
Using cameras, surveillance cameras, web cameras, smart wearable device cameras, robotic cameras, drone cameras, computer remote cameras, mobile phone cameras, etc., as long as the video file can be used as the video source to be analyzed, the video source is directly processed at the data site. Computing, executes high-throughput AI at the edge of the data, enabling the data site to obtain real-time AI insights and AI-driven actions, reversing the drastic changes and reduction of the need to input analysis data to the AI ​​workstation for long-term calculations The amount of data that must be sent to the cloud.

### Innovative data flow design
The first data stream: real-time on-site calculation and result output based on high-performance edge computing equipment and algorithms; the second data stream: The purpose of enabling the second data stream is to facilitate the analysis of historical data, image data, and quick shooting through mobile phones Video data, etc. for accurate analysis. The computing point of the second channel of data is deployed in the cloud, and a high-performance artificial intelligence workstation is used to bear the computing power. The management method of the second channel data is: the user only needs to log in to Baidu Cloud Disk and upload the video to be analyzed. After 24 hours, open the Baidu Cloud Disk to see the corresponding folder, which contains: 18 points coordinates of the video file and Its key line segments are displayed, with 18 key point data packet collections sampled 3 times per second, and big data visual analysis results are displayed. For the Internet of Things, breakthroughs in edge computing technology mean that many controls will be implemented through local devices without having to be transferred to the cloud, and processing will be done at the local edge computing layer.
The dual-channel data stream architecture is a creation. Its core idea is that one channel of data is edge computing for on-site collection and analysis of data, and the other channel of data is transmitting video files to a remote AI workstation for multi-algorithm calculation and analysis, and supports Millions of ready-made devices, such as security cameras, surveillance cameras, home gimbal cameras, etc., which are traditional erect video capture devices, are used as data entry points for data collection, allowing millions of devices to have real-time body posture immediately Analysis function, and provide millions of users with scientific research work and value-added services based on human pose recognition. This method combines the difficulty of collecting data for human pose recognition, the complex features of human poses during the recovery of different illnesses, and the complex acquisition of training data for different illnesses. It uses a flexible supporting device to obtain more application scenarios and data in the industry (double Road data design, as shown in Figure 2-7, the theoretical basis is big data technology + artificial intelligence technology. This method can make deep learning not only start from "now and now" and "set up equipment", but can fully Use the existing data and existing equipment as the original startup resources, such as non-intrusive equipment, traditional equipment such as long-established cameras, video images, scientific research materials, etc., can become the analysis object and training data). The design of the dual-channel data stream allows us to collect data for unauthorized scenes, and we can use rich video images as training data.
## License description
General human keypoint detection and batch video analysis software license agreement Academic or non-profit organization non-commercial research use Pre-license: Because this software uses multi-party open source software, it must first follow the LICENCEs of the open source software involved

By using or downloading the software, you agree to the terms of this license agreement. If you do not agree to these terms, you must not use or download the software. The use and reference of any contained code, block diagram, and other contents need to contact the author himself and authorize it by email, and publish the authorization code and private key file in the host's / src / licence directory.


# 通用的人体姿态分析系统
## 简述
  该方法提出了一种建立在边缘计算设备上的人体姿态观察与分析方法，不仅满足了边缘计算中实施行的要求（试验结果在jetson nano上通过安装TensoRT tf-pose实现了达到8fts的效果），而且通过第二路数据的传输采用人工智能工作站使用openpose对现场视频进行进一步分析，并将分析数据使用微软PowerBI进行可视化分析。 

## 产品设计与技术实现方法
### 设计目的
对目标人群的身体姿态进行观察、分析目标人群的正常姿态和步态或是非正常姿态和步态的判断，用于在运动中比对或是调整其姿态或是步态；其在社会治安、自动驾驶、驾驶员姿态监测、体育运动、健身减肥、医学领域中的疾病康复等具有重要的作用。传统的人体姿态识别：通常是基于身体绑定传感器和动作捕捉装置，采集人体18个关键点的三维坐标，从而通过计机3D建模软件生成人体的动作模型。需要待分析目标人群与科研该方法的局限性还在于穿着通过传感器的运动捕捉装置本身是需要捆绑在待分析人员的身体上，再加上数据线的缠绕和长度有限，又要依赖一台计算机进行分析，偏置运动产生束缚和影响，更难以连续多空间的对待分析目标进行跟踪分析。

### 设计目标
利用摄像头为姿态捕捉设备，用人工智能图像处理的方法对人体的姿态进行数字化和数据分析。我们采用人工智能的多个算法，采用人工智能的视觉识别的方法定位人体关键点，并通过建立“人体关键点之间线段”技术识别人体姿态，并采用大数据技术对数据进行记录，利用采集到的数据进行可视化分析。 实现人体姿态识别算法：通过摄像头将待分析目标进行基于18个点以及18个关键点之间的矢量化方式，对待分析目标进行数字化和量化。人体姿态识别系统支持搭载在机器人上或建立滑轨等，因此人工智能计算设备可以运行在边缘设备上进行人体姿态辨识。

### 边缘计算效果提升
为了在带观测现场实时的观察计算结果，采用了以下的一系列基于图像处理技术和人工智能技术的方法：

1. 摄像头捕捉视频；
2. 视频存储到边缘计算设备的tf卡；
3. 将视频进行自动修整，包括：自动将视频转化为800*600分辨率的视频，无论采用任何星河的摄像头；
4. 视频修正规则：
4-1.确保画面中主体居中，包括待识别/分析对象； 
4-2.当画面中识别到多个人体的时候，以距离镜头（画面最下边缘）为距离镜头最近的主体； 
4-3.分辨率修正到800*600像素； 
4-4.视频格式修正到avi或mp4格式；
5. 将修整好的视频转化为黑白像素；
6. 在视频中以每秒钟8次的频率在每秒钟x帧（x为摄像头的每秒钟拍摄帧数）的原画视频中进行采样；
7. 采用open-pose的TensoRT Pose算法，即本git仓库中的gait_pose_analysis算法进行实时计算； 
7-1. 在图像上采用语意分割算法识别人体； 
7-2.在语意分割后的图像中做反向选择，去掉人物背景； 
7-3.采用OpenCV识别人体的眼镜、耳朵、嘴巴、鼻子、左肩膀、右肩膀、肩膀中心点、左臂关节、右臂关节、左腿关节、右腿关节等人体18关键点； 
7-4.采用SVM算法，在本地做模型优化OpenCV的识别准确度（每1000张图片中采样50张进行人工校验），确保召回率在3%以下； 
7-5.采用视觉处理中的随机森林算法，识别人体关键点之间的线段，并采用电影电视或医学资料素材在图形工作站进行深度学习，建立“最大概率人体关键点之间线段为骨骼线段”的模型； 
7-6.将建立的模型下载到边缘计算设备进行离线现场判断；
8. 将计算过的每秒钟8帧的视频合成新视频；
9. 在显示器实时显示合成的具有建立“最大概率人体关键点之间线段为骨骼线段”新视频。

### 边缘计算实现方式
建立基础：用推断引擎(Inference Engine)替代掉人工智能算法引擎(DNN/ANN Engine)，将GPU计算的算力分担。推断引擎(Inference Engine)支持硬件指令集层面的深度学习模型加速运行，同时对传统的OpenCV图像处理库也进行了指令集优化，有显着的性能与速度提升。
首先识别人体关键点，其次是进行关键点之间的连线，我们称之为“基于人体关键点的骨骼线段”，最后是通过深度学习确定点与点之间的线段可以定义为骨骼（即建立的2D模型）正确的最大概率，而后将点连接起来。
a) 输入图像；
b) 从输入图像中检测部分边界框；
c) 检测出肢体；
d) 区分图中每个人。
基于卷积神经网络（CNN），可扩展计算机视觉（OpenCV）工作负载，从而最大限度地提高性能。而创新指出在于在AI工作站部署工具包（DLDT），而非在边缘计算设备上进行。


### 产品设计思路
人体姿态识别是人工智能应用的一个重要场景，姿态识别技术不仅可以通过对单人的姿态识别了解目标对象的身体姿态情况、人体运动轨迹、肢体运动轨迹、肢体之间的协调和角度、目标对象与背景景物的位置关系等，还可以通过对群体的姿态识别获取人群的分布情况、运动状态、反常个体发现与追踪等。计算结果将被边缘计算主机输出显示器、穿戴设备、远程终端等。

### 技术实现思路
利用摄像头、监控摄像头、网络摄像头、智能穿戴设备摄像头、机器人摄像头、无人机摄像头、电脑远程摄像头、手机摄像头等，只要能以视频文件作为待分析的视频源，在数据现场直接对视频源进行计算，在数据的边缘执行即时的高吞吐量AI，使得数据现场获取实时的AI洞察的结论以及由AI驱动的动作，扭转了需要将带分析数据输入到AI工作站进行长时间计算的巨变以及减少必须发送到云的数据量。

### 创新的数据流设计
第1路数据流：基于高性能边缘计算设备和算法的现场实时计算与结果输出； 第2路数据流：启用第2路数据流的目的是以便于分析历史数据、影像资料、通过手机快捷拍摄的视频数据等进行精确的分析。第2路数据的运算点部署在云端，采用高性能人工智能工作站承担计算算力。第2路数据的管理方法是：用户只需要登录到百度云盘，上传需要分析的视频，24小时后，打开百度云盘就可以看到对应的文件夹，里面包含：视频文件18点坐标及其关键线段显示、以每秒3次采样的18个关键点数据包集合、大数据可视化分析结果展示。对物联网而言，边缘计算技术取得突破，意味着许多控制将通过本地设备实现而无需交由云端，处理过程将在本地边缘计算层完成。
采用双路数据流架构是一个创造，它的核心思路是一路数据是现场采集与分析数据的边缘计算，另一路数据流是将视频文件传输到远程AI工作站进行多算法的计算与分析，并且支持将传统的已经架设好的视频采集设备例如安防摄像头、监控摄像头、家庭云台摄像头等数以百万计的现成设备作为采集数据的数据入口，让数以百万的设备立刻具有实时的人体姿态分析功能，并为数以百万的用户提供基于人体姿态识别的科研工作与增值服务。该方式结合了人体姿态识别采集数据难度大、不同病症康复过程中人体姿态特征复杂、不同病症训练数据获取复杂等特点，采用了支持灵活的搭载设备，在行业获取更多应用场景与数据（双路数据设计，如图2-7所示，理论基础是大数据技术+人工智能技术，该方式可以让深度学习不仅仅从“此时此刻”与“架设设备”基础上开始，而是可以充分利用现有数据、现有设备作为原始启动资源，例如非侵入式设备、传统设备如架设已久的摄像头、录像影像、科研资料等，均可以成为分析对象与训练数据）。双路数据流的设计，我们可以针对未经授权的场景进行数据采集，而且可以利用丰富的视频影像资料作为训练数据。
## 使用授权说明

通用人体关键点检测与批量视频分析软件许可协议
学术或非营利组织非商业研究用途
前置授权：
因本软件中使用了多方开源软件，因此必须优先遵循涉及到的开源软件的LICENCEs

使用或下载软件，即表示您同意本许可协议的条款。如果您不同意这些条款，则不得使用或下载该软件。
任何包含的代码、框图等全部内容的使用、引用，需要联系作者本人并通过电子邮件授权，将授权码和私钥文件公布在主机的/src/licence目录下。
