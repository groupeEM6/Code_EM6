// Avant de commencer j´ai besoin de définir sur quel pin on a mit les encodeurs.
// Et aussi les pins pour la tension et le sens du moteur.
const pin_encodeur = ;
const pin_tension = ;
const pin_direction = ;




// Puis je défini le gain (qu´on va trouver par manque de temps en essaie erreur)
// Et la vitesse de référence qu´on va utiliser pour le calcul de l´erreur.
float K = ;
float vref = ;
int tension = ;
int tension_min = ;
int tension_max = ;


// On doit définir le temps d´un cycle en ms.
// Et le bombre de ticks par tours.
const int temps_1_cycle = 50;
int tpt = ;
const unsigned Ts_ms = 50; // le temps que ca a pris pr réaliser les tpt.



// Vu que l´encodeur fonction en tick, J´ai besoin de définir une variable pour stocker les ticks
// Et comme elle va changer au cours du temps, c´est mieux de mettre volatile.
volatile long tick = 0;
int long lastMs = 0;



// on "interrompt" le code pour compter exactement le nombre de ticks
void interr(){
  tick++;
}

void setup() {
  // On définit la fonction des pins.
  pinMode(pin_encodeur, INPUT_PULLUP);
  pinMode(pin_tension, OUTPUT);
  pinMode(pin_direction, OUTPUT);
  // Le sens du moteur.
  if (tension > 0){
    digitalWrite(pin_direction, HIGH);
  } else{
    digitalWrite(pin_direction, LOW);
    tension = - tension;
  }
  // Comptage.
  attachInterrupt(digitalPinToInterrupt(pin_encodeur), interr, RISING);
  Serial.begin(115200);
  lastMs = millis();

}

void loop() {
  unsigned long now = millis();
  if (now - lastMs >= Ts_ms) {
    // Récupérer les ticks 
    noInterrupts();
    long ticksWindow = tick;
    tick = 0;
    interrupts();

    // Calculer la vitesse
    float revolutions = (float)ticksWindow / tpt;
    float minutes = (float)Ts_ms / 60000.0;
    float v_rpm = revolutions / minutes;

    // Régulation P
    float e = vref - v_rpm;
    float u = K * e;            

    
    int pwm = (int)round(tension);
    if (pwm < tension_min) pwm = tension_min;
    if (pwm > tension_max) pwm = tension_max;

    analogWrite(pin_tension, pwm);

    lastMs += Ts_ms;
  }

}
