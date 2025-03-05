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

En aquest problema ens demanen representar els decimals d'una fracció amb diferents colors. La clau és que si treballem amb nombres de coma flotant (és a dir, si calculem `a/b`) no obtindrem un resultat prou precís com per poder resoldre correctament els casos grans. Així doncs, hem de treballar només amb enters, utilitzant l'algorisme habitual de divisió per anar trobant els dígits un a un. 

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

Per resoldre aquest problema cal adonar-se que tot i que estigui formulat en termes d'una única cua, on els religiosos s'ordenen depenent del seu rang, en realitat això és equivalent a tenir 3 cues diferents, una per cada tipus de religiós.

Així doncs, mantenim una cua per cada un dels 3 tipus de religiós i cada cop que arriba una nova persona l'afegim a la cua que pertoqui. Quan marxa una persona, busquem quina és la primera cua no buida (de major a menor rang) i n'extreiem el primer element.

Per implementar-ho, podem utilitzar l'estructura `std::queue`, que té una operació `pop()`, que extreu el primer element de la cua, i una operació `push()`, que afegeix un element al final de la cua. Aquestes dues operacions triguen $\mathcal O(1)$ cadascuna, així que la complexitat total serà $\mathcal O(n)$.

<details> <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main() {
  char tipus;
  vector<queue<string>> v(3);
  string const rangs = "mbc";
  while(cin >> tipus and tipus != 'f') {
    if(tipus == 's') {
      bool fet = false;
      for(int i = 2; i >= 0 and not fet; --i) {
        if(not v[i].empty()) {
          cout << v[i].front() << endl;
          v[i].pop();
          fet = true;
        }
      }
      if(not fet) cout << "ERROR" << endl;
    }
    else {
      string nom;
      char rang;
      cin >> nom >> rang;
      for(int i = 0; i < 3; ++i) {
        if(rang == rangs[i]) {
          v[i].push(nom);
        }
      }
    }
  }
  cout << string(10, '-') << endl;
  for(int i = 2; i >= 0; --i) {
    while(not v[i].empty()) {
      cout << v[i].front() << endl;
      v[i].pop();
    }
  }
}
```
</details>

Existeix una solució alternativa utilitzant una estructura de dades més complexa que es coneix com a *cua de prioritat* (a C++, `std::priority_queue`). Aquesta estructura té també una operació `push()`, que afegeix un element a la cua, i una operació `pop()`, que extreu l'element més gran que hi hagi a la cua (sense importar l'ordre d'arribada). Per resoldre el nostre problema, podem codificar cada religiós com una parella `{x, nom}`, on `nom` és la string amb el nom del religiós i `x = n * t - i`, on `t` és el tipus del religiós (el de menor rang és un 0, el de major rang un 2) i `i` és la posició d'arribada. Al codificar-ho així, aconseguim que es compleixi que el religiós amb major valor de `x` és el que ha de sortir abans de la cua.

La cua de prioritat triga $\mathcal O(\log n)$ operacions per cada inserció/extracció, així que la complexitat total d'aquesta solució és $\mathcal O(n \log n)$. Això és pitjor que la solució anterior però serà acceptat pel Jutge si s'implementa de manera prou eficient.

## [Problema Q2. Graf especial](https://jutge.org/problems/P78645) <a name="Q2"/>

Un graf amb 7 vèrtexos pot tenir com a molt $\binom{7}{2} = 21$ arestes. Això vol dir que hi ha $2^{21} \cong 2\cdot 10^6$ grafs possibles. Una solució possible és iterar per tots ells i comprovar per cadascun si contenen algun cicle de longitud 6. Depenent de com de bé s'implementi la solució (és a dir, depenent de quant s'aprofitin les simetries del problema per reduir el nombre de grafs a comprovar), el codi pot trigar des d'uns pocs segons fins a uns quants minuts a executar-se. Tanmateix, el problema només ens demana entrar la resposta final, així que podem deixar el programa executant-se en segon pla mentre resolem altres problemes.

A continuació us donem una implementació relativament ràpida:

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n = 7; // nombre de vèrtexs
    int m = n * (n-1) / 2; // nombre màxim d'arestes
    vector<int> best_deg(n, 0); // millor seqüència de graus trobada fins ara
    int best_mask = 0; // graf que assoleix la millor seqüència de graus
    vector<bool> vist(n);
    for(int mask = 0; mask < 1<<m; ++mask) {
        // El bit i-èssim de la representació binària de 'mask' ens diu si la
        // aresta i-èssima està present al graf (ordenant les arestes primer 
        // segons el vèrtex més petit, i després segons el vèrtex més gran).

        // Imprimim el percentatge de grafs que hem comprovat ja.
        if(mask%10000 == 0) 
            cout << mask*100 / (1 << m)  << "\% completat" << endl;

        // Construim el graf.
        vector<vector<int>> G(n); // graf actual
        vector<int> deg(n, 0); // seqüència de graus de G
        int b = 0; // índex de la següent aresta a processar
        bool invalid = false;
        for(int i = 0; i < n and not invalid; ++i) {
            for(int j = i + 1; j < n and not invalid; ++j) {
                if((1 << b)&mask) {
                    // afegim aresta i <-> j
                    G[i].push_back(j);
                    G[j].push_back(i);
                    deg[i]++;
                    deg[j]++;
                }
                ++b;

                // només considerem els grafs amb seqüència de graus decreixent
                // (així ens estalviem de comprovar grafs que, si reordenem els 
                //  vèrtexos, són equivalents a grafs que ja hem vist)
                if(i and deg[i] > deg[i-1]) invalid = true;
            }
        }

        // v := vèrtex que estem processant actualment
        // len := longitud del camí que portem (en nombre d'arestes)
        // inici := vèrtex on hem començat el camí
        function<void(int,int,int)> BuscaCicle = [&](int v, int len, int inici) {
            if(invalid) return;
            if(len == 5) {
                // mirem si podem completar el cicle tornant a l'inici.
                for(int u : G[v]) {
                    if(u == inici) {
                        invalid = true;
                    }
                }
                return;
            }
            vist[v] = true;
            for(int u : G[v]) {
                if(not vist[u]) {
                    // provem d'ampliar el camí anant cap a 'u'
                    BuscaCicle(u, len + 1, inici);
                    
                    // desmarquem 'u' com a visitat, abans de provar el següent vèrtex
                    vist[u] = false;
                }
            }
        };

        // Com que el cicle ha de passar per 6 dels 7 vèrtexos, sabem que ha de contenir per
        // força el vèrtex 0 o el vèrtex 1. 
        fill(vist.begin(), vist.end(), false); // marquem tots els vèrtexos com a no visitats.
        BuscaCicle(0, 0, 0); // Busquem cicles que comencin pel vèrtex 0.
        fill(vist.begin(), vist.end(), false); // marquem tots els vèrtexos com a no visitats.
        BuscaCicle(1, 0, 1); // Busquem cicles que comencin pel vèrtex 1.
        if(not invalid and deg > best_deg) {
            best_deg = deg;
            best_mask = mask;
        }
    }   

    // Imprimim la solució
    cout << endl << endl << " solucio: ";
    for(int x : best_deg) 
        cout << x << " ";
    cout << endl << endl;

    // Imprimim el graf buscat
    cout << "graf:" << endl;
    int b = 0;
    for(int i = 0; i < n; ++i) {
        for(int j = i + 1; j < n; ++j) {
            if((1<<b)&best_mask)
                cout << i << " - " << j << endl;
            ++b;
        }
    }
}
```
</details>

