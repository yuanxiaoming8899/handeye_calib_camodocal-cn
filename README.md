<div class="Box-sc-g0xbh4-0 bJMeLZ js-snippet-clipboard-copy-unpositioned" data-hpc="true"><article class="markdown-body entry-content container-lg" itemprop="text"><div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">ROS + CamOdoCal 手眼校准</font></font></h1><a id="user-content-ros--camodocal-hand-eye-calibration" class="anchor" aria-label="永久链接：ROS + CamOdoCal 手眼校准" href="#ros--camodocal-hand-eye-calibration"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"></font><a href="https://github.com/hengli/camodocal"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这是一个集成了CamOdoCal</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">中实现的手眼校准的 ROS 节点</font><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">请参阅此</font></font><a href="http://robotics.stackexchange.com/questions/7163/hand-eye-calibration" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">堆栈交换问题，解释手眼校准的工作原理</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">示例用途包括确定 a 的位置和方向的精确变换：</font></font></p>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">安装在地板上的相机是相对于机器人手臂的</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">相机连接到相对于机器人手臂的机器人手臂尖端</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">连接到移动车辆的一组摄像机（这是 camodocal 本身实现的）</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">两个机器人手臂用螺栓固定在一起</font></font></li>
</ul>
<p dir="auto"><a href="https://www.icloud.com/keynote/AwBUCAESEJ6BPEHy1-J_58iY9QpDxmMaKWYQ8JT4T8wVxHSfiNn7vMJH1IuI3bnUaJeS8H0P8z768Qw95BLoFg2qMCUCAQEEILObYmsh9SaWe-3YhL6v1kNVeciwzjsktabBmlhsO661#Optimal_Hand_Eye_Calibration" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">主题演讲</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">为感兴趣的人解释了有关手眼校准的许多细节。</font><font style="vertical-align: inherit;">下面可以找到校准机器人的实用代码和说明。</font></font></p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer nofollow" href="https://camo.githubusercontent.com/d320889a02f949011c9f4534b8001e27aa2fd474ed6b94f2f564a6b7f3543dc4/687474703a2f2f692e737461636b2e696d6775722e636f6d2f376b3444332e6a7067"><img src="https://camo.githubusercontent.com/d320889a02f949011c9f4534b8001e27aa2fd474ed6b94f2f564a6b7f3543dc4/687474703a2f2f692e737461636b2e696d6775722e636f6d2f376b3444332e6a7067" alt="手眼校准基础知识" data-canonical-src="http://i.stack.imgur.com/7k4D3.jpg" style="max-width: 100%;"></a></p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer nofollow" href="https://camo.githubusercontent.com/7c8a5b5afae9babf135822abfe83f33f37d35d3b6b0b62b6a62834a0086a0ccb/687474703a2f2f692e737461636b2e696d6775722e636f6d2f64346e56622e6a7067"><img src="https://camo.githubusercontent.com/7c8a5b5afae9babf135822abfe83f33f37d35d3b6b0b62b6a62834a0086a0ccb/687474703a2f2f692e737461636b2e696d6775722e636f6d2f64346e56622e6a7067" alt="手眼校准的两种常见解决方案" data-canonical-src="http://i.stack.imgur.com/d4nVb.jpg" style="max-width: 100%;"></a></p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer nofollow" href="https://camo.githubusercontent.com/dfb2bc02828f42f4a15bcf4a812f4bcafbb5acee0ea2280d9c16d3f921d2aeb7/687474703a2f2f692e737461636b2e696d6775722e636f6d2f77644f79672e6a7067"><img src="https://camo.githubusercontent.com/dfb2bc02828f42f4a15bcf4a812f4bcafbb5acee0ea2280d9c16d3f921d2aeb7/687474703a2f2f692e737461636b2e696d6775722e636f6d2f77644f79672e6a7067" alt="AX=XB 手眼校准液" data-canonical-src="http://i.stack.imgur.com/wdOyg.jpg" style="max-width: 100%;"></a></p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer nofollow" href="https://camo.githubusercontent.com/b1221e21fb04c23314d762d1a41b2e55de55f5610dad3559d88975769e50c797/687474703a2f2f692e737461636b2e696d6775722e636f6d2f7a525131692e6a7067"><img src="https://camo.githubusercontent.com/b1221e21fb04c23314d762d1a41b2e55de55f5610dad3559d88975769e50c797/687474703a2f2f692e737461636b2e696d6775722e636f6d2f7a525131692e6a7067" alt="AX=ZB 手眼校准液" data-canonical-src="http://i.stack.imgur.com/zRQ1i.jpg" style="max-width: 100%;"></a></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将数据输入 CamOdoCal</font></font></h2><a id="user-content-feeding-data-into-camodocal" class="anchor" aria-label="永久链接：将数据输入 CamOdoCal" href="#feeding-data-into-camodocal"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ol dir="auto">
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在不同时间、位置和方向进行的每次测量都会缩小可以表示未知 X 的可能变换范围</font></font></p>
</li>
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">记录在不同时间步之间或相对于第一个时间步进行的许多变换 A 和 B 的列表</font></font></p>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">旋转采用 AxisAngle = UnitAxis*Angle 格式，或 [x_axis,y_axis,z_axis]*𝜃_angle
</font></font><ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">||单位轴||=1</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">|| </font><font style="vertical-align: inherit;">轴角|| </font><font style="vertical-align: inherit;">= 𝜃_角度</font></font></li>
</ul>
</li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">翻译采用正常的 [x,y,z] 格式</font></font></li>
</ul>
</li>
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将两个向量传递给 EstimateHandEyeScrew()</font></font></p>
</li>
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以 4x4 变换估计的形式返回 X</font></font></p>
</li>
</ol>
<p dir="auto"><a target="_blank" rel="noopener noreferrer nofollow" href="https://camo.githubusercontent.com/1797a89a61cf79002011bd05219a5717951b410e04e69de0c77945e7ef1b32f5/687474703a2f2f692e737461636b2e696d6775722e636f6d2f43766337352e6a7067"><img src="https://camo.githubusercontent.com/1797a89a61cf79002011bd05219a5717951b410e04e69de0c77945e7ef1b32f5/687474703a2f2f692e737461636b2e696d6775722e636f6d2f43766337352e6a7067" alt="Camodocal手眼校准细节" data-canonical-src="http://i.stack.imgur.com/Cvc75.jpg" style="max-width: 100%;"></a></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">当与机器人手臂一起使用时，将其移动到各种姿势和方向，确保落后的任何数据源稳定下来，然后记录机器人底座和机器人尖端之间以及眼睛/眼睛之间的每对姿势相机底座及其正在查看的标记、基准点或 AR 标签。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这将保存一个包含结果的 yaml 文件。</font><font style="vertical-align: inherit;">请务必使用旋转矩阵、双四元数或四元数+平移格式的数据将结果加载到系统中。</font><font style="vertical-align: inherit;">横滚、俯仰、偏航可能会退化并且常常不准确！</font></font></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">安装</font></font></h2><a id="user-content-installation" class="anchor" aria-label="永久链接：安装" href="#installation"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Linux</font></font></h3><a id="user-content-linux" class="anchor" aria-label="永久链接：Linux" href="#linux"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">所有依赖项都可以通过</font><font style="vertical-align: inherit;">或上的</font></font><a href="https://github.com/ahundt/robotics_setup"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">robots_setup</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">存储库中的脚本进行安装</font><font style="vertical-align: inherit;">。</font></font><code>Ubuntu 14.04</code><font style="vertical-align: inherit;"></font><code>Ubuntu 16.04</code><font style="vertical-align: inherit;"></font></p>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">苹果系统</font></font></h3><a id="user-content-macos" class="anchor" aria-label="永久链接：MacOS" href="#macos"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在 OS X 上，您可以使用</font></font><a href="http://brew.sh" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">homebrew</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><a href="https://github.com/ahundt/homebrew-robotics"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">homebrew-robotics</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> tap 来安装所有依赖项。</font></font></p>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">ROS（Linux + MacOS）</font></font></h3><a id="user-content-ros-both-linux--macos" class="anchor" aria-label="永久链接：ROS（Linux + MacOS）" href="#ros-both-linux--macos"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">完成 Linux 或 MacOS 步骤后，请按照正常的 ros 源包安装过程进行 catkin 构建。</font></font></p>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">依赖关系</font></font></h3><a id="user-content-dependencies" class="anchor" aria-label="永久链接：依赖关系" href="#dependencies"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果手动安装，请务必遵循每个库的说明，因为根据您的操作系统，需要执行特定的步骤。</font></font></p>
<ul dir="auto">
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/ros.org"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">ROS 靛蓝或动力</font></font></a></li>
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/opencv.org"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">OpenCV 2 或 3</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以及（推荐的）非自由组件
</font></font><ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">注意 handeye_calib_camodocal 不会调用任何非自由组件，但有些用户在配置 CMake 时遇到困难，无法在没有它们的情况下编译和安装所有其他依赖项。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">OpenCV3 将其非自由组件放在</font></font><a href="https://github.com/opencv/opencv_contrib"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">opencv-contrib</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">中。</font></font></li>
</ul>
</li>
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/eigen.tuxfamily.org"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">本征3</font></font></a></li>
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/ceres-solver.org"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">ceres 解算器</font></font></a></li>
<li><a href="https://github.com/google/glog"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">格洛格</font></font></a>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您遇到有关 的错误</font></font><code>providing "FindGlog.cmake" in CMAKE_MODULE_PATH</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，请尝试从源代码安装 glog。</font></font></li>
</ul>
</li>
<li><a href="https://github.com/gflags/gflags"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">全局标志</font></font></a></li>
</ul>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">例子</font></font></h2><a id="user-content-examples" class="anchor" aria-label="永久链接：示例" href="#examples"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">该文件夹中有预先记录的示例转换</font></font><code>example</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，默认情况下所有配置文件都应位于</font></font><code>handeye_calib_camodocal/launch</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">该文件夹中，但如果这不起作用，请尝试检查该</font></font><code>~/.ros/</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">文件夹。</font></font></p>
<ul dir="auto">
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/launch/handeye_example.launch"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">启动/handeye_example.launch</font></font></a>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这配置了加载和保存的文件转换，以及读取实时数据时的 rostopic。</font></font></li>
</ul>
</li>
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/example/TransformPairsOutput.yml"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">示例/TransformPairsOutput.yml</font></font></a>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这包含您使用启动脚本记录的一组变换，这些变换将输入到解算器中。</font></font></li>
</ul>
</li>
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/example/CalibratedTransform.yml"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">示例/CalibrateTransform.yml</font></font></a>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">该变换是求解器找到的最终结果。</font></font></li>
</ul>
</li>
</ul>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">要验证软件是否正常运行：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>roslaunch handeye_calib_camodocal handeye_example.launch
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="roslaunch handeye_calib_camodocal handeye_example.launch" tabindex="0" role="button">
    
