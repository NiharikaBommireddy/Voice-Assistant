# Voice-Assistant

import speech_recognition as sr
import warnings
import pyttsx3
import pywhatkit as pwk
import datetime
import wikipedia
import calendar
import pyjokes
import random
import os
import webbrowser
import ctypes
import winshell
import PyPDF2
import smtplib
from email.message import EmailMessage


warnings.filterwarnings("ignore")
listener = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)


def talk(text):
    engine.say(text)
    engine.runAndWait()


def record_audio():
    try:
        with sr.Microphone() as source:
            print('Listening...')
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            print(command)
    except:
        pass
    return command


def say_hello():
    response = ["hi", "hey", "hola", "greetings", "wassup", "hello", "howdy", "what's good", "hey there"]
    return random.choice(response)+"."


def today_date():
    now = datetime.datetime.now()
    date_now = datetime.datetime.today()
    week_now = calendar.day_name[date_now.weekday()]
    month_now = now.month
    day_now = now.day
    months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
    ]
    ordinals = [
        "1st",
        "2nd",
        "3rd",
        "4th",
        "5th",
        "6th",
        "7th",
        "8th",
        "9th",
        "10th",
        "11th",
        "12th",
        "13th",
        "14th",
        "15th",
        "16th",
        "17th",
        "18th",
        "19th",
        "20th",
        "21st",
        "22nd",
        "23rd",
        "24th",
        "25th",
        "26th",
        "27th",
        "28th",
        "29th",
        "30th",
        "31st"
    ]
    return f'Today is {week_now}, {months[month_now-1]} the {ordinals[day_now-1]}.'


def note(command):
    date = datetime.datetime.now()
    file_name = str(date).replace(":", "-") + "-note.txt"
    with open(file_name, "w") as f:
        f.write(command)
        os.startfile(file_name)


def send_mail(receiver, subject, message):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.starttls()
    server.login('bommireddyniharika@gmail.com', 'niharikareddy')
    email = EmailMessage()
    email['From'] = 'bommireddyniharikagmail.com'
    email['To'] = receiver
    email['Subject'] = subject
    email.set_content(message)
    server.send_message(email)
    server.close()


