# SEGUIDOR-DE-LINEA-
#include <16f887.h>
//#fuses XT,NOWDT,NOPROTECT,NOLVP,PUT,BROWNOUT,INTRC_IO
#fuses BROWNOUT, HS, NOWDT, NOLVP
#use DELAY(clock = 4000000)
#byte trisb=0x86
#byte portb=0x06

int Timer2,Poscaler;

int16 xtrleft;
int16 xtrright;
int16 midleft;
int16 midright;
int16 finalleft;
int16 finalright;

void main(void){
Timer2=249;
Poscaler=1;
setup_timer_2(t2_div_by_4,Timer2,Poscaler);

setup_adc_ports(sAN1|sAN2 |sAN3 |sAN4 |sAN5 |sAN6 |sAN7 |  VSS_VDD);
setup_adc(adc_clock_internal);
setup_ccp1(ccp_pwm);
setup_ccp2(ccp_pwm);
int spd=90;

do{

}while(INPUT(PIN_B0)==1);


output_high(PIN_B2);
delay_ms(400);
output_low(PIN_B2);
delay_ms(400);

output_high(PIN_B2);
delay_ms(400);
output_low(PIN_B2);
delay_ms(400);

output_high(PIN_B2);
delay_ms(400);
output_low(PIN_B2);
delay_ms(400);

while(1){
        set_adc_channel(7); 
        finalleft=read_adc(); 
        set_adc_channel(6); 
        midleft=read_adc(); 
        set_adc_channel(5); 
        xtrleft=read_adc(); 
        set_adc_channel(3);
        xtrright=read_adc(); 
        set_adc_channel(2); 
        midright=read_adc(); 
        set_adc_channel(1); 
        finalright=read_adc(); 
        
  if(xtrleft>235)
  {
        output_low(PIN_B3);
        output_high(PIN_B4); 
        set_pwm2_duty(spd+15); 
        output_high(PIN_A0);
        output_low(PIN_B5);
        set_pwm1_duty(spd);
  }
  else if(xtrright>235)
  {
         output_high(PIN_B3);
         output_low(PIN_B4); 
        set_pwm2_duty(spd);
            output_low(PIN_A0);
        output_high(PIN_B5);
        set_pwm1_duty(spd+15);
  }
  
   else if(midright>235)
  {
         output_high(PIN_B3);
         output_low(PIN_B4); 
        set_pwm2_duty(spd);
        output_low(PIN_A0);
        output_high(PIN_B5);
              set_pwm1_duty(spd+50);
  }
   else if(midleft>235)
  {
         output_low(PIN_B3);
         output_high(PIN_B4); 
        set_pwm2_duty(spd);
        
        output_high(PIN_A0);
        output_low(PIN_B5);
        set_pwm1_duty(spd+50);
  }
   else if(finalright>235)
  {
         output_high(PIN_B3);
         output_low(PIN_B4); 
        set_pwm2_duty(spd-20);
        
        output_low(PIN_A0);
        output_high(PIN_B5);
        set_pwm1_duty(spd+110);
  }
   else if(finalleft>235)
  {
         output_low(PIN_B3);
         output_high(PIN_B4); 
        set_pwm2_duty(spd-20);
        output_high(PIN_A0);
        output_low(PIN_B5);
        set_pwm1_duty(spd+110);
  }
  else{
        output_high(PIN_A0);
        output_low(PIN_B5);
        set_pwm1_duty(spd); 
         output_high(PIN_B3);
         output_low(PIN_B4); 
        set_pwm2_duty(spd);
  }
}

}