</svg>
      
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">您应该看到如下所示的输出：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code># INFO: Before refinement: H_12 =
  -0.962926   -0.156063     0.22004 -0.00802514
  -0.176531    0.981315  -0.0765322   0.0242905
  -0.203985   -0.112539   -0.972484   0.0550756
          0           0           0           1
Ceres Solver Report: Iterations: 89, Initial cost: 1.367791e+01, Final cost: 6.005694e-04, Termination: CONVERGENCE
# INFO: After refinement: H_12 =
  -0.980558    0.184959   0.0655414  0.00771561
  0.0495028  -0.0900424    0.994707   0.0836796
   0.189881    0.978613   0.0791359 -0.00867321
          0           0           0           1
Result from /ee_link to /ar_marker_0:
  -0.980558    0.184959   0.0655414  0.00771561
  0.0495028  -0.0900424    0.994707   0.0836796
   0.189881    0.978613   0.0791359 -0.00867321
          0           0           0           1
Translation (x,y,z) :  0.00771561   0.0836796 -0.00867321
Rotation (w,x,y,z): -0.046193, 0.0871038, 0.672938, 0.733099

Result from /ar_marker_0 to /ee_link:
  -0.980558    0.184959   0.0655414  0.00771561
  0.0495028  -0.0900424    0.994707   0.0836796
   0.189881    0.978613   0.0791359 -0.00867321
          0           0           0           1
