#include <Servo.h>
#include <PS2X_lib.h> 

Servo servoGarra;
Servo servoAntebraco;
Servo servoBraco;
Servo servoBase;
Servo garra3;

const int pinoServoGarra = 3;
const int pinoServoAntebraco = 7;
const int pinoServoBraco = 5;
const int pinoServoBase = 6;
const int pinoGarra3 = 4;

int posGarra = 90;
int posAntebraco = 90;
int posBraco = 90;
int posBase = 90;
int posGarra3 = 90;

PS2X ps2x;
int error = 0;
byte type = 0;
byte vibrate = 0;

void setup() {
    Serial.begin(57600);
  servoGarra.attach(pinoServoGarra);
  servoAntebraco.attach(pinoServoAntebraco);
  servoBraco.attach(pinoServoBraco);
  servoBase.attach(pinoServoBase);
  garra3.attach(pinoGarra3);
servoGarra.write(posGarra);
  servoAntebraco.write(posAntebraco);
  servoBraco.write(posBraco);
  servoBase.write(posBase);
  garra3.write(posGarra3);

    error = ps2x.config_gamepad(13, 11, 10, 12, true, true);

  if (error == 0) {
    Serial.println("Controle PS2 conectado com sucesso.");
  } else {
    Serial.println("Erro ao conectar o controle PS2.");
  }
type = ps2x.readType();
  switch (type) {
    case 0:
      Serial.println("Tipo de controle desconhecido");
      break;
    case 1:
      Serial.println("Controle DualShock encontrado");
      break;
    case 2:
      Serial.println("Controle GuitarHero encontrado");
      break;
  }
}

void loop() {
  if (error == 1) return;  // Se não houver controle conectado, sai do loop

  ps2x.read_gamepad(false, vibrate);  // Atualiza os dados do controle

  if (ps2x.Button(PSB_L1)) {
    posGarra = max(0, posGarra - 2);
    servoGarra.write(posGarra);
    Serial.print("Garra fechando: ");
    Serial.println(posGarra);
  }
  if (ps2x.Button(PSB_R1)) {
    posGarra = min(180, posGarra + 2);
    servoGarra.write(posGarra);
    Serial.print("Garra abrindo: ");
    Serial.println(posGarra);
  }

  if (ps2x.Button(PSB_TRIANGLE)) {
    posAntebraco = min(180, posAntebraco + 2);
    servoAntebraco.write(posAntebraco);
    Serial.print("Antebraço subindo: ");
    Serial.println(posAntebraco);
  } 
if (ps2x.Button(PSB_CROSS)) {
    posAntebraco = max(0, posAntebraco - 2);
    servoAntebraco.write(posAntebraco);
    Serial.print("Antebraço descendo: ");
    Serial.println(posAntebraco);
  }

  if (ps2x.Button(PSB_SQUARE)) {
    posBraco = max(0, posBraco - 2);
    servoBraco.write(posBraco);
    Serial.print("Braço recolhendo: ");
    Serial.println(posBraco);
  }
  if (ps2x.Button(PSB_CIRCLE)) {
    posBraco = min(180, posBraco + 2);
    servoBraco.write(posBraco);
    Serial.print("Braço estendendo: ");
    Serial.println(posBraco);
  }

    if (ps2x.Button(PSB_L2)) {
    posBase = max(0, posBase - 2);
    servoBase.write(posBase);
    Serial.print("Base girando para a esquerda: ");
    Serial.println(posBase);
  }
  if (ps2x.Button(PSB_R2)) {
    posBase = min(180, posBase + 2);
    servoBase.write(posBase);
    Serial.print("Base girando para a direita: ");
    Serial.println(posBase);
  }


  if (ps2x.Button(PSB_PAD_LEFT)) {
    posGarra3 = max(0, posGarra3 - 2);
    garra3.write(posGarra3);
    Serial.print("Garra3 fechando: ");
    Serial.println(posGarra3);
  }
  if (ps2x.Button(PSB_PAD_RIGHT)) {
    posGarra3 = min(180, posGarra3 + 2);
    garra3.write(posGarra3);
    Serial.print("Garra3 abrindo: ");
    Serial.println(posGarra3);
  }

  delay(20);
} 
