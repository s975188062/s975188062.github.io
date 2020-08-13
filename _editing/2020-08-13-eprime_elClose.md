# 

```VB
tracker.setOfflineMode  ' set offline mode so we can transfer file Sleep 500 ' delay so tracker is ready tracker.closeDataFile ' close data file tracker.receiveDataFile edfFileName, "" ' get the edf file to display pcSet tracker = Nothing ' release tracker objectSet elutil  = Nothing ' release eyelink util objectErrorHandle:	If Err <> 0 Then 		Set tracker = Nothing ' release tracker object		Set elutil  = Nothing ' release eyelink util object		MsgBox Err.Number & ":" & Err.Description		'Exit Sub   if this is not the last inline, we need an Exit Sub    End If
```