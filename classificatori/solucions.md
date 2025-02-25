# Solucions Classificatori OICat 2025

## Taula de continguts
* [Problema C1. Només un igual a la suma dels altres](#C1)
* [Problema C2. Sumant en romà](#C2)
* [Problema Q1. Any inusual](#Q1)
* [Problema C3. Xifratge de Vigenère](#C3)
* [Problema G1. Art decimal](#G1)
* [Problema C4. Trobada de religiosos](#C4)
* [Problema Q2. Graf especial](#Q2)
* [Problema G2. Dissenyant estovalles](#G2)
* [Problema C5. Saltant amunt](#C5)
* [Problema C6. Camins màxims en un graf](#C6)
* [Problema C7. Estacions de radar](#C7)


## [Problema C1. Només un igual a la suma dels altres](https://jutge.org/problems/P20891) <a name="C1"/>

En aquest problema ens donen 4 nombres enters i hem de dir si n'hi ha només un que sigui igual a la suma dels altres. Per resoldre-ho de manera senzilla, podem iterar per cada un dels nombres, comprovar si la condició es compleix, i en cas que així sigui incrementar en 1 un comptador. Al final, la resposta serà `sí` si el comptador és igual a 1.

<details>
<summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  vector<int> v(4);
  for(int i = 0; i < 4; ++i)
    cin >> v[i];

  int count = 0;
  for(int i = 0; i < 4; ++i) {
    int suma = 0;
    for(int j = 0; j < 4; ++j) {
      if(i != j) suma += v[j];
    }
    if(suma == v[i]) count++;
  }
  cout << (count == 1 ? "si" : "no") << endl;
}
```
</details>

Per evitar haver de recalcular la suma cada cop, podríem calcular la suma total, i utilitzar que un nombre `x` és igual a la suma dels altres si es compleix que `total - x == x`. En aquest cas això no afecta massa al temps d'execució, perquè només tenim 4 nombres, però si tinguessim $n$ nombres, això faria que el codi passés de fer $\mathcal O(n^2)$ operacions a $\mathcal O(n)$ operacions.
<details>
  <summary><b>Codi alternatiu (C++)</b></summary>
  
  ```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  vector<int> v(4);
  int suma = 0;
  for(int i = 0; i < 4; ++i) {
    cin >> v[i];
    suma += v[i];
  }

  int count = 0;
  for(int i = 0; i < 4; ++i) {
    if(suma - v[i] == v[i]) count++;
  }
  cout << (count == 1 ? "si" : "no") << endl;
}
  ```
</details>

Per acabar, una solució més concisa en Python:

<details>
  <summary><b>Codi (Python3)</b></summary>

```py
from easyinput import read

v = [read(int) for _ in range(4)]
ans = [sum(v)-x == x for x in v]
print("si" if sum(ans) == 1 else "no")
```
</details>

Observeu que al fer `ans = [sum(v)-x == x for x in v]` estem creant una llista de variables booleanes (és a dir, `True` / `False`) i al fer `sum(ans)` els valors de `True` es converteixen a `1` i els valors de `False` es converteixen a `0`. Així doncs, `sum(ans) == 1` equival a dir que només hi ha un `x` en `v` tal que `sum(v)-x == x`.


## [Problema C2. Sumant en romà](https://jutge.org/problems/P71583) <a name="C2"/>

En aquest problema ens demanen calcular la suma de dos nombres romans. Per resoldre-ho, necessitem implementar una funció que ens passi de nombres romans (donats com una `string`) a nombres "normals" i una altra funció que ens passi de nombres "normals" a nombres romans.

Hi ha moltes maneres de fer-ho. Per exemple, podem implementar la primera amb un `map<string,int>`, i la segona amb un `vector<string>` (on a la posició $n$-èssima hi guardem el nombre romà corresponent a `n`).

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  vector<string> nums_a_romans = {"?", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI", "XII", "XIII", "XIV", "XV", "XVI", "XVII", "XVIII", "XIX", "XX"};
  map<string,int> romans_a_nums;
  for(int i = 0; i < int(nums_a_romans.size()); ++i) {
    romans_a_nums[nums_a_romans[i]] = i;
  }

  string a, b;
  while(cin >> a >> b >> b) {
    int suma = romans_a_nums[a] + romans_a_nums[b];
    cout << a << " + " << b << " = " << nums_a_romans[suma] << endl;
  }
}
```
</details>

Observeu que, per no haver d'inicialitzar els elements del mapa d'un en un (i.e. fent `romans_a_nums["I"] = 1`, `romans_a_nums["II"] = 2`, ...), hem fet un bucle on utilitzem `nums_a_romans` per inicialitzar `romans_a_nums`.


## [Problema Q1. Any inusual](https://jutge.org/problems/P80489) <a name="Q1"/>

Per resoldre aquest problema hem d'iterar per tots els nombres majors a 2025 fins a trobar-ne un que compleixi la propietat. Per "partir en dos trossos" un nombre de manera senzilla, el podem convertir a `string` amb la funció `to_string()` i després utilitzar la funció `s.substr()` per seleccionar la part de la `string` que volguem. Al final, per tornar a convertir-ho a un enter, utilitzem la funció `stoi()`. 

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  for(int n = 2025; n <= 10000; ++n) {
    string s = to_string(n);
    int k = s.size();
    for(int i = 1; i < k; ++i) {
      // partim el nombre en 2 trossos:
      // a = s[0..i-1], b = s[i..k-1]
      string a = s.substr(0,i); // des de posició 0, longitud = i
      string b = s.substr(i); // des de posició i, fins al final
      int na = stoi(a);
      int nb = stoi(b);
      if((na + nb)*(na + nb) == n) {
        cout <<"(" << a << " + " << b << ")^2 = " << n << endl;
      } 
    }
  }
}
```
</details>

## [Problema C3. Xifratge de Vigenère](https://jutge.org/problems/P51849) <a name="C3"/>

Aquest problema està basat en un mètode de xifratge real, que es va utilitzar en diferents variants al llarg de l'Edat Moderna i fins i tot a la Guerra Civil Americana (<a href=https://ca.wikipedia.org/wiki/Xifratge_de_Vigen%C3%A8re>enllaç</a>).

Per resoldre'l, va bé saber que C++ ens permet operar amb els caràcters com si fossin enters. Per exemple, `ìnt('c'-'a')` retorna `2`, ja que la lletra `'c'` està 2 posicions per davant de la `'a'`, i de la mateixa manera `char('a' + 2)` ens retorna `'c'`. Així doncs, per xifrar el caràcter `x` amb el caràcter `y` podem fer `y += (x - 'a')`. El problema és que aleshores ens podem passar de llarg (podem acabar amb un caràcter més enllà de la `'z'`), així que hem de restar-li `'a'`, prendre mòdul per la mida de l'alfabet, i tornar-li a sumar `'a'`. Al final obtenim la implementació següent:

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main() {
  string s, t;
  while(cin >> s >> t) {
    int n = s.size();
    int m = t.size();
    for(int i = 0; i < m; ++i) {
      if(t[i] == '_') cout << " ";
      else {
        int x = t[i] - 'a' + s[i%n] - 'a';
        x %= 26;
        cout << char('a' + x);
      }
    }
    cout << endl;
  }
}
```
</details>

## [Problema G1. Art decimal](https://jutge.org/problems/P43122) <a name="G1"/>

<details><summary><b>Codi (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

m, n = read(int, int)
colors = [read(str) for _ in range(10)]
a, b = read(int, int)

img = Image.new('RGB', (m, n), 'White')
dib = ImageDraw.Draw(img)

for y in range(n):
  for x in range(m):
    a *= 10
    digit = a // b
    a %= b
    dib.point((x, y), colors[digit])

img.save('output.png')
```
</details>

## [Problema C4. Trobada de religiosos](https://jutge.org/problems/P41094) <a name="C4"/>

## [Problema Q2. Graf especial](https://jutge.org/problems/P78645) <a name="Q2"/>

## [Problema G2. Dissenyant estovalles](https://jutge.org/problems/P28763) <a name="G2"/>

## [Problema C5. Saltant amunt](https://jutge.org/problems/P57741) <a name="C5"/>

## [Problema C6. Camins màxims en un graf](https://jutge.org/problems/P51388) <a name="C6"/>

## [Problema C7. Estacions de radar](https://jutge.org/problems/P45007) <a name="C7"/>

