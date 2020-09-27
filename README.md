# Google-Assignment

import speech_recognition as sr
import webbrowser
import time
import playsound
import os
import random
from gtts import gTTS
from time import ctime 
r = sr.Recognizer()
def record_audio(ask = False):
 with sr.Microphone() as source:

    if ask:
        alexis_speak(ask)
    audio = r.listen(source)
    voice_data = ''
    try:
     voice_data = r.recognize_google(audio)
     
    except sr.UnknownValueError:
        alexis_speak("sorry,I did not get that")
    except sr.RequestError:
        alexis_speak("Sorry,my speech service is down")
        return voice_data
def alexis_speak(audio_string):
    tts = gTTS(text=audio_string,lang='en')
    r = random.randint(1,10000000)
    audio_file = 'audio-' + str(r) + '.mp3'
    tts.save(audio_file)
    playsound.playsound(audio_file)
    print(audio_string)
    os.remove(audio_file)

    def respond(voice_data):
        if 'What is your name' in voice_data:
            alexis_speak('my name is prem')
        if 'what time is it' in voice_data:
            alexis_speak(ctime())

        if 'search' in voice_data:
            search = record_audio("what do you want in search")
            url ='https://google.com/search?q=' + search + '&anp:'
            webbrowser.get().open(url)
            alexis_speak('Here is What I found for ' + search)

        if 'location' in voice_data:
             location = record_audio("what is the location")
             url ='https://location.ml/maps/place/' + location + '&anp:'
             webbrowser.get().open(url)
             alexis_speak('Here is the location of ' + location)
        if 'exit' in voice_data:
            exit()     

time.sleep(1)
alexis_speak('How can I help you?')
while 1:
    voice_data = record_audio()
    respond(voice_data)
        
