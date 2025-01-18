import kivy
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput
from kivy.uix.image import Image
from kivy.uix.scrollview import ScrollView
from kivy.graphics.texture import Texture
from kivy.clock import Clock
from bleak import BleakClient
import asyncio
from PIL import Image as PILImage, ImageDraw, ImageFont
import numpy as np
from datetime import datetime
import requests
from kivy.uix.popup import Popup
from kivy.uix.gridlayout import GridLayout

# Reemplaza con tu API Key de JSONbin
api_key = "$2a$10$awNadPbbdJ8TWvO2TVYBx.HcALLpvhuWejF50qx/pBAU7EjGf8G8O"
bin_id = "67897c6e1ea5ae6cf028ff8a"

direccion_impresora = "60:6E:41:75:FE:BD"
characteristic_uuid = "0000ff02-0000-1000-8000-00805f9b34fb"

logos = [
    ("ALFAROMERO", "assets/ALFAROMERO.JPG"),
    ("AUDI", "assets/AUDI.JPG"),
    ("BAIC", "assets/BAIC.JPG"),
    ("BMW", "assets/BMW.JPG"),
    ("BYD", "assets/BYD.JPG"),
    ("BRILLIANCE", "assets/BRILLIANCE.JPG"),
    ("CHANGAN", "assets/CHANGAN.JPG"),
    ("CHERY", "assets/CHERY.JPG"),
    ("CHEVROLET", "assets/CHEVROLET.JPG"),
    ("CITROEN", "assets/CITROEN.JPG"),
    ("CORVETTE", "assets/CORVETTE.JPG"),
    ("CUPRA", "assets/CUPRA.JPG"),
    ("DAEWOO-1", "assets/DAEWOO-1.JPG"),
    ("DAEWOO-2", "assets/DAEWOO-2.JPG"),
    ("DAF", "assets/DAF.JPG"),
    ("DAIHATSU", "assets/DAIHATSU.JPG"),
    ("DFM", "assets/DFM.JPG"),
    ("DFSK", "assets/DFSK.JPG"),
    ("DOGDE", "assets/DOGDE.JPG"),
    ("DONGFENG", "assets/DONGFENG.JPG"),
    ("FAW", "assets/FAW.JPG"),
    ("FIAT", "assets/FIAT.JPG"),
    ("FORD", "assets/FORD.JPG"),
    ("FOTON", "assets/FOTON.JPG"),
    ("FREIGHTLINER", "assets/FREIGHTLINER.JPG"),
    ("GAC", "assets/GAC.JPG"),
    ("GMC", "assets/GMC.JPG"),
    ("GREAT WALL", "assets/GREAT WALL.JPG"),
    ("HAVAL", "assets/HAVAL.JPG"),
    ("HINO", "assets/HINO.JPG"),
    ("HONDA", "assets/HONDA.JPG"),
    ("HUMMER", "assets/HUMMER.JPG"),
    ("HYUNDAI", "assets/HYUNDAI.JPG"),
    ("INTERNATIONAL", "assets/INTERNATIONAL.JPG"),
    ("IVECO", "assets/IVECO.JPG"),
    ("JAC", "assets/JAC.JPG"),
    ("JAGUAR", "assets/JAGUAR.JPG"),
    ("JEEP", "assets/JEEP.JPG"),
    ("JETOUR", "assets/JETOUR.JPG"),
    ("JIM", "assets/JIM.JPG"),
    ("JMC", "assets/JMC.JPG"),
    ("KAIYI", "assets/KAIYI.JPG"),
    ("KARRY", "assets/KARRY.JPG"),
    ("KIA", "assets/KIA.JPG"),
    ("KIA NUEVO", "assets/KIA NUEVO.JPG"),
    ("KYC", "assets/KYC.JPG"),
    ("LAND ROVER", "assets/LAND ROVER.JPG"),
    ("LEXUS", "assets/LEXUS.JPG"),
    ("LIFAN", "assets/LIFAN.JPG"),
    ("MACK", "assets/MACK.JPG"),
    ("MAHINDRA", "assets/MAHINDRA.JPG"),
    ("MERCEDEZ", "assets/MERCEDEZ.JPG"),
    ("MG", "assets/MG.JPG"),
    ("MINI", "assets/MINI.JPG"),
    ("NISSAN", "assets/NISSAN.JPG"),
    ("OPEL", "assets/OPEL.JPG"),
    ("PETERBILT", "assets/PETERBILT.JPG"),
    ("PEUGEOT-1", "assets/PEUGEOT-1.JPG"),
    ("PEUGEOT-2", "assets/PEUGEOT-2.JPG"),
    ("PORSHE", "assets/PORSHE.JPG"),
    ("PROTON", "assets/PROTON.JPG"),
    ("RENAULT", "assets/RENAULT.JPG"),
    ("RAM", "assets/RAM.JPG"),
    ("SCANIA", "assets/SCANIA.JPG"),
    ("SEAT", "assets/SEAT.JPG"),
    ("SHACMAN", "assets/SHACMAN.JPG"),
    ("SKODA", "assets/SKODA.JPG"),
    ("SSANGYONG", "assets/SSANGYONG.JPG"),
    ("SUBARU", "assets/SUBARU.JPG"),
    ("TATA", "assets/TATA.JPG"),
    ("TOYOTA", "assets/TOYOTA.JPG"),
    ("UAZ", "assets/UAZ.JPG"),
    ("VOLKSWAGEN", "assets/VOLKSWAGEN.JPG"),
    ("VOLVO", "assets/VOLVO.JPG")
]

