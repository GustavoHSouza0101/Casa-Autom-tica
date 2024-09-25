#include <Servo.h>
#include <MFRC522.h>
#include <SPI.h>


#define SS_PIN 10
#define RST_PIN 9


MFRC522 rfid(SS_PIN, RST_PIN);


Servo motor;
void setup() {
  Serial.begin(9600);
  SPI.begin();
  rfid.PCD_Init();


  motor.attach(2);
  motor.write(0);


}
void loop() {
  leituraRfid();
}
 


void leituraRfid(){
  if (!rfid.PICC_IsNewCardPresent() || !rfid.PICC_ReadCardSerial()) //VERIFICA SE O CARTÃO PRESENTE NO LEITOR É DIFERENTE DO ÚLTIMO CARTÃO LIDO. CASO NÃO SEJA, FAZ
    return; //RETORNA PARA LER NOVAMENTE
 
  String strID = "";
  for (byte i = 0; i < 4; i++) {
    strID +=
    (rfid.uid.uidByte[i] < 0x10 ? "0" : "") +
    String(rfid.uid.uidByte[i], HEX) +
    (i!=3 ? ":" : "");
  }
  strID.toUpperCase();


  if (strID.indexOf("27:41:AA:AB") >= 0) {
    motor.write(95);
    delay(3000);
    motor.write(0);
  }
 
  rfid.PICC_HaltA(); //PARADA DA LEITURA DO CARTÃO
  rfid.PCD_StopCrypto1(); //PARADA DA CRIPTOGRAFIA NO PCD
  }
