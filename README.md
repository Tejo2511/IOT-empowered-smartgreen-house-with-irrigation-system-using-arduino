https://thingspeak.com/channels/1684094/charts/1?bgcolor=%23ffffff&color=%23d62020&dynamic=true&results=60&type=line&update=15


https://thingspeak.com/channels/1684094/charts/2?bgcolor=%23ffffff&color=%23d62020&dynamic=true&results=60&type=line&update=15


https://thingspeak.com/channels/1684094/charts/3?bgcolor=%23ffffff&color=%23d62020&dynamic=true&results=60&type=line&update=15


https://thingspeak.com/channels/1684094/charts/4?bgcolor=%23ffffff&color=%23d62020&dynamic=true&results=60&type=line&update=15

#Complete source code of the project

#include <LiquidCrystal.h>
LiquidCrystal lcd(8,9,10,11,12,13);
const char* ssid = "project";
const char* password = "project123";
char res[130];
#include<dht.h>
#define dht_dpin A0
dht DHT;
int temperature, humidity,Temp,fire1;
const int soil=15;
const int light=16;
const int fan=5;
int val;
int TEMP;
int gas;
int TEMP1;
void serialFlush(){
while(Serial.available() > 0) {
char t = Serial.read();
}
}
int hbeat=0,tempe=0,hum=0,ecg=0,raj=0;
char check(char* ex,int timeout)
{
int i=0;
int j = 0,k=0;
while (1)
{
sl:
if(Serial.available() > 0)
{
res[i] = Serial.read();
if(res[i] == 0x0a || res[i]=='>' || i == 100)
{
i++;
res[i] = 0;break;
}
i++;
}
j++;
if(j == 30000)
{
k++;
//Serial.println("kk");
j = 0;
}
if(k > timeout)
{
// Serial.println("timeout");
return 1;
}
}//while 1
if(!strncmp(ex,res,strlen(ex)))
{
//Serial.println("ok..");
return 0;
}
else
{
// Serial.print("Wrong ");
// Serial.println(res);
i=0;
goto sl;
}
}
char buff[200];
void upload1();
void setup() {
int i=0;
char ret;
pinMode(soil,INPUT);
pinMode(light,INPUT);
Serial.begin(9600);
lcd.begin(16,2);
lcd.clear();lcd.setCursor(0, 0);lcd.print("WELCOME");
delay(3000);
serialFlush();
st:
Serial.println("ATE0");
ret = check((char*)"OK",50);
Serial.println("AT");
ret = check((char*)"OK",50);
if(ret != 0)
{
delay(100);
delay(100);
goto st;
}
delay(1000);
Serial.println("AT+CWMODE = 1");
ret = check((char*)"OK",50);
lcd.clear();lcd.setCursor(0, 0);lcd.print("CONNECTING");
cagain:
delay(1000);
serialFlush();
Serial.print("AT+CWJAP=\"");
Serial.print(ssid);
Serial.print("\",\"");
Serial.print(password);
Serial.println("\"");
if(check((char*)"OK",300))
{
lcd.clear();lcd.setCursor(0, 0);lcd.print("CONNECTING");
goto cagain;
}
delay(1000);
lcd.clear();lcd.setCursor(0, 0);lcd.print("CONNECTED");
Serial.println("AT+CIPMUX=1");
delay(1000);delay(1000);
lcd.clear();
}
unsigned long int duration = 0;
int upload=0,count=0;
void loop() {
{
DHT.read11(dht_dpin);
lcd.clear();
lcd.setCursor(0,0);
lcd.print("H=");
lcd.print(humidity=DHT.humidity);
lcd.print("%");
lcd.setCursor(6,0);
lcd.print("T=");
lcd.print(temperature=DHT.temperature);
// lcd.write(1);
//lcd.print("C");
delay(100);
//val = analogRead(1);float T=( val/1024.0)*5000; TEMP= T/10;
//lcd.setCursor(0,0);lcd.print("TEMP:");lcd.setCursor(6,0);lcd.print(TEMP);delay(50);
/* gas = analogRead(0); TEMP1=( gas/1024.0)*5000;
lcd.setCursor(11,0);lcd.print("G=");//lcd.setCursor(6,1); lcd.print(TEMP1);delay(50);*/
if(digitalRead(soil) == 0)
{
lcd.setCursor(0, 1);lcd.print("WET LAND");
ecg = 1;
upload = 1;
}
else
{
ecg = 0;
}
delay(500);
upload = 1;
}
if(digitalRead(light) == 0)
{
lcd.setCursor(0, 1);lcd.print("LIGHT DETECTED");
fire1 = 1;
upload = 1;
}
else
{
fire1 = 0;
}
count++;
if(upload == 1||count == 2)
{
count =0;
upload = 0;
upload1();
}
}
void upload1()
{
delay(100);
// lcd.setCursor(0, 1);lcd.print("UPLOADING"); serialFlush();
Serial.println("AT+CIPSTART=4,\"TCP\",\"184.106.153.149\",80");
// if(!check((char*)"Linked",200))
{
delay(1000);
serialFlush();
Serial.println("AT+CIPSEND=4,100");
// if(!check((char*)">",50))
{
delay(500);
serialFlush();
Serial.print("GET /update?api_key=OJTYMTVMAJIQBHXV&");
sprintf(buff,"&field1=%04u",temperature); Serial.print(buff);
sprintf(buff,"&field2=%04u",humidity);
Serial.print(buff);
sprintf(buff,"&field3=%04u",ecg);
Serial.print(buff);
sprintf(buff,"&field4=%04u",fire1);
Serial.print(buff);
Serial.println("");
// if(!check((char*)"OK",200))
{
delay(500);
Serial.println("AT+CIPCLOSE");
}
}//>
}//Linke
}
