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
<summary>Solució (C++)</summary>

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

Per evitar haver de recalcular la suma cada cop, podríem calcular la suma total, i utilitzar que un nombre és igual a la suma dels altres si `total - x == x`. En aquest cas això no afecta massa al temps d'execució, perquè només tenim 4 nombres, però sí que afectaria significativament per quantitats més grans.
<details>
  <summary>Solució alternativa (C++)</summary>
  
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
  <summary>Solució (Python3)</summary>

```py
from easyinput import read

v = [read(int) for _ in range(4)]
ans = [sum(v)-x == x for x in v]
print("si" if sum(ans) == 1 else "no")
```
</details>

Observeu que al fer `ans = [sum(v)-x == x for x in v]` estem creant una llista de variables booleanes (és a dir, `True` / `False`) i al fer `sum(ans)` els valors de `True` es converteixen a `1` i els valors de `False` es converteixen a `0`. Així doncs, `sum(ans) == 1` equival a dir que només hi ha un `x` en `v` tal que `sum(v)-x == x`.


## [Problema C2. Sumant en romà](https://jutge.org/problems/P71583) <a name="C2"/>

## [Problema Q1. Any inusual](https://jutge.org/problems/P80489) <a name="Q1"/>

## [Problema C3. Xifratge de Vigenère](https://jutge.org/problems/P51849) <a name="C3"/>

## [Problema G1. Art decimal](https://jutge.org/problems/P43122) <a name="G1"/>

## [Problema C4. Trobada de religiosos](https://jutge.org/problems/P41094) <a name="C4"/>

## [Problema Q2. Graf especial](https://jutge.org/problems/P78645) <a name="Q2"/>

## [Problema G2. Dissenyant estovalles](https://jutge.org/problems/P28763) <a name="G2"/>

## [Problema C5. Saltant amunt](https://jutge.org/problems/P57741) <a name="C5"/>

## [Problema C6. Camins màxims en un graf](https://jutge.org/problems/P51388) <a name="C6"/>

## [Problema C7. Estacions de radar](https://jutge.org/problems/P45007) <a name="C7"/>

