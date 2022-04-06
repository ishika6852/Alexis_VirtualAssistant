# Alexis_VirtualAssistant
This assistant is designed to understand and comprehend voice commands of the user to perform the requested tasks. This app helps you ease your day to day tasks, such as telling you date- time, playing songs on YouTube, searching on Wikipedia , sending automated mails and Whatsapp messages , accessing Flipkart just via voice command.


import time  # module to get time
import pyjokes as pyjokes  # module to get a random joke/tongue-twister
import pyttsx3  # module to convert text-too-speech
import pywhatkit  # module to send whatsapp messages
import randfacts as randfacts  # module to get a random fact
import speech_recognition as sr  # module used to recognise speech
import webbrowser  # module used to connect to web/internet
import datetime  # module to get time
import wikipedia  # module used to get wikipedia content
import random  # module used to generate a randon number
import self as self  # module used to support other functions
from flask import Flask, request  # module used to support other functions
from tkinter import *  # module used to generate the GUI


# this method is for taking the commands and recognizing the command from the
# speech_Recognition module we will use the recognizer method for recognizing
def takeCommand():
    r = sr.Recognizer()

    # the microphone module is used to take in the audio input from user
    with sr.Microphone() as source:
        print('Listening')

        # the assistants will wait for sometime before it considers the command to be complete
        r.pause_threshold = 0.7
        audio = r.listen(source)

        # if the microphone is working properly then the command is taken
        # otherwise the program will throw an exception
        try:
            print("Recognizing")
            # the assistant is set to understand only english language
            Query = r.recognize_google(audio, language='en-in')
            print("Command: ", Query)

        except Exception as e:
            print(e)
            print("I did not get that can you please repeat that")
            return "None"

        return Query


def speak(audio):
    # init function to get an engine instance for the speech synthesis
    engine = pyttsx3.init()
    # getter method of engine to get the audio from the system
    voices = engine.getProperty('voices')

    # setter method to set the voice of alexis
    # [0]=male voice and [1]=female
    engine.setProperty('voice', voices[1].id)

    # Method for the speaking of the assistant
    engine.say(audio)

    # puts the system in waiting state so that the existing commands can execute
    engine.runAndWait()


def tellDay():
    # This function is for telling the day of the week
    day = datetime.datetime.today().weekday() + 1

    Day_dict = {1: 'Monday', 2: 'Tuesday',
                3: 'Wednesday', 4: 'Thursday',
                5: 'Friday', 6: 'Saturday',
                7: 'Sunday'}

    if day in Day_dict.keys():
        day_of_the_week = Day_dict[day]
        print("The day is " + day_of_the_week)
        speak("The day is " + day_of_the_week)


# This method will give the time
def tellTime():
    Time = str(datetime.datetime.now())
    hour = Time[11:13]
    minutes = Time[14:16]
    print("The Time is " + hour + " Hours and " + minutes + " Minutes ")
    speak("The Time is. " + hour + " rrs. and " + minutes + " Minutes")


# Function to show  that the assistant is activated
def Hello():
    print("hello User I am your desktop assistant Alexis. Tell me how may I  help you")
    speak("hello User I am your desktop assistant Alexis. Tell me how may I  help you")


