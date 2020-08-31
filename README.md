import pyttsx3 #pip install pyttsx3
import speech_recognition as sr #pip install speechRecognition
import datetime
import wikipedia #pip install wikipedia
import webbrowser
import os
import smtplib

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
# print(voices[1].id)
engine.setProperty('voice', voices[0].id)


def speak(audio):
    engine.say(audio)     #This is defined for speaking
    engine.runAndWait()


def wishMe():            #This is defined to wish
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning!")

    elif hour>=12 and hour<18:
        speak("Good Afternoon!")   

    else:
        speak("Good Evening!")  

    speak("Initializing Jarvis. Connection succesfull to satellite number 87. Command")      

def takeCommand():
    #It takes microphone input from the user and returns string output(for listening to what I say)

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
        # print(e)    
        print("Say that again please...")  
        return "None"
    return query

if __name__ == "__main__":
    wishMe()
    while True:
    # if 1:
        query = takeCommand().lower()

        # Logic for executing tasks based on query
        if 'wikipedia' in query:            #For searching wikipedia
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(results)
            speak(results)

        elif 'open google' in query:
            webbrowser.open("google.com")        

        elif 'open youtube' in query:
            webbrowser.open("youtube.com")          

        elif 'open facebook' in query:
            webbrowser.open("facebook.com")

        elif 'who are you' in query:
            speak("I am Jarvis, your personal A I voice assistant") 
            print("I am J.A.R.V.I.S, your personal A.I. voice assistant")

        elif 'open whatsapp' in query:
            webbrowser.open("web.whatsapp.com")          

        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")    
            speak(f"Sir, the time is {strTime}")

        elif 'quit' in query:
            speak("Thank you sir for your time! Farewell...........")    
            quit()
 
