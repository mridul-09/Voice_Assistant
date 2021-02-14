# macc-voice-assistant
voice assistant

LIST OF MODULES WE IMPORTED

import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import pywhatkit
import webbrowser
import os
import json
from urllib.request import urlopen
import wolframalpha
app = wolframalpha.Client("7GA3HJ-VAKW885PLU")


PYTHON CODES


engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour < 12:
       speak("Good Morning!")
    elif hour >= 12 and hour < 18:
       speak("Good Afternoon!")
    else:
       speak("Good Evening!")
    speak("I am your personal assistant Sir. Please tell me how may I help you")


def takeCommand():

    r = sr.Recognizer()
    with sr.Microphone() as source:
       print("Listening...")
       r.pause_threshold = 1
       audio = r.listen(source)

    try:
       print("Recognizing...")
       query = r.recognize_google(audio, language='en-in')
       print(f"User said: {query}\n")

    except Exception as e:
       print("Say that again please...")
       speak("Say that again please...")
       return "None"
    return query


if __name__ == "__main__":
     wishMe()
     while True:

         query = takeCommand().lower()
         print(query)

         if 'play' in query:
             song = query.replace('play', '')
             speak('playing ' + song)
             pywhatkit.playonyt(song)

         elif 'wikipedia' in query:
             speak('Searching Wikipedia...')
             query = query.replace("wikipedia", "")
             results = wikipedia.summary(query, sentences=2)
             speak("According to Wikipedia")
             print(results)
             speak(results)

         elif 'open facebook' in query:
             speak("Here you go to Facebook\n")
             webbrowser.open("facebook.com")

         elif 'start music' in query:
             speak("Here you go with music")
             music_dir = 'E:\music'
             songs = os.listdir(music_dir)
             print(songs)
             os.startfile(os.path.join(music_dir, songs[0]))

         elif 'current time' in query:
             strTime = datetime.datetime.now().strftime("%H:%M:%S")
             speak(f"Sir, the time is {strTime}")
             print(strTime)

         elif 'ouside temperature' in query:
              res = app.query(query)
              speak(next(res.results).text)
              print(next(res.results).text)

         elif 'how are you' in query:
             speak("I am fine, Thank you")
             speak("How are you, Sir")

         elif 'fine' in query or "mast" in query:
             speak("It's good to know that your fine")

         elif "who made you" in query or "who created you" in query:
             speak("I have been created by Mridul ,   abhijeet ,   Anas,   Ashutosh.")
             print("I have been created by Mridul ,abhijeet , Anas, Ashutosh. ")

         elif "who i am" in query or "main kaun hun" in query:
             speak("If you talk then definately your human.")
             print("If you talk then definately your human.")

         elif 'what is love' in query or " what's love" in query:
             speak("It is 7th sense that destroy all other senses")

         elif "who are you" in query:
             speak("I am your Personal assistant created by Mridul,  abhijeet,  anas,  ashutosh ")
             print("I am your Personal assistant created by Mridul,  abhijeet  ,anas,  ashutosh ")

         elif 'reason' in query:
             speak("I was created as a Minor project by Master Mridul")

         elif "why you came to world" in query:
             speak("Thanks to Mridul. furthner It's a secret")

         elif "write a note" in query:
             speak("What should i write, sir")
             note = takeCommand()
             file = open('data.txt', 'w')
             speak("Sir, Should i include date and time")
             snfm = takeCommand()
             if 'yes' in snfm or 'sure' in snfm:
                 strTime = datetime.datetime.now().strftime("% H:% M:%S")
                 file.write(strTime)
                 file.write(" :- ")
                 file.write(note)
                 print(strTime)
             else:
                 file.write(note)

         elif "tell note" in query or "tell notes" in query or "tell me my notes" in query:
                 speak("Showing Notes")
                 file = open("data.txt", "r")
                 print(file.read())
                 speak(file.read(6))


         elif 'tell me news' in query:
             try:
                 jsonObj = urlopen('''http://newsapi.org/v2/everything?domains=wsj.com&apiKey=83d3a7ff48f54250805df0302fb6eecf''')
                 data = json.load(jsonObj)
                 i = 1

                 speak('here are some top news from the times of india')
                 print('''=============== TIMES OF INDIA ============'''+ '\n')
                 for item in data['articles']:
                    print(str(i) + '. ' + item['title'] + '\n')
                    print(item['description'] + '\n')
                    speak(str(i) + '. ' + item['title'] + '\n')

             except Exception as e:
                 print(str(e))




