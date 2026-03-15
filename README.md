# Semáforo com Botão para Pedestres

Este projeto simula um **semáforo para veículos com controle de travessia de pedestres**, desenvolvido no **Tinkercad** e implementável em Arduino real.  

O objetivo é ensinar conceitos de **lógica de programação, controle de fluxo, entradas digitais e saídas digitais** usando LEDs e um botão, simulando o funcionamento de um semáforo urbano.  

Quando o **botão de pedestre é pressionado**, o semáforo muda de fase, permitindo a travessia segura do pedestre, enquanto os carros aguardam no vermelho. O sistema também possui uma fase em que o LED do pedestre **pisca**, indicando que o tempo de travessia está terminando.

---

## 🔗 Projeto no Tinkercad

A versão interativa do circuito pode ser acessada aqui:

➡️ https://www.tinkercad.com/things/fR8i2OtD7hb-semaforo-com-botao-para-pedestres

---

## 🧩 Componentes

**Componentes eletrônicos**
- Arduino Uno
- Protoboard
- 3 LEDs para carros (vermelho, amarelo, verde)
- 2 LEDs para pedestres (vermelho, verde)
- 1 botão de pressão
- Resistores de 220 Ω (para todos os LEDs)
- Jumpers (fios de ligação)

---

## 🔌 Diagrama de ligação

**LEDs Semáforo de Carros**
- Pino 10 → LED vermelho  
- Pino 9 → LED amarelo  
- Pino 8 → LED verde  

**LEDs Semáforo de Pedestres**
- Pino 3 → LED vermelho  
- Pino 2 → LED verde  

**Botão**
- Pino 7 → botão de pedestre  

Todos os terminais negativos dos LEDs vão ao **GND** do Arduino. É recomendado usar resistores de 220 Ω em cada LED e, para estabilidade, resistor pull-down ou `INPUT_PULLUP` no botão.

---

## 🧠 Lógica do programa

O Arduino executa um **loop contínuo** com detecção do botão, controlando as fases do semáforo:

1. **Fase 1 — Verde carros / Vermelho pedestre**  
2. **Fase 2 — Amarelo carros / Verde pedestre**  
3. **Fase 3 — Vermelho carros / Verde pedestre**  
4. **Fase 4 — Vermelho carros / Pedestre vermelho piscando**  

A cada pressão do botão, o semáforo avança para a próxima fase, retornando à fase 1 após completar a sequência. A fase 4 simula o LED piscando do pedestre, indicando que o tempo de travessia está acabando.

---

## Código Arduino

```cpp
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
