#include <LiquidCrystal.h>
#define trigger 7
#define echo 6

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);
float distance = 0, previous_distance = 0;
volatile float rapid_dist = 0, prev_rapid_dist = 0;
unsigned long start = 0, cycle_one_millis = 0, end = 0;
const long cycle_one_interval = 10;

void setup()
{
  Serial.begin(9600);
  lcd.begin(16, 2);
  pinMode(trigger, OUTPUT);
  pinMode(echo, INPUT);
  printToScreen(lcd, 0, 0, "Ultrasonic", false);

  printToScreen(lcd, 0, 1, "Distance Meter", false);

  delay(1000); // wait 1 second
  lcd.print("Calibrating...");
  // check distance here
  getDistance(&distance);
  delay(500);
  lcd.clear();
  // say ready to move
  lcd.print("Done!");
  previous_distance = distance;
  delay(1000);
  lcd.clear();
  Serial.println("D= " + String(distance) + "\tPD= " + String(previous_distance));
}

void loop()
{
  // check distance
  while (1)
  {
    getDistance(&distance);
    start = millis();
    if (!isDeviated(5))
      return;
    Serial.print("Deviation:");
    Serial.print(distance - previous_distance);
    Serial.println('cm');
    previous_distance = distance;
  }
  while (1)
  {
    if (millis() - cycle_one_millis >= cycle_one_interval)
    {
      // update previous_distance
      if (previous_distance == rapid_dist)
      {
        end = millis();
        break;
      }
      cycle_one_millis = millis();

      // rapid_dist = previous_distance
    }
  }
}

double getTime()
{

  double t = (millis() - start) / 1000;

  start = millis();

  return t;
}

bool isDeviated(int margin)
{
  return distance - previous_distance >= margin;
}
//
void getDistance(float *dist)
{
  digitalWrite(trigger, LOW);
  delayMicroseconds(2);
  digitalWrite(trigger, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigger, LOW);
  delayMicroseconds(2);
  float time = pulseIn(echo, HIGH);
  *dist = time * 340 / 20000;
}

void printToScreen(LiquidCrystal obj, int x, int y, char content[], bool clearScreen)
{
  if (clearScreen)
    obj.clear();
  obj.setCursor(x, y);
  obj.print(content);
}

void printToScreen(LiquidCrystal obj, int x, int y, double content, bool clearScreen)
{
  if (clearScreen)
    obj.clear();
  obj.setCursor(x, y);
  obj.print(content);
}
