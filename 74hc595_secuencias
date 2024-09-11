#include "mbed.h"

// alias
#define ds D4 // Pin de datos para el 74HC595
#define STCP D3 // Pin de latch (RCLK) para el 74HC595
#define SHCP D2 // Pin de reloj (SRCLK) para el 74HC595
#define RST D7 // resetear

#define tiempo 1ms // Intervalo de tiempo entre encender y apagar el LED

// prototipos
void correr_datos(uint32_t data);
void enviar_datos(void);

// hilos y herramientas del sistema
Thread T_enviar_datos(osPriorityNormal, 4096, NULL, NULL);

// pines y puertos
DigitalOut serial_data(ds);
DigitalOut latchCLK(STCP);
DigitalOut serial_clock(SHCP);
DigitalOut reset(RST);

//const int NUM_SECUENCIAS = 5;
uint32_t secuencias[5] = {0x80, 0xE0, 0xFD, 0xFF, 0xAA};

int main()
{
    // inicializar
    serial_data = 0;
    latchCLK = 0;
    serial_clock = 0;
    reset = 1;

    // encender el hilo para enviar datos
    T_enviar_datos.start(enviar_datos);

    while (true) {
        
    }
}

void correr_datos(uint32_t dato){
    reset=0;
    for (int i = 0; i < 24; i++){
        if ((dato & 0x01) == 1) {
            serial_data = 1;
        } else {
            serial_data = 0;
        }
        serial_clock = 0;
        wait_us(25000);
        serial_clock = 1;
        wait_us(25000);
        latchCLK=0;
        wait_us(50000);
        latchCLK=1;
        wait_us(50000);
        dato = dato >> 1;
    }
}

void enviar_datos(void){
    reset=0; 

    while (true){
        for (int i = 0; i < 5; i++) {
            correr_datos(secuencias[i]);
            ThisThread::sleep_for(tiempo);
        }
    }
}