Inverted translation (x,y,z) : 0.00507012 0.0145954 -0.083056
Inverted rotation (w,x,y,z): -0.046193, 0.0871038, 0.672938, 0.733099
0.046193 0.0871038 0.672938 0.733099

Writing calibration to "/home/cpaxton/catkin_ws/src/handeye_calib_camodocal/launch/CalibratedTransform.yml"...
[handeye_calib_camodocal-1] process has finished cleanly
log file: /home/cpaxton/.ros/log/a829db0a-f96b-11e6-b1dd-fc4dd43dd90b/handeye_calib_camodocal-1*.log
all processes on machine have died, roslaunch will exit
shutting down processing monitor...
... shutting down processing monitor complete
done
</code></pre><div class="zeroclipboard-container">
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">完整的终端会话可以在以下位置找到：</font></font></p>
<ul dir="auto">
<li><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/example/terminal_session.txt"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">示例/terminal_session.txt</font></font></a></li>
</ul>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">记录您自己的变换</font></font></h2><a id="user-content-recording-your-own-transforms" class="anchor" aria-label="永久链接：记录您自己的变换" href="#recording-your-own-transforms"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">要记录您自己的会话，请修改</font></font><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/launch/handeye_file.launch"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">launch/handeye_file.launch</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以指定将发布您希望校准的姿势的 ROS 主题，然后运行：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>roslaunch handeye_calib_camodocal handeye_file.launch
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="roslaunch handeye_calib_camodocal handeye_file.launch" tabindex="0" role="button">
     
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您遇到困难，我们将在下面的故障排除部分介绍我们遇到的几乎所有问题。</font><font style="vertical-align: inherit;">它还可以帮助您查看</font></font><a href="http://robotics.stackexchange.com/questions/7163/hand-eye-calibration" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">解释手眼校准如何工作的堆栈交换问题</font></font></a></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">跑步后，请务必备份</font></font><code>TransformPairsInput.yml</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，</font></font><code>CalibratedTransform.yml</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这样您就不会丢失保存的所有变换和位置！</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如何在相机和它看到的物体之间进行变换？</font></font></h4><a id="user-content-how-do-i-get-transforms-between-the-camera-and-an-object-it-sees" class="anchor" aria-label="永久链接：如何在相机和它看到的物体之间进行变换？" href="#how-do-i-get-transforms-between-the-camera-and-an-object-it-sees"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果相机经过校准，则可以估计从相机到已知尺寸的印刷图案的变换。</font><font style="vertical-align: inherit;">我不建议使用棋盘进行手眼校准，因为图案不明确。</font><font style="vertical-align: inherit;">使用类似的东西：</font></font></p>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">艺术工具包.org</font></font></li>
<li><a href="https://github.com/ros-perception/ar_track_alvar"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">https://github.com/ros-perception/ar_track_alvar</font></font></a></li>
</ul>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">它们提供了有关如何设置相机和创建可用于生成变换的图案的说明。</font></font></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">故障排除</font></font></h2><a id="user-content-troubleshooting" class="anchor" aria-label="永久链接：故障排除" href="#troubleshooting"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">保存的文件未加载？</font></font></h4><a id="user-content-saved-files-not-loading" class="anchor" aria-label="永久链接：保存的文件未加载？" href="#saved-files-not-loading"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您无法找到保存的文件，ROS应用程序的默认工作目录就在该</font></font><code>~/.ros/</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">文件夹中，因此请尝试在那里查找。</font><font style="vertical-align: inherit;">请务必检查您的启动文件，通常为
</font></font><a href="/jhu-lcsr/handeye_calib_camodocal/blob/master/launch/handeye_file.launch"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">launch/handeye_file.launch，</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这决定是否从正在运行的机器人或保存的文件加载变换，以及保存文件的放置位置。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">收集足够的数据</font></font></h4><a id="user-content-collecting-enough-data" class="anchor" aria-label="永久链接：收集足够的数据" href="#collecting-enough-data"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们建议您至少收集约 36 个准确的变换，以实现良好的校准。</font><font style="vertical-align: inherit;">如果它无法收敛（即您没有得到好的结果），那么您的变换可能以错误的方式翻转，或者数据中存在太多噪声，无法找到足够准确的校准。</font></font></p>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">消除传感器噪声</font></font></h3><a id="user-content-eliminating-sensor-noise" class="anchor" aria-label="永久链接：消除传感器噪声" href="#eliminating-sensor-noise"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">帮助处理此问题的一种简单方法是创建一个新节点来读取您想要读取的数据并保存姿势的滚动平均值。</font><font style="vertical-align: inherit;">这有助于稳定结果。</font><font style="vertical-align: inherit;">有更好的方法，例如卡尔曼滤波器，可以更好地处理这个问题。</font><font style="vertical-align: inherit;">如果您采用滚动平均值，请确保每次获取数据时，在采用滚动平均值的整个持续时间内将机器人设置在单个位置，因为此处的任何错误都会导致结果不正确。</font></font></p>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">获取数据时“噪音太大”的示例</font></font></h3><a id="user-content-examples-of-too-much-noise-when-taking-data" class="anchor" aria-label="永久链接：获取数据时“噪音太大”的示例" href="#examples-of-too-much-noise-when-taking-data"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果噪音太大，您可能会看到以下错误：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>normalization could not be handled. Your rotations and translations are probably either not aligned or not passed in properly
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="normalization could not be handled. Your rotations and translations are probably either not aligned or not passed in properly" tabindex="0" role="button">
     
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这意味着您正在读取的数据可能存在太多变化，无法获得准确的解决方案。</font><font style="vertical-align: inherit;">例如，如果您观察 AR 标签的姿势，它会稍微晃动或翻转，这将导致无法找到准确的解决方案。</font><font style="vertical-align: inherit;">实现此目的的一种方法是确保系统完全静止，然后在多个帧中插入（平均）姿势，再次确保系统在记录帧之前完全静止，然后最终移动到下一个位置并重复该过程。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">您的相机必须经过校准</font></font></h4><a id="user-content-your-cameras-must-be-calibrated" class="anchor" aria-label="永久链接：您的相机必须经过校准" href="#your-cameras-must-be-calibrated"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">相机校准非常重要！</font><font style="vertical-align: inherit;">如果它们没有经过校准，那么输入到算法中的姿势将不准确，不会对应，因此算法将无法找到合适的近似解，并且只会退出并打印错误。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">您的机器人和相机必须牢固固定</font></font></h4><a id="user-content-your-robot-and-cameras-must-be-rigidly-fixed" class="anchor" aria-label="永久链接：您的机器人和相机必须牢固固定" href="#your-robot-and-cameras-must-be-rigidly-fixed"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">手眼校准求解刚体变换，因此如果整个系统没有严格固定，您要求解的变换就会不断变化，因此无法准确找到。</font><font style="vertical-align: inherit;">例如，如果您有摄像头和固定的机器人底座，请检查您的机器人是否已牢固地固定在表面上。</font><font style="vertical-align: inherit;">拧紧这些螺栓！</font><font style="vertical-align: inherit;">还要确保相机以类似的方式牢固地固定到位。</font><font style="vertical-align: inherit;">检查是否有任何晃动，并确保在获取数据点之前等待一切都静止。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">通过物理测量进行健全性检查</font></font></h4><a id="user-content-sanity-check-by-physically-measuring" class="anchor" aria-label="永久链接：通过物理测量进行健全性检查" href="#sanity-check-by-physically-measuring"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">当您握住手臂时，如果手臂轻微移动，时间戳的轻微扭曲或变化仍然可能导致其脱落。</font><font style="vertical-align: inherit;">另一种测试方法是让手臂移动到两个遥远的位置，并且假设您可以保持方向不变，棋盘姿势的变化长度应等于末端执行器尖端姿势的变化长度。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">通过模拟进行健全性检查</font></font></h4><a id="user-content-sanity-check-via-simulation" class="anchor" aria-label="永久链接：通过模拟进行健全性检查" href="#sanity-check-via-simulation"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您担心这是算法中的错误，您可以使用 v-rep 或gazebo（os + v-rep python 脚本位于存储库中）模拟运行它来验证它的工作原理，因为这将避免所有物理测量问题。</font><font style="vertical-align: inherit;">从那里您可以考虑获取更多真实数据并合并真实数据以缩小问题根源的范围。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">完整性检查转换以及从文件加载时</font></font></h4><a id="user-content-sanity-check-transforms-and-when-loading-from-files" class="anchor" aria-label="永久链接：健全性检查转换以及从文件加载时" href="#sanity-check-transforms-and-when-loading-from-files"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您从手动修改的文件加载，请检查您的矩阵是否转置、反转，或者在非常不寻常的情况下，甚至可能只转置 4x4 旋转矩阵的 3x3 旋转分量。</font></font></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">输出示例</font></font></h2><a id="user-content-example-output" class="anchor" aria-label="固定链接：示例输出" href="#example-output"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以下是成功执行运行时您应该期望的输出示例：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>Writing pairs to "/home/cpaxton/catkin_ws/src/handeye_calib_camodocal/launch/TransformPairsInput.yml"...
q[ INFO] [1473813682.393291696]: Calculating Calibration...
# INFO: Before refinement: H_12 =
-0.00160534     0.99916   0.0409473 -0.00813108
-0.00487176  -0.0409546    0.999149     0.10692
  0.999987  0.00140449  0.00493341   0.0155885
         0           0           0           1
