# chrome_remote_setup_tutorial
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo dpkg -i google-chrome-stable_current_amd64.deb

wget https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb

sudo apt install ./chrome-remote-desktop_current_amd64.deb

mkdir ~/.config/chrome-remote-desktop



#----- go to google chrome to setup Desktop then continue the step ----#


/opt/google/chrome-remote-desktop/chrome-remote-desktop --stop

sudo cp /opt/google/chrome-remote-desktop/chrome-remote-desktop /opt/google/chrome-remote-desktop/chrome-remote-desktop.orig

sudo gedit /opt/google/chrome-remote-desktop/chrome-remote-desktop

#---------------Follow the step to chenge file "chrome-remote-desktop"----------------#
    
Find DEFAULT_SIZES and amend to the remote desktop resolution. For example:

    DEFAULT_SIZES = "1920x1080"

In my case, I set it to “1920x1200,3840x2400” since the desktop had dual-monitors.

    Set the X display number to the current display number (obtain it with echo $DISPLAY from any terminal). On Ubuntu 17.10 and lower, this is usually 0, and on Ubuntu 18.04, this is usually 1:

    FIRST_X_DISPLAY_NUMBER = 0

In my case, it happened to be 1.

    Comment out sections that look for additional displays:

    #while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
    # display += 1

    Reuse the existing X session instead of launching a new one. Alter launch_session() by commenting out launch_x_server() and launch_x_session() and instead setting the display environment variable, so that the function definition ultimately looks like the following:

    def launch_session(self, x_args):
    self._init_child_env()
    self._setup_pulseaudio()
    self._setup_gnubby()
    #self._launch_x_server(x_args)
    #self._launch_x_session()
    display = self.get_unused_display_number()
    self.child_env["DISPLAY"] = ":%d" % display

#-----------------------------------------------------------------------------------------------#


/opt/google/chrome-remote-desktop/chrome-remote-desktop --start

sudo reboot
