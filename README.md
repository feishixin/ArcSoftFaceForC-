# ArcSoftFaceForC#
1.	简介
1.1	运行环境

Windows平台
最低硬件配置
Intel® CoreTM i5-2300@2.80GHz 或者同级别芯片

推荐硬件配置
Intel® CoreTMi7-4600U@2.1GHz 或者同级别芯片

1.2	系统要求
Windows7及以上
1.3	开发工具
VS2010以上版本
1.4	环境要求
.Net Framework 4.0以上
1.5	支持的颜色空间格式
支持图像的颜色空间格式: BGR24
1.6	产品功能简介
1.6.1	人脸检测
从图片中检测人脸信息，人脸在图片中的位置坐标信息。如果图片中有多个人脸，请使用多人脸检测方法：ASFDetectFaces
1.6.2	年龄检测
对图片中对应的人脸图片信息数据进行年龄检测。对应方法：ASFGetAge
1.6.3	性别检测
对图片中对应的人脸图片信息数据进行性别检测。对应方法：ASFGetGender
1.6.4	人脸识别
将从图片中提取的两个人脸特征信息，通过人脸识别SDK中人脸比对的方法：ASFFaceFeatureCompare，对两个特征值进行比较，通过返回的相似度判断两个人是否是一个人。
2.	快速上手
1.	安装VS2012环境安装包(vcredist_x86_vs2012.exe)、VS2013环境安装包（vcredist_x86_vs2013.exe）
2.	从官网申请sdk  http://www.arcsoft.com.cn/ai/arcface.html  ，下载对应的sdk版本(x86或x64)并解压
3.	将libs中的“libarcsoft_face.dll”、“libarcsoft_face_engine.dll”拷贝到工程bin目录的对应平台的debug或release目录下
4.	将对应appid和appkey替换App.config文件中对应内容
5.	在Debug或者Release中选择配置管理器，选择对应的平台
6.	按F5启动程序
7.	点击“注册人脸”按钮增加人脸库图片
8.	点击“选择识别图”按钮增加人脸图片
9.	点击“开始匹配”按钮进行比较
10.	根据下面文本框查看相关信息

3.	接入指南
3.1	示例代码
3.1.1	引擎激活
 
3.1.2	初始化引擎
 

初始化时要先将用的方法类型设置好；应用程序关闭时，必须销毁引擎，否则会造成内存泄漏
 
3.1.3	人脸检测
使用人脸检测功能需要在初始化引擎时将人脸检测方法类型(FaceEngineMask.ASF_FACE_DETECT)做初始化，从图片文件中提取图像数据byte[]，一般为RGB格式图像数据，最后将其作为参数传入FaceUtil.DetectFace (IntPtr pEngine, string imagePath)的人脸检测方法即可：
 
3.1.4	提取特征
提取特征功能需要在初始化引擎时将人脸识别功能类型初始化(FaceEngineMask.ASF_FACERECOGNITION)，将图像数据byte[]和人脸检测结果Rect传入FaceUtil.ExtractFeature(IntPtr pEngine, Image image)方法来提取人脸特征信息：
 
3.1.5	人脸比对
人脸比对功能是通过对比两个人脸特征信息，返回两者的相似程度。通过人脸检测，提取特征后，通过ASFFunctions.ASFFaceFeatureCompare(IntPtr pEngine, IntPtr faceFeature1, IntPtr faceFeature2, ref float similarity)的人脸比对方法对比两个人脸特征信息，获取它们的相似度。
 

3.2	通用方法
3.2.1	从Bitmap中读取BGR数据
从Bitmap中读取BGR数据的方法比较复杂，可以参考ImageUtil.ReadBMP(Image image)方法。

4.	常见问题
4.1	常见问题问答
问题	参考回复
启动后引擎初始化失败	1.	请选择对应的平台，如x64,x86 
2.	删除bin下面对应的asf_install.dat，freesdk_132512.dat；
3.	请确保App.config下的appid，和appkey与当前sdk一一对应。
SDK支持那些格式的图片人脸检测？	目前SDK支持的图片格式有jpg，jpeg，png，bmp等。
使用人脸检测功能对图片大小有要求吗？	推荐的图片大小最大不要超过2M，因为图片过大会使人脸检测的效率不理想，当然图片也不宜过小，否则会导致无法检测到人脸。
使用人脸识别引擎提取到的人脸特征信息是什么？	人脸特征信息是从图片中的人脸上提取的人脸特征点，是byte[]数组格式。 
SDK人脸比对的阈值设为多少合适？	推荐值为0.8，用户可根据不同场景适当调整阈值。
可不可以将人脸特征信息保存起来，等需要进行人脸比对的时候直接拿保存好的人脸特征进行比对？	可以，当人脸个数比较多时推荐先存储起来，在使用时直接进行比对，这样可以大大提高比对效率。存入数据库时，请以Blob的格式进行存储，不能以string或其他格式存储。
在.Net项目中出现堆栈溢出问题	.Net平台设置的默认堆栈大小为256KB，SDK中需要的大小为512KB以上，推荐调整堆栈的方法为：
new Thread(new ThreadStart(delegate {
                    ASF_MultiFaceInfo multiFaceInfo = FaceUtil.DetectFace(pEngine, imageInfo);
                }), 1024 * 512).Start();
X86模式下批量注册人脸有内存溢出或图片空指针	请增加虚拟内存或每次批量注册人脸控制在20张图片范围内
图片中有人脸，但是检测时未检测到人脸	1.	请调整detectFaceScaleVal的值；
2.	请确认图片的宽度是否为4的倍数；
3.	请确认图片是否通过ImageUtil.ReadBMP方法进行数据调整。
更多常见问题请访问https://ai.arcsoft.com.cn/manual/faqs.html。
4.2	其他帮助
如您想要了解更多虹软的产品，请访问虹软官网http://www.arcsoft.com.cn/，或者您在开发的过程中遇到了问题，或者对我们的人脸识别SDK有什么意见或建议，欢迎在虹软官方论坛https://ai.arcsoft.com.cn//bbs/portal.php上发帖提问，我们的工作人员会竭力为您解答。
