To configure Ubuntu to prompt you to select a browser for every link you click, you can create a custom script that utilizes Zenity to display a selection dialog. Here’s how to set it up:

### Step-by-Step Instructions

1. **Install Zenity**: Zenity is a tool that allows you to create GTK+ dialogs. You can install it using the following command:

   ```bash
   sudo apt install zenity
   ```

2. **Create the Browser Selection Script**: Open a terminal and create a new script file:

   ```bash
   sudo nano /usr/bin/select-browser
   ```

   Paste the following code into the file:

   ```bash
   #!/bin/bash

   BROWSERS='TRUE Firefox FALSE Chromium FALSE Vivaldi FALSE Opera'

   HEIGHT=210

   LINK=$*

   BROWSER=$(zenity --list --radiolist --text "${LINK:0:100}" --column='' --column='Browsers' --height=$HEIGHT --title='Select browser to open link' $BROWSERS)

   if [ ! -z "$BROWSER" ]; then
       ${BROWSER,,} "$LINK" &
   fi
   ```

   This script will show a dialog with the available browsers each time you click a link.

3. **Make the Script Executable**: Run the following command to make your script executable:

   ```bash
   sudo chmod +x /usr/bin/select-browser
   ```

4. **Create a .desktop File**: To associate the script with URL handling, create a .desktop file:

   ```bash
   nano ~/.local/share/applications/select-browser.desktop
   ```

   Paste the following content into the file:

   ```ini
   [Desktop Entry]
   Version=1.1
   Type=Application
   Name=Select browser
   Exec=/usr/bin/select-browser %U
   Categories=Network;WebBrowser;
   MimeType=x-scheme-handler/http;x-scheme-handler/https;
   ```

5. **Set the Script as the Default Browser**: Use the following command to set your new script as the default browser:

   ```bash
   xdg-settings set default-web-browser select-browser.desktop
   ```

6. **Test Your Setup**: Click on any link from a non-browser application, and the Zenity dialog should appear, allowing you to select which browser to use.

### Conclusion

This setup allows you to choose which browser to use every time you click a link, providing flexibility in managing different browsing contexts. You can modify the list of browsers in the script as needed to fit your preferences.

Citations:
[1] https://help.ubuntu.com/stable/ubuntu-help/net-default-browser.html
[2] https://askubuntu.com/questions/16621/how-to-set-the-default-browser-from-the-command-line
[3] https://unix.stackexchange.com/questions/487203/choose-which-browser-to-open-link-in
[4] https://itsfoss.com/ubuntu-change-browser/
[5] https://larsee.com/blog/2023/04/select-browser-dialog-in-linux/
