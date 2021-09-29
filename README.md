# melikehocavekodlama-RFID Kart Okutma

#include <Arduino.h>
#include <Wire.h>
#include <SoftwareSerial.h>
#include <SPI.h>
#include <MFRC522.h>

MFRC522 mfrc522(7,6); 




void setup() {
  pinMode(8, OUTPUT); 
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  Serial.begin(9600);
  SPI.begin(); 
  mfrc522.PCD_Init();
  
  Serial.println(String("RFID OKUYUCU"));
  Serial.println();
  

}

void loop() {
  if(!mfrc522.PICC_IsNewCardPresent()) { 
    return;
  }
  if(!mfrc522.PICC_ReadCardSerial()) { 
    return;
  }
  Serial.print("UID Etiketi:");
  String kartid="";
  for (byte i=0; i<mfrc522.uid.size; i++) {
    kartid.concat(String(mfrc522.uid.uidByte[i], HEX));
  }
kartid.toUpperCase(); //büyükharffe dönüştü
Serial.println(kartid);
if(kartid=="96BDEB2" ) {
    yetkilierisim();
  }
  else {
    yetkisizerisim();
  }
}
void yetkilierisim() {
  Serial.println();
  Serial.println("Mesaj: YETKİLİ ERİŞİM GERÇEKLEŞTİ.");
  digitalWrite(6,HIGH);
  for(byte i=0; i<3; i++) {
    digitalWrite(8, HIGH); delay(200);//1.LED
    digitalWrite(8, LOW); delay(200);
    digitalWrite(10, HIGH); delay(200);//BUZZER
    digitalWrite(10, LOW); delay(200);
  }

}

void yetkisizerisim() {
  Serial.println();
  Serial.println("Mesaj: YETKİSİZ ERİŞİM GİREMEZSİNİZ.");
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
  delay(5000);
}
