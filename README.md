# Talking-robot2
Python code for ropot talking arabic
import speech_recognition as sr
from gtts import gTTS
import playsound
import os

# التحدث بصوت عربي
def speak_arabic(text):
    print("الروبوت:", text)
    tts = gTTS(text=text, lang='ar')
    filename = "voice.mp3"
    tts.save(filename)
    playsound.playsound(filename)
    os.remove(filename)

# الاستماع من المايكروفون وتحويله إلى نص
def listen():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print(" الروبوت يستمع...")
        audio = r.listen(source)
    try:
        text = r.recognize_google(audio, language='ar-SA')  # العربية السعودية
        print(" المستخدم:", text)
        return text
    except sr.UnknownValueError:
        speak_arabic("لم أفهم ما قلت، هل يمكنك التكرار؟")
        return ""
    except sr.RequestError:
        speak_arabic("حدث خطأ في الاتصال بالإنترنت.")
        return ""

# ردود بسيطة
def get_response(user_input):
    if "مرحبا" in user_input or "أهلاً" in user_input:
        return "أهلاً وسهلاً! كيف يمكنني مساعدتك؟"
    elif "اسمك" in user_input:
        return "أنا روبوت ذكي، تم إنشائي بواسطة بايثون!"
    elif "كيف حالك" in user_input:
        return "أنا بخير، شكرًا لسؤالك!"
    elif "مع السلامة" in user_input or "وداعا" in user_input:
        return "مع السلامة! سعدت بالتحدث معك."
    else:
        return "لا أفهم تمامًا، هل يمكنك إعادة صياغة سؤالك؟"

# البرنامج الرئيسي
def main():
    speak_arabic("مرحبًا بك! تحدث الآن لأستمع إليك.")
    while True:
        user_input = listen()
        if user_input == "":
            continue
        if "خروج" in user_input or "مع السلامة" in user_input:
            speak_arabic("إلى اللقاء! تم إيقاف النظام.")
            break
        response = get_response(user_input)
        speak_arabic(response)

# تشغيل الروبوت
main()
