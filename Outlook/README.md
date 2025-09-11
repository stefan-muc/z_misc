# Outlook Tipps and Tricks

## Disable Shortcuts

### Alt-S

To disalble the [Alt]-[S] shortcut for sending emails, navigate to registry key `HKEY_CURRENT_USER\Software\Policies\Microsoft\office\16.0\outlook\DisabledShortcutKeysCheckBoxes` (you might have to create last key) and create a new `REG_SZ` Key with name `AltS` (can be choosen freely) and value `83,16` ([Source](https://robert365.com/article/disable-sending-via-ctrl-enter-or-alt-s))

## Ask for confirmation on [Ctrl]-[S]

This was a feature of Outlook classic that got removed. There's (currently) no way to get it back ([Source](https://learn.microsoft.com/en-us/answers/questions/4656221/ctrl-enter-combination-is-not-asking-a-confirmatio))

## Export and upload calendar

To export the calendar (anonymized - only availability information) use this Visual Basic for Applications scripts:

### General Script

```VBA
' https://stackoverflow.com/questions/12257985/outlook-vba-run-a-code-every-half-an-hour
Declare PtrSafe Function SetTimer Lib "user32" (ByVal hwnd As LongLong, ByVal nIDEvent As LongLong, ByVal uElapse As LongLong, ByVal lpTimerfunc As LongLong) As LongLong
Declare PtrSafe Function KillTimer Lib "user32" (ByVal hwnd As LongLong, ByVal nIDEvent As LongLong) As LongLong

Public TimerID As LongLong 'Need a timer ID to eventually turn off the timer. If the timer ID <> 0 then the timer is running

Public Sub ActivateTimer(ByVal nMinutes As LongLong)
  nMinutes = nMinutes * 1000 * 60 'The SetTimer call accepts milliseconds, so convert to minutes
  If TimerID <> 0 Then Call DeactivateTimer 'Check to see if timer is running before call to SetTimer
  TimerID = SetTimer(0, 0, nMinutes, AddressOf TriggerTimer)
  If TimerID = 0 Then
    MsgBox "The timer failed to activate."
  'Else
  '  MsgBox "Timer activated."
  End If
End Sub

Public Sub DeactivateTimer()
Dim lSuccess As LongLong
  lSuccess = KillTimer(0, TimerID)
  If lSuccess = 0 Then
    MsgBox "The timer failed to deactivate."
  Else
    TimerID = 0
  End If
End Sub

Public Sub TriggerTimer(ByVal hwnd As Long, ByVal uMsg As Long, ByVal idevent As Long, ByVal Systime As Long)
  'MsgBox "The TriggerTimer function has been automatically called!"
  ThisOutlookSession.ExportCalendar
End Sub
```

### This Outlook Session

```VBA
Option Explicit
Private m_dtNextTime As Date

' based on https://docs.microsoft.com/en-us/office/vba/api/Outlook.CalendarSharing
Public Sub ExportCalendar()

 Dim oNamespace As NameSpace
 Dim oFolder As Folder
 Dim oCalendarSharing As CalendarSharing

 On Error GoTo ErrRoutine

 ' Get a reference to the Calendar default folder
 Set oNamespace = Application.GetNamespace("MAPI")
 Set oFolder = oNamespace.GetDefaultFolder(olFolderCalendar)

 ' Get a CalendarSharing object for the Calendar default folder.
 Set oCalendarSharing = oFolder.GetCalendarExporter

 ' Set the CalendarSharing object to export the contents of
 ' the entire Calendar folder, including attachments and
 ' private items, in full detail.
 With oCalendarSharing
 .CalendarDetail = olFreeBusyOnly ' this one makes sure to just export availability, so no confidential information is exported
 .IncludeAttachments = False
 .IncludePrivateDetails = False
 .IncludeWholeCalendar = False
 .StartDate = DateAdd("d", -7, Now)
 .EndDate = DateAdd("d", 365, Now)
 End With

 ' Export calendar to an iCalendar calendar (.ics) file.
 oCalendarSharing.SaveAsICal "D:\your\path\calendar.ics" ' TODO

 ' Might yield "invalid procedure call or argument", try if it will work - otherwise call script manually
 'Shell "D:\your\path\upload.bat", vbHide

EndRoutine:
 On Error GoTo 0
 Set oCalendarSharing = Nothing
 Set oFolder = Nothing
 Set oNamespace = Nothing
 ' restart Timer
 'StartCalendarTimer
Exit Sub

ErrRoutine:
 Select Case Err.Number
 Case 287 ' &H0000011F
 ' The user denied access to the Address Book.
 ' This error occurs if the code is run by an
 ' untrusted application, and the user chose not to
 ' allow access.
 MsgBox "Access to Outlook was denied by the user.", _
 vbOKOnly, _
 Err.Number & " - " & Err.Source
 Case -2147467259 ' &H80004005
 ' Export failed.
 ' This error typically occurs if the CalendarSharing
 ' method cannot export the calendar information because
 ' of conflicting property settings.
 MsgBox Err.Description, _
 vbOKOnly, _
 Err.Number & " - " & Err.Source
 Case -2147221233 ' &H8004010F
 ' Operation failed.
 ' This error typically occurs if the GetCalendarExporter method
 ' is called on a folder that doesn't contain calendar items.
 MsgBox Err.Description, _
 vbOKOnly, _
 Err.Number & " - " & Err.Source
 Case Else
 ' Any other error that may occur.
 MsgBox Err.Description, _
 vbOKOnly, _
 Err.Number & " - " & Err.Source
 End Select

 GoTo EndRoutine
End Sub

' based on https://stackoverflow.com/a/211779
Private Sub StartCalendarTimer()
    SetTimer 0, 1, 0, AddressOf ExportCalendar
    'm_dtNextTime = DateAdd("h", 1, Now)
    'Application.OnTime m_dtNextTime, "ExportCalendar"
End Sub

Private Sub Application_Startup()
    Call ActivateTimer(15) ' for first calendar export
End Sub

Private Sub Application_Quit()
  If TimerID <> 0 Then Call DeactivateTimer 'Turn off timer upon quitting **VERY IMPORTANT**
End Sub
```

### Upload script

```bat
@echo off
```
