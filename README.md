# Evaluacion-Unidad-II
# Código Evaluación en Python 
--------
import time


from machine import Pin, ADC, I2C, PWM

import ssd1306

from hcsr04 import HCSR04  #Librería del sensor HC-SR04

# Pines y configuraciones
potentiometer_pin = 34  # Pin analógico para el potenciómetro

led_red = PWM(Pin(25), freq=500)    # LED RGB Rojo

led_green = PWM(Pin(26), freq=500)  # LED RGB Verde

led_blue = PWM(Pin(27), freq=500)   # LED RGB Azul

trigger_pin = 5  # Pin Trigger del HC-SR04

echo_pin = 18    # Pin Echo del HC-SR04

i2c = I2C(scl=Pin(22), sda=Pin(21))  # Configuración de la pantalla OLED

# Inicialización de componentes

potentiometer = ADC(Pin(potentiometer_pin))

potentiometer.atten(ADC.ATTN_11DB)  

oled = ssd1306.SSD1306_I2C(128, 64, i2c, addr=0x3C)

sensor = HCSR04(trigger_pin=trigger_pin, echo_pin=echo_pin)

# Umbral de distancia para cambio de color
distance_threshold = 10  # Distancia en cm

def setup():

oled.fill(0)  # Limpia la pantalla
    
oled.text("Distancia y LED:", 0, 0)
    
oled.show()

def set_rgb_color(r, g, b):
  
  # Ajusta el color del LED RGB
  
  led_red.duty(r)
  
  led_green.duty(g)
  
  led_blue.duty(b)

def loop():
  
  # Lee el valor del potenciómetro
  
  pot_value = potentiometer.read()
    
  intensity = int(pot_value / 4095 * 1023)  # Escala el valor a rango PWM (0-1023)

  # Lee la distancia del sensor HC-SR04
  
  distance = sensor.distance_cm()  # Esta función devuelva la distancia correctamente

  # Muestra la distancia en la pantalla OLED
  
  oled.fill(0)
  
  oled.text("Distancia:", 0, 0)
  
  oled.text(f"{distance:.2f} cm", 0, 10)
  
  oled.show()

  # Control de intensidad y color del LED RGB
  
  if distance < distance_threshold:
  
  # Si está cerca, cambia el color del LED a rojo
  
  set_rgb_color(1023, 0, 0)  # Rojo a máxima intensidad
    
  else:
  
  # Si está lejos, muestra un color basado en el potenciómetro
        
  set_rgb_color(0, intensity, intensity)  # Cambia verde y azul según el potenciómetro

  time.sleep(0.1)  # Pequeña espera

# Inicializa y ejecuta
setup()

while True:

  loop()

# Video de Práctica Evaluación 
------------
https://drive.google.com/file/d/1Ze6oEwqJXbAhN7KM9dIZNQ2EQGhv7Wf8/view?usp=sharing 

# Videos de Actividades en Clase 
-------
https://drive.google.com/drive/folders/1D5nSi6YMQynpjUH-QFJvLfiUgCJiVds_?usp=drive_link 
# Examenes de Curso Python 
-------