Ceres Solver Report: Iterations: 99, Initial cost: 1.882582e-05, Final cost: 1.607494e-05, Termination: CONVERGENCE
# INFO: After refinement: H_12 =
-0.00282176     0.999009    0.0444162  -0.00746998
  0.0121142   -0.0443789     0.998941     0.101617
   0.999923   0.00335684    -0.011977 -0.000671928
          0            0            0            1
Result:
-0.00282176     0.999009    0.0444162  -0.00746998
  0.0121142   -0.0443789     0.998941     0.101617
   0.999923   0.00335684    -0.011977 -0.000671928
          0            0            0            1
Translation:  -0.00746998     0.101617 -0.000671928
Rotation: -0.48498 0.513209 0.492549 0.513209
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="Writing pairs to &quot;/home/cpaxton/catkin_ws/src/handeye_calib_camodocal/launch/TransformPairsInput.yml&quot;...
q[ INFO] [1473813682.393291696]: Calculating Calibration...
# INFO: Before refinement: H_12 =
-0.00160534     0.99916   0.0409473 -0.00813108
-0.00487176  -0.0409546    0.999149     0.10692
  0.999987  0.00140449  0.00493341   0.0155885
         0           0           0           1
Ceres Solver Report: Iterations: 99, Initial cost: 1.882582e-05, Final cost: 1.607494e-05, Termination: CONVERGENCE
# INFO: After refinement: H_12 =
-0.00282176     0.999009    0.0444162  -0.00746998
  0.0121142   -0.0443789     0.998941     0.101617
   0.999923   0.00335684    -0.011977 -0.000671928
          0            0            0            1
