# Ficheros en java

Podemos abrir un fichero de texto para leer usando la clase FileReader. Esta clase tiene métodos que nos permiten leer caracteres. Sin embargo, suele ser habitual querer las líneas completas, bien porque nos interesa la línea completa, bien para poder analizarla luego y extraer campos de ella. FileReader no contiene métodos que nos permitan leer líneas completas, pero sí BufferedReader. Afortunadamente, podemos construir un BufferedReader a partir del FileReader de la siguiente forma:

      File archivo = new File ("C:\\archivo.txt");
      FileReader fr = new FileReader (archivo);
      BufferedReader br = new BufferedReader(fr);
      ...
      String linea = br.readLine();
 
 ## Lectura de un fichero de texto en java

La apertura del fichero y su posterior lectura pueden lanzar excepciones que debemos capturar. Por ello, la apertura del fichero y la lectura debe meterse en un bloque try-catch.

Además, el fichero hay que cerrarlo cuando terminemos con él, tanto si todo ha ido bien como si ha habido algún error en la lectura después de haberlo abierto. Por ello, se suele poner al try-catch un bloque finally y dentro de él, el close() del fichero. Solo hacer notar que el fichero se mete dentro de un FileReader y este dentro de un BufferedReader. Todos tienen dentro el mismo fichero abierto una sola vez. Por ello, basta con hacer close() de cualquiera de ellos para cerrar el fichero.

El siguiente es un código completo con todo lo mencionado.

      import java.io.*;

      class LeeFichero {
         public static void main(String [] arg) {
            File archivo = null;
            FileReader fr = null;
            BufferedReader br = null;

            try {
               // Apertura del fichero y creacion de BufferedReader para poder
               // hacer una lectura comoda (disponer del metodo readLine()).
               archivo = new File ("C:\\archivo.txt");
               fr = new FileReader (archivo);
               br = new BufferedReader(fr);

               // Lectura del fichero
               String linea;
               while((linea=br.readLine())!=null)
                  System.out.println(linea);
            }
            catch(Exception e){
               e.printStackTrace();
            }finally{
               // En el finally cerramos el fichero, para asegurarnos
               // que se cierra tanto si todo va bien como si salta 
               // una excepcion.
               try{                    
                  if( null != fr ){   
                     fr.close();     
                  }                  
               }catch (Exception e2){ 
                  e2.printStackTrace();
               }
            }
         }
      }

Como opción para leer un fichero de texto línea por línea, podría usarse la clase Scanner en vez de el FileReader y el BufferedReader.

## Escritura de un fichero de texto en java

El siguiente código escribe un fichero de texto desde cero. Pone en él 10 líneas

            import java.io.*;

            public class EscribeFichero
            {
                public static void main(String[] args)
                {
                    FileWriter fichero = null;
                    PrintWriter pw = null;
                    try
                    {
                        fichero = new FileWriter("c:/prueba.txt");
                        pw = new PrintWriter(fichero);

                        for (int i = 0; i < 10; i++)
                            pw.println("Linea " + i);

                    } catch (Exception e) {
                        e.printStackTrace();
                    } finally {
                       try {
                       // Nuevamente aprovechamos el finally para 
                       // asegurarnos que se cierra el fichero.
                       if (null != fichero)
                          fichero.close();
                       } catch (Exception e2) {
                          e2.printStackTrace();
                       }
                    }
                }
            }
Si queremos añadir al final de un fichero ya existente, simplemente debemos poner un flag a true como segundo parámetro del constructor de FileWriter.

             FileWriter fichero = new FileWriter("c:/prueba.txt",'''true''');
