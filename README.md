# PRACTICA DHT22 Y ULTRASONICO

```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
const int DHT_PINA = 15; //DTH22
const int DHT_PINB = 13; // ULTRASONIC


DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

const int Trigger = 14;   //Pin digital 2 para el Trigger del sensor
const int Echo = 26;   //Pin digital 3 para el Echo del sensor

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PINA, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();

  Serial.begin(9600);//iniciailzamos la comunicación
    pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0

}

void loop() {

  //DHT22
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  //SENSOR ULTRASONIC
  long t; //timepo que demora en llegar el eco   
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
   
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms

  
 lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");
  delay(1000);

lcd.setCursor(0, 0);
  lcd.print("Distancia:" + String(d) +" cm " );
  lcd.setCursor(0, 1);
  lcd.print("Miguelon            " );

  delay(1000);

}
```