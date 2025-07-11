# PRODIGY_AD_01
from kivy.app import App
from kivy.lang import Builder
from kivy.uix.popup import Popup
from kivy.uix.label import Label
from kivy.properties import BooleanProperty, ListProperty

KV = '''
BoxLayout:
    orientation: 'vertical'
    spacing: 10
    padding: 15
    canvas.before:
        Color:
            rgba: app.bg_color
        Rectangle:
            pos: self.pos
            size: self.size

    BoxLayout:
        size_hint_y: 0.1
        spacing: 10
        Label:
            text: "SMART CALCULATOR"
            font_size: 30
            bold: True
            color: app.text_color
        ToggleButton:
            text: "Light" if app.is_light_mode else "Dark"
            on_state: app.toggle_theme(self.state)
            size_hint_x: 0.2
            background_color: 0.9, 0.9, 0.9, 1

    TextInput:
        id: input_box
        font_size: 36
        size_hint_y: 0.15
        multiline: False
        halign: "right"
        valign: "middle"
        readonly: True
        background_color: 0.95, 0.95, 0.95, 1
        foreground_color: app.text_color
        padding: 15

    GridLayout:
        cols: 4
        spacing: 8
        size_hint_y: 0.65

        Button:
            text: "("
            on_press: input_box.text += self.text
        Button:
            text: ")"
            on_press: input_box.text += self.text
        Button:
            text: "DEL"
            on_press: input_box.text = input_box.text[:-1]
        Button:
            text: "C"
            on_press: input_box.text = ""

        Button:
            text: "7"
            on_press: input_box.text += self.text
        Button:
            text: "8"
            on_press: input_box.text += self.text
        Button:
            text: "9"
            on_press: input_box.text += self.text
        Button:
            text: "/"
            on_press: input_box.text += self.text

        Button:
            text: "4"
            on_press: input_box.text += self.text
        Button:
            text: "5"
            on_press: input_box.text += self.text
        Button:
            text: "6"
            on_press: input_box.text += self.text
        Button:
            text: "*"
            on_press: input_box.text += self.text

        Button:
            text: "1"
            on_press: input_box.text += self.text
        Button:
            text: "2"
            on_press: input_box.text += self.text
        Button:
            text: "3"
            on_press: input_box.text += self.text
        Button:
            text: "-"
            on_press: input_box.text += self.text

        Button:
            text: "."
            on_press: input_box.text += self.text
        Button:
            text: "0"
            on_press: input_box.text += self.text
        Button:
            text: "="
            on_press: app.calculate()
        Button:
            text: "+"
            on_press: input_box.text += self.text

    Button:
        text: " View History"
        size_hint_y: 0.1
        on_press: app.show_history()
'''

class CalculatorApp(App):
    is_light_mode = BooleanProperty(True)
    bg_color = ListProperty([1, 1, 1, 1])         # light background
    text_color = ListProperty([0, 0, 0, 1])       # dark text
    history = []

    def build(self):
        return Builder.load_string(KV)

    def toggle_theme(self, state):
        if state == "down":
            self.is_light_mode = False
            self.bg_color = [0.1, 0.1, 0.1, 1]     # dark background
            self.text_color = [1, 1, 1, 1]         # light text
        else:
            self.is_light_mode = True
            self.bg_color = [1, 1, 1, 1]
            self.text_color = [0, 0, 0, 1]

    def calculate(self):
        input_box = self.root.ids.input_box
        try:
            result = str(eval(input_box.text))
            self.history.append(f"{input_box.text} = {result}")
            input_box.text = result
        except:
            input_box.text = "Error"

    def show_history(self):
        content = "\n".join(self.history[-10:] or ["No history yet."])
        popup = Popup(title='Calculation History',
                      content=Label(text=content, font_size=18),
                      size_hint=(0.9, 0.6))
        popup.open()

if __name__ == '__main__':
    CalculatorApp().run()