El problema també es podia resoldre raonant-ho per casos, sense cap eina informàtica. Està clar que els dos primers vèrtexos han de tenir grau 6, ja que el graf amb graus `6-6-2-2-2-2-2` no té cap cicle de longitud 6 (sabríeu justificar per què?).

<details><summary><b>Resposta</b></summary>

Suposem que tenim un cicle de longitud 6 en el graf amb graus `6-6-2-2-2-2-2` (és a dir, els vèrtexos 1 i 2 connectats amb tots els altres vèrtexos). Totes les arestes del graf són adjacents al vèrtex 1 o al vèrtex 2. Així doncs, cada 2 vèrtexos consecutius del cicle, almenys un ha de ser el 1 o el 2. Com el cicle té longitud 6, això voldria dir que tres vèrtexos del cicle són el 1 o el 2, així que un dels dos hauria d'estar repetit.
</details>

 Provant d'afegir una aresta addicional, es pot veure que el graf `6-6-3-3-2-2-2` continua sense tenir cap cicle de longitud 6. Per veure que és òptim, cal comprovar que les dues maneres d'afegir una altra aresta (`6-6-4-3-3-2-2` i `6-6-4-4-2-2-2`) contenen cadascuna un cicle de longitud 6, així que no hi ha cap solució millor.

