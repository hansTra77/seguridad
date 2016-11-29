# Proyecto Software Seguridad  
**Universidad Icesi**  
**Curso:** Seguridad de la Información  
**Docente:** Juan Manuel Madrid  
**Tema:**  API criptográfica Java  
**Estudiantes:**    
Juan Pablo Medina Mora 11112010  

## Objetivos
* Crear una aplicación usando el API criptográfica de Java
* Cifrar y descifrar archivos con AES de 128 bits

## Descripción
Debe construirse una aplicación que sea capaz de cifrar y descifrar archivos, empleando el algoritmo AES de 128 bits. La clave debe derivarse a partir de una palabra o frase clave que el usuario introduzca por teclado. Junto con el archivo cifrado debe almacenarse el hash SHA-1 del archivo sin cifrar, de manera que, al descifrar el archivo, se pueda comprobar que el proceso fue ejecutado correctamente mediante la comparación de hashes.

## Implementación
Con el fin de desarrollar el proyecto se desarrolló una aplicación en Java que sirve para cifrar y descifrar archivos, y para comparar los hashes que tienen tanto el archivo antes de cifrar como después de descifrar.

El programa se realizó previa investigación haciendo uso de las librerías java.security, javax.crypto, org.apache.commons y org.apache.io en el modelo, y javax.swing y java.awt en la vista. En el caso del modelo las librerías se usaron para realizar los cifrados y descifrados, y también para transformar los archivos a cadenas de bytes y codificar en base 46 para posteriormente realizar los cifrados.

A continuación, se muestran fragmentos del código.

```java
   public static void decrypt(byte[] stringToDecrypt, String extension) {
    	
        try {
        	
        	FileOutputStream fos = new FileOutputStream("c:/Users/casa/Desktop/myfiledes" + "." + extension);
            Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5PADDING");
            cipher.init(Cipher.DECRYPT_MODE, secretKey);
            CipherOutputStream cout = new CipherOutputStream(fos, cipher);
            cout.write(stringToDecrypt);     
            fos.flush();
            cout.close();
            
        } catch (Exception e) {
         
        	e.printStackTrace();
        	
        }
        
    }
    
    public static void encrypt(byte[] stringToEncrypt, String extension) {
    	
        try {
        	
        	FileOutputStream fos = new FileOutputStream("c:/Users/casa/Desktop/myfile" + "." + extension);
            Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding"); 
            cipher.init(Cipher.ENCRYPT_MODE, secretKey);
            //setEncryptBytes(Base64.encodeBase64String(cipher.doFinal(stringToEncrypt)));
            CipherOutputStream cout = new CipherOutputStream(fos, cipher);
            cout.write(stringToEncrypt);
            fos.flush();
            cout.close();
        
        } catch (Exception e) {
            
        	e.printStackTrace();
        	
        }
        
    }
```
El código anterior se realizó a partir de lo aprendido en clase anteriormente sobre la API de cifrado de Java.

![][1]

En la Figura 1 se puede observar la aplicación, que consiste en dos paneles, uno para poder cifrar y descifrar los archivos y otro para comparar los hashes.

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
El anterior fragmento de código muestra como por medio de la clase MessageDigest se crea el hash de sha-1 que se necesita guardar para posteriormente comprobar que el proceso de cifrado - descifrado funciona correctamente.  En este código se especifica la función hash con que se quiere cifrar la clave, posteriormente la clave cifrada se trunca a 16 bits y se construye posteriormente una llave secreta.
Este método junto con el siguiente permiten que se muestren y guarden los hash que se compararan.

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
Con este método se intenta mostrar el hash generado en hexadecimal haciendo uso nuevamente de MessageDigest.

El proceso normal para usar la aplicación se observa en las figuras 2 a 6. Este proceso básicamente consiste en poner una palabra clave, posteriormente cifrar un archivo a elección propia, guardar el archivo cifrado y su clave en sha 1; proseguir a la desencripcion escogiendo el archivo previamente encriptado y guardando el nuevo archivo descifrado. Para finalizar se comparan los sha de los dos archivos, inicial y final.

![][2]
![][3]
![][4]
![][5]
![][6]

Los archivos resultantes se pueden observar a continuación.

![][7]
![][8]
![][9]
![][10]
![][11]

## Dificultades encontradas
* La mayor dificultad que se encontró al momento de realizar el proyecto fue encontrar los ejemplos adecuados para su realización y entender de esta manera el correcto uso de las herramientas que brinda el lenguaje de programación.

* Otro importante punto a resaltar es que el programa cumple satisfactoriamente su función de cifrar y descifrar archivos de texto plano, pero al momento de realizar el proceso en otro tipo de archivos estos no se generan adecuadamente.

* Finalmente se tuvo el problema de que al intentar cifrar diversos tipos de archivos era sumamente difícil saber qué tipo de archivo era para generar el nuevo archivo descifrado, dado que el "número mágico" en la cabecera de bytes no es de la misma longitud para cada tipo de archivo, y además el tratamiento a darle a cada tipo de archivo tampoco es estándar.

## Conclusiones
* Con la realización del presente proyecto pudimos comprobar que con las herramientas actuales el cifrado de archivos es relativamente fácil de realizar, lo más importante es buscar cómo se debe usar las herramientas.  

* Además se logró observar que el algoritmo de hash sha 1 no toma en consideración el nombre del archivo, únicamente su contenido y su extensión.  

* También se pudo ver que al momento de cifrar diversos tipos de archivos es de vital importancia el saber cómo transformar estos archivos correctamente, y parte crucial de esto es el conocimiento de los "números mágicos" en las cabeceras de los archivos.

* Finalmente se observó uno de los usos de los algoritmos de hash y porque son tan importantes, este es el poder confirmar la integridad o no de un archivo tras realizarse por ejemplo un cifrado y descifrado sobre este.

## Referencias
* https://docs.oracle.com/javase/7/docs/api/javax/crypto/spec/SecretKeySpec.html
* https://es.wikipedia.org/wiki/Secure_Hash_Algorithm
* https://docs.oracle.com/javase/7/docs/api/java/security/MessageDigest.html
* http://programtalk.com/java-api-usage-examples/java.security.MessageDigest/
* http://commons.apache.org/proper/commons-io/download_io.cgi
* https://commons.apache.org/proper/commons-codec/download_codec.cgi
* http://www.lawebdelprogramador.com/foros/Java/1113731-Encriptar-Archivos.html
* http://muchojava.com/como-crear-programa-para-cifrar-archivos-con-cifrado-aes/
* http://www.jc-mouse.net/java/encriptacion-simetrica-en-java
* https://bit502.wordpress.com/2014/06/27/codigo-java-encriptar-y-desencriptar-texto-usando-el-algoritmo-aes-con-cifrado-por-bloques-cbc-de-128-bits/
* http://pastebin.com/h6DaAPha
* http://stackoverflow.com/questions/4895523/java-string-to-sha1
* http://stackoverflow.com/questions/2091014/get-real-file-extension-java-code
* http://stackoverflow.com/questions/12136558/encryption-and-decryption-of-image-file


[1]: final/0.JPG
[2]: final/2.JPG
[3]: final/3.JPG
[4]: final/4.JPG
[5]: final/5.JPG
[6]: final/6.JPG
[7]: final/7.JPG
[8]: final/8.JPG
[9]: final/9.JPG
[10]: final/10.JPG
[11]: final/11.JPG
