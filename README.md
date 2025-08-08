# pavan-mankala
import datetime
import webbrowser
import pyttsx3
import speech_recognition as sr

class Jarvis:
    def __init__(self):
        self.engine = pyttsx3.init()
        self.engine.setProperty('rate', 175)
        self.engine.setProperty('volume', 0.9)

    def speak(self, text):
        print(f"Jarvis: {text}")
        self.engine.say(text)
        self.engine.runAndWait()

    def listen(self):
        recognizer = sr.Recognizer()
        with sr.Microphone() as source:
            print("Listening...")
            recognizer.pause_threshold = 1
            audio = recognizer.listen(source)
        try:
            print("Recognizing...")
            query = recognizer.recognize_google(audio, language='en-in')
            print(f"You: {query}")
        except Exception as e:
            self.speak("Sorry, I did not understand that. Can you please repeat?")
            return ""
        return query.lower()

    def greet(self):
        hour = datetime.datetime.now().hour
        if hour < 12:
            self.speak("Good morning!")
        elif hour < 18:
            self.speak("Good afternoon!")
        else:
            self.speak("Good evening!")
        self.speak("I am Jarvis, your assistant. How can I help you today?")

    def run(self):
        self.greet()
        while True:
            query = self.listen()
            if 'open google' in query:
                self.speak("Opening Google.")
                webbrowser.open('https://www.google.com')
            elif 'time' in query:
                current_time = datetime.datetime.now().strftime('%H:%M:%S')
                self.speak(f"The current time is {current_time}")
            elif 'exit' in query or 'quit' in query or 'stop' in query:
                self.speak("Goodbye! Have a great day!")
                break
            elif query:
                self.speak("Sorry, I don't know how to do that yet.")

if __name__ == "__main__":
    jarvis = Jarvis()
    jarvis.run()
