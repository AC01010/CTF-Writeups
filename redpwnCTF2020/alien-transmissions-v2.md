**Alien-Transmissions-V2 (crypto/77 solves)**

This challenge required you to do frequency analysis on an extremely large dataset of "random" characters. We can take every 399th character and do cryptanalysis on each set of numbers whose indexes are congruent mod 399. It is obvious which characters are spaces, as they are far more frequent than any other character. The following script finds the XOR of the two keys:

```java
import java.util.ArrayList;
import java.util.Scanner;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
public class asdf
{
    public static void main(String[] args) {
        try{
            for (int offset = 0; offset<512; offset++) {
                int i = 0;
                File in = new File("asdf.txt");
                int[] freq = new int[512];
                for (int j: freq) {
                    j = 0;
                }
                Scanner sc = new Scanner(in);
                while (sc.hasNext()) {
                    int pass = sc.nextInt();
                    if (i%399==offset) {
                        freq[pass]++;
                    }
                    i++;
                }
                for (int j = 0; j<512; j++) {
                    if (freq[j]>150) {
                        System.out.println(j^481);
                    }
                }
            }
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

This python script then takes the output of the Java program above and recovers both halves of the flag. Credit to PMPuns for writing the entirety of this script:

```python
vals = {}

i = 0
j = 0
c = 0
while c < 399:
    vals[(i, j)] = int(input())
    i += 1
    j += 1
    i %= 21
    j %= 19
    c += 1
assert vals[(0, 0)] ^ vals[(0, 1)] == vals[(1, 0)] ^ vals[(1, 1)]

st = [vals[(0, i)] ^ vals[(0, i+1)] for i in range(18)]
st2 = [vals[(i, 0)] ^ vals[(i+1, 0)] for i in range(20)]

for i in range(256):
    r = i
    for j in st:
        print(chr(r), end="")
        r ^= j
    print()

for i in range(256):
    r = i
    for j in st2:
        print(chr(r), end="")
        r ^= j
    print()
```

Flag: flag{h3r3'5_th3_f1r5t_h4lf_th3_53c0nd_15_th15}
