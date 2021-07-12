---
title: "Matlab/PsychToolBox EyelinkPicture.m"
excerpt: "EyelinkPicture.m 是学习 Eyelink 的 PsychToolBox 编程的最好脚本。本文将着重介绍脚本中与 Eyelink 相关的关键函数。"
read_time: false
header:
  overlay_image: /assets/images/3rd-ptb-overview.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Matlab
  - PsychToolBox
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EL_3rd
---

---

---

与 E-Prime 不同的是，Matlab 的代码通常是用一个脚本装下所有。

因此，在下文的代码中，将涵盖`与眼动仪建立、断开连接`、`基本参数设置`、`相机校准`、`Message标记`和`数据传输`的全部内容。Charlie 会尽量在注释中标记清楚。

如果你是第一次接触眼动程序的话，在阅读代码之前，请移步[>>>Eyelink 第三方应用程序接口简述<<<](/eyelink/3rd-intro/)了解眼动实验程序的基本构造。{: .notice--info}

``` matlab
function EyeLinkPicture

% 基于 Eyelink 和 Psychophysics Toolboxes 的简短示例
% 这就是在 BRMIC 中关于 EyelinkToolbox 的文章（Cornelissen, Peters and Palmer 2002）所提及的程序
% 截至目前几经更新，使用了一些新的函数。
% 
% 改编自 "Psychtoolbox\PsychHardware\EyelinkToolbox\EyelinkDemos\ShortDemos\EyelinkExample.m"
%
% 更新历史：
%
% mm/dd/yy
% 07/01/08 js 	 重写实验结构，并添加 DV Message 相关代码
% 07/14/08 js 	 增加自定义 EDF 文件名称的代码
% 07/13/10 fwc 	 适配新工具箱，在校准过程中得以显示眼睛的图像
%			   	增加 cleanup() 函数，重新启用 try-catch
% 09/20/12 srresearch 更新如下功能:
%                1. 发送刺激内容到主试机 (STEP 7.1)
%                2. 更改校准颜色设置，开关校准声音 (STEP 6)
%                3. 通过反应盒结束试次 (STEP 7.5)
% 12/20/13  srresearch
%                增加对 octave 的支持
%                修复 Eyelink('message' ...) 函数传递非整数参数的问题
%                移除 cleanup() 函数，使用 Eyelink('ShutDown'); Screen('CloseAll') 代替
%



clear;

if ~IsOctave
    commandwindow;
else
    more off;
end

% list of images used for the trial
imageList = {'town.jpg' 'town_blur.jpg' 'composite.jpg'};
dummymode=0;
try 
    % STEP 1
    % Added a dialog box to set your own EDF file name before opening 
    % experiment graphics. Make sure the entered EDF file name is 1 to 8 
    % characters in length and only numbers or letters are allowed.
    if IsOctave
        edfFile = 'DEMO';
    else

    prompt = {'Enter tracker EDF file name (1 to 8 letters or numbers)'};
    dlg_title = 'Create EDF file';
    num_lines= 1;
    def     = {'DEMO'};
    answer  = inputdlg(prompt,dlg_title,num_lines,def);
    %edfFile= 'DEMO.EDF'
    edfFile = answer{1};
    fprintf('EDFFile: %s\n', edfFile );
    end

    % STEP 2
    % Open a graphics window on the main screen
    % using the PsychToolbox's Screen function.
    screenNumber=max(Screen('Screens'));
    [window, wRect]=Screen('OpenWindow', screenNumber, 0,[],32,2);
    Screen(window,'BlendFunction',GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);


    % STEP 3
    % Provide Eyelink with details about the graphics environment
    % and perform some initializations. The information is returned
    % in a structure that also contains useful defaults
    % and control codes (e.g. tracker state bit and Eyelink key values).
    el=EyelinkInitDefaults(window);

    % STEP 4
    % Initialization of the connection with the Eyelink Gazetracker.
    % exit program if this fails.
    if ~EyelinkInit(dummymode)
        fprintf('Eyelink Init aborted.\n');
        cleanup;  % cleanup function
        return;
    end

    % the following code is used to check the version of the eye tracker
    % and version of the host software
    sw_version = 0;

    [v vs]=Eyelink('GetTrackerVersion');
    fprintf('Running experiment on a ''%s'' tracker.\n', vs );

    % open file to record data to
    i = Eyelink('Openfile', edfFile);
    if i~=0
        fprintf('Cannot create EDF file ''%s'' ', edffilename);
        Eyelink( 'Shutdown');
        Screen('CloseAll');
        return;
    end

    Eyelink('command', 'add_file_preamble_text ''Recorded by EyelinkToolbox demo-experiment''');
    [width, height]=Screen('WindowSize', screenNumber);


    % STEP 5    
    % SET UP TRACKER CONFIGURATION
    % Setting the proper recording resolution, proper calibration type, 
    % as well as the data file content;
    Eyelink('command','screen_pixel_coords = %ld %ld %ld %ld', 0, 0, width-1, height-1);
    Eyelink('message', 'DISPLAY_COORDS %ld %ld %ld %ld', 0, 0, width-1, height-1);                
    % set calibration type.
    Eyelink('command', 'calibration_type = HV9');
    % set parser (conservative saccade thresholds)

    % set EDF file contents using the file_sample_data and
    % file-event_filter commands
    % set link data thtough link_sample_data and link_event_filter
    Eyelink('command', 'file_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK,MESSAGE,BUTTON,INPUT');
    Eyelink('command', 'link_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK,MESSAGE,BUTTON,INPUT');

    % check the software version
    % add "HTARGET" to record possible target data for EyeLink Remote
    if sw_version >=4
        Eyelink('command', 'file_sample_data  = LEFT,RIGHT,GAZE,HREF,AREA,HTARGET,GAZERES,STATUS,INPUT');
        Eyelink('command', 'link_sample_data  = LEFT,RIGHT,GAZE,GAZERES,AREA,HTARGET,STATUS,INPUT');
    else
        Eyelink('command', 'file_sample_data  = LEFT,RIGHT,GAZE,HREF,AREA,GAZERES,STATUS,INPUT');
        Eyelink('command', 'link_sample_data  = LEFT,RIGHT,GAZE,GAZERES,AREA,STATUS,INPUT');
    end

    % allow to use the big button on the eyelink gamepad to accept the 
    % calibration/drift correction target
    Eyelink('command', 'button_function 5 "accept_target_fixation"');
   
    
    % make sure we're still connected.
    if Eyelink('IsConnected')~=1 && dummymode == 0
        fprintf('not connected, clean up\n');
        Eyelink( 'Shutdown');
        Screen('CloseAll');
        return;
    end



    % STEP 6
    % Calibrate the eye tracker
    % setup the proper calibration foreground and background colors
    el.backgroundcolour = [128 128 128];
    el.calibrationtargetcolour = [0 0 0];

    % parameters are in frequency, volume, and duration
    % set the second value in each line to 0 to turn off the sound
    el.cal_target_beep=[600 0.5 0.05];
    el.drift_correction_target_beep=[600 0.5 0.05];
    el.calibration_failed_beep=[400 0.5 0.25];
    el.calibration_success_beep=[800 0.5 0.25];
    el.drift_correction_failed_beep=[400 0.5 0.25];
    el.drift_correction_success_beep=[800 0.5 0.25];
    % you must call this function to apply the changes from above
    EyelinkUpdateDefaults(el);

    % Hide the mouse cursor;
    Screen('HideCursorHelper', window);
    EyelinkDoTrackerSetup(el);



    % STEP 7
    % Now starts running individual trials;
    % You can keep the rest of the code except for the implementation
    % of graphics and event monitoring 
    % Each trial should have a pair of "StartRecording" and "StopRecording" 
    % calls as well integration messages to the data file (message to mark 
    % the time of critical events and the image/interest area/condition 
    % information for the trial)
    
    for i=1:3
        % Now within the scope of each trial;
        imgfile= char(imageList(i));  
        
        % STEP 7.1 
        % Sending a 'TRIALID' message to mark the start of a trial in Data 
        % Viewer.  This is different than the start of recording message 
        % START that is logged when the trial recording begins. The viewer
        % will not parse any messages, events, or samples, that exist in 
        % the data file prior to this message. 
        Eyelink('Message', 'TRIALID %d', i);

        % This supplies the title at the bottom of the eyetracker display
        Eyelink('command', 'record_status_message "TRIAL %d/%d  %s"', i, 3, imgfile); 
        % Before recording, we place reference graphics on the host display
        % Must be offline to draw to EyeLink screen
        Eyelink('Command', 'set_idle_mode');
        % clear tracker display and draw box at center
        Eyelink('Command', 'clear_screen 0')
        Eyelink('command', 'draw_box %d %d %d %d 15', width/2-50, height/2-50, width/2+50, height/2+50);

        %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
        %transfer image to host
        transferimginfo=imfinfo(imgfile);

        fprintf('img file name is %s\n',transferimginfo.Filename);


        % image file should be 24bit or 32bit bitmap
        % parameters of ImageTransfer:
        % imagePath, xPosition, yPosition, width, height, trackerXPosition, trackerYPosition, xferoptions
        transferStatus =  Eyelink('ImageTransfer',transferimginfo.Filename,0,0,transferimginfo.Width,transferimginfo.Height,width/2-transferimginfo.Width/2 ,height/2-transferimginfo.Height/2,1);
        if transferStatus ~= 0
            fprintf('*****Image transfer Failed*****-------\n');
        end

        WaitSecs(0.1);
        %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


        % STEP 7.2
        % Do a drift correction at the beginning of each trial
        % Performing drift correction (checking) is optional for
        % EyeLink 1000 eye trackers.
        EyelinkDoDriftCorrection(el);  
        
        % STEP 7.3
        % start recording eye position (preceded by a short pause so that 
        % the tracker can finish the mode transition)
        % The paramerters for the 'StartRecording' call controls the
        % file_samples, file_events, link_samples, link_events availability
        Eyelink('Command', 'set_idle_mode');
        WaitSecs(0.05);
%         Eyelink('StartRecording', 1, 1, 1, 1);    
        Eyelink('StartRecording');    
        % record a few samples before we actually start displaying
        % otherwise you may lose a few msec of data 
        WaitSecs(0.1);
    
    	% STEP 7.4
        % Prepare and show the screen. 
     	Screen('FillRect', window, el.backgroundcolour);   
        imdata=imread(imgfile);
        imageTexture=Screen('MakeTexture',window, imdata);
        Screen('DrawTexture', window, imageTexture);
        Screen('DrawText', window, 'Press the SPACEBAR or BUTTON 5 to end the recording of the trial.', floor(width/5), floor(height/2), 0);
        Screen('Flip', window);
        % write out a message to indicate the time of the picture onset
        % this message can be used to create an interest period in EyeLink
        % Data Viewer.

        Eyelink('Message', 'SYNCTIME');
        
        % Send an integration message so that an image can be loaded as 
        % overlay backgound when performing Data Viewer analysis.  This 
        % message can be placed anywhere within the scope of a trial (i.e.,
        % after the 'TRIALID' message and before 'TRIAL_RESULT')
        % See "Protocol for EyeLink Data to Viewer Integration -> Image 
        % Commands" section of the EyeLink Data Viewer User Manual.
        Eyelink('Message', '!V IMGLOAD CENTER %s %d %d', imgfile, width/2, height/2);
        
        stopkey=KbName('space');
 
        % STEP 7.5
        % Monitor the trial events;
        while 1 % loop till error or space bar is pressed
            % Check recording status, stop display if error
            error=Eyelink('CheckRecording');
            if(error~=0)
                break;
            end


            % ending by pressing button 5
            buttonResult = Eyelink('ButtonStates');
            if buttonResult
                if(bitshift(buttonResult, -4)==1)  %fprintf('button 5 pressed\n');
                    Eyelink('Message','Button 5 pressed');
                    break;
                end
            end

            % check for keyboard press
            [keyIsDown,secs,keyCode] = KbCheck;
            % if spacebar was pressed stop display
            if keyCode(stopkey)
                Eyelink('Message', 'Key pressed');
                break;
            end
        end % main loop

        
        % STEP 7.6
        % Clear the display
        Screen('FillRect', window, el.backgroundcolour);
     	Screen('Flip', window);
        Eyelink('Message', 'BLANK_SCREEN');
        % adds 100 msec of data to catch final events
        WaitSecs(0.1);
        % stop the recording of eye-movements for the current trial
        Eyelink('StopRecording');
        
        
        % STEP 7.7
        % Send out necessary integration messages for data analysis
        % Send out interest area information for the trial
        % See "Protocol for EyeLink Data to Viewer Integration-> Interest 
        % Area Commands" section of the EyeLink Data Viewer User Manual
        % IMPORTANT! Don't send too many messages in a very short period of
        % time or the EyeLink tracker may not be able to write them all 
        % to the EDF file.
        % Consider adding a short delay every few messages.

        % Please note that  floor(A) is used to round A to the nearest
        % integers less than or equal to A

        WaitSecs(0.001);
        Eyelink('Message', '!V IAREA ELLIPSE %d %d %d %d %d %s', 1, floor(width/2)-50, floor(height/2)-50, floor(width/2)+50, floor(height/2)+50,'center');
        Eyelink('Message', '!V IAREA RECTANGLE %d %d %d %d %d %s', 2, floor(width/4)-50, floor(height/2)-50, floor(width/4)+50, floor(height/2)+50,'left');
        Eyelink('Message', '!V IAREA RECTANGLE %d %d %d %d %d %s', 3, floor(3*width/4)-50, floor(height/2)-50, floor(3*width/4)+50, floor(height/2)+50,'right');
        Eyelink('Message', '!V IAREA RECTANGLE %d %d %d %d %d %s', 4, floor(width/2)-50, floor(height/4)-50, floor(width/2)+50, floor(height/4)+50,'up');
        Eyelink('Message', '!V IAREA RECTANGLE %d %d %d %d %d %s', 5, floor(width/2)-50, floor(3*height/4)-50, floor(width/2)+50, floor(3*height/4)+50,'down');


        % Send messages to report trial condition information
        % Each message may be a pair of trial condition variable and its
        % corresponding value follwing the '!V TRIAL_VAR' token message
        % See "Protocol for EyeLink Data to Viewer Integration-> Trial
        % Message Commands" section of the EyeLink Data Viewer User Manual
        WaitSecs(0.001);
        Eyelink('Message', '!V TRIAL_VAR index %d', i)        
        Eyelink('Message', '!V TRIAL_VAR imgfile %s', imgfile)               
        
        % STEP 7.8
        % Sending a 'TRIAL_RESULT' message to mark the end of a trial in 
        % Data Viewer. This is different than the end of recording message 
        % END that is logged when the trial recording ends. The viewer will
        % not parse any messages, events, or samples that exist in the data 
        % file after this message.
        Eyelink('Message', 'TRIAL_RESULT 0')
    end   
    
    % STEP 8
    % End of Experiment; close the file first   
    % close graphics window, close data file and shut down tracker
        
    Eyelink('Command', 'set_idle_mode');
    WaitSecs(0.5);
    Eyelink('CloseFile');

    % download data file
    try
        fprintf('Receiving data file ''%s''\n', edfFile );
        status=Eyelink('ReceiveFile');
        if status > 0
            fprintf('ReceiveFile status %d\n', status);
        end
        if 2==exist(edfFile, 'file')
            fprintf('Data file ''%s'' can be found in ''%s''\n', edfFile, pwd );
        end
    catch
        fprintf('Problem receiving data file ''%s''\n', edfFile );
    end
    
    % STEP 9
    % close the eye tracker and window
      Eyelink('ShutDown');
      Screen('CloseAll');

catch
     %this "catch" section executes in case of an error in the "try" section
     %above.  Importantly, it closes the onscreen window if its open.
      Eyelink('ShutDown');
      Screen('CloseAll');
%      commandwindow;
%      rethrow(lasterr);
end %try..catch.

```




---

以上。