Result:
-0.00282176     0.999009    0.0444162  -0.00746998
  0.0121142   -0.0443789     0.998941     0.101617
   0.999923   0.00335684    -0.011977 -0.000671928
          0            0            0            1
Translation:  -0.00746998     0.101617 -0.000671928
Rotation: -0.48498 0.513209 0.492549 0.513209" tabindex="0" role="button">
     
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">请注意，这次运行并不是完美的，在 1 m 的运动中存在 5 mm 的误差。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">成本</font></font></h4><a id="user-content-cost" class="anchor" aria-label="永久链接：成本" href="#cost"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">一个关键信息是成本函数的输出，它是一种表示解决方案精度估计的度量：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>Initial cost: 1.882582e-05, Final cost: 1.607494e-05
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="Initial cost: 1.882582e-05, Final cost: 1.607494e-05" tabindex="0" role="button">
    
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果运行非常好，校准已经完全结束，最终成本应该约为 1e-13 或 1e-14。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">结果</font></font></h4><a id="user-content-results" class="anchor" aria-label="永久链接：结果" href="#results"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">现在让我们看一下结果：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>Translation:  -0.00746998     0.101617 -0.000671928
Rotation: -0.48498 0.513209 0.492549 0.513209
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="Translation:  -0.00746998     0.101617 -0.000671928
Rotation: -0.48498 0.513209 0.492549 0.513209" tabindex="0" role="button">
    
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">平移采用 xyz 格式，旋转采用四元数格式。</font><font style="vertical-align: inherit;">值得注意的是，该工具和 camodocal 使用特征四元数格式，该格式对存储在四元数 wxyz 中的四个值进行排序。</font><font style="vertical-align: inherit;">ROS启动文件，相比之下以xyzw的顺序存储数据。</font><font style="vertical-align: inherit;">这意味着将结果复制到 ROS 时，必须将旋转的第一个条目移动到末尾。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以下是上面所有 7 个数字正确放入 ros 启动文件的示例：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>&lt;node pkg="tf" type="static_transform_publisher" name="endpoint_to_marker" args=" -0.00746998     0.101617 -0.000671928  0.513209 0.492549 0.513209  -0.48498   $(arg ee_frame) /endpoint_marker 10"/&gt;
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="<node pkg=&quot;tf&quot; type=&quot;static_transform_publisher&quot; name=&quot;endpoint_to_marker&quot; args=&quot; -0.00746998     0.101617 -0.000671928  0.513209 0.492549 0.513209  -0.48498   $(arg ee_frame) /endpoint_marker 10&quot;/>" tabindex="0" role="button">
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">问题？</font><font style="vertical-align: inherit;">这是我们需要知道的。</font></font></h2><a id="user-content-questions-here-is-what-we-need-to-know" class="anchor" aria-label="永久链接： 有问题吗？ 这是我们需要知道的。" href="#questions-here-is-what-we-need-to-know"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您尝试运行此程序并有疑问，请创建您的用例图表，以便我们了解您如何设置方程，然后创建一个</font></font><a href="https://github.com/jhu-lcsr/handeye_calib_camodocal/issues"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">github 问题</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">请参阅此</font></font><a href="http://robotics.stackexchange.com/questions/7163/hand-eye-calibration" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">堆栈交换问题，解释手眼校准如何工作，</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以获得此类图表的示例。</font></font></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">作者</font></font></h2><a id="user-content-authors" class="anchor" aria-label="永久链接：作者" href="#authors"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">安德鲁·洪特</font></font><a href="mailto:ATHundt@gmail.com"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">ATHundt@gmail.com</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">
菲利克斯·乔纳森</font></font><a href="mailto:fjonath1@jhu.edu"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">fjonath1@jhu.edu</font></font></a></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">致谢</font></font></h2><a id="user-content-acknowledgements" class="anchor" aria-label="永久链接：致谢" href="#acknowledgements"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><a href="https://www.informatik.uni-kiel.de/inf/Sommer/doc/Publications/kd/ijrr99.pdf" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用双四元数进行手眼校准</font></font></a></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>@article{daniilidis1999hand,
  title={Hand-eye calibration using dual quaternions},
  author={Daniilidis, Konstantinos},
  journal={The International Journal of Robotics Research},
  volume={18},
  number={3},
  pages={286--298},
  year={1999},
  publisher={SAGE Publications}
}
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="@article{daniilidis1999hand,
  title={Hand-eye calibration using dual quaternions},
  author={Daniilidis, Konstantinos},
  journal={The International Journal of Robotics Research},
  volume={18},
  number={3},
  pages={286--298},
  year={1999},
  publisher={SAGE Publications}
}" tabindex="0" role="button">
    
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><a href="https://github.com/hengli/camodocal"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">卡奥多卡尔</font></font></a></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>Lionel Heng, Bo Li, and Marc Pollefeys,
CamOdoCal: Automatic Intrinsic and Extrinsic Calibration of a Rig with Multiple Generic Cameras and Odometry,
In Proc. IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), 2013.