# Function used to categorize queries
def Take_query():
    # Calling the hello() function
    Hello()
    # setting an infinite loop for the assistant working
    # the loop exits when the exit command is encountered
    while True:

        # take the command from user and convert it into a string
        query = takeCommand().lower()

        # 1. Greeting type-A
        if "hello" in query:
            answer = ["Hello User. How can i help you", "Hi. how are you",
                      "Hello. what would you like me to do today"]
            ans = answer[random.randint(0, len(answer) - 1)]
            print(ans)
            speak(ans)

        # 2. Greeting type-B
        elif "i am good" in query:
            answer = ["That's great", "Good to know", "What would you like to do today",
                      "That makes me happy"]
            ans = answer[random.randint(0, len(answer) - 1)]
            print(ans)
            speak(ans)

        # 3. Greeting type-C
        elif "i am sad" in query:
            answer = ["don't be. I am here", "Cheer up", "would you like to share",
                      "well i can tell you a joke just say the words"]
            ans = answer[random.randint(0, len(answer) - 1)]
            print(ans)
            speak(ans)

        # 4. What can alexis do
        elif "what can you do" in query:
            answer = ["tell you day and time", "entertain you via jokes and games", "play songs for you",
                      "search the web", "tell you about anything or anyone in this world",
                      " locate any place in the world", "make notes for you"]
            ans = answer[random.randint(0, len(answer) - 1)]
            print("I can do a lot of things like " + ans + " just say the word")
            speak("I can do a lot of things like. " + ans + " just say the word")

        # 5. What is the Gender of Alexis
        elif "what is your gender" in query:
            answer = ["I don't have a gender. i am a machine",
                      "I am a man. Just kidding. I am just a robot, so no gender for me",
                      "I am a girl. Just kidding. I am just a robot, so no gender for me"]
            ans = answer[random.randint(0, len(answer) - 1)]
            print(ans)
            speak(ans)

        # 6. Jokes
        elif "tell me a joke" in query:
            jk = pyjokes.get_joke(language='en', category='all')
            print(jk)
            speak('Here is a funny one.  ' + jk)

        # 7. Tongue Twister
        elif "tongue twister" in query:
            twist = pyjokes.get_joke(language='en', category='twister')
            print(twist)
            speak('Try this tongue twister')
            speak(twist)

        # 8. Who created Alexis
        elif "who created you" in query:
            print("I was created by Ishikas for their sixth semester project")
            speak("I was created by Ishikas for their sixth semester project")
            continue

        # 9. Play Games
        elif "game" in query:
            # we have 2 games 1- two player tic-tac-toe
            print("do you want to play a two player tic tac toe say yes or no")
            speak("do you want to play a two player tic tac toe. Say yes or no")
            query = takeCommand().lower()
            if "yes" in query:
                pass
            elif "no" in query:
                print("Goodbye and have a fabulous day ahead")
                speak("Goodbye and have a fabulous day ahead")
                exit()

        # 10. Asking Alexis for its name
        elif "tell me your name" in query:
            print("I am Alexis. Your desktop Assistant")
            speak("I am Alexis. Your desktop Assistant")

        # 11. Browse content on YouTube
        elif "play" in query:
            search_term = query.split("play")[-1]
            # method of pywhatkit module is used to play the specified song directly on YouTube
            pywhatkit.playonyt(search_term)
            print("Here is what i found for " + search_term + " on youtube")
            speak("Here is what i found for " + search_term + ". On youtube")

        # 12. Perform a search on internet
        elif "search" in query:
            search_term = query.split("search")[-1]
            # method of pywhatkit module is used to perform google searches
            pywhatkit.search(search_term)
            print("Here is what i found for " + search_term + " on Google")
            speak("Here is what i found for " + search_term + ". On Google")

        # 13. Locate a place using Google Maps
        elif "locate" in query:
            search_term = query.split("locate")[-1]
            url = "https://www.google.com/maps/place/" + search_term
            webbrowser.get().open(url)
            print("This is what i found for " + search_term + " on Google Maps")
            speak("This is what i found for " + search_term + " on Google Maps")

        # 14. Send automated messages to a selected user
        elif "send message on whatsapp" in query:
            numbers = {"ishika": "+919324476914", "mom": "+918958234644", "dad": "+919412205712",
                       "raghav": "+918439261655", "disha": "+919690833467"}
            print("Who do you want to send the message")
            speak("Who do you want to send the message")
            person = takeCommand().lower()
            print("What is the message")
            speak("What is the message")
            msg = takeCommand().lower()
            print("Message: " + msg)
            if "ishika" in person:
                pywhatkit.sendwhatmsg_instantly("+919324476914", msg)
            elif "mom" in person:
                pywhatkit.sendwhatmsg_instantly("+918958234644", msg)
            elif "dad" in person:
                pywhatkit.sendwhatmsg_instantly("+919412205712", msg)
            elif "disha" in person:
                pywhatkit.sendwhatmsg_instantly("+919690833467", msg)
            else:
                pywhatkit.sendwhatmsg_instantly("+918439261655", msg)
            print("Check the whatsapp terminal for message correctness before sending the message")
            speak("Check the whatsapp terminal for message correctness before sending the message")

        # 15. Go to flipkart
        elif "flipkart" in query:
            print("Opening Flipkart")
            speak("Opening Flipkart")
            webbrowser.open("www.flipkart.com")

        # 16. Go to Amazon
        elif "amazon" in query:
            print("Opening Amazon")
            speak("Opening Amazon")
            webbrowser.open("www.amazon.com")

        # 17. Go to Netflix
        elif "netflix" in query:
            print("Opening Netflix")
            speak("Opening Netflix")
            webbrowser.open("www.netflix.com")

        # 18. Go to Amazon Prime
        elif "amazon prime" in query:
            print("Opening Amazon Prime")
            speak("Opening Amazon Prime")
            webbrowser.open("www.primevideo.com")

        # 19. Open Spotify
        elif "spotify" in query:
            print("Opening Spotify")
            speak("Opening Spotify")
            webbrowser.open("www.spotify.com")

        # 20. Make notes using Google Docs
        elif "make a note" in query:
            print("Opening Google Docs")
            speak("Opening Google Docs")
            url = "https://www.google.com/docs/about/"
            webbrowser.get().open(url)

        # 21. Open Geeks for Geeks
        elif "open make my trip" in query:
            print("Opening Make My Trip ")
            speak("Opening Make My Trip ")
            webbrowser.open("www.makemytrip.com")
            continue

        # 22. Open Google
        elif "open google" in query:
            print("Opening Google")
            speak("Opening Google ")
            webbrowser.open("www.google.com")
            continue

        # 23. Tell the Day
        elif "which day it is" in query:
            tellDay()
            continue

        # 24. Tell the Time
        elif "tell me the time" in query:
            tellTime()
            continue

        # 25. Open YouTube
        elif "open youtube" in query:
            print("Opening YouTube")
            speak("Opening YouTube")
            webbrowser.open("www.youtube.com")
            continue

        # 26. search things on wikipedia
        elif "from wikipedia" in query:
            print("Checking the wikipedia ")
            speak("Checking the "
                  "wikipedia ")
            search_term = query.split("wikipedia")[-1]
            # speaks approx 4 lines found on wiki
            result = wikipedia.summary(search_term, sentences=3)
            speak("According to wikipedia")
            print(result)
            speak(result)

        # 27. get information about a specific topic
        elif "get information" in query:
            search_term = query.split("information")[-1]
            result = pywhatkit.info(search_term, lines=3)
            print(result)
            speak("here is what i found for "+search_term)
            speak(result)

        # 28. get a random fact about a random topic
        elif "random fact" in query:
            result = randfacts.get_fact()
            print(result)
            speak("The fact of the day is")
            speak(result)

        # 29. thank you, Alexis
        elif "thank you alexis" in query:
            answer = ["no problem", "happy to help", "that is my job", "glad to help",
                      "i got you", "you are welcome", "my pleasure"]
            ans = answer[random.randint(0, len(answer) - 1)]
            print(ans)
            speak(ans)

        # 30. Exit command Type-A
        elif "bye" in query:
            print("Goodbye and have a good day")
            speak("Goodbye and have a good day")
            exit()


class Widget:
    def clicked(self):
        Take_query()

    def __init__(self):
        root = Tk()
        root.title("Alexis")
        root.geometry('600x650')

        photo = PhotoImage(file="VAType1.png")
        ishika_label = Label(image=photo)
        ishika_label.pack(side='right', fill='both', expand='no')

        userText = StringVar()

        userText.set('Your Virtual Assistant')
        userFrame = LabelFrame(root, text='Alexis', font=('Railways', 25, 'bold'))
        userFrame.pack(fill='both', expand='yes')

        top = Message(userFrame, textvariable=userText, bg='black', fg='white')
        top.config(font=("Century Gothic", 15, 'bold'))
        top.pack(side='top', fill='both', expand='yes')

        btn1 = Button(root, text='Speak', font=('railways', 10, 'bold'), bg='red', fg='white', command=self.clicked)
        btn1.pack(fill='x', expand='no')
        btn2 = Button(root, text='Close', font=('railways', 10, 'bold'), bg='yellow', fg='black', command=root.destroy)
        btn2.pack(fill='x', expand='no')
        root.mainloop()
        exit()


if __name__ == '__main__':
    widget = Widget()

time.sleep(1)

while 1:
    query = takeCommand()
    speak(query)
