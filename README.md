from kivy.app import App
from kivy.uix.boxlayout import BoxLayout

from kivy.uix.image import Image
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput


from gtts import gTTS
import pygame
import time
import os

pygame.mixer.init()

def speak(text):
    tts = gTTS(text=text, lang='en')
    tts.save("voice.mp3")
    pygame.mixer.music.load("voice.mp3")
    pygame.mixer.music.play()
    while pygame.mixer.music.get_busy():
        time.sleep(0.3)
    os.remove("voice.mp3")

class AIBox(BoxLayout):


    def __init__(self, **kwargs):
        super().__init__(orientation='vertical', **kwargs)

        self.avatar = Image(source='idle.png')  # Start with idle image
        self.add_widget(self.avatar)

        self.input_text = TextInput(hint_text="Say something...")
        self.add_widget(self.input_text)

        self.button = Button(text="Talk")
        self.button.bind(on_press=self.react)
        self.add_widget(self.button)

    def react(self, instance):
        text = self.input_text.text.strip()
        if text:
            self.avatar.source = 'talking.png'  # Show talking pose
            self.avatar.reload()
            speak(text)
            self.avatar.source = 'idle.png'  # Return to idle
            self.avatar.reload()
            self.input_text.text = ""

class AIGirlApp(App):
    def build(self):
        return AIBox()

if __name__ == '__main__':
    AIGirlApp().run()