def run_assistant():
    command = record_audio()
    if 'play' in command:
        song = command.replace('play', '')
        talk('playing'+song)
        pwk.playonyt(song)
    elif 'time' in command:
        time = datetime.datetime.now().strftime('%H : %M')
        print(time)
        talk('Current time is ' + time)
    elif 'who is' in command:
        person = command.replace('who is', '')
        info = wikipedia.summary(person, 2)
        print(info)
        talk(info)
    elif 'shutdown system' in command or 'system shutdown' in command or 'shutdown' in command:
        os.system("shutdown /y")
    elif "date" in command or "day" in command or "month" in command:
        get_today = today_date()
        print(get_today)
        talk(get_today)
    elif "what is your name" in command:
        print("My name is assistant your assistant")
        talk("My name is assistant your assistant")
    elif "what is your age" in command:
        print("old enough to use internet.")
        talk("old enough to use internet.")
    elif "where do you live" in command:
        print("I am stuck inside a device.")
        talk("I am stuck inside a device.")
    elif 'joke' in command:
        joke = pyjokes.get_joke()
        print(joke)
        talk(joke)
    elif 'read pdf' in command:
        book = open(input("Enter the pdf name:"), 'rb')
        pdf_reader = PyPDF2.PdfFileReader(book)
        pages = pdf_reader.numPages
        speaker = pyttsx3.init()
        n = int(input("Enter the page number :"))
        talk("Enter the page number :")
        for num in range(n, pages):
            page = pdf_reader.getPage(n)
            text = page.extractText()
            speaker.say(text)
            print(text)
            speaker.runAndWait()
    elif 'email' in command or 'gmail' in command or 'mail' in command:
        receiver = input("Enter the receiver address:")
        talk("tell me the subject")
        subject = record_audio()
        talk("tell me the message")
        message = record_audio()
        send_mail(receiver, subject, message)
        talk('message send')
    elif 'hi' in command or 'hello' in command or 'hey' in command or 'hai' in command:
        speak = say_hello()
        print(speak)
        talk(speak)
    elif 'i love you' in command:
        print('thank you so much.I am happy to listen it.')
        talk('thank you so much.I am happy to listen it.')
    elif 'do you love me' in command:
        print('I am nothing with out you')
        talk('I am nothing with out you')
    elif 'are you single' in command:
        print('I am still waiting for the right electronic device to steal my heart. ')
        talk('I am still waiting for the right electronic device to steal my heart. ')
    elif 'who are you' in command or 'define yourself' in command:
        print('I am your assistant. And I am here to help you')
        talk('I am your assistant. And I am here to help you')
    elif 'how are you' in command:
        print('I am feeling positively tip top thanks.And what about you?')
        talk('I am feeling positively tip top thanks.And what about you?')
        ans = record_audio()
        if 'good' in ans or 'fine' in ans:
            print("I am happy to listen this.")
            talk("I am happy to listen this.")
        elif 'bad' in ans or 'not good' in ans:
            print("OH,I am sorry.")
            talk("OH,I am sorry")
    elif 'thank you' in command or 'thanks' in command:
        print('You are always welcome and I am here for you.')
        talk('You are always welcome and I am here for you.')
    elif 'open' in command:
        if 'chrome' in command:
            print('Opening google chrome.')
            talk('opening google chrome.')
            os.startfile(
                r'C:\Program Files\Google\Chrome\Application\chrome.exe'
            )
        elif 'word' in command:
            print('Opening word.')
            talk('Opening word.')
            os.startfile(
                r"C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE"
            )
        elif 'excel' in command:
            print('Opening Excel.')
            talk('opening excel.')
            os.startfile(
                r"C:\Program Files\Microsoft Office\root\Office16\EXCEL.EXE"
            )
        elif 'youtube' in command:
            print('Opening youtube.')
            talk('Opening youtube.')
            webbrowser.open("https://youtube.com/")
        elif 'google meet' in command:
            print('Opening google meet.')
            talk('Opening google meet.')
            webbrowser.open("https://meet.google.com/")
        elif 'dominos' in command:
            print('Opening dominos.')
            talk('Opening dominos.')
            webbrowser.open("https://pizzaonline.dominos.co.in/")
        elif 'wiki' in command or 'viki' in command:
            print("Opening viki")
            talk("opening viki")
            webbrowser.open("https://www.viki.com/apps")
        elif 'amazon' in command:
            print("opening amazon.")
            talk("opening amazon")
            webbrowser.open("https://www.amazon.in")
        elif 'facebook' in command:
            print('Opening facebook.')
            talk('Opening facebook.')
            webbrowser.open("https://facebook.com/")
        elif 'store' in command:
            print("Opening play store.")
            talk("Opening play store")
            webbrowser.open("https://play.google.com/store/apps")
        elif 'google' in command:
            print('Opening google.')
            talk('Opening google.')
            webbrowser.open("https://google.com")
        elif 'myntra' in command:
            print("Opening Myntra.")
            talk("Opening Myntra")
            webbrowser.open("https://www.myntra.com")
        elif 'duo' in command:
            print("Opening google duo.")
            talk("Opening google duo")
            webbrowser.open("https://duo.google.com/about")
        elif 'hotstar' in command:
            print("Opening hotstar")
            talk("Opening hotstar")
            webbrowser.open("https://www.hotstar.com/in")
        elif 'zomato' in command:
            print("Opening zomato")
            talk("Opening zomato")
            webbrowser.open("https://www.zomato.com/face")
        elif 'swiggy' in command:
            print("Opening Swiggy")
            talk("Opening swiggy")
            webbrowser.open("https://www.swiggy.com")
        elif 'paytm' in command:
            print("Opening paytm.")
            talk("Opening paytm")
            webbrowser.open("https://paytm.com")
        elif 'netflix' in command:
            print("Opening Netflix.")
            talk("opening netflix")
            webbrowser.open("https://www.netflix.com")
        elif 'irctc' in command:
            print("Opening IRCTC")
            talk("Opening IRCTC")
            webbrowser.open("https://www.irctc.co.in")
        elif 'snapdeal' in command:
            print("Opening snapdeal")
            talk("Opening snapdeal")
            webbrowser.open("https://www.snapdeal.com/")
        elif 'prime' in command:
            print("Opening Amazon Prime")
            talk("Opening Amazon prime")
            webbrowser.open("https://www.primevideo.com")
        elif 'flipkart' in command:
            print("Opening flipkart.")
            talk("Opening flipkart.")
            webbrowser.open("https://www.flipkart.com")
        elif 'pinterest' in command:
            print("Opening pinterest")
            talk("Opening pinterest")
            webbrowser.open("https://www.pinterest.com")
        elif 'powerpoint' in command:
            print("Opening power point")
            talk("Opening power point")
            os.startfile(r"C:\Program Files\Microsoft Office\root\Office16\POWERPNT.EXE")
        elif 'c' in command or 'si' in command:
            print("Opening C.")
            talk("Opening C.")
            os.startfile(r'c:')
        elif 'notepad' in command:
            print("Opening Notepad.")
            talk("Opening notepad")
            os.startfile(r'Notepad.exe')
    elif 'search' in command:
        ind = command.split().index("search")
        search = command.split()[ind+1:]
        webbrowser.open("https://www.google.com/search?q=" + " + ".join(search))
        talk('This are the results.')
    elif 'empty recycle bin' in command:
        winshell.recycle_bin().empty(confirm=True, show_progress=False, sound=True)
        print('Recycle Bin emptied')
        talk('Recycle bin emptied.')
    elif 'change background' in command or 'change wallpaper' in command:
        img = r"C:\Users\nihar\Desktop\wallpapers"
        list_img = os.listdir(img)
        img_choice = random.choice(list_img)
        ran = os.path.join(img, img_choice)
        ctypes.windll.user32.SystemParametersInfoW(20, 0, ran, 0)
        talk("wallpaper changed successfully.")
    elif 'note' in command or 'remember this' in command:
        talk("what do you like me to write down?")
        note_text = record_audio()
        note(note_text)
        talk("noted successfully.")
    elif 'where' in command:
        ind = command.lower().split().index("is")
        location = command.split()[ind+1:]
        url = "https://www.google.com/maps/place/"+"".join(location)
        webbrowser.open(url)
        talk("This is where the location of  " + str(location) + "is :")
    elif 'thank you' in command or 'thanks' in command:
        print("You are welcome.It is my duty to help you.")
        talk("You are welcome .It is my duty to help you.")
    elif 'what are you doing' in command:
        print("I was just coming up with compliment to give you, like this one :")
        talk("I was just coming up with compliment to give you like this one")
        print("There are roughly 100 billion trillion stars in the universe and you are brighter than them all.")
        talk("there are roughly 100 billion trillion stars in the universe and you are brighter than them all.")
    elif 'who am i' in command:
        print("probably a human.")
        talk("probably a human.")
    elif "explore" in command and 'store' in command:
        ind = command.split().index("explore")
        command = command.replace('in store', '')
        search = command.split()[ind+1:]
        webbrowser.open("https://play.google.com/store/search?q=" + " + ".join(search)+"&c=apps")
        talk("This are the results")
    elif 'why do you exist' in command:
        print("I made to help you.")
        talk("I made to help you.")
    elif 'shall i leave' in command or 'i am going to leave' in command:
        print("okay bye bye. come again")
        talk("Okay bye bye. come again.")
    elif 'get male voice' in command:
        voices = engine.getProperty('voices')
        engine.setProperty('voice', voices[0].id)
        print("Now I am your assistant")
        talk("Now i am your assistant")
    elif 'get female voice' in command:
        voices = engine.getProperty('voices')
        engine.setProperty('voice', voices[1].id)
        print("Now I am your assistant")
        talk("Now i am your assistant")
    elif 'exit' in command:
        talk("exiting")
        exit()
    else:
        talk('please say the command again.')


while True:
    run_assistant()
