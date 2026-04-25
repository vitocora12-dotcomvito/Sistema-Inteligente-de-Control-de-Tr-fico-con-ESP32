# Sistema-Inteligente-de-Control-de-Tr-fico-con-ESP32

https://github.com/vitocora12-dotcomvito/Sistema-Inteligente-de-Control-de-Tr-fico-con-ESP32.git
//Sistema Inteligente de Control de Tráfico con ESP32
//Nombre: Victor Manuel Coraizaca Naula
//Simulacion: https://wokwi.com/projects/462226520448811009

// PINES
#define LED_ROJO     25
#define LED_AMARILLO 26
#define LED_VERDE    27
#define SENSOR       4


int contador = 0;
int estado = 0;

unsigned long tiempoAnterior = 0;


// SETUP

void setup() {
  Serial.begin(115200);

  pinMode(SENSOR, INPUT_PULLUP);
  pinMode(LED_VERDE, OUTPUT);
  pinMode(LED_ROJO, OUTPUT);
  pinMode(LED_AMARILLO, OUTPUT);
}

// LOOP

void loop() {

  // Simulación de 1 segundo
  if (millis() - tiempoAnterior >= 1000) {
    tiempoAnterior = millis();
    contador++;
  }

  bool vehiculo = digitalRead(SENSOR) == LOW;


  // LED VERDE
 
  if (estado == 0) {

    digitalWrite(LED_ROJO, LOW);
    digitalWrite(LED_AMARILLO, LOW);
    digitalWrite(LED_VERDE, HIGH);

    int tiempoVerde = vehiculo ? 8 : 5;

    if (contador >= tiempoVerde) {
      contador = 0;
      estado = 1;
      digitalWrite(LED_VERDE, LOW);
    }
  }


  // LED AMARILLO

  else if (estado == 1) {

    digitalWrite(LED_ROJO, LOW);
    digitalWrite(LED_AMARILLO, HIGH);
    digitalWrite(LED_VERDE, LOW);

    if (contador >= 2) {
      contador = 0;
      estado = 2;
    }
  }


  // LED ROJO

  else if (estado == 2) {

    digitalWrite(LED_ROJO, HIGH);
    digitalWrite(LED_AMARILLO, LOW);
    digitalWrite(LED_VERDE, LOW);

    if (contador >= 5) {
      contador = 0;
      estado = 0;
    }
  }
}