Lionel Heng, Mathias Bürki, Gim Hee Lee, Paul Furgale, Roland Siegwart, and Marc Pollefeys,
Infrastructure-Based Calibration of a Multi-Camera Rig,
In Proc. IEEE International Conference on Robotics and Automation (ICRA), 2014.

Lionel Heng, Paul Furgale, and Marc Pollefeys,
Leveraging Image-based Localization for Infrastructure-based Calibration of a Multi-camera Rig,
Journal of Field Robotics (JFR), 2015.
</code></pre><div class="zeroclipboard-container">
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">参考</font></font></h2><a id="user-content-references" class="anchor" aria-label="永久链接：参考文献" href="#references"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Strobl, K. 和 Hirzinger, G. (2006)。</font><font style="vertical-align: inherit;">最佳手眼校准。</font><font style="vertical-align: inherit;">2006 年 IEEE/RSJ 智能机器人和系统国际会议（第 4647-4653 页），2006 年 10 月。</font></font></li>
<li><a href="http://campar.in.tum.de/Chair/HandEyeCalibration" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">慕尼黑工业大学 (TUM) CAMP 实验室 wiki </font></font></a></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">K. Daniilidis，“使用双四元数进行手眼校准”，Int。</font><font style="vertical-align: inherit;">抢劫杂志。</font><font style="vertical-align: inherit;">研究，卷。</font><font style="vertical-align: inherit;">18、没有。</font><font style="vertical-align: inherit;">3，第 286-298 页，1999 年 6 月。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">E. Bayro–Corrochano、K. Daniilidis 和 G. Sommer，“3D 运动学的电机代数：手眼校准案例”，数学杂志。</font><font style="vertical-align: inherit;">成像与视觉，卷。</font><font style="vertical-align: inherit;">13、没有。</font><font style="vertical-align: inherit;">2，第 79-100 页，2000 年 10 月。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">F. Dornaika 和 R. Horaud，“同步机器人世界和手眼校准”，IEEE Trans。</font><font style="vertical-align: inherit;">论罗布斯。</font><font style="vertical-align: inherit;">和自动卷。</font><font style="vertical-align: inherit;">14、没有。</font><font style="vertical-align: inherit;">4，第 617-622 页，1998 年 8 月。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">注：图表和文本来自不同来源，包括演示作者、引用的各种论文以及 TUM wiki。</font></font></li>
</ul>
</article></div>
