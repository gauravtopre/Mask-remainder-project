# Mask-remainder-project
#include <LiquidCrystal.h>
#include <Keypad.h> 

LiquidCrystal lcd(12,11,A4,A5,13,10);
const byte ROWS = 4; 
const byte COLS = 4;
const int pir1 = A3;
const int pir2 = A2;

char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = { 2, 3, 4, 5 };
byte colPins[COLS] = { 6, 7, 8, 9 };
Keypad kpd = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );

int mask = 0;

void setup() {
  Serial.begin(9600);
  lcd.begin(16, 2);
  pinMode(pir1, INPUT);
  pinMode(pir2, INPUT);
  pinMode(1, OUTPUT);

  
}

void loop() {

  int temp = 0;
  while(mask==0){
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Number of mask:");
    lcd.setCursor(0,1);
    lcd.print("Max 100: ");
   

    char key = kpd.getKey();
    if(key){
      if(key == '0') temp = temp * 10 + 0;
      if(key == '1') temp = temp * 10 + 1;
      if(key == '2') temp = temp * 10 + 2;
      if(key == '3') temp = temp * 10 + 3;
      if(key == '4') temp = temp * 10 + 4;
      if(key == '5') temp = temp * 10 + 5;
      if(key == '6') temp = temp * 10 + 6;
      if(key == '7') temp = temp * 10 + 7;
      if(key == '8') temp = temp * 10 + 8;
      if(key == '9') temp = temp * 10 + 9;
      if(key == 'A') temp = temp / 10;

      lcd.setCursor(9,1);
      lcd.print(temp);
      lcd.print(" ");
      if(key == 'D'){
        
        if(temp > 100){ 
          mask = -1;
        }else{
          mask = temp;
        }
        temp = 0;
      }
    }
  }
  
  while(mask>0){
    
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Current amount:");
    lcd.setCursor(0,1);
    lcd.print(mask);  
    lcd.print("             ");
    delay(1000);

    
    int check1 = digitalRead(pir1);
    int check2 = digitalRead(pir2);
    if(check1 >= 1){
     tone(A0, 300, 800);
      delay(1000);
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Wait!Wait!Wait!");
    lcd.setCursor(0,1);
    lcd.print("Do U have Mask?");
    delay(4000);
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("If NO, Then....");
    lcd.setCursor(0,1);
    lcd.print("Pick 1 from BOX");
    delay(4000);
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("If YES, Then....");
    lcd.setCursor(0,1);
    lcd.print("Move On!!!");
    delay(8000);
      if(check2 >= 1){
        mask--;
        lcd.clear();
    	lcd.setCursor(0,0);
    	lcd.print("Next Time,Dont");
    	lcd.setCursor(0,1);
    	lcd.print("Forget Ur Mask ");
       	delay(5000);
        if (mask > 1){
          
        }else if (mask == 1){
          mask = -2; 
        }
      }else if(check2 != 1){
         lcd.clear();
    	lcd.setCursor(0,0);
    	lcd.print("Thank U,YOU ARE");
    	lcd.setCursor(0,1);
    	lcd.print("TAKING CARE");
       	delay(5000);
      }
    }
  }  
  while(mask==-1){
    lcd.setCursor(0,0);
    lcd.print("Invalid number!");
    lcd.setCursor(0,1);
    lcd.print("Any key to redo ");
    char key = kpd.getKey();
    if(key){
      mask = 0;
    } 
  } 
  while(mask ==-2){
    lcd.setCursor(0,0);
    lcd.print("Out of facemask!");
    lcd.setCursor(0,1);
    lcd.print("Any key to input");
    char key = kpd.getKey();
    if(key){
      mask = 0;
    }
  }
  }
