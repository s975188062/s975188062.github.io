---
title: "第三方编程 Command 函数可用参数参考文档"
excerpt: "Command函数是对眼动仪进行配置的关键函数，根据输入参数的不同可以实现不同功能。"
read_time: false
header:
  overlay_image: /assets/images/header_Dracula.png
  overlay_filter: 0.5
categories:
  - Eyelink
  - 3rd-Programing
tags:
  - 3rd-Programing
toc: true
comments: true
author_profile: true
classes: wide
---

---

Command函数是对眼动仪进行配置的关键函数，根据输入参数的不同可以实现不同功能。

~~~
# EyeLink CL COMMANDS.INI FILE
# November 25 2005	
# *** WARNING: DO NOT MODIFY THIS FILE DIRECTLY!!! PLEASE CUT AND PASTE   ***
# *** SELECTED COMMANDS TO FINAL.INI AND MAKE MODIFICATIONS IN THAT FILE! ***
# NOTE: some settings may be overridden by LASTRUN.INI

	
    ## online_dcorr_trigger <x ref coord> <y ref coord>
	;; Performs as a mouse click for either on-line drift correction or for parallax (static or vergence-based) correction in scene camera mode. The gaze coordinates are the position of the fixation point (online drift correct) or the position of the object the subject is fixating in the video overlay. Fixation data collected previous to this command will be evaluated to see if a valid fixation has been found to supply computed gaze position. This command can fail, returning one of the following error codes:
    ;; -11 : "Drift correction failed: recalibrate" means online drift correction failed to find a correction.
  	;; -12 : "No valid fixation in range" means no valid fixation data preceding the command was found (if not in a fixation or it is too early in the current fixation, the last data in the  previous fixation is checked for recency).
  	;; -13 : "Drift correction angle too large" means the angular distance  from the computed gaze to the reference point is too large.  This is rejected as it may indicate inattention or a	misfixation.
    ;; -10 : "Not in proper mode for online drift/depth correction" means the tracker is not in the correct mode for this command.  
	;;	
 	;;	The modes and functionality are:
	;;	- RECORD mode (non-scene camera) : drift correction.  
	;;  The computed gaze is compared to the reference point set 
	;;      with "online_dcorr_refposn".  If a reference position is given,
	;;      it replaces the current reference position.
	;;   - RECORD mode (scene camera) : static correction.  
	;;      The computed gaze is compared to the reference point given
	;;      with this command, assumed to be the point on the video 
	;;      of the object the subject is fixating.
	;;   - SCENE CAMERA DEPTH FIXUP mode (scene camera) : 
	;;      Vergence or static correction.  The computed gaze is compared 
	;;      to the reference point given with this command, assumed to be 
	;;      the point on the video  of the object the subject is fixating.  
	;;      The first point collected does static correction; after enough
	;;      vergence data is collected the vergence-based correction 
	;;     is computed.
;; online_dcorr_trigger


	## drift_correction <offset x> <offset y> <gaze x> <gaze y> [list of options]
  	;; Performs a drift correction, using a display position and an offset.
  	;; The offset is (target position - actual fixation)
  	;; The drift correction happens immediately, and will cause 
  	;; a jump in eye-movement data. It is used in the GCONTROL experiment sample.
  	;; OPTIONS:
  	;; L or 0 = do left eye
  	;; R or 1 = do right eye
  	;; (if neither specified does one or both depending on eyes tracked)
  	;; HREF = data and frift correction is in HREF coordinates
  	;; if HREF is not in the option list = simply add offset to gaze position
;; drift_correction	
		
	
	## acceptarget_fixation
	## collect_target_fixation <start time> <end time>
	;; Both these act to trigger manual accept of a target fixation.
	;; The time arguments for "collect_target_fixation" are ignored for
	;; EyeLink CL.  The "accept_target_fixation" command is used by many
	;; API programming examples and the API function 
	;; eyelink_accept_trigger().  
	;; Most examples program button 5 to issue this command when pressed.
