---
title: "Matlab/PsychToolBox EyelinkPicture.m"
excerpt: "EyeLink_SimplePicture.m 是学习 Eyelink 的 PsychToolBox 编程的最好脚本。本文将着重介绍脚本中与 Eyelink 相关的关键函数。"
read_time: false
header:
  overlay_image: /assets/images/3rd-ptb-overview.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Matlab
  - PsychToolBox
toc: false
comments: true
author_profile: false
classes: wide
sidebar:
  title: "目录"
  nav: EL_3rd
---

---

此脚本来源于**本地示例文件夹**中的 `EyeLink_SimplePicture.m`，如果您已经安装 `2.2.1` 版本的  `Eyelink Development Kit`，您可以在 “C:\Users\Public\Documents\EyeLink/SampleExperiments/MATLAB Psychtoolbox/Psychtoolbox/PsychHardware/EyelinkToolbox/EyelinkDemos/SR-ResearchDemos/SimplePicture” 中找到它。
{: .notice--info}

您也可以通过[>>>点击此处(提取码：fjs3)<<<](https://pan.baidu.com/s/1Qw68CGlqE_jGTC5iwf0IrA)下载完整的 SampleExperiments 文件夹。
{: .notice--info}

2021.08.16 - 此版本相较于**目前**PTB内置的 Picture.m 更新，代码函数是互通的，用法略有区别。
{: .notice--info}

---

与 E-Prime 不同的是，Matlab 的代码通常是用一个脚本装下所有。

因此，在下文的代码中，将涵盖`与眼动仪建立、断开连接`、`基本参数设置`、`相机校准`、`Message标记`和`数据传输`的全部内容。Charlie 会尽量在注释中标记清楚。

另外，本文中将着重讲解与 Eyelink 有关的代码，Matlab 和 PsychToolBox 的基本函数部分略。

如果你是第一次接触眼动程序的话，在阅读代码之前，请移步[>>>Eyelink 第三方应用程序接口简述<<<](/eyelink/3rd-intro/)了解眼动实验程序的基本构造。{: .notice--info}

``` matlab
function EyeLink_SimplePicture(screenNumber)
% 这是一个简单的EyeLink集成示例程序，当图像出现在屏幕上时，同步记录眼球运动。
% 按下 空格键 或 老式Botton反应盒 结束试次。
%
% 运行方法:
% Eyelink_SimplePicture(screenNumber)
%
% screenNumber 是一个可选参数，用来给 Screen('OpenWindow', ...) 函数指定在哪个显示器上呈现刺激内容。如果需要在多显示器的电脑上制定刺激显示器的话，通常需要填写显示器的编号。
% 如果没有指定 screenNumber 或 screenNumber 为空，则执行默认设置:
% screenNumber = max(Screen('Screens'));
% screenNumber 将设定为显示器ID的最大值。

% 如果已经打开命令行窗口，则将其置于显示的最前面。
if ~IsOctave; commandwindow; end;

% 如果未指定 screenNumber，则将其设定为显示器ID的最大值。
if (nargin < 1)
    screenNumber = [];
end

try
    %% STEP 1: 初始化 EYELINK 连接；打开EDF文件；获取主试机系统版本
    
    % 初始化 EyeLink 链接 (dummymode = 0) 
    % 或
    % 进入 "Dummy Mode"，不连接眼动仪调试程序 (dummymode = 1);
    dummymode = 0;
    
    % 可选操作：将 Eyelink 主试机 IP 设置为非默认值。
    % 一般情况下不会用到此选项，除非您需要将整套眼动仪设备连接进其他现有局域网中。
    % 如果涉及到相关设计，建议咨询 Eyelink 售后支持
    % 如果需要连接到一个非默认 IP 的主试机上时，请在初始化 Eyelink 链接之前执行下面的命令。
    % Eyelink('SetAddress', '10.10.10.240');
    
    EyelinkInit(dummymode); % 初始化 EyeLink 连接
    status = Eyelink('IsConnected');
    if status < 1 % 如果 EyeLink 没有成功连接
        dummymode = 1; 
    end
       
    % 开启一个对话框，在对话框中输入 Eyelink 数据文件名称。
    % 文件名最多支持 8 位字符，仅支持英文字母、数字和下划线的组合，且英文字母不区分大小写。
    prompt = {'Enter EDF file name (up to 8 characters)'};
    dlg_title = 'Create EDF file';
    def = {'demo'}; % 创建一个默认的 EDF 文件名
    answer = inputdlg(prompt, dlg_title, 1, def); % 提示输入自定义的 EDF 文件名    
    % 如果没有输入任何文件名，则在 Matlab 的命令行窗口中显示一些提示。
    if  isempty(answer)
        fprintf('Session cancelled by user\n')
        error('Session cancelled by user'); % 中断实验 (详情请查看 cleanup 函数)
    end    
    edfFile = answer{1}; % 将文件名保存为变量    
    % 如果文件名长度超过了8位，则在 Matlab 的命令行窗口中显示一些提示。
    if length(edfFile) > 8
        fprintf('Filename needs to be no more than 8 characters long (letters, numbers and underscores only)\n');
        error('Filename needs to be no more than 8 characters long (letters, numbers and underscores only)');
    end
 
    % 创建一个 EDF 文件，并命名。
    % 此创建文件操作是在主试机上创建 EDF 文件，所有的数据都将暂存于主试机。
    % 在实验结束后，需要通过代码将其下载到被试机上
    failOpen = Eyelink('OpenFile', edfFile);
    if failOpen ~= 0 % 如果没有成功创建文件，则中断实验。
        fprintf('Cannot create EDF file %s', edfFile); % 在 Matlab 的命令行窗口中显示一些提示。
        error('Cannot create EDF file %s', edfFile); % 在 Matlab 的命令行窗口中显示一些提示。
    end
    
    % 获取 EyeLink 眼动仪和主试机版本 
    % <ver> 如果没有连接到眼动仪，则返回 0 
    % <versionstring> 返回 'EYELINK I', 'EYELINK II x.xx', 'EYELINK CL x.xx' 
    % 'x.xx' 是主试机系统版本
    ELsoftwareVersion = 0; % dummy mode 下默认的眼动仪版本
    [ver, versionstring] = Eyelink('GetTrackerVersion');
    if dummymode == 0 % 如果已经连接上眼动仪
        % 获取主试机版本号
        % 获取小数点前的EL版本
        [r1 vnumcell] = regexp(versionstring,'.*?(\d)\.\d*?','Match','Tokens');
        % EyeLink I 返回 1，EyeLink II 返回 2，EyeLink 1K 返回 3 或 4，EyeLink 1KPlus 返回 5，Portable Duo 返回 6
        ELsoftwareVersion = str2double(vnumcell{1}{1}); 
        % 在 Matlab 的命令行窗口中显示一些提示。
        fprintf('Running experiment on %s version %d\n', versionstring, ver );
    end
    % 可选操作：向 EDF 文件中添加一条文字记录，标示当前的实验名和被试信息。
    % 如果你的标记文本是以 "RECORDED BY " 开始的，则您可以在 DataViewer 的 Inspector 的窗口中点击对应的数据条目来查看 "RECORDED BY" 属性以查看标记的信息
    preambleText = sprintf('RECORDED BY Psychtoolbox demo %s session name: %s', mfilename, edfFile);
    Eyelink('Command', 'add_file_preamble_text "%s"', preambleText);
    
    
    %% STEP 2: 选择采集 SAMPLE 或 EVENT 数据
    % 详情请查看 EyeLinkProgrammers Guide manual > Useful EyeLink Commands > File Data Control & Link Data Control
    
    % 设置在 EDF 中保存哪些眼动事件
    % 以防万一的做法是将所有都保存下来
    Eyelink('Command', 'file_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK,MESSAGE,BUTTON,INPUT');
    % 设置哪些眼动事件可以在在线传递，这对于需要获取眼动数据更新刺激内容的实验是必须的
    % 以防万一的做法是将所有事件都设置为在线传递
    Eyelink('Command', 'link_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK,BUTTON,FIXUPDATE,INPUT');
    % 设置在 EDF 中保存哪些 Sample 数据
    % 以防万一的做法是将所有都保存下来
    if ELsoftwareVersion > 3  % 检查眼动仪版本，如果版本大于 3，则在 Remote Mode 下时保存头部追踪贴纸 Target 相关的 Sample 数据
        Eyelink('Command', 'file_sample_data  = LEFT,RIGHT,GAZE,HREF,RAW,AREA,HTARGET,GAZERES,BUTTON,STATUS,INPUT');
        Eyelink('Command', 'link_sample_data  = LEFT,RIGHT,GAZE,GAZERES,AREA,HTARGET,STATUS,INPUT');
    else
        Eyelink('Command', 'file_sample_data  = LEFT,RIGHT,GAZE,HREF,RAW,AREA,GAZERES,BUTTON,STATUS,INPUT');
        Eyelink('Command', 'link_sample_data  = LEFT,RIGHT,GAZE,GAZERES,AREA,STATUS,INPUT');
    end
    
    %% STEP 3: 开启图形窗口
    
    % 在指定的显示器上开启图形窗口
    if isempty(screenNumber)
        screenNumber = max(Screen('Screens')); % 未指定显示器 ID 的时候使用默认值
    end
    window = Screen('OpenWindow', screenNumber, [128 128 128]); % 开启图形窗口
    Screen('Flip', window);
    % 返回窗口或显示器的宽高尺寸，单位为像素。
    [width, height] = Screen('WindowSize', window);
    
    
    %% STEP 4: 设置校准环节的颜色和声音；向 Eyelink 主试机和 DataViewer 提供窗口的像素尺寸；设置校准参数；校准
    
    % 以默认参数创建 Eyelink 连接，返回一个名为 el 的结构体
    el = EyelinkInitDefaults(window);
    % 设置 calibration/validation/drift-check(or drift-correct) 过程中校准点的尺寸和颜色
    % 重要的是，上述过程中的背景颜色要在亮度上和刺激屏保持基本一致。
    % 过大的亮度变化将会因通过对光反射缩放而导致注视点漂移（数据误差变大）。
    el.calibrationtargetsize = 3;% 校准点外圈尺寸占屏幕总尺寸的百分比
    el.calibrationtargetwidth = 0.7;% 校准点内圈尺寸占屏幕总尺寸的百分比
    el.backgroundcolour = [128 128 128];% RGB 灰
    el.calibrationtargetcolour = [0 0 0];% RGB 黑
    % 设置 "Camera Setup" 说明文本的颜色不同于校准屏的背景色
    el.msgfontcolour = [0 0 0];% RGB 黑
        
    % 使用图片代替默认校准点 
    % 注释或删除下面两行可以使用默认校准点
    el.calTargetType = 'image';
    el.calImageTargetFilename = [pwd '/' 'fixTarget.jpg'];
    
    % 设置校准提示音 (0 = 关闭声音，1 = 开启声音)
    el.targetbeep = 1;  % 当校准点出现时，播放提示音
    el.feedbackbeep = 1;  % 当校准点通过时，播放提示音
    
    % 重要！
    % 通过下面的函数来更新对 el 结构体所进行的更改
    EyelinkUpdateDefaults(el);
    
    % 设置在主试机上显示的边界坐标，输入参数为被试机窗口的左、上、右、下边界坐标值。
    % 此项设置会影响主试机同步监控实验过程中的主试机屏幕显示。
    Eyelink('Command','screen_pixel_coords = %ld %ld %ld %ld', 0, 0, width-1, height-1);
    % 通过 DISPLAY_COORDS message 向 EDF 文件中写入屏幕的分辨率
    % 此项设置会影响 EDF 数据文件中的分辨率记录。
    % 详情请查看 DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Pre-trial Message Commands
    Eyelink('Message', 'DISPLAY_COORDS %ld %ld %ld %ld', 0, 0, width-1, height-1);    
    % 设置校准模式：H3、HV3、HV5、HV9 或 HV13
    Eyelink('Command', 'calibration_type = HV9'); % HV9 
    % 可选操作：设置通过反映手柄的按键 5 接受校准点
    Eyelink('Command', 'button_function 5 "accept_target_fixation"');
    % 隐藏鼠标
    HideCursor(screenNumber);
    % 开始监听键盘输入。防止按键输入到 Matlab 的窗口中。
    ListenChar(-1);
    Eyelink('Command', 'clear_screen 0'); % 清空主试机显示
    % 开始相机校准
    EyelinkDoTrackerSetup(el);
    
    
    %% STEP 5: TRIAL 循环
    
    spaceBar = KbName('space');% 为后续试次定义结束按键的按键码
    imgList = {'img1.bmp' 'img2.bmp'};% 提供两个试次的图片列表
    for i = 1:length(imgList)
        
        % STEP 5.1: 试次开始；在主试机上显示试次信息；在主试机上显示图片背景或绘制反馈图形；漂移矫正
        
        % 向 EDF 中写入 "TRIALID " message 以在 DataViewer 中标记试次的开始
        % 详情请见 DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Defining the Start and End of a Trial
        Eyelink('Message', 'TRIALID %d', i);
        % 向 EDF 中 写入 "!V CLEAR " message，清空 DataViewer 中的背景显示或在背景上进行一些显示绘制
        % 详情请见 DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Simple Drawing
        Eyelink('Message', '!V CLEAR %d %d %d', el.backgroundcolour(1), el.backgroundcolour(2), el.backgroundcolour(3));
        % 在主试机显示器上创建一个提示文本以显示试次的编号信息
        Eyelink('Command', 'record_status_message "TRIAL %d/%d"', i, length(imgList));
        
        % 在 Eyelink 主试机上呈现刺激材料
        % 主试机上的刺激材料同步是需要用下面的代码手动设置的
        % 您可以查阅 COMMANDS.INI 来获取可用 Command 命令列表
               
        imgName = char(imgList(i)); % 获取当前试次的文件名
        imgInfo = imfinfo(imgName); % 获取图片信息
        Eyelink('SetOfflineMode');  % 开始记录和显示主试机背景前，先将主试机设置为 idle/offline 模式
        Eyelink('Command', 'clear_screen 0'); % 清空主试机显示
        % 可选操作：将一个 24-bit 或 32-bit bitmap 格式的图片发送到主试机上作为刺激背景
        % [status] = Eyelink('ImageTransfer', imagePath, xs, ys, width, height, xd, yd, options);
        % xs, ys: 原图片传输范围的左上位置坐标，传递完整图片时设置为 “0, 0”
        % width, height: 原图片传输范围的宽高，传递完整图片时设置为 “0, 0”
        % xd, yd: 传递图片在主试机上显示时的左上角坐标，注意与被试机图片保持位置相同
        % 此传输操作不会更改或压缩图片尺寸，如果图片尺寸与实际呈现尺寸不同，请在此操作前更改图片尺寸
        transferStatus = Eyelink('ImageTransfer', imgInfo.Filename, 0, 0, 0, 0, round(width/2-imgInfo.Width/2), round(height/2-imgInfo.Height/2));
        if dummymode == 0 && transferStatus ~= 0 % 如果已经连接到 Eyelink 但图片传输失败
            fprintf('Image transfer Failed\n'); % 在 Matlab 的命令行窗口中显示一些提示。
        end
        
        % 可选操作：在主试机上绘制线框以代替背景图片
        % 当一个试次内，在被试机上呈现位置或内容连续变化的刺激材料时，主试机无法同步更新刺激材料的显示
        % 但是我们可以通过绘制不同颜色的线框来标记处不同阶段刺激材料出现的位置，以此检测实验进程
        % "draw_line <x1> <y1> <x2> <y2> <color>"
        % "draw_box <x1> <y1> <x2> <y2> <color>"
        % "draw_filled_box <x1> <y1> <x2> <y2> <color>"
        % "draw_text <x1> <y1> <color> <text>"
        % "draw_cross <x> <y>"
        % <color>: 0-15
        % 详情请见 EyeLink Programmers Guide manual ->  section 25.7 'Drawing Commands'
        Eyelink('Command', 'draw_box %d %d %d %d 15', round(width/2-imgInfo.Width/2), round(height/2-imgInfo.Height/2), round(width/2+imgInfo.Width/2), round(height/2+imgInfo.Height/2));
        
        % 呈现 drift check/correction.
        % 可以自定义漂移矫正的校准点出现的位置，缺省则默认呈现在屏幕正中心
        EyelinkDoDriftCorrection(el, round(width/2), round(height/2));
        
        %STEP 5.2: 开始记录
        
        % 在开始记录前，建议使用 Eyelink('SetOfflineMode') 来使眼动仪进入 idel/offline 模式
        % 如果您使用了旧版的 Eyelink('Command', 'set_idle_mode') 命令，则需要在此之后设置等待 50ms 来让眼动仪完成准备（如下面两行所注释的那样）
        % Eyelink('Command', 'set_idle_mode');% 在开始记录前令眼动仪进入 idel/offline 模式
        % WaitSecs(0.05); % 状态过渡准备时间        
        Eyelink('SetOfflineMode');% 在开始记录前令眼动仪进入 idel/offline 模式
        Eyelink('StartRecording'); % 开始记录
        WaitSecs(0.1); % 刺激呈现前预记录一写眼动数据
        
        % STEP 5.3: 呈现刺激材料；创建 DataViewer 数据背景和兴趣区
        
        % 准备和呈现刺激材料                       
        Screen('FillRect', window, el.backgroundcolour);% 在后台缓存中创建一个灰色背景的刺激屏
        imgData = imread(imgName); % 从文件中读取图片
        imgTexture = Screen('MakeTexture',window, imgData); % 转换图像纹理
        Screen('DrawTexture', window, imgTexture); % 在后台缓存中绘制图像
        Screen('TextSize', window, 30); % 指定文本尺寸
        Screen('DrawText', window, 'Press space or button to end trial', 5, height-35, 0); % 在后台缓存中写入文本
        [~, RtStart] = Screen('Flip', window); % 将后台缓存呈现在屏幕上
        
        % 向 EDF 文件中写入 Message 以标记刺激呈现的开始
        Eyelink('Message', 'STIM_ONSET'); 
               
        % 向 EDF 文件写入 "!V IMGLOAD " message 来标记背景图片信息
        % "!V IMGLOAD FILL <relative_image_path>"
        % "!V IMGLOAD TOP_LEFT <relative_image_path> <x_position> <y_position> [width] [height]"
        % "!V IMGLOAD CENTER <relative_image_path> <x_position> <y_position> [width] [height]"
        % []内是可缺省参数
        % 详情请见 DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Image Commands
        Eyelink('Message', '!V IMGLOAD CENTER %s %d %d', imgName, width/2, height/2);  
              
        % 向 EDF 文件写入 "!V IAREA " message 来创建图片的兴趣区
        % "!V IAREA RECTANGLE <id> <left> <top> <right> <bottom> [label string]"
        % "!V IAREA ELLIPSE <id> <left> <top> <right> <bottom> [label string]"
        % "!V IAREA FREEHAND <id> <x1,y1> <x2 ,y2> ... <xn ,yn> [label string]"
        % []内是可缺省参数
        % 详情请见 DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Interest Area Commands
        Eyelink('Message', '!V IAREA RECTANGLE %d %d %d %d %d %s', 1, round(width/2-imgInfo.Width/2), round(height/2-imgInfo.Height/2), round(width/2+imgInfo.Width/2), round(height/2+imgInfo.Height/2),'IMAGE_IA');
        
        % STEP 5.4: 等待按键反馈；显示空屏；停止记录
        
        while 1 % 循环监听，直到运行错误、按下空格或反应盒按键
            % 检查眼动仪运行情况，是否还在记录。如果记录已经停止，即程序运行出错，则关闭数据文件并传递实验数据。
            err = Eyelink('CheckRecording');
            if(err ~= 0)
                fprintf('EyeLink Recording stopped!\n');
                % 将数据文件副本下载到被试机
                Eyelink('SetOfflineMode');% 令眼动仪进入 idel/offline 模式
                Eyelink('CloseFile'); % 关闭 EDF 文件
                Eyelink('Command', 'clear_screen 0'); % 在实验结束的时候清空主试机监控显示
                WaitSecs(0.1); % 等待屏幕完成清空显示
                % 将数据文件副本下载到被试机
                transferFile; % 详情请查看后面的 transferFile() 函数
                error('EyeLink is not in record mode when it should be. Unknown error. EDF transferred from Host PC to Display PC, please check its integrity.');
            end
            % 按空格键结束试次
            [~, RtEnd, keyCode] = KbCheck;
            if keyCode(spaceBar)
                % 向 EDF 文件中写入 message 来标记按键时间
                Eyelink('Message', 'KEY_PRESSED');
                reactionTime = round((RtEnd-RtStart)*1000); % 计算反应时，单位毫秒，取整
                break; % 退出循环
            end
            % 按反映手柄的 5 号按键结束试次
            % 按键编码 bitshift 计算公式：(button number * -1) + 1 
            % 例：5 号按键的 bitshift 编码为 -4
            buttonResult = Eyelink('ButtonStates');
            if buttonResult
                if bitshift(buttonResult, -4) == 1
                    % 向 EDF 文件中写入 message 来标记按键时间
                    Eyelink('Message', 'BUTTON_PRESSED');
                    reactionTime = round((GetSecs-RtStart)*1000); % 计算反应时，单位毫秒，取整
                    break; % 退出循环
                end
            end
        end % 循环结束
        
        % 在试次结束的时候呈现空屏
        Screen('FillRect', window, el.backgroundcolour); % 在后台缓存中准备一个灰色的空屏
        Screen('Flip', window); % 呈现空屏
        % 向 EDF 文件中写入 message 来标记空屏呈现的时间
        Eyelink('Message', 'BLANK_SCREEN');
        % 向 EDF 文件中写入 "!V CLEAR " message 来在 DataViewer 中标记空屏呈现
        % 详情请见：DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Simple Drawing
        Eyelink('Message', '!V CLEAR %d %d %d', el.backgroundcolour(1), el.backgroundcolour(2), el.backgroundcolour(3));
        
        % 在每个试次结束的时候停止记录眼动数据
        WaitSecs(0.1); % 结束记录前额外记录 100ms 数据
        Eyelink('StopRecording'); % 停止眼动仪记录
        
        % STEP 5.5: 为 DataViewer 记录试次变量；结束试次
        
        % 向 EDF 文件中写入 "!V TRIAL_VAR " messages 来在 DataViewer 中记录试次的变量
        % "!V TRIAL_VAR <variable_name> <variable_value>"
        % 详情请见 DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Trial Message Commands
        Eyelink('Message', '!V TRIAL_VAR iteration %d', i); % 试次重复次数
        Eyelink('Message', '!V TRIAL_VAR image %s', imgName); % 图片名称
        WaitSecs(0.001); % 在多条 Message 间保留一点时间间隔。同时写入过多 Message 可能会导致某些 Message 丢失
        Eyelink('Message', '!V TRIAL_VAR rt %d', reactionTime); % 反应时
        % 向 EDF 文件中写入 "TRIAL_RESULT" message 来在 DataViewer 中标记试次的结束
        % 详情请见 DataViewer manual section: Protocol for EyeLink Data to Viewer Integration > Defining the Start and End of a Trial
        Eyelink('Message', 'TRIAL_RESULT 0');
        WaitSecs(0.01); % 试次结束后等待一小会儿
        
        % 每个试次结束后清空 Screen() 
        Screen('Close', imgTexture);
    end % 结束试次循环
    
    
    %% STEP 6: 关闭 EDF 文件；下载 EDF 文件到被试机；断开 Eyelink 连接；完成实验
    
    % 在关闭数据文件前，建议使用 Eyelink('SetOfflineMode') 来使眼动仪进入 idel/offline 模式
    % 如果您使用了旧版的 Eyelink('Command', 'set_idle_mode') 命令，则需要在此之后设置等待 50ms 来让眼动仪完成准备（如下面两行所注释的那样）
    % Eyelink('Command', 'set_idle_mode');% 在开始记录前令眼动仪进入 idel/offline 模式
    % WaitSecs(0.05); % 状态过渡准备时间    
    Eyelink('SetOfflineMode'); % 眼动仪进入 idel/offline 模式
    Eyelink('Command', 'clear_screen 0'); % 在实验结束之前清空主试机的显示
    WaitSecs(0.5); % 在关闭和传输 EDF 文件之前等待一小会儿
    Eyelink('CloseFile'); % 在主试机上关闭 EDF 文件
    % 从主试机上将 EDF 文件的副本下载到被试机上
    transferFile; % 详情请查看后面的 transferFile() 函数
catch % 如果检测到同步失败
    % 在 Matlab 的命令行窗口中显示错误内容
    psychrethrow(psychlasterror);
end
cleanup;

% 前面脚本中使用的 cleanup() 函数
    function cleanup
        try
            Screen('CloseAll'); % 如果打开了任何窗口，则关闭他们
        end
        Eyelink('Shutdown'); % 断开 Eyelink 连接
        ListenChar(0); % 回复 Matlab 窗口的键盘监听
        ShowCursor; % 回复 Matlab 窗口的鼠标控制
        if ~IsOctave; commandwindow; end; % 将命令行窗口置于最前
    end

% 用于将 EDF 文件副本从主试机下载到被试机的函数
% 允许指定一个不同于当前实验文件夹的下载路径
    function transferFile
        try
            if dummymode ==0 % 如果连接到了 Eyelink
                % 显示 'Receiving data file...' 直到传输完成
                Screen('FillRect', window, el.backgroundcolour); % 在后台缓存中准备一个灰色背景的空屏
                Screen('DrawText', window, 'Receiving data file...', 5, height-35, 0); % 在后台缓存中的灰色空屏上准备文字
                Screen('Flip', window); % 显示缓存屏
                fprintf('Receiving data file ''%s.edf''\n', edfFile); % 在 Matlab 的命令行窗口中显示一些提示信息
                
                % 从主试机下载 EDF 文件
                % [status =] Eyelink('ReceiveFile',['src'], ['dest'], ['dest_is_path'])
                status = Eyelink('ReceiveFile');
                % 可选操作：您可以通过下面注释的代码来更改EDF保存时的名称
                % % <src> 缺省则默认传输最后新采集的 EDF 文件
                % % <dest> 缺省则将数据文件按照 EDF 文件的现有文件名保存
                % % 若 <dest_is_path> 不为 0，则按照 EDF 文件的现有文件名保存，但是保存到 <dest> 所指定的路径下
                % newName = ['Test_',char(datetime('now','TimeZone','local','Format','y_M_d_HH_mm')),'.edf'];                
                % status = Eyelink('ReceiveFile', [], newName, 0);
                
                % 检测 EDF 文件是否完成传输，并在 Matlab 的命令行窗口中显示文件大小
                if status > 0
                    fprintf('EDF file size: %.1f KB\n', status/1024); % 将文件大小转换为 KB
                end
                % 在 Matlab 的命令行窗口中显示 EDF 文件的保存路径
                fprintf('Data file ''%s.edf'' can be found in ''%s''\n', edfFile, pwd);
            else
                fprintf('No EDF file saved in Dummy mode\n');
            end
        catch % 如果传输错误，则在 Matlab 的命令行窗口中显示提示
            fprintf('Problem receiving data file ''%s''\n', edfFile);
            psychrethrow(psychlasterror);
        end
    end
end
```

---

以上。
