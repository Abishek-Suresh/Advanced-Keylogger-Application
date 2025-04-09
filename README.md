# Advanced-Keylogger-Application
![Image](https://github.com/user-attachments/assets/a26ddebf-dd2c-48c9-afe5-c1267095e7bb)
<br>
<br><br>
This keylogger application is build with python and Compiled with Nuitka. The functionality of this keylogger doesn't stop at capturing keystrokes, it can take screenshots, record audio, collect important information such as system info and even get the stored wifi SSIDs and passwords from the system and send it to an email address, we just have to mention the number of iterations we want the keylogger program to run along with the time span for each iterations. For ethical reasons, I've not uploaded the executable file here nor the whole code in github, as it can evade antivirus such as malwarebytes or even windows defender. I've used Nuitka to compile the python script in such a way that it evades the antivirus and it does everything under the radar. This is a proof of concept and can be developed even further. 
<br>
<br>

## Possible future developments
- I've already tried to add screen recording feature using the python module ffmpeg. Yet to develop
- Introducing Cryptography, we can encrypted the log files that we collect and then sent it via mail such that others can't access to the data without decrpyting it.
- Not just stopping with mail, we can further develop this application such that it can send all the collected information to a server.
- steganograph this executable into a picture making it a great payload
- and so on
  <br>
  <br>
  
## Code 
### Importing the Required Libraries: 
- **smtplib | MIMEMultipart |MIMEText| MIMEBase | encoders** : For sending email
- **socket | Platform | OS | tempfile | time | getpass** : For collecting information about the host and file operations
- **win32clipboard from pywin32** : To get the clipboard data from the victim machine
- **pynput** : To listen and grab the keystrokes
- **write from scipy.io.wavfile | sounddevice** : To record and save from microphone
- **Imagegrab from pillow** : To take screenshots of the victim machine
- **subprocess | re** : for grabbing wifi informations
<br>

### Main Functions:
- **send_email()** : Takes three input arguments, filename, attachment path, To address for sending the mail with all the log files, audio recording and screenshots.
- **computer_info()** : Utilizing socket and platform modules, this function gets the victim's system info such as user name, public IP Address, processor, os version and host name, writing all the information to a log file "computer_info.txt"
- **clipboard_data()** : Utilizing the function GetClipboardData from win32clipboard, this function grabs the clipboard data from victim's machine and then write it in a log file named "clipboard_data.txt".
- **microphone_recording()** : providing the sample rate, desired second to capture the microphone recording and this function saves the recorded file.
- **screenshot()**: This function captures screenshot from the victim machine and saves it.
- **wifi_info()** : Using the subprocess module we gonna run the command "netsh wlan show profiles" and then capture the output from the terminal. Using regular expression (re), we can extract the available wifi-profiles and then look for security keys and then write them to a logfile named: "wifi_info.txt".
#### NOTE : All the log files are being saved in the temp folder of the victim machine so that we can do this all under the radar. Using gettempdir() method from tempfile module, we can get the tempfolder path in the victim's machine and then save the files there.
<br>

### Grabbing Key strokes and setting a desired timing for the keylogger:

#### We have defined three functions here:
- **on_press()** : This function gets called when a key is pressed, this takes the key as input appends it to a empty list and then gets written in the log file named "keys_log.txt" utilizing the next function in our list, i.e write_file()
- **write_file()** : This function takes in our keys as input argument and then writes it to the log file when called.
- **on_release()** : There's literally two times this function gets called, when esc key is pressed or when the assigned time for the keylogger's iteration ends. Next we gonna see this counter's functionality.
<br>

### Counter Functionality:
#### Defined variables for this counter and timer functionality:
- **number_of_iterations**: This is the initialization of the counter .
- **currentTime** : initial time, used time module here
- **stopingTime** : stop time of the timer
- **number_of_iterations_end** : This is where we mention the number of iterations we want to run the whole keylogger's functionality. i.e each main functions will be called and a mail will be sent with all the information for each iteration.
- **time_iteration** : Time interval for the keylogger to run in seconds for each iteration [for capturing the key strokes]

#### NOTE: All the main functions are kept inside the while loop utilizing the above variables for our timer and counter function to work.
<br>

### Flying under the radar - Leaving NO TRACE:
- As I mentioned earlier, all the log files and recordings are getting stored in the temporary folder of the victim's machine. Once the iteration is completed and the mails have been sent, after 60 seconds, all those log files that got created will be deleted leveraging the OS module leaving no trace...
<br>

### OUTPUT:

#### This is how an email is sent out with all the information per iteration:

I was keep on testing and this is how the inbox looked like :)
![Image](https://github.com/user-attachments/assets/b5f20214-f0c3-427a-a9a4-f2a4fb475927)
<br>

Anyways, these are the output files that are being sen over mail:
![Image](https://github.com/user-attachments/assets/d0d02c37-0263-4c94-9e09-e1953312b330)
<br>
<br>

### Obstacles I stumbled upon:

- First obstacle that I faced is when I tried to send the mail using smtplib, Logging in with your gmail account [Used a burner account] and password no longer works as google have removed less secure ways of signing in, so **how to solve this** : Create an app password after turning on Two-factor Authorization, and use that password instead of the account's password.
  
- The timer and counter functionality took me more time to implement properly.
  
- Compiling the python file into an executable, is where I literally spend more time in this project, especially in order to evade the antivirus. Initially I used **pyinstaller** and compiled it regularly, it got detected. I tried using **--windowed**, **--noconsole flags** in order to get an executable which dont open a terminal explicitly when executed. It Also got detected. Then I did some workaround and referred to David Bombal's youtube video, finally I got an executable which evaded the antivirus but it opened a terminal everytime I opened the executable, which is no good as you can't use it as a proper payload.  [NOTE: I packed the executable as onefile].
  
- In order to prevent this terminal getting opened, I tried to implement some sort of GUIs, so that the GUI get's opened with some deceiving notifications eg: Your device will restart to update outside of active hours (estimate: 5 min).
  
- Experimented with these modules one by one: tkinter [not worked, it showed the notification message along with the blank black terminal] , pythonnet [a module which utilizes .net dll's such as windows forms - didn't worked either].
  
- Finally my brain cell told me to try alternate compilers: Compiled using **cx_freeze**, guess what this one evaded the antivirus but I was not able to bundle the executable into onefile, for this too you literally have to package all required libraries including python so your executable runs independently of system Python, another FAILURE...
  
- Atlast went with **nuitka** and did some workaround, it was a success. The executable did run as a background process and didn't opened any terminal.
<br>

**OUTPUT EXE USING CX_FREEZE:** 

As you can see, this one's dependent on other dll and dependencies

![Image](https://github.com/user-attachments/assets/16d29711-087f-469f-8fad-e3eea199a3e1)

<br>

**OUTPUT EXE USING NUITKA:**

This executable runs without any dependencies as a onefile.

![Image](https://github.com/user-attachments/assets/21bdfba8-d991-4f34-9c49-62edf2ea9239)


### REFERENCES:

#### Thanks to David Bombal, Grant Collis, TechwithTim

##### Videos I referred:

https://youtu.be/25um032xgrw?si=iZw8bPnPW-RrVj9C <br>
https://youtu.be/LBM3EzBXhdY?si=5fAR1UJe-FDcsXrx <br>
https://youtu.be/qaZ-IbssPDI?si=Tw1JdovpCyzwpHnE <br>
https://youtu.be/TbMKwl11itQ?si=rG4P9TRmU2XqTbUJ <br>
 
