# Proyectoarduino
Sistema de puerta inteligente 
```cpp
#include <Servo.h>

// Definición de pines
const int TrigPin = 9;
const int EchoPin = 8;
const int BuzzerPin = 10;
const int ServoPin = 11;

// Variables para lógica
long duracion;
int distancia;
Servo miServo;

void setup() {
pinMode(TrigPin, OUTPUT); // Pin de disparo del ultrasonido
pinMode(EchoPin, INPUT); // Pin de recepción del ultrasonido
pinMode(BuzzerPin, OUTPUT);

miServo.attach(ServoPin);
miServo.write(0); // Inicia en posición cerrada (0 grados)

Serial.begin(9600); // Para monitorear la distancia en la PC
}

void loop() {
// Generar el pulso del ultrasonido
digitalWrite(TrigPin, LOW);
delayMicroseconds(2);
digitalWrite(TrigPin, HIGH);
delayMicroseconds(10);
digitalWrite(TrigPin, LOW);

// Calcular la distancia
duracion = pulseIn(EchoPin, HIGH);
distancia = duracion * 0.034 / 2;

// Lógica de detección (10 cm)
if (distancia > 0 && distancia <= 10) {
miServo.write(90); // Abre el motor a 90 grados
digitalWrite(BuzzerPin, HIGH); // Enciende el buzzer
Serial.println("Objeto detectado: ABRIENDO");
}
else {
miServo.write(0); // Cierra el motor
digitalWrite(BuzzerPin, LOW); // Apaga el buzzer
Serial.println("Área despejada: CERRANDO");
}

delay(100); // Pequeña pausa para estabilidad del sensor             