# Definir los usuarios y sus contraseñas
usuarios = {
    "Admin": "1212",
    "Juan Pablo": "lmnop7",
    "Yahir": "234abc",
    "NOX": "zxy321",
    "qwerty": "456abc",
}

def verificar_usuario(usuario, contrasena):
    return usuarios.get(usuario) == contrasena

def registrar_impresion(usuario, patente, logo):
    url = "https://api.jsonbin.io/v3/b"
    headers = {
        "Content-Type": "application/json",
        "X-Master-Key": api_key  
    }
    data = {
        "usuario": usuario,
        "patente": patente,
        "logo": logo,
        "fecha": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }

    try:
        response = requests.post(url, headers=headers, json=data)
        response.raise_for_status()  
        bin_data = response.json()
        print(bin_data)
    except requests.exceptions.HTTPError as errh:
        print(f"Error HTTP: {errh}")
    except requests.exceptions.ConnectionError as errc:
        print(f"Error de conexión: {errc}")
    except requests.exceptions.Timeout as errt:
        print(f"Error de tiempo de espera: {errt}")
    except requests.exceptions.RequestException as err:
        print(f"Error en la solicitud: {err}")
    except requests.exceptions.JSONDecodeError as errj:
        print(f"Error al decodificar JSON: {errj}")

def imagen_a_escpos(imagen):
    imagen = imagen.convert("1")
    pixels = np.array(imagen)

    escpos_bytes = b'\x1D\x76\x30\x00'
    ancho = (imagen.width + 7) // 8
    alto = imagen.height
    escpos_bytes += ancho.to_bytes(2, 'little')
    escpos_bytes += alto.to_bytes(2, 'little')

    for y in range(alto):
        for x in range(0, ancho * 8, 8):
            byte = 0
            for bit in range(8):
                if x + bit < imagen.width and pixels[y, x + bit] == 0:
                    byte |= 1 << (7 - bit)
            escpos_bytes += bytes([byte])

    return escpos_bytes

