// ==========================================
// ARDUINO KODU - İklim Simülasyonu
// ==========================================

#define POTANSIYOMETRE A0    // Potansiyometre pin
#define SU_SENSOR A1         // Su sensörü pin
#define BUTON_KOTU 2         // İhmal senaryosu butonu
#define BUTON_IYI 3          // Kurtarma senaryosu butonu
#define BUZZER 4             // Buzzer pin

int aktifMod = 0;            // 0: Bekleme, 1: Kötü, 2: İyi

void setup() {
  // Seri haberleşme başlat
  Serial.begin(9600);
  
  // Pin modları
  pinMode(BUTON_KOTU, INPUT_PULLUP);
  pinMode(BUTON_IYI, INPUT_PULLUP);
  pinMode(BUZZER, OUTPUT);
  
  // Başlangıç melodisi
  tone(BUZZER, 523, 200); delay(200);
  tone(BUZZER, 659, 200); delay(200);
  tone(BUZZER, 784, 200);
  
  Serial.println("Sistem hazir!");
}

void loop() {
  // Butonları oku (ters mantık - pull-up)
  bool btnKotu = !digitalRead(BUTON_KOTU);
  bool btnIyi = !digitalRead(BUTON_IYI);
  
  // Mod değiştirme
  if (btnKotu && aktifMod != 1) {
    aktifMod = 1;
    tone(BUZZER, 300, 300); // Düşük ses - kötü senaryo
    delay(200);
  }
  else if (btnIyi && aktifMod != 2) {
    aktifMod = 2;
    tone(BUZZER, 1000, 200); // Yüksek ses - iyi senaryo
    delay(200);
  }
  
  // Sensörleri oku
  int potDeger = analogRead(POTANSIYOMETRE);
  int suDeger = analogRead(SU_SENSOR);
  
  // Processing'e veri gönder (format: MOD:1|POT:512|SU:300)
  Serial.print("MOD:");
  Serial.print(aktifMod);
  Serial.print("|POT:");
  Serial.print(potDeger);
  Serial.print("|SU:");
  Serial.println(suDeger);
  
  // Buzzer feedback (sadece aktif modda)
  if (aktifMod == 1) {
    // Potansiyometre değişimine göre ses
    int frekans = map(potDeger, 0, 1023, 200, 400);
    tone(BUZZER, frekans, 50);
  } 
  else if (aktifMod == 2) {
    // Su seviyesine göre ses
    int frekans = map(suDeger, 0, 1023, 400, 1500);
    tone(BUZZER, frekans, 50);
  }
  
  delay(50); // 50ms bekle
}