;; accept_target_fixation
;; collect_target_fixation


	## apply_last_drift_correction <optional fraction>
	;; When a drift correction is done using "start_drift_correction" 
	;; issued through the link, the actual drift correction is not done.  
	;; This command applies the computed correction.  If a fraction is 
	;; supplied, then only a portion of the error is corrected. 
	;; This command is used by the API function eyelink_apply_driftcorr().
;; apply_last_drift_correction	


	## restore_old_calibration
	;; Supposed to reload the last calibration and drift correction.
	;; This happens automatically now, so these may have no effect.
;; restore_old_calibration



	## start_bitmap_transfer <type> <destx> <desty> <width> <height>
	;; Starts transfer of record-background bitmap to tracker. 
	;; It returns specifications for bitmap conversion. 
	;; This command is used the API function gdi_bitmap_core(). 
	;; This command is only legal in Offline and Output menu modes. 
  	;;	<type>: 0 if color, 1 if greyscale
  	;;	<destx>, <desty>: position of top-left in gaze coords on tracker display.
  	;;	<width>, <height>: size of bitmap in gaze coords on tracker display.
 	;;
 	;; Returns: required bitmap specifications as a string:
 	;; "<width> <height> <topdrop> <botdrop> <levels> <compsize>"
  	;;	<width>, <height>: required bitmap size to send
  	;;	<topdrop>, <botdrop>: amount of bitmap to drop at top and bottom
        ;;             	(nonzero for EyeLink 1, 0 for EyeLink II / EyeLink CL).
  	;;	<levels>: number or greyscale or for each of R, G, and B components.
 	;;	<compsize>: 0 if no compresssion, else compression block size in bytes.
        ;;	      (currently always 0).
;; start_bitmap_transfer 


	## stop_bitmap_transfer
	;; Aborts transfer of record-background bitmap to tracker, typically
	;; if an error occurs. 
	;; This command is used the API function gdi_bitmap_core().
;; stop_bitmap_transfer


	## start_playback
	;; Starts last-trial playback which is limited to Offline mode. 
	;; This command is used the API function eyelink_playback_start().
;; start_playback


	## abort_playback
  	;; Ends last-trial playback.
  	;; This command is used the API function eyelink_playback_stop().
;; abort_playback


	## mark_playback_start
	;; Marks the location in the data file from which playback will begin
	;; at the next call to eyelink_playback_start().  
	;; When this command is not used (or on older tracker versions), 
	;; playback starts from the beginning of the previous recording block.  
	;; This default behavior is suppressed after this command is used, 
	;; until the tracker software is shut down.
;; mark_playback_start


	## print_position <column><line>
	;; Coordinates are text row and column, similar to C gotoxy() function. 
	;; NOTE: row cannot be set higher than 25.  
	;; Use 'draw_text' command to print anywhere on the tracker display.
	;;	<col>: text column, 1 to 80
	;;	<row>: text line, 1 to 25
;; print_position 


	## clear_screen <color: 0 to 15>
	;; Clear tracker screen for drawing background graphics or messages.
;; clear_screen


	## draw_line <x1> <y1> <x2> <y2> <color>
	;; Draws line, coordinates are gaze-position display coordinates.
	;;	<x1>,<y1>: start point of line
	;;	<x2>,<y2>: end point of line
	;;	<color>: 0 to 15
;; draw_line


	## draw_box <x1> <y1> <x2> <y2> <color>
	;; Draws empty box, coordinates are gaze-position display coordinates.
	;;	<x1>,<y1>: corner of box
	;;	<x2>,<y2>: opposite corner of box
	;;	<color>: 0 to 15
;; draw_box


	## draw_filled_box <x1> <y1> <x2> <y2> <color>
	;; Draws a solid block of color, coordinates are gaze-position display
	;; coordinates.
	;; 	<x1>,<y1>: corner of box
	;; 	<x2>,<y2>: opposite corner of box
	;; 	<color>: 0 to 15
