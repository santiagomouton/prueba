  **Primer Programa**
  A compilar se ve un problema de sintaxis y es que la variable que devuelve la funcion calculo de series  
  esta mal escrita, por lo que se arregla "return **series_value**".  
  Hecho esto el programa ya compila perfectamente y obtenemos el ejecutable. Ingresamos valores de prueba y se comprueba que  el resultado esta mal. Ej: para un valor de x = 2 y n = 3 el valor del calculo de series es -6.98615e-10.  
  Ingresamos al gdb y analizando el codigo con comando step y print, observamos que el valor de retorno de la funcion   factorial es erroneo; Por lo que sabemos que el error se encuentra en esta funcion:  
  
  int factorial(int number) {
   &nbsp;int fact;                               
  &nbsp; &nbsp; for (int j = 1; j < number; j++) {
    &nbsp; &nbsp; &nbsp; fact = fact * j;
  &nbsp; &nbsp; }
&nbsp; return fact;
}
  
  Para corregir iniciamos la variable fact en 1 y sumamos el limite del for (number +1), tal como se muestra:
  
int factorial(int number) {
   &nbsp;**int fact = 1**;                               
  &nbsp; &nbsp; for (int j = 1; j < **number + 1** ; j++) {
    &nbsp; &nbsp; &nbsp; fact = fact * j;
  &nbsp; &nbsp; }
&nbsp; return fact;
}
  
**Segundo Programa**  
  No hay errores de sintaxis. Al compilar el programa y luego ejecutarlo se genera core.  
  Vemos el coredump generado y tambien abrimos el GDB con el comando coredumpctl info, y tenemos el siguiente mensaje por parte del paquete core:  
  
  ! [coredumperrorpuntero](https://github.com/santiagomouton/prueba/blob/master/coredumperrorpuntero.png)  

                
  Y por GDB:              
  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Program received signal SIGSEGV, Segmentation fault.
  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; main () at debugg2.c:10
  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 10	    temp[3]='F';

  GDB nos seniala la linea 10, como una violacion de segmento o memoria; al parecer estamos accediendo a una memoria que no debemos acceder, osea no valida. ESto se da porque estamos queriendo escribir en un espacio de memoria donde ya esta definida cuando iniciamos el puntero. Esto lo arreglamos definiendo la variable temp como un arreglo de char y no como un puntero a caracteres, es decir:
  
  **char temp[5] = "Paras"** y no como **char** ***temp = "Paras"**
  
  **Respuestas**
  1) ¿Cómo puedo obtener el coredump de un proceso que esta corriendo?
  Se puede obtener el coredump de un proceso accediendo a la lista de coredumps e identificando el PID del paquete para luego buscar con el comando **coredumpctl dump (PID)**; accediendo a traves de su nombre o su directorio. O simplemente acceder al ultimo core generado con el comando **coredumpctl info**.  
  
  2) ¿Cómo se llama por defecto los archivos de core dump?
        Los archivos Por defecto del coredump se llaman core, seguido de numeros que son los datos de PID, GID, UID.
 3) ¿Qué restricciones de tamaño tienen por defecto y con qué comando y función puedo modificarlas?
       Se tiene una restriccion de 128 bytes y se puede cambiar con la funcion RLIMIT_CORE, que cambia el limite del archivo core al maximo para un programa. Mientras que el comando ULIMIT seguido de un parametro, modifica el tamanio maximo permitido para los core dump.
         
