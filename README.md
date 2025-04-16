import kivy
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button
from kivy.uix.scrollview import ScrollView
from kivy.uix.label import Label
from datetime import datetime

class CaesarDecoderApp(App):
    def build(self):
        self.layout = BoxLayout(orientation='vertical', padding=10, spacing=10)
        self.input_text = TextInput(hint_text='أدخل النص المشفر هنا', size_hint_y=0.2)
        self.result_label = Label(size_hint_y=None, text_size=(400, None))
        self.result_label.bind(texture_size=self.resize_label)

        decode_button = Button(text='فك التشفير', size_hint_y=None, height=50)
        decode_button.bind(on_press=self.decode)

        scroll = ScrollView()
        scroll.add_widget(self.result_label)

        self.layout.add_widget(self.input_text)
        self.layout.add_widget(decode_button)
        self.layout.add_widget(scroll)

        return self.layout

    def resize_label(self, instance, size):
        self.result_label.height = size[1]

    def decode(self, instance):
        ciphertext = self.input_text.text
        results = ''
        current_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        results += f'وقت فك التشفير: {current_time}\n\n'
        
        for key in range(1, 26):
            decrypted = ''
            for char in ciphertext:
                if char.isalpha():
                    base = ord('A') if char.isupper() else ord('a')
                    decrypted += chr((ord(char) - base - key) % 26 + base)
                else:
                    decrypted += char
            results += f'المفتاح {key}: {decrypted}\n'
        self.result_label.text = results

if name == 'main':
    CaesarDecoderApp().run()