;; draw_filled_box


	## draw_text <x1> <y1> <color> <text>
	;; Draws text, coordinates are gaze-position display coordinates.
	;;	<x1>,<y1>: center point of text
	;;	<color>: 0 to 15
	;;	<text>: text of line, in quotes
;; draw_text


	## echo <text>
	;; Prints text at current print position to tracker screen, gray on 
	;; black only
	;; <text>: text to print in quotes
;; echo 


	## draw_cross <x> <y>
	;; Draws a small '+' to mark a target point.
	;;	<x1>,<y1>: center point of cross
	;;	<color>: 0 to 15
;; draw_cross


	## include <filename>
	;; Reads the contents of <filename> and then continues reading current
	;; INI file. The target file consisting of commands and system 
	;; variable settings. Configuration files may include any other
	;; configuration file, but not itself.
;; include
	
	
	## begin_macro <name>
	;; Starts (opens) a macro definition.  <name> can be any word that
	;; is not an existing command.  All macro_line commands until the
	;; next end_macro will be added to this macro.
;; begin_macro


	## end_macro
	;; Closes the current macro definition.
;; end_macro


	## macro_line <rest of line as command>
	;; Adds all text following "macro_line as a line of the currently 
	;; open macro. This includes comments, etc.  
	;; Must be preceded by "begin_macro" and followed by "end_macro"
;; macro_line


	## do_macro <name>
	;; Executes all lines of the macro <name>.  
	;; These will be executed as a block, but may not be contiguous in time.  
	;; Any data and image processing needs of the tracker take precedence 
	;; over command execution, and thus changing data-control parameters
	;; while data is being recorded may result in undefined behavior.
;; do_macro


	## delete_macro  <name>
	;; Deletes the macro <name>.
;; delete_macro

 	
	## flush_logfile
	;; Forces all buffered log file data to be written. 
	;; This could be used to ensure logfile integrity, or to reduce the 
	;; probability of disk writes happen for a short period.
;; flush_logfile


	## start_file_transfer <optional file name>
	;; Starts send of and EDF file.  If a file name is supplied, 
	;; it is converted to be in the specified directory or, 
	;; if no directory is given, in the current EDF data path. 
	;; If no file name is given, the current (or last) open EDF file is used.
	;; This command is used by the API function eyelink_request_file_read().
;; start_file_transfer


	## abort_file_transfer
	;; Halts current EDF file transfer. This command is used by the API
	;; function eyelink_end_file_transfer().
;; abort_file_transfer


	## add_file_preamble_text   <Descriptive text message>
	;; Text messages describing a file's contents may be written into a
	;; datafile's preamble, which is viewable with any text editor. 
	;; This text must be written and the preamble closed before data may
	;; be written to the file.
;; add_file_preamble_text


	## open_data_file <name of data file>
	;; This command opens an eye tracker data file (.EDF extension), 
	;; destroying any file with the same name without warning.  
	;; If no path is given, the file will be written into the directory 
	;; the eye tracker is running from.  
	;; Returns error message or "<filename> successfully created".
;; open_data_file

	
	## close_data_file
	;; Closes any open EDF file.  Attempts to clean up file structure if 
	;; closing while data is being recorded.
;; close_data_file


	## data_file_name
	;; READ-ONLY
	;; Returns the full name of the current or last EDF file opened.
;; data_file_name

	## call_setup_menu_mode
	## call_option_menu_mode
	;; Designed for use from user menus--these execute the mode, then 
	;; on exit return to the current mode (or menu).
;; call_setup_menu_mode
;; call_option_menu_mode


	## display_user_menu <menu number>
	;; Starts a user menu, numbered from 1 to 3 (1 to 4 for EyeLink 1).
	;; User menu 1 will be called up on record abort (CTRL-ALT-A) by 
	;; the API function record_abort_handler().
;; display_user_menu


	## option_menu_mode
	;; This calls up the "Set Options" menu screen. 
	;; No data output is available.
;; option_menu_mode


	## output_menu_mode <optional data control switches>
	;; This calls up the Output menu screen.  
	;; Data output is available in EyeLink 2.0 and later.
