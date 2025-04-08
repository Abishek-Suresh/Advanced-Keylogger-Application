# Advanced-Keylogger-Application


![Image](https://github.com/user-attachments/assets/a26ddebf-dd2c-48c9-afe5-c1267095e7bb)

This keylogger application is build with python and Compiled with Nuitka. The functionality of this keylogger doesn't stop at capturing keystrokes, it can take screenshots, record audio, collect important information such as system info and even get the stored wifi SSIDs and passwords from the system and send it to an email address, we just have to mention the number of iterations we want the keylogger program to run along with the time span for each iterations. For ethical reasons, I've not uploaded the executable file here in github, as it can evade antivirus such as malwarebytes or even windows defender. I've used Nuitka to compile the python script in such a way that it evades the antivirus and it does everything under the radar. This is a proof of concept and can be developed even further. 

## Possible future developments

- I've already tried to add screen recording feature using the python module ffmpeg. Yet to develop
- Introducing Cryptography, we can encrypte the log files that we collect and then sent it via mail such that others can't access to the data without decrpyting it.
- Not just stopping with mail, we can further develop this application such that it can send all the collected information to a server.
- steganograph this executable into a picture making it a great payload
- and so on

## Code 

### Importing the Required Libraries: 
- smtplib | MIMEMultipart |MIMEText| MIMEBase | encoders : For sending email
- socket | Platform | OS | tempfile | time | getpass : For collecting information about the host and file operations
- win32clipboard from pywin32 : To get the clipboard data from the victim machine
- pynput : To listen and grab the keystrokes
- write from scipy.io.wavfile | sounddevice : To record and save from microphone
- Imagegrab from pillow : To take screenshots of the victim machine
- subprocess | re : for grabbing wifi informations

### Main Functions:
- **send_email()** : Takes three input arguments, filename, attachment path, To address for sending the mail with all the log files, audio recording and screenshots.
- **computer_info()** : Utilizing socket and platform modules, this function gets the victim's system info such as user name, public IP Address, processor, os version and host name, writing all the information to a log file "computer_info.txt"
- **clipboard_data()** : Utilizing the function GetClipboardData from win32clipboard, this function grabs the clipboard data from victim's machine and then write it in a log file named "clipboard_data.txt".
- **microphone_recording()** : providing the sample rate, desired second to capture the microphone recording and this function saves the recorded file.
- **screenshot()**: This function captures screenshot from the victim machine and saves it.
- **wifi_info()** : Using the subprocess module we gonna run the command "netsh wlan show profiles" and then capture the output from the terminal. Using regular expression (re), we can extract the available wifi-profiles and then look for security keys and then write them to a logfile named: "wifi_info.txt".
#### NOTE : All the log files are being saved in the temp folder of the victim machine so that we can do this all under the radar. Using gettempdir() method from tempfile module, we can get the tempfolder path in the victim's machine and then save the files there.

### Grabbing Key strokes and setting a desired timing for the keylogger:



