# BindWinEvent
BindWinEvent is a small framework that allows multiple bindings to Windows message events.  From the VFP Help file in the BindEvent() topic:

*When binding to Windows message (Win Msg) events, only one hWnd to Windows message pairing can exist.*

This can cause conflicts when multiple VFP programs are trying to bind to the same Windows message event. This framework works around the issue by effectively translating the Win Msg events to standard Fox events and tracking them accordingly.

This framework is used by [FoxTabs](https://github.com/VFPX/FoxTabs), [ProjectExplorer](https://github.com/DougHennig/ProjectExplorer), and possibly others. If you have an IDE utility that needs to bind to Win Msg events, it is recommended to use this framework rather than standard VFP BindEvent() calls to avoid conflicts with other utilities.

**BindWinEvent should only be used for binding to Windows message (Win Msg) events. VFP object event binding should continue to be handled with standard VFP BindEvent() and UnbindEvents().**

# Installation
Include the following files in your project:

- bindwinevent.prg
- bindwineventapi.prg
- unbindwinevents.prg
- vfpxwin32eventhandler.prg

# Usage
Simply replace your calls to BindEvent() and UnbindEvents() for Windows messages with BindWinEvent()  and UnbindWinEvents().


BindWinEvent() has the same interface as the standard BindEvent() function:

**BindWinEvent(hWnd | 0, nMessage, oEventHandler, cDelegate [, nFlags])**

UnBindEvents() is similar to the standard UnBindEvents() function, but adds a couple of parameters.  Since multiple bindings are now possible, you have to specify which event handler/delegate you want to unbind:

**UnBindWinEvents(hWnd | 0, nMessage | 0, oEventHandler, cDelegate)**

You can pass 0 to nMessage if you want to unbind all the messages for a hWnd, such as when a window is closed/destroyed. The following syntax is also supported if you want to unbind all events from an event handler object:

**UnBindWinEvents(oEventHandler)**

# History
Years ago, [Greg Green](https://github.com/ggreen86) reported that FoxTabs was conflicting with [his custom VFP editors](https://github.com/ggreen86/VFP-Editors), due to VFP's limitations binding to Win Msg events. Greg wrote this framework to deal with the limitation, and I made some modifications/enhancements. I wrote about this on [my blog](http://www.joelleach.net/2010/04/11/foxtabs-092-and-multiple-windows-event-bindings/), and BindWinEvent has been included with FoxTabs ever since.

More recently, I found that FoxTabs was conflicting with Doug Hennig's ProjectExplorer for the same reason, so I [implemented BindWinEvent in ProjectExplorer](https://github.com/DougHennig/ProjectExplorer/pull/196). Doug reported back with some fixes to issues he found.  I then realized that BindWinEvent needed a separate repository of its own, so here it is!  