;; output_menu_mode DATA = 1 1 1 1 


	## setup_menu_mode
	;; This calls up the Setup menu in EyeLink 1, and the Camera Setup 
	;; menu (with image sending off) in EyeLink II / CL.  
	;; No data output is available.
;; setup_menu_mode


	## start_recording <optional data control switches>
	;; The main data-output mode, optimized for best analog and link data
	;; quality.  Used internally by many API functions.  
	;; Data control can also be set using "record_data_defaults"
;; start_recording DATA = 1 1 1 1


	## start_calibration <optional data control switches>
	;; Starts calibration.  Data output is available in EyeLink 2.0 and later.
;; start_calibration DATA = 1 1 1 1


	## start_validation <optional data control switches>
	;; Starts validation. Data output is available in EyeLink 2.0 and later.
;; start_validation DATA = 1 1 1 1


	## start_drift_correction <optional data control switches>
	;; Starts drift correction. Used internally by the API function 
	;; eyelink_driftcorr_start(). 
	;; Data output is available in EyeLink 2.0 and later.
;; start_drift_correction DATA = 1 1 1 1


	## set_idle_mode <optional data control switches>
	;; Enters Offline mode.  This is used by the API functions 
	;; eyelink_abort() and set_offline_mode().
 	;; Data output is available in EyeLink 2.0 and later.
;; set_idle_mode DATA = 0 0 0 0


	## set_imaging_mode
	;; Enters camera-image sending mode.  On EyeLink II / CL, this is just
	;; turns on image sending in the Camera Setup screen, but it was a 
	;; completely separate screen in EyeLink 1.  
	;; No data output is available.
;; set_imaging_mode


	## call_setup_menu_mode
	## call_option_menu_mode
	;; Designed for use from user menus--these execute the mode, 
	;; then on exit return to the current mode (or menu).
;; call_setup_menu_mode
;; call_option_menu_mode


	## refresh_buttons
	;; A command that forces redraw of mode elements.  This is used after
	;; commands that change settings, to cause mode display to reflect these.
;; refresh_buttons


	## link_connect_command = <command string to execute>
	;; Sets command to be executed whenever a computer connects.  
	;; Use ' ' or " " if spaces in command.
;; link_connect_command = "echo 'LINK OPENED'"


	## link_shutdown_command =  <command string to execute>
	;; Sets command to be executed whenever a computer disconnects.  
	;; Use ' ' or " " if spaces in command.
;; link_shutdown_command =  "echo 'LINK CLOSED'"


	## exit_program
	;; Exits the EyeLink CL executable, closing any open EDF file.
;; exit_program

	
	## screen_dump <optional file name>
	;; Saves the display to a PCX file.  
	;; If no file name is given, creates a numbered file name. 
	;; Adds the extension "PCX" to the file name if needed.
;; screen_dump	


	## data_message <message text>
	;; This places a message in the EDF file.  
	;; This was intended to allow message sending from digital inputs.  
	;; NOTE: prior to EyeLink II v2.0, the message timestamp was when
	;; the command was executed, not when the command was issued.
;; data_message


	## record_status_message <text message to be displayed in record mode>
	;; Sets title displayed in Record mode.  
	;; Use "" or ' ' quotes if contains spaces. 
;; record_status_message 'Trial title message'

	
	## reset_record_lock
	;; Resets the record duration counter to 0
;; reset_record_clock


	## set_href_point <point index> <x coord> <y coord>
	;; Sets individual reference gaze positions to be converted to HREF
	;; on a sample-by-sample basis.
	;;	<point index>: 1 to 4
	;;	<x coord>, <ycoord>: gaze coordinate of reference point.
;; set_href_point = 1 0 0
;; set_href_point = 2 1023 0
;; set_href_point = 3 0 767
;; set_href_point = 4 1023 767


	## clear_href_points
	;; Resets all gaze reference points to "MISSING".  
	;; This can be used if points are to be removed (to reduce sample size).
;;  clear_href_points
~~~

---

以上。
