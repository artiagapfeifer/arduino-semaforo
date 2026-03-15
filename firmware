int pinVermelho = 10;
int pinAmarelo = 9;
int pinVerde = 8;
int pinBotao = 7;
int pinPedestreVermelho = 3;
int pinPedestreVerde = 2;

int faseSemaforo;

int estadoBotao;
int estadoAnteriorBotao;

int tempoPisca;
int estadoPisca;

void setup() {
  pinMode(pinVerde, OUTPUT);
  pinMode(pinAmarelo, OUTPUT);
  pinMode(pinVermelho, OUTPUT);
  pinMode(pinBotao, INPUT);
  pinMode(pinPedestreVerde, OUTPUT);
  pinMode(pinPedestreVermelho, OUTPUT);

  faseSemaforo = 1;
  estadoAnteriorBotao = digitalRead(pinBotao);
}

void loop() {

  estadoBotao = digitalRead(pinBotao);
  
  tempoPisca = 0;
  estadoPisca = HIGH;

  // Detecta subida do botão (LOW → HIGH)
  if ((estadoBotao == HIGH) && (estadoAnteriorBotao == LOW)) {
    if (faseSemaforo < 4) {
      faseSemaforo++;
    } else {
      faseSemaforo = 1;
    }
  }

  estadoAnteriorBotao = estadoBotao;

  // FASE 1 — VERDE
  if (faseSemaforo == 1) {
    digitalWrite(pinVerde, HIGH);
    digitalWrite(pinAmarelo, LOW);
    digitalWrite(pinVermelho, LOW);

    digitalWrite(pinPedestreVerde, LOW);
    digitalWrite(pinPedestreVermelho, HIGH);
  }

  // FASE 2 — AMARELO
  if (faseSemaforo == 2) {
    digitalWrite(pinVerde, LOW);
    digitalWrite(pinAmarelo, HIGH);
    digitalWrite(pinVermelho, LOW);

    digitalWrite(pinPedestreVerde, HIGH);
    digitalWrite(pinPedestreVermelho, LOW);
  }

  // FASE 3 — VERMELHO
  if (faseSemaforo == 3) {
    digitalWrite(pinVerde, LOW);
    digitalWrite(pinAmarelo, LOW);
    digitalWrite(pinVermelho, HIGH);

    digitalWrite(pinPedestreVerde, HIGH);
    digitalWrite(pinPedestreVermelho, LOW);
  }
  
  if (faseSemaforo == 4) {   // SEMÁFORO PEDESTRE PISCANDO
    digitalWrite(pinVerde, LOW);
    digitalWrite(pinAmarelo, LOW);
    digitalWrite(pinVermelho, HIGH);
    
    tempoPisca = tempoPisca + 1;
    if (tempoPisca == 400) {
      estadoPisca = !estadoPisca;
      tempoPisca = 0;
    }

    digitalWrite(pinPedestreVerde, LOW);
    digitalWrite(pinPedestreVermelho, estadoPisca);
  }

  delay(1);
}
