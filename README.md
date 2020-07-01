This is a stripped down version of the [**Open Speech Recording**](https://github.com/petewarden/open-speech-recording) written by Pete Warden. For the full version that integrates with the google cloud please see the original repository. This version is optimized to run directly on your local machine.
The only requirement is to install flask:
```pip install flask```

You can then run the server locally by running:
```
export FLASK_APP=main.py
python -m flask run
```

Then simply follow the link provided in the terminal to run the application. Note: 1) You will have to give the application permission to use your microphone, 2) We have found that the application works best when **run in a private or icognito window** which avoids any cacheing issues.

To update the words you are recording and how many recordings of each word you are collecting, change the counts and values at the top of: ```static/scripts/app.js```.

This repository also contains scripts written by Pete to manipulate the audio files that come out of the app. Instructions for using these scripts are included in the Wake Words assignment.

Adapted by the CS249r F2020 team.
