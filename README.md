# daily-log====================================================
Debian + XFCE + IBus Post-Install Notes
====================================================

OS:
- Debian (trixie)
- Desktop: XFCE

----------------------------------------------------
1. Installation Options (Tasksel)
----------------------------------------------------
[x] Debian desktop environment
[x] XFCE
[x] Standard system utilities
[ ] GNOME
[ ] KDE
[ ] Cinnamon
[ ] MATE

NOTE:
- "Debian desktop environment" does NOT mean GNOME.
- It is a meta-package providing essential desktop infrastructure
  (sudo, NetworkManager, sound, input method support).

----------------------------------------------------
2. Initial System Status After Install
----------------------------------------------------
- sudo: OK
- NetworkManager: OK
- Sound stack: OK
- Korean fonts: OK
- Main issue: IBus configuration is difficult on XFCE

----------------------------------------------------
3. Korean Input Method Overview
----------------------------------------------------
Input Method:
- IBus + ibus-hangul

Notes:
- fcitx5 was tested but caused package availability and integration issues.
- Final decision: Use IBus (most realistic choice on Debian XFCE).

----------------------------------------------------
4. Observed Problems
----------------------------------------------------
- Alt+Shift, Right Shift, and Hangul key conflicts
- Right Shift alone does not switch language
- Hangul key must be pressed again after Right Shift
- Input behavior may change after reboot

----------------------------------------------------
5. Root Cause Analysis
----------------------------------------------------
Keyboard control conflicts between:
1) XFCE keyboard layout switching
2) XKB default options
3) IBus input method switching

Result:
- Multiple layers control the same keys simultaneously.

----------------------------------------------------
6. Stabilization Principle (IMPORTANT)
----------------------------------------------------
Rule:
- XFCE must NOT manage keyboard layout switching.
- IBus must be the ONLY component responsible for language switching.

----------------------------------------------------
7. Recommended Stable Configuration
----------------------------------------------------

7.1 XFCE Keyboard Layout
Path:
Settings -> Keyboard -> Layout

- Disable "Use system defaults"
- Remove Korean layout
- Keep ONLY: English (US)
- Disable ALL layout switching options

----------------------------------------------------

7.2 XFCE Application Shortcuts
Path:
Settings -> Keyboard -> Application Shortcuts

Remove ALL shortcuts related to:
- Alt+Shift
- Shift+Alt
- Super+Space
- Right Shift

----------------------------------------------------

7.3 IBus Configuration
Command:
ibus-setup

Input Method tab:
- Korean - Hangul
- English - English (US)
(Keep ONLY these two)

General tab:
- Next input method:
  - Hangul (recommended)
  - Shift + Space (optional)
- DO NOT use Alt+Shift
- DO NOT use Right Shift

Best practice:
- Use ONLY the Hangul key for switching.

----------------------------------------------------
7.4 Status Check
----------------------------------------------------
Command:
ibus engine

Expected output:
- English: xkb:us::eng
- Korean: hangul

Right Shift should NOT change the engine.

----------------------------------------------------
7.5 Logout Requirement
----------------------------------------------------
- 반드시 로그아웃 후 재로그인
- Without logout, old keyboard state may persist

----------------------------------------------------
8. Korean Fonts (Reference)
----------------------------------------------------
Usually already installed, but if missing:

sudo apt install -y fonts-noto-cjk

----------------------------------------------------
9. Practical Conclusion
----------------------------------------------------
- XFCE does not integrate input methods at the desktop level.
- IBus is GNOME-oriented and requires manual tuning on XFCE.

Summary:
XFCE = lightweight but manual
GNOME = heavier but integrated

----------------------------------------------------
10. One-Line Summary
----------------------------------------------------
On Debian + XFCE, Korean fonts work out of the box,
but IBus stability depends on fully disabling XFCE keyboard switching
and delegating all language switching to IBus only.
====================================================
