# Proyecto Software Seguridad  
**Universidad Icesi**  
**Curso:** Seguridad de la Informacion  
**Docente:** Juan Manuel Madrid  
**Tema:**  API criptografica Java  
**Estudiantes:**    
Juan Pablo Medina Mora 11112010  

## Objetivos
* Crear una aplicacion usando el API criptografica de Java
* Cifar y descifrar archivos con AES de 128 bits

## Descripción
Debe construirse una aplicación que sea capaz de cifrar y descifrar archivos, empleando el algoritmo AES de 128 bits. La clave debe derivarse a partir de una palabra o frase clave que el usuario introduzca por teclado. Junto con el archivo cifrado debe almacenarse el hash SHA-1 del archivo sin cifrar, de manera que al descifrar el archivo, se pueda comprobar que el proceso fue ejecutado correctamente mediante la comparación de hashes.

## Implementacion
Con el fin de desarrollar el proyecto se desarrollo una aplicacion en Java que sirve para cifrar y descifrar archivos, y para comparar los hashes que tienen tanto el archivo antes de cifrar como despues de descifrar.

El programa se realizo previa investigacion haciendo uso de las librerias java.security, javax.crypto, org.apache.commons y org.apache.io en el modelo, y javax.swing y java.awt en la vista. En el caso del modelo las librerias se usaron para realizar los cifrados y descifrados, y tambien para transformar los archivos a cadenas de bytes y codificar en base 46 para posteriormente realizar los cifrados.

A continuacion se muestran fragmentos del codigo.

```java
public static void encrypt(byte[] stringToEncrypt) {
    	
        try {
        	
            Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");        
            cipher.init(Cipher.ENCRYPT_MODE, secretKey);
            setEncryptBytes(Base64.encodeBase64String(cipher.doFinal(stringToEncrypt)));
        
        } catch (Exception e) {
            
        	e.printStackTrace();
        	
        }
        
    }
    
    public static void decrypt(byte[] stringToDecrypt) {
    	
        try {
        	
            Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5PADDING");
            cipher.init(Cipher.DECRYPT_MODE, secretKey);
            setDecryptBytes(new String(cipher.doFinal(Base64.decodeBase64(stringToDecrypt))));
            
        } catch (Exception e) {
         
        	e.printStackTrace();
        	
        }
        
    }
```
El codigo anterior se realizo a partir de lo aprendido en clase anteriormente sobre la API de cifrado de Java.



En la Figura 1 se puede observar la aplicacion, que consiste en dos paneles, uno para poder cifrar y descifrar los archivos y otro para compparar los hashes.

```java
MessageDigest sha = null;
        
        try {
        	
            keyBytes = key.getBytes("UTF-8");
            sha = MessageDigest.getInstance("SHA-1");
            keyBytes = sha.digest(keyBytes);
            keyBytes = Arrays.copyOf(keyBytes, 16); 
            secretKey = new SecretKeySpec(keyBytes, "AES");
            
        }
```
El anterior fragmento de codigo muestra como por medio de la clase MessageDigest se crea el hash de sha-1 que se necesita guardar para posteriormente comprobar que el proceso de cifrado - descifrado funciona correctamente.  En este codigo se especifica la funcion hash con que se quiere cifrar la clave, posteriormente la clave cifrada se trunca a 16 bits y se construye posteriormente una llave secreta.
Este metodo junto con el siguiente permiten que se muestren y guarden los hash que se compararan.

```java
      MessageDigest algorithm = MessageDigest.getInstance("SHA1");
			algorithm.reset();
			algorithm.update(hashBytes);

			byte messageDigest[] = algorithm.digest();
			StringBuffer hexString = new StringBuffer();
			
			for (int i = 0; i < messageDigest.length; i++) {
				
				hexString.append(Integer.toHexString(0xFF & messageDigest[i]));
				
			}
			
			return hexString.toString();
```
Con este metodo se intenta mostrar el hash generado en hexadecimal haciendo uso nuevamente de MessageDigest.

El proceso normal para usar la aplicacion se observa en las figuras 2 a 6. Este proceso basicamente consiste en poner una palabra clave, posteriormente cifrar un archivo a eleccion propia, guardar el archivo cifrado y su clave en sha 1; proeseguir a la desencripcion escogiendo el archivo previamente encriptado y guardando el nuevo aarchivo descifrado. Para finalizar se comparan los sha de los dos archivos, inicial y final.



Los archivos resultantes se pueden observar a continuacion.



## Dificultades encontradas
* La mayor dificultad que se encontro al momento de realizar el proyecto fue encontrar los ejemplos adecuados para su realizacion y entender de esta manera el correcto uso de las herramientas que brinda el lenguaje de programacion.

* Otro importante punto a resaltar es que el programa cumple satisfactoriamente su funcion de cifrar y descifrar archivos de texto plano, pero al momento de realizar el proceso en otro tipo de archivos estos no se generan adecuadamente.

* Finalmente se tuvo el problema de que al intentar cifrar diversos tipos de archivos era sumamente dificil saber que tipo de archivo era para generar el nuevo archivo descifrado, dado que el "numero magico" en la cabezera de bytes no es de la misma longitud para cada tipo de archivo, y ademas el tratamiento a darle a cada tipo de archivo tampoco es estandar.

## Conclusiones
* Con la realizacion del presente proyecto pudimos comprobar que con las herramientas actuales el cifrado de archivos es relativamente facil de realizar, lo mas importante es buscar como se debe usar las herramientas.  

* Ademas se logro observar que el algoritmo de hash sha 1 no toma en concideracion el nombre del archivo, unicamente su contenido y su extension.  

* Tambien se pudo ver que al momento de cifrar diversos tipos de archivos es de vital importancia el saber como transformar estos archivos correctamente, y parte crucial de esto es el conocimiento de los "numeros magicos" en las cabeceras de los archivos.

* Finalmente se observo uno de los usos de los algoritmos de hash y porque son tan importantes, este es el poder confirmar la integridad o no de un archivo tras realizarce por ejemplo un cifrado y descifrado sobre este.

## Referencias
https://docs.oracle.com/javase/7/docs/api/javax/crypto/spec/SecretKeySpec.html
https://es.wikipedia.org/wiki/Secure_Hash_Algorithm
https://docs.oracle.com/javase/7/docs/api/java/security/MessageDigest.html
http://programtalk.com/java-api-usage-examples/java.security.MessageDigest/
http://commons.apache.org/proper/commons-io/download_io.cgi
https://commons.apache.org/proper/commons-codec/download_codec.cgi
http://www.lawebdelprogramador.com/foros/Java/1113731-Encriptar-Archivos.html
http://muchojava.com/como-crear-programa-para-cifrar-archivos-con-cifrado-aes/
http://www.jc-mouse.net/java/encriptacion-simetrica-en-java
https://bit502.wordpress.com/2014/06/27/codigo-java-encriptar-y-desencriptar-texto-usando-el-algoritmo-aes-con-cifrado-por-bloques-cbc-de-128-bits/
http://pastebin.com/h6DaAPha
http://stackoverflow.com/questions/4895523/java-string-to-sha1
http://stackoverflow.com/questions/2091014/get-real-file-extension-java-code
http://stackoverflow.com/questions/12136558/encryption-and-decryption-of-image-file
