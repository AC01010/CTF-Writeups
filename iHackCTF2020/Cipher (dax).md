**1 - Write your own decryption algorithm (8)**

We are given the source `main.c`, which encrypts a string and outputs it into a file, `file1.enc`. 

Looking in the source, it is easy to see that the default password is "Pa55w0rd!", and we can use the default password to decode the encoded flag.

Here is the given encrypton algorithm:
```c
void encryption(char password[], FILE* fichierIn, FILE* fichierOut)
{
    int i = 0;
    int caractere = 0;
    char encryptChar = 0;
    int secret = 0;

    do
    {
        secret += password[i]; //takes the sum of all of the characters in the password
        i++;

    } while (password[i] != '\0');

    rewind(fichierIn);
    rewind(fichierOut);

    caractere = fgetc(fichierIn);

    i = 0;

    while (caractere != EOF)
    {
        encryptChar = password[i]; //takes one character of the password

        if (encryptChar == '\0') //iterates through the password by character, then loops back to the beginning at the end
        {
            i = 0; 
            encryptChar = password[i];
        }

        caractere = caractere - (strlen(password) + (secret % 10)) + encryptChar; //takes a character of the input string, and modifies it

        fputc(caractere, fichierOut);

        caractere = fgetc(fichierIn);

        i++;
    }
}
```
Given the password, we can calculate that `(strlen(password) + (secret % 10))` is equal to 16. Therefore, we can iterate through the password, taking each character, add 16 to the number, and subtract the corresponding password value from it.

```java
import java.util.Scanner;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
public class Main
{
    public static void main(String[] args) {
        try{
            File in = new File("rocksmall.txt");
            Scanner sc = new Scanner(in);
            while (sc.hasNext()) {
                //String pass = sc.next();
                String pass = "Pa55w0rd!";
                int secret = 0;
                for (int i = 0; i<pass.length(); i++) {
                    secret+=pass.charAt(i);
                }
                secret%=10;
                int[] encrypted = {153, 192, 154, 69, 205, 143, 215, 194, 117, 96, 178, 69, 139, 211, 129, 201,117, 49, 124, 189, 134, 92, 155, 149, 201, 182, 69, 166, 188, 134, 90, 214,133, 151, 185, 123, 115, 188, 153, 143, 155, 141, 148, 203, 71, 178, 202,94, 136, 157, 141, 160, 94};
                int len = pass.length();
                String fin = "";
                for (int i = 0; i<encrypted.length; i++) {
                    fin+=((char)(encrypted[i]-pass.charAt(i%pass.length())+(pass.length()+secret)));
                }
                if (fin.contains("flag")) {
                    System.out.println(fin);
                    System.exit(0);
                }
            }
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
Flag: You found a flag! \<la74ugb4fka5oe5ej3ktj4m2w6ry9c6m>

**2 - Homemade bruteforce (10)**

I can modify the decryption algorithm from the first challenge to bruteforce every password found in `rocksmall.txt`. 

```java
import java.util.Scanner;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
public class Main
{
    public static void main(String[] args) {
        try{
            File in = new File("rocksmall.txt");
            Scanner sc = new Scanner(in);
            while (sc.hasNext()) {
                String pass = sc.next();
                int secret = 0;
                for (int i = 0; i<pass.length(); i++) {
                    secret+=pass.charAt(i);
                }
                secret%=10;
                int[] encrypted = {197, 201, 219, 75, 143, 152, 163, 218, 190, 134, 140, 73, 143, 154, 205,193, 135, 75, 101, 98, 168, 227, 200, 203, 163, 96, 144, 158, 158, 204, 213,	156, 157, 92, 158, 162, 140, 158, 151, 152, 139, 166, 162, 147, 158, 97,147, 156, 153, 220, 147, 164, 53};
                int len = pass.length();
                String fin = "";
                for (int i = 0; i<encrypted.length; i++) {
                    fin+=((char)(encrypted[i]-pass.charAt(i%pass.length())+(pass.length()+secret)));
                }
                if (fin.contains("flag")) {
                    System.out.println(fin);
                }
            }
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Flag: You found a flag! <9zwnex7gp2roqt3p628lobx6986jskp9>
