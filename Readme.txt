adasdasd
https://arduinoinfo.mywikis.net/wiki/Arduino-PWM-Frequency

Arduino Kontrol Kodu:

float ref_voltage=2; // Do not forget, Ardinocan read up to 5.2 Vansd will scale the values read upto 5.2 V
float V_ref; //will be up to 1024
float Vtri=4;
float deltaD=0;

int data_read = A5;
int pwm_pin = 9;
float D = 0;
float feedback = 0; // will be up to 1024

void setup() {
  Serial.begin(9600);
  pinMode(pwm_pin,OUTPUT);

TCCR1B = TCCR1B & B11111000 | B00000001; //9. pin 31250kH yaptık

V_ref= ref_voltage/5.2*1024;// this isthe analog correspondence of reference set voltage above

}



void loop()
{
feedback=0;
for (int i=0; i <= 4; i++){
      feedback=analogRead(data_read)+feedback;    // averaj data
      delayMicroseconds(1);
   }

feedback=feedback/5;
feedback=feedback/1024*5.2;  // feedbackı gercek voltaja cevırdık
deltaD=abs(feedback-ref_voltage)/Vtri;

if (feedback<ref_voltage)
{
D=D+deltaD;
}
 else 
{
D=D-deltaD;
}

if (D>0.45)
{
D=0.45;
}
else if (D<0.1) 
{
D=0.1;
}

D=D*255; // D=1 means D=255 in arduino

analogWrite(pwm_pin, D);



           
}
