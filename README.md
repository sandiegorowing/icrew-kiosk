# icrew-kiosk

Configuration instructions for a Raspberry Pi that drives an iCrew kiosk

# Assumptions

Date: 2024-12-26

*   Operating system: Debian GNU/Linux 12 (bookworm)
*   Windowing System: Wayland
*   Compositor: Labwc
*   Hardware: Raspberry Pi 5

# Configuration steps

1.  Build a new Raspbian release on an SD card
    
2.  Boot the raspberry Pi and follow the instructions to configure the locale, t  
    imezone, and other standard parameters
    
3.  Create a new user named 'kiosk'
    
    *   `adduser kiosk`
        
    *   set a password and accept all defaults
        
4.  Login as the kiosk user
    
5.  Create the file ~kiosk/.config/labwc/environment containing the following information
    
    ```javascript
    XKBDEFAULT_LAYOUT=us
    XKBDEFAULT_MODEL=pc105
    XKBDEFAULT_VARIANT= 
    XKBDEFAULT_OPTIONS= 
    
    ```
    
6.  Create the file ~kiosk/.config/labwc/autostart containing the following
    
    ```
    /usr/bin/lwrespawn /usr/bin/pcmanfm --desktop --profile LXDE-pi &
    /usr/bin/lwrespawn /usr/bin/chromium-browser --start-maximized --fast --fast-start --no-sandbox --no-first-run --noerrdialogs --disable-translate --disable-notifications --disable-session-crashed-bubble --disable-infobars   --check-for-update-interval=604800 --disable-pinch --disable-features=TranslateUI --disk-cache-dir=/dev/null --ozone-platform=wayland --enable-features=OverlayScrollbar --overscroll-history-navigation=0 --window-position=0,0 --incognito --kiosk 'http://example.com/' &
    /usr/bin/kanshi &
    /usr/bin/swayidle -w timeout 3600 'wlopm --off \*' resume 'wlopm --on \*' &
    /usr/bin/lxsession-xdg-autostart
    ```
    
7.  As 'root' edit the file /etc/lightdm and enable the SeatDefaults section as follows
    
    ```
    [SeatDefaults]
    autologin-user=kiosk
    autologin-user-timeout=0
    user-session=xfce
    ```
