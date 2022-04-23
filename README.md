# ARC_ListView
; #FUNCTION# ==================================================================================================================== ; Name ..........: _GUICtrlListView_AutoResizeColumn ; Description ...: Auto resize ListView column when the window is resized! ; Syntax ........: _GUICtrlListView_AutoResizeColumn($hWnd, $hListView) ; Parameters ....: $hWnd                - A window handle value. ;                  $hListView           - A control handle value. ; Return values .: Success         - Returns 1. ;                   Failure         - Returns 0. ; Author ........: JScript ; Modified ......: ; Remarks .......: Using AdlibRegister function. ; Related .......: ; Link ..........: ; Example .......: _GUICtrlListView_AutoResizeColumn($hWnd, $hListView) ; =============================================================================================================================== Func _GUICtrlListView_AutoResizeColumn($hWnd, $hListView, $iRefresh = 250)     Local Static $iRLV_Flag = 0      If Not $hWnd And Not $hListView Then         Return 0     EndIf     If Not $iRLV_Flag Then         Global $__hARC_ListView = $hListView         Global $__hARC_Win_hWnd = $hWnd         $iRLV_Flag = 1         AdlibRegister("__ARC_ListView", $iRefresh)         Return 1     EndIf     Return 0 EndFunc   ;==>_GUICtrlListView_AutoResizeColumn  ; #INTERNAL_USE_ONLY# =========================================================================================================== ; Name ..........: __ARC_ListView ; Return values .: None ; Author ........: JScript ; =============================================================================================================================== Func __ARC_ListView()     Local Static $iRLV_Flag = 0, $aiOldSize     Local $iColCount, $aiGetSize, $iWidth     Local $hWnd = Eval("__hARC_Win_hWnd"), $hListView = Eval("__hARC_ListView")      If $hWnd = "" Or $hListView = "" Or Not WinExists($hWnd) Then         AdlibUnRegister("__ARC_ListView")         Return 0     EndIf     If Not $iRLV_Flag Then         $iRLV_Flag = 1         $aiOldSize = WinGetClientSize($hWnd)     EndIf     $aiGetSize = WinGetClientSize($hWnd)     If $aiOldSize[0] &lt;> $aiGetSize[0] Then         $iColCount = _GUICtrlListView_GetColumnCount($hListView)         $iWidth = Int($aiGetSize[0] / $iColCount)         For $i = 0 To $iColCount             _GUICtrlListView_SetColumnWidth($hListView, $i, $iWidth)         Next         $aiOldSize = $aiGetSize     EndIf EndFunc   ;==>__ARC_ListView
