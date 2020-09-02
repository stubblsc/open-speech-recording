### Open Speech Recording

This is a stripped down version of the [**Open Speech Recording**](https://github.com/petewarden/open-speech-recording) written by Pete Warden. For the full version that integrates with the google cloud please see the original repository. This version is optimized to run directly on your local machine. This repository also contains scripts written by Pete to manipulate the audio files that come out of the app.  

### You can use the app by taking the following steps:

1. Clone the repository, initialize the submodules, and install the only requirement, flask:
  ```
  git clone https://github.com/tinyMLx/open-speech-recording.git
  cd open-speech-recording
  git submodule update --init --recursive
  pip install flask
  ```
2. To update the words you are recording and how many recordings of each word you are collecting, change the counts and values at the top of the app file in the open-speech-recording repository: ```open-speech-recording/static/scripts/app.js```. The default is 5 copies of each wake word (in this case just "hello") and 1 copy of each other/unknown/filler word (in this case just "world"). You can then run the server locally (from within the open-speech-recording folder) by running:
  ```
  export FLASK_APP=main.py
  python -m flask run
  ```

3. Then open the link provided in the terminal in a web browser to run the application. Make sure to **run the application in a private or incognito window** which avoids any cacheing issues. Also we've found that the app works best when **using Chrome**. Once the app opens you'll need give access to your microphone, and then you can click ```Record```. Once you finish recording all of the words a popup will appear and ask if you'd like to download the data. Simply click ```OK``` and the files will be downloaded into the folder from which you are running the flask app (which should be the open-speech-recording folder). You can then close the browser window and kill the app with ```cntrl+c```.

Note: if you want to change the words or counts recorded make sure to kill and re-start the app and open the link in a new incognito window!

### You can use the scripts to manipulate the data as follows:

1. You now have a large collection of ```.ogg``` files in your directory. We now need to convert them to ```.wav``` files using ```ffmpeg```:
  ```
  sudo apt-get install ffmpeg
  mkdir wavs
  find *.ogg -print0 | xargs -0 basename -s .ogg | xargs -I {} ffmpeg -i {}.ogg -ar 16000 wavs/{}.wav
  ```

2. Then we need to trim them using Pete's tool. Note: background noise does not need to be trimmed and does not need to be recorded by the tool &mdash; you simply need to convert them to ```.wav``` files. In fact, they are expected to be longer. See the [original dataset](https://storage.googleapis.com/download.tensorflow.org/data/speech_commands_v0.02.tar.gz) for examples. 
  ```
  mkdir trimmed_wavs
  make -C extract_loudest_section/
  /tmp/extract_loudest_section/gen/bin/extract_loudest_section 'wavs/*.wav' trimmed_wavs/
  ```
3. Finally we need to create the directory structure as the training script expects that the data is organized into folders where there is one folder for each of the ```wanted_words``` filled with ```.wav``` files of the wanted words (as 1 second clips) and an additional folder called ```_background_noise_``` containing longer files of background noise. We will then compress this into a zip file so it can be easily uploaded to Colab. 
  ```
  python organize_wavs.py
  cd output_wavs
  zip -r my_dataset.zip *
  ```

-Adapted by the Harvard CS249r F2020 team
