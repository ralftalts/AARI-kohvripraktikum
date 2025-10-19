import RPi.GPIO as GPIO
import time

# Led muutujad
auto_punane = 17
auto_kollane = 27
auto_roheline = 22
jk_punane = 5
jk_roheline = 6
nupp = 23
nupp_tuli = 19

# Et asi töötaks
GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

for pin in [auto_punane, auto_kollane, auto_roheline, jk_punane, jk_roheline, nupp_tuli]:
    GPIO.setup(pin, GPIO.OUT)
    GPIO.output(pin, GPIO.LOW)

GPIO.setup(nupp, GPIO.IN, pull_up_down=GPIO.PUD_UP)

# Misc muutujad
jk_vajutus = False  
lock = False  

# Katkestuse defineerimine
def nupp_pressed(channel):
    global jk_vajutus, lock
    if not lock:
        jk_vajutus = True
        lock = True
        GPIO.output(nupp_tuli, GPIO.HIGH)  # süüta sinine tuli
        print("Nupp vajutatud!")

# --- Katkestuse registreerimine ---
GPIO.add_event_detect(nupp, GPIO.FALLING, callback=nupp_pressed, bouncetime=300)

try:
    GPIO.output(jk_punane, GPIO.HIGH)  

    while True:
        # Auto punane
        GPIO.output(auto_punane, GPIO.HIGH)
        GPIO.output(auto_kollane, GPIO.LOW)
        GPIO.output(auto_roheline, GPIO.LOW)

        if jk_vajutus:
            GPIO.output(jk_punane, GPIO.LOW)
            GPIO.output(jk_roheline, GPIO.HIGH)
        else:
            GPIO.output(jk_punane, GPIO.HIGH)
            GPIO.output(jk_roheline, GPIO.LOW)

        time.sleep(5)

        # Pärast punast lõpetame jalakäijate tsükli
        GPIO.output(jk_roheline, GPIO.LOW)
        GPIO.output(jk_punane, GPIO.HIGH)
        jk_vajutus = False
        lock = False
        GPIO.output(nupp_tuli, GPIO.LOW)

        # Auto kollane
        GPIO.output(auto_punane, GPIO.LOW)
        GPIO.output(auto_kollane, GPIO.HIGH)
        time.sleep(1)

        # Auto roheline
        GPIO.output(auto_kollane, GPIO.LOW)
        GPIO.output(auto_roheline, GPIO.HIGH)
        time.sleep(5)

        # Tagasi punaseks, kollane vilgub
        GPIO.output(auto_roheline, GPIO.LOW)
        for _ in range(3):
            GPIO.output(auto_kollane, GPIO.HIGH)
            time.sleep(0.33)
            GPIO.output(auto_kollane, GPIO.LOW)
            time.sleep(0.33)

except KeyboardInterrupt:
    GPIO.cleanup()
    print("Programm peatatud ja GPIO puhastatud.")