## [Problema G2. Dissenyant estovalles](https://jutge.org/problems/P28763) <a name="G2"/>

Per resoldre el problema, podem guardar-nos una llista amb 3 nombres per cada píxel, representant els valors `(r, g, b)` que portem acumulats. Per cada franja, iterem per tots els píxels que inclou i augmentem els valors de `(r, g, b)` per cadascun d'ells. Al final, prenem el mínim entre el valor que tenim i 255, per tal de no passar-nos del màxim. 

<details><summary><b>Codi (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

m, n = read(int, int)
img = Image.new('RGB', (m, n), 'Black')
dib = ImageDraw.Draw(img)

k = read(int)
color = [[[0,0,0] for _ in range(m)] for _ in range(n)]
for _ in range(k):
  tipus, e, d, r, g, b = read(chr, int, int, int, int, int)
  increment = [r, g, b]
  if tipus == 'V':
    for x in range(e, d+1):
      for y in range(n):
        for z in range(3):
          color[y][x][z] += increment[z]
  else:
    for x in range(m):
      for y in range(e, d+1):
        for z in range(3):
          color[y][x][z] += increment[z]

for x in range(m):
  for y in range(n):
    for z in range(3):
      color[y][x][z] = min(color[y][x][z], 255)
    dib.point((x, y), tuple(color[y][x]))

img.save('output.png')
```
</details>

Tot i que la implementació anterior ja dona AC, podem resoldre el problema de manera més eficient si en lloc d'iterar per tots els píxels que conté cada franja, iterem per les coordenades verticals i/o horitzontals, i només al final (quan estem pintant els píxels), calculem la suma entre les aportacions de les franges verticals i horitzontals. Això redueix la complexitat asimptòtica de $\mathcal O(f \cdot m \cdot n)$ a $\mathcal O(f \cdot m + f \cdot n + m \cdot n)$.

<details><summary><b>Codi millorat (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

m, n = read(int, int)
img = Image.new('RGB', (m, n), 'Black')
dib = ImageDraw.Draw(img)

k = read(int)
color_vertical = [[0, 0, 0] for _ in range(m)]
color_horitzontal = [[0, 0, 0] for _ in range(n)]
for _ in range(k):
  tipus, e, d, r, g, b = read(chr, int, int, int, int, int)
  color = color_vertical if tipus == 'V' else color_horitzontal
  for x in range(e, d+1):
    color[x] = [sum(z) for z in zip(color[x], [r, g, b])]

for x in range(m):
  for y in range(n):
    color = map(lambda z : min(z, 255), map(sum, zip(color_vertical[x], color_horitzontal[y])))
    dib.point((x, y), tuple(color))


img.save('output.png')
```
</details>

Encara es pot reduir més la complexitat fins a $\mathcal O(f + m \cdot n)$. Sabríeu com fer-ho?

<details><summary><b>Pista</b></summary>
Proveu de plantejar el problema a partir de l'array de diferències.
</details>

## [Problema C5. Saltant amunt](https://jutge.org/problems/P57741) <a name="C5"/>

Aquest problema és un exemple típic de <a href="https://aprende.olimpiada-informatica.org/algoritmia-dinamica-1">programació dinàmica</a>. Si `cost[i]` és el cost de trepitjar l'esglaó $i$-èssim, i definim `dp[i][j]` com el cost mínim d'arribar fins a l'esglaó $i$-èssim amb un últim salt de longitud $j$, aleshores podem calcular `dp[i][j]` a partir dels valors de `cost[i]` i de `dp[i-j][1]`, ..., `dp[i-j][s]`.

Així doncs, podem calcular els valors de `dp[i][j]` iterant per totes les $i$ i $j$, en ordre creixent de $i$. Per calcular cada `dp[i][j]` hem d'iterar com a molt per $s$ altres valors, així que la complexitat total és $\mathcal O(n \cdot s^2)$. 

<details><summary><b>Codi iteratiu (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std; 
 
int main(){
  int s, n;
  while(cin >> s >> n) {
    vector<int> cost(n);
    for(int& x : cost) 
      cin >> x;
    cost.push_back(0); // Afegim un esglaó extra al final de cost 0, per simplificar el càlcul de la DP.

    int const INF = 1e9;
    // dp[i][j] := minim cost per arribar a l'esglaó i-èssim, on a l'últim pas hem saltat j esglaons.
    vector<vector<int>> dp(n+1, vector<int>(s+1, INF));
    // Podem suposar que al principi venim d'un salt de mida 1, ja que llavors al proper podem saltar
    // qualsevol longitud des de 1 fins a s.
    dp[0][1] = cost[0];
    for(int i = 1; i <= n; ++i) {
      for(int last = 1; last <= s; ++last) {
        for(int salt = 1; salt <= min(i, s + 1 - last); ++salt) {
          dp[i][salt] = min(dp[i][salt], dp[i - salt][last] + cost[i]);
        }
      }
    }
    int ans = INF;
    for(int salt = 1; salt <= s; ++salt)
      ans = min(ans, dp[n][salt]);
    cout << ans << endl;
  }
}
```
</details>

<details><summary><b>Codi recursiu (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std; 

int n, s; // nombre d'esglaons i màxima longitud de salt.
vector<int> cost; // cost[i] := cost que hem de pagar per trepitjar l'esglaó i-èssim
vector<vector<int>> dp; // dp[i][j] := minim cost per arribar a l'esglaó i-èssim, 
                        // on a l'últim pas hem saltat j esglaons.
                        // Si dp[i][j] == -1, vol dir que encara no ho hem calculat.
int const INF = 1e9;

// Calcula dp[i][salt].
// Si ja l'hem calculat abans, en retorna el valor directament, per evitar repetir
// càlculs innecessàriament.
int GetDP(int i, int salt) {
    if(dp[i][salt] != -1) return dp[i][salt]; // si ja l'hem calculat anteriorment.

    // Cas base de la recursió (quan estem en el primer esglaó). 
    if(i == 0)
      return cost[0];

    // És impossible que l'últim salt sigui més gran que el nombre d'esglaó on ens trobem. 
    if(i < salt)
      return INF;

    // Calculem dp[i][salt]:
    dp[i][salt] = INF;
    for(int salt_previ = 1; salt_previ <= s + 1 - salt; ++salt_previ) {
        dp[i][salt] = min(dp[i][salt], cost[i] + GetDP(i - salt, salt_previ));
    }
    return dp[i][salt];
}


int main(){
  while(cin >> s >> n) {
    cost = vector<int>(n);
    for(int& x : cost) 
      cin >> x;
    cost.push_back(0); // Afegim un esglaó extra al final de cost 0, per simplificar el càlcul de la DP.

    dp = vector<vector<int>>(n+1, vector<int>(s+1, -1));

    int ans = INF;
    for(int salt = 1; salt <= s; ++salt)
      ans = min(ans, GetDP(n, salt));
    cout << ans << endl;
  }
}
```
</details>

## [Problema C6. Camins màxims en un graf](https://jutge.org/problems/P51388) <a name="C6"/>

Si només volguéssim resoldre el problema des d'un vèrtex, podríem fer un recorregut en amplada (BFS) o en profunditat (DFS) des del vèrtex donat, i retornar la major de les distàncies que hem calculat.

Això no tindria perquè funcionar en un graf arbitrari, ja que ni el BFS ni el DFS tenen per què explorar els vèrtexos en l'ordre necessari per trobar el camí més llarg. Ara bé, com que ens diuen que el graf donat no té cicles, sabem que entre cada parella de vèrtexos hi haurà com a molt un únic camí que va d'un a l'altre sense repetir vèrtexos. Això voldrà dir que obtindrem les distàncies correctes sense importar el criteri que utilitzem per recórrer el graf.

Repetint el procediment per cada un dels $n$ vèrtexos, obtenim una solució $\mathcal O(n^2)$, que és suficient per rebre la puntuació parcial: 

<details><summary><b>Codi - Solució parcial (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    while(cin >> n >> m) {
        vector<vector<int>> G(n);
        while(m--) {
            int a, b;
            cin >> a >> b;
            G[a].push_back(b);
            G[b].push_back(a);
        }
        vector<bool> vist(n);

        function<int(int)> DFS = [&](int v) {
            vist[v] = true;
            int ans = 0;
            for(int u : G[v]) {
                if(not vist[u]) {
                    ans = max(ans, 1 + DFS(u));
                }
            }
            return ans;
        };

        for(int v = 0; v < n; ++v) {
            fill(vist.begin(), vist.end(), false);
            int ans = DFS(v);
            cout << ans;
            cout << (v == n-1? '\n' : ' ');
        }
    }
}
```
</details>

Per rebre la puntuació completa cal millorar l'algorisme fins a $\mathcal O(n)$. Una possible manera és la següent:

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    cin.tie(NULL);
    ios_base::sync_with_stdio(false);
    int n, m;
    while(cin >> n >> m) {
        vector<vector<int>> G(n);
        while(m--) {
            int u, v;
            cin >> u >> v;
            G[u].push_back(v);
            G[v].push_back(u);
        }

        // dist_fill[v] := distància més gran que podem aconseguir anant cap a un fill de v
        vector<int> dist_fill(n, 0); 
        
        // millor_fill[v] := fill que assoleix dist_fill[v]         
        vector<int> millor_fill(n, -1); 
        
        // segon_dist_fill[v] := distància més gran que podem aconseguir anant cap a un fill
        //                       de v diferent de millor_fill[v]
        vector<int> segon_dist_fill(n, 0);            
        vector<int> pare(n, -1); // pare[v] := pare de v en l'arbre
        vector<bool> vist(n, false); // vist[v] := true si ja hem visitat el vèrtex v

        // Calcula els valors de dist_fill[v] i segon_dist_fill[v]
        function<void(int)> DFS_Down = [&](int v) {
            vist[v] = true;
            for(int u : G[v]) {
                if(u == pare[v]) continue;
                pare[u] = v;
                DFS_Down(u);
                if(dist_fill[v] < dist_fill[u] + 1) {
                    segon_dist_fill[v] = dist_fill[v];
                    dist_fill[v] = dist_fill[u] + 1;
                    millor_fill[v] = u;
                }
                else if(segon_dist_fill[v] < dist_fill[u] + 1) {
                    segon_dist_fill[v] = dist_fill[u] + 1;
                }
            }
        };

        for(int i = 0; i < n; ++i) {
            if(not vist[i])
                DFS_Down(i);
        }

        // dist_pare[v] := distància més gran que podem aconseguir anant cap al pare de v
        vector<int> dist_pare(n, 0); 

        // Calcula el valor de dist_pare[v]
        function<void(int)> DFS_Up = [&](int v) {
            vist[v] = true;
            if(pare[v] == -1) return;
            if(not vist[pare[v]])
                DFS_Up(pare[v]);
            dist_pare[v] = 1 + max(dist_pare[pare[v]], (millor_fill[pare[v]] == v? segon_dist_fill[pare[v]] : dist_fill[pare[v]]));
        };

        vist = vector<bool>(n, false);
        for(int i = 0; i < n; ++i) {
            if(not vist[i])
                DFS_Up(i);
        }
        for(int i = 0; i < n; ++i) {
            if(i) cout << ' ';
            cout << max(dist_fill[i], dist_pare[i]);
        }
        cout << endl;
    }
}
```
</details>

Aquest codi utilitza que els grafs sense cicles són una unió d'arbres (el que, per raons òbvies, es coneix en llenguatge matemàtic com un *bosc*). Per cada arbre, fixem arbitràriament un vèrtex com a arrel. Aleshores, cada vèrtex de l'arbre té un *pare* (el vèrtex veí que té en direcció a l'arrel) i zero o més *fills* (els veïns que té en direcció contrària a l'arrel).

Per cada vèrtex, calculem el camí més llarg que es pot aconseguir anant cap a un fill (que serà el màxim dels camins dels fills més 1) i el camí més llarg que es pot aconseguir anant cap al pare. La primera quantitat la podem calcular fent un DFS des de l'arrel cap avall, i la segona fent un DFS en direcció contrària. La única dificultat que ens trobem és que, per calcular el camí més llarg anant cap al pare, hem de descartar els camins que van al pare i tornen a baixar cap al mateix vèrtex. Per fer-ho, ens guardem no només la longitud del camí més llarg cap als fills, sinó també quin fill és el que l'assoleix i quina és la longitud del camí més llarg que no passa per aquell fill. Així, al calcular el camí més llarg anant cap al pare, comprovem si el vèrtex actual és el fill del pare que assoleix el camí més llarg, i en aquest cas, utilitzem el segon camí més llarg (del pare als fills) en lloc del camí més llarg.

<details><summary><b>Repte</b></summary>
Sabríeu trobar una solució alternativa basada en trobar el diàmetre de cada arbre? 
</details>

## [Problema C7. Estacions de radar](https://jutge.org/problems/P45007) <a name="C7"/>

En aquest problema ens donen fins a 1000 punts amb coordenades enteres en el pla, i ens demanen comptar el nombre de triangles acutangles (és a dir, amb els tres angles menors que 90 graus) que podem formar amb ells.

La solució trivial és $\mathcal O(n^3)$ (iterant per cada trio de punts i comprovant si formen un triangle acutangle) i és massa lenta per superar els casos grans. Per millorar-ho, ens hem de fixar en el fet que cada triangle té com a molt un únic angle que mesuri $\geq 90$ graus. Així doncs, podem deduir el nombre de triangles acutangles a partir del nombre total de triangles ($\binom{n}{3} = n(n-1)(n-2)/6$) i del nombre d'angles aguts, utilitzant que 

```math
\begin{align*}
\text{nombre de angles aguts} &= 3 \cdot \text{nombre de triangles acutangles} + 2 \cdot \text{nombre de triangles no acutangles} \\
&= 3 \cdot \text{nombre de triangles acutangles} + 2 \cdot (\text{nombre total de triangles} -\text{nombre de triangles acutangles}) \\
&= \text{nombre de triangles acutangles} + 2\binom{n}{3}
\end{align*}
```

Per calcular el nombre d'angles aguts, fixem un punt (el vèrtex central de l'angle) i ordenem la resta de punts angularment al seu voltant. Aleshores iterem per la resta de punts mantenint un segon punter que apunta al primer punt que fa un angle $\geq 90$ graus amb el punt central i el punt pel que estem iterant en aquell moment. El nombre d'angles aguts serà doncs la diferència entre els dos punters (menys 1).

Per suposat, tot i que parlem de punters en l'explicació anterior (i aquesta tècnica es coneix habitualment com a <a href="https://usaco.guide/silver/two-pointers?lang=cpp">two-pointers method</a>) és molt més còmode iterar utilitzant els índexos de l'array, anant amb compte quan el segon punter "dona la volta" i torna a començar pel principi.

Per a la implementació, és recomanable fer totes les operacions amb enters. Així s'eviten errors de precisió i es redueix el factor constant de la complexitat asimptòtica, que és de $\mathcal O(n^2)$.

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;

struct Punt {
    int x, y;
    Punt operator-(Punt const& other) const {
        return Punt{x - other.x, y - other.y};
    }
    Punt Rota90Horari() const {
        return Punt{y, -x};
    }
};

ll CrossProduct(Punt const& p, Punt const& q) {
    return ll(p.x) * q.y - ll(p.y) * q.x;
}

// Retorna true si Angle(p) es troba entre [0, 180) graus.
// (En cas que p == (0,0), retorna true).
bool Up (Punt p) {
  return p.y > 0 or (p.y == 0 and p.x >= 0);
}

// Retorna true si Angle(a) < Angle(b), ordenant els angles en l'interval [-180, 180).
bool Compara(Punt a, Punt b){
    if (Up(a) == Up(b)) 
        return CrossProduct(a, b) > 0;
    return Up(a) < Up(b);    
}

// Retorna true si l'angle des de p fins a q en sentit antihorari és menor de 90 graus.
bool AngleMenysDe90(Punt const& p, Punt const& q) {
    return CrossProduct(p, q) > 0 and CrossProduct(q.Rota90Horari(), p) > 0;
}

int main() {
    int n;
    while(cin >> n) {
        vector<Punt> points(n);
        for (int i = 0; i < n; ++i) 
            cin >> points[i].x >> points[i].y;

        ll total_triangles = ll(n) * (n - 1) * (n - 2) / 6;
        ll angles_bons = 0; // nombre de angles < 90 graus

        for (int i = 0; i < n; ++i){
            vector<Punt> v; // vectors entre el punt i-èssim i la resta de punts.
            for (int j = 0; j < n; ++j){
                if (i != j) 
                    v.push_back(points[j] - points[i]);
            }
            sort(v.begin(), v.end(), Compara);  // ordenem els vectors per angle.

            int m = v.size();
            int k = 1; // primer index tal que l'angle entre v[j] i v[k] es >= 90 graus.
            for(int j = 0; j < m; ++j) {
                k = max(k, j + 1);
                while(k%m != j and AngleMenysDe90(v[j], v[k%m])) {
                    ++k;
                }
                angles_bons += k - j - 1;
            }
        }
        // angles_bons = 3 * triangles_acutangles + 2 * (total_triangles - triangles_acutangles)
        // ==>  triangles_acutangles = angles_bons - 2 * total_triangles
        cout << angles_bons - 2 * total_triangles << endl;
    }
}
```
</details>