async def imprimir_imagen(imagen, usuario, patente, logo):
    try:
        print("Estableciendo conexión con la impresora Bluetooth...")
        async with BleakClient(direccion_impresora, timeout=30.0) as client:
            print("Conexión establecida con la impresora Bluetooth")

            if not client.is_connected:
                print("No se pudo establecer la conexión con la impresora")
                return

            await client.get_services()
            await client.write_gatt_char(characteristic_uuid, b'\x1B\x40', response=False)
            await asyncio.sleep(2)

            imagen_bytes = imagen_a_escpos(imagen)
            datos_completos = b'\x1B\x40' + imagen_bytes + b'\x0A\x0A\x1B\x40' + b'\x1D\x56\x42\x00'

            max_tamano_bloque = 203
            bloques = [datos_completos[i:i + max_tamano_bloque] for i in range(0, len(datos_completos), max_tamano_bloque)]

            for bloque in bloques:
                await client.write_gatt_char(characteristic_uuid, bloque, response=False)
                await asyncio.sleep(0.1)

            registrar_impresion(usuario, patente, logo)
    except Exception as e:
        print(f"Error al imprimir: {e}")

class MyApp(App):
    def build(self):
        self.login_screen = BoxLayout(orientation='vertical', padding=10, spacing=10)
        self.login_label = Label(text="Inicio de Sesión", font_size=36)
        self.login_screen.add_widget(self.login_label)

        self.usuario_input = TextInput(hint_text='Usuario', multiline=False, font_size=20)
        self.login_screen.add_widget(self.usuario_input)

        self.contrasena_input = TextInput(hint_text='Contraseña', multiline=False, password=True, font_size=20)
        self.login_screen.add_widget(self.contrasena_input)

        self.login_button = Button(text="Iniciar Sesión", on_press=self.login, size_hint_y=None, height=50)
        self.login_screen.add_widget(self.login_button)

        return self.login_screen

    def login(self, instance):
        usuario = self.usuario_input.text.strip()
        contrasena = self.contrasena_input.text.strip()
        if verificar_usuario(usuario, contrasena):
            self.usuario = usuario  # Guardar usuario autenticado
            self.root.clear_widgets()
            self.connect_to_printer()  # Conectar a la impresora al iniciar sesión
            if usuario == "Admin":
                self.root.add_widget(self.admin_screen())  # Mostrar panel de administración
            else:
                self.root.add_widget(self.main_screen())  # Mostrar pantalla principal
        else:
            self.login_label.text = "Usuario o contraseña incorrectos. Intente nuevamente."

    async def connect_to_printer(self):
        try:
            print("Conectando a la impresora Bluetooth...")
            async with BleakClient(direccion_impresora, timeout=30.0) as client:
                print("Conexión establecida con la impresora Bluetooth")
        except Exception as e:
            print(f"Error al conectar con la impresora: {e}")

    def main_screen(self):
        self.imagen_preview = None
        self.logo_seleccionado = ""

        main_layout = BoxLayout(orientation='horizontal', padding=10, spacing=10)

        # ScrollView para los logos
        scroll_view = ScrollView(size_hint=(0.3, 1))
        logo_layout = BoxLayout(orientation='vertical', size_hint_y=None)
        logo_layout.bind(minimum_height=logo_layout.setter('height'))

        for nombre_logo, logo_path in logos:
            logo_box = BoxLayout(size_hint_y=None, height=50)
            logo_button = Button(text=nombre_logo, size_hint=(0.7, 1))
            logo_button.logo_path = logo_path
            logo_button.bind(on_press=self.on_logo_select)
            logo_box.add_widget(logo_button)

            # Cargar y mostrar la imagen del logo
            logo_image = PILImage.open(logo_path).resize((50, 50))
            logo_image.save(f'temp_{nombre_logo}.png')
            logo_img = Image(source=f'temp_{nombre_logo}.png', size_hint=(0.3, 1))
            logo_box.add_widget(logo_img)

            logo_layout.add_widget(logo_box)

        scroll_view.add_widget(logo_layout)
        main_layout.add_widget(scroll_view)

        # Layout principal
        layout = BoxLayout(orientation='vertical', padding=10, spacing=10)

        self.label = Label(text="GRABAPRIME", font_size=36, size_hint_y=None, height=50)
        layout.add_widget(self.label)

        self.text_input = TextInput(hint_text='Ingrese el texto para imprimir', multiline=True, font_size=20, size_hint_y=None, height=100)
        layout.add_widget(self.text_input)

        self.generate_button = Button(text="Generar Previsualización", on_press=self.generate_preview, size_hint_y=None, height=50)
        layout.add_widget(self.generate_button)

        self.image_display = Image(size_hint_y=None, height=300)
        layout.add_widget(self.image_display)

        # Botón de flecha para cambiar formato de impresión
        self.arrow_button = Button(text="→", on_press=self.change_format, size_hint_y=None, height=50)
        layout.add_widget(self.arrow_button)

        self.print_button = Button(text="Imprimir", on_press=self.print_image, size_hint_y=None, height=50)
        layout.add_widget(self.print_button)

        main_layout.add_widget(layout)

        # Ejecutar el bucle de eventos asyncio en Kivy
        Clock.schedule_interval(self.run_asyncio_events, 0)

        return main_layout

    def on_logo_select(self, instance):
        self.logo_seleccionado = instance.logo_path.split("\\")[-1].split(".")[0]  # Guardar el nombre del logo seleccionado
        logo_image = PILImage.open(instance.logo_path).convert("RGBA").resize((150, 150))
        self.imagen_preview = logo_image
        self.update_image_display(logo_image)  # Mostrar el logo seleccionado

    def generate_preview(self, instance):
        texto = self.text_input.text.strip()
        if texto and self.imagen_preview:
            imagen = PILImage.new('RGBA', (600, 400), color='white')
            draw = ImageDraw.Draw(imagen)
            try:
                fuente = ImageFont.truetype("arial.ttf", 74)
            except IOError:
                fuente = ImageFont.load_default()

            logo_resized = self.imagen_preview.resize((150, 150))
            temp_image = PILImage.new('RGBA', logo_resized.size)
            temp_image.paste(logo_resized, (0, 0), logo_resized)

            imagen.paste(temp_image, ((imagen.width - logo_resized.width) // 2, 10), temp_image)

            text_bbox = draw.textbbox((0, 0), texto, font=fuente)
            text_width = text_bbox[2] - text_bbox[0]
            text_height = text_bbox[3] - text_bbox[1]
            text_x = (imagen.width - text_width) // 2
            text_y = (imagen.height - text_height - 50) // 2
            draw.text((text_x, text_y), texto, font=fuente, fill="black")

            # Reflejar la imagen horizontalmente (espejo)
            imagen = imagen.transpose(PILImage.FLIP_LEFT_RIGHT)
            imagen = imagen.rotate(90, expand=True)

            self.imagen_preview = imagen
            self.update_image_display(imagen)
        else:
            print("Por favor, ingrese algún texto para generar la previsualización")

    def change_format(self, instance):
        texto = self.text_input.text.strip()
        if texto:
            # Generar previsualización sin logo, con letra más grande, espejo y rotado 90 grados
            imagen = PILImage.new('RGBA', (600, 400), color='white')
            draw = ImageDraw.Draw(imagen)
            try:
                fuente = ImageFont.truetype("arial.ttf", 90)  # Tamaño de letra más grande
            except IOError:
                fuente = ImageFont.load_default()

            text_bbox = draw.textbbox((0, 0), texto, font=fuente)
            text_width = text_bbox[2] - text_bbox[0]
            text_height = text_bbox[3] - text_bbox[1]
            text_x = (imagen.width - text_width) // 2
            text_y = (imagen.height - text_height) // 2
            draw.text((text_x, text_y), texto, font=fuente, fill="black")

            # Reflejar la imagen horizontalmente (espejo) y rotar 90 grados
            imagen = imagen.transpose(PILImage.FLIP_LEFT_RIGHT).rotate(90, expand=True)

            self.imagen_preview = imagen
            self.update_image_display(imagen)
        else:
            print("Por favor, ingrese algún texto para generar la previsualización")

    def update_image_display(self, pil_image):
        pil_image = pil_image.convert('RGBA')
        w, h = pil_image.size
        texture = Texture.create(size=(w, h))
        texture.blit_buffer(pil_image.tobytes(), colorfmt='rgba', bufferfmt='ubyte')
        texture.flip_vertical()
        self.image_display.texture = texture

    def print_image(self, instance):
        if self.imagen_preview:
            print("Iniciando el proceso de impresión...")
            asyncio.ensure_future(imprimir_imagen(self.imagen_preview, self.usuario, self.text_input.text.strip(), self.logo_seleccionado))
        else:
            print("Por favor, cargue una imagen para imprimir")

    def run_asyncio_events(self, dt):
        loop = asyncio.get_event_loop()
        loop.stop()
        loop.run_forever()

    def admin_screen(self):
        admin_layout = BoxLayout(orientation='vertical', padding=10, spacing=10)

        # Título
        admin_label = Label(text="Panel de Administración", font_size=36)
        admin_layout.add_widget(admin_label)

        # Lista de usuarios
        self.user_boxes = []  # Para almacenar las cajas de usuario
        for usuario in usuarios.keys():
            user_box = BoxLayout(size_hint_y=None, height=50)
            user_label = Label(text=usuario, size_hint=(0.7, 1))
            delete_button = Button(text="Borrar", size_hint=(0.15, 1))
            change_button = Button(text="Cambiar Contraseña", size_hint=(0.15, 1))

            delete_button.bind(on_press=lambda instance, u=usuario: self.borrar_usuario(u))
            change_button.bind(on_press=lambda instance, u=usuario: self.cambiar_contrasena(u))

            user_box.add_widget(user_label)
            user_box.add_widget(delete_button)
            user_box.add_widget(change_button)
            admin_layout.add_widget(user_box)
            self.user_boxes.append(user_box)  # Agregar a la lista

        return admin_layout

    def borrar_usuario(self, usuario):
        if usuario in usuarios:
            del usuarios[usuario]
            print(f"Usuario {usuario} borrado.")
            self.actualizar_interfaz()  # Actualizar la interfaz

    def cambiar_contrasena(self, usuario):
        # Crear un diálogo para cambiar la contraseña
        layout = GridLayout(cols=1, padding=10)
        self.nueva_contrasena_input = TextInput(hint_text='Nueva Contraseña', multiline=False, password=True)
        layout.add_widget(self.nueva_contrasena_input)

        cambiar_button = Button(text="Cambiar Contraseña")
        cambiar_button.bind(on_press=lambda instance: self.confirmar_cambio_contrasena(usuario))
        layout.add_widget(cambiar_button)

        popup = Popup(title=f"Cambiar Contraseña para {usuario}", content=layout, size_hint=(0.8, 0.4))
        popup.open()

    def confirmar_cambio_contrasena(self, usuario):
        nueva_contrasena = self.nueva_contrasena_input.text.strip()
        if nueva_contrasena:
            usuarios[usuario] = nueva_contrasena
            print(f"Contraseña de {usuario} cambiada a {nueva_contrasena}.")
            self.actualizar_interfaz()  # Actualizar la interfaz
        else:
            print("La nueva contraseña no puede estar vacía.")

    def actualizar_interfaz(self):
        # Limpiar la interfaz actual y volver a construirla
        self.root.clear_widgets()
        if self.usuario == "Admin":
            self.root.add_widget(self.admin_screen())
        else:
            self.root.add_widget(self.main_screen())

if __name__ == "__main__":
    MyApp().run()
