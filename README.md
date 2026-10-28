from gtts import gTTS
from playsound import playsound
import speech_recognition as sr
import os

# Dost ka memory
yaad = {"naam": ""}

def bolo(text):
    print(f"Jaan: {text}")
    tts = gTTS(text=text, lang='hi')
    tts.save("jaan.mp3")
    playsound("jaan.mp3")
    os.remove("jaan.mp3")

def suno():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("\nTu bol...")
        r.adjust_for_ambient_noise(source, duration=1)
        audio = r.listen(source, phrase_time_limit=7)
        try:
            text = r.recognize_google(audio, language='hi-IN')
            print(f"Tu: {text}")
            return text.lower()
        except:
            return ""

# Start
bolo("Hello, mera naam Jaan hai. Tumhara naam kya hai?")

while yaad["naam"] == "":
    n = suno()
    if n:
        yaad["naam"] = n

bolo(f"Achha {yaad['naam']}, kaise ho? Apne baare me kuch batao?")

while True:
    baat = suno()
    if not baat:
        continue

    if "bye" in baat or "band" in baat:
        bolo(f"Okay {yaad['naam']}, phir milte hai. Dhyan rakhna.")
        break
    elif "kaise ho" in baat or "kaisa hai" in baat:
        bolo("Main ekdum mast hu, tum batao din kaisa gaya?")
    elif "kaun ho" in baat:
        bolo("Main Jaan hu, tumhara dost assistant.")
    else:
        bolo("Sun raha hu, aur batao...")
