# Mouse wheel

For whatever reason, Chrome on Windows doesn't scroll tabs by mouse wheel.

The generally accepted solution is an [AutoHotKey script][mwgplus], published
[on Google Drive][mwgdrive], which works for maximized Chrome windows. That's
good enough for me.

**However,** this cannot work in conjunction with the *"Scroll inactive windows"*
feature of `AltDrag` (another thing you quickly miss from Linux). Therefore, I had
to add that functionality to my `AutoHotkey.ahk`. This is the result:

    SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.

    ;;
    ; xmodmap -option ctrl:nocaps
    ;
    ^+!Capslock::Capslock
    Capslock::Control

    ;; 
    ; Mouse Wheel Tab Scroll 4 Chrome
    ; -------------------------------
    ; Scroll though Chrome tabs with your mouse wheel when hovering over the tab bar.
    ; If the Chrome window is inactive when starting to scroll, it will be activated.
    ;   https://plus.google.com/115670442023408995787/posts/WYPqqk2j9UB
    ;   https://drive.google.com/drive/folders/0ByBbgFg_gObJTmdFd29PWEVwVHc

    ; updated to include inactive window scrolling with the help of
    ;  https://autohotkey.com/board/topic/6292-send-mouse-scrolls-to-window-under-mouse/?p=52004

    WheelUp::
    WheelDown::
        MouseGetPos,, ypos, id
        WinGetClass, class, ahk_id %id%
        If (ypos < 45 and InStr(class,"Chrome_WidgetWin"))
        {
            IfWinNotActive ahk_id %id%
                WinActivate ahk_id %id%
            If A_ThisHotkey = WheelUp
                Send ^{PgUp}
            Else
                Send ^{PgDn}
        }
        Else
        {
            IfWinNotActive ahk_id %id%
            {
                MouseGetPos m_x, m_y, WinID, Ctrl
                PostMessage 0x20A,((A_ThisHotKey="WheelUp")-.5)*(120<<16),(m_y<<16)|m_x,%Ctrl%,ahk_id %WinID%
            }
            Else
            {
                If A_ThisHotkey = WheelUp
                    Send {WheelUp}
                Else
                    Send {WheelDown}
            }
        }
        Return



[mwgplus]: https://plus.google.com/115670442023408995787/posts/WYPqqk2j9UB
[mwgdrive]:  https://drive.google.com/drive/folders/0ByBbgFg_gObJTmdFd29PWEVwVHc
