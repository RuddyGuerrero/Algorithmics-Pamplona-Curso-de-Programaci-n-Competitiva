# 🔵—🟢 Recorrido en grafos: BFS (Búsqueda en anchura)

## Breve contexto histórico

El algoritmo de **búsqueda en anchura (Breadth-First Search, BFS)** surge de los primeros estudios formales sobre grafos y estructuras de datos durante el siglo **XX**, cuando comenzaron a desarrollarse los **primeros algoritmos de exploración sistemática de redes**.

BFS se popularizó con el desarrollo de la informática moderna, debido a que se adapta de forma natural al uso de **colas**, una estructura fundamental en programación.

> BFS es uno de los algoritmos básicos que todo estudiante de ciencias de la computación debe dominar.

## ✔️ Importancia del recorrido en grafos

Recorrer un grafo significa **visitar todos sus nodos siguiendo un criterio específico**.  
La búsqueda en anchura es especialmente importante porque:

- Permite encontrar **la distancia mínima** entre nodos en grafos no ponderados.
- Es base para numerosos algoritmos más avanzados.
- Se utiliza en **mapas**, **redes sociales**, **sistemas distribuidos** y **juegos**.
- Modela exploraciones **por niveles**, comunes en problemas reales.

## 🌐 Fundamentos de la búsqueda en anchura (BFS)

### 1. ¿Qué es BFS?

La **búsqueda en anchura (BFS)** es un algoritmo de recorrido que explora los nodos de un grafo **nivel por nivel**, partiendo desde un nodo inicial.

- Primero visita todos los vecinos directos
- Luego los vecinos de esos vecinos
- Continúa hasta recorrer todo el grafo

### 2. Idea principal de BFS

La idea fundamental de BFS es:

> **Visitar primero los nodos más cercanos al origen.**

Para lograr esto, el algoritmo utiliza una **cola (queue)** que sigue el principio:

- **FIFO (First In, First Out)**

Esto garantiza que los nodos se procesen en el orden correcto.

### 3. Estructuras necesarias

Para implementar BFS se necesitan tres elementos básicos:

- Un **registro de nodos visitados**
- Una **cola**
- Un **nodo inicial**

Estas estructuras evitan:

- Visitas repetidas
- Ciclos infinitos

## 🔄 Funcionamiento del algoritmo BFS

### Pasos del algoritmo

1. Marcar el nodo inicial como visitado.
2. Insertarlo en una cola.
3. Mientras la cola no esté vacía:
   - Extraer el primer nodo
   - Visitar todos sus vecinos no visitados
   - Marcar y encolar cada vecino

### Ejemplo paso a paso

Partiendo del nodo **0** en el siguiente grafo:

- **Grafo no dirigido (lista de adyacencia):**

```
0: 1, 2
1: 0, 3, 4
2: 0, 5
3: 1
4: 1, 5
5: 2, 4
```

**Orden de recorrido BFS desde 0:**

```
0 → 1 → 2 → 3 → 4 → 5
```

Primero se recorren los nodos más cercanos (nivel 1: 1 y 2), luego los del nivel 2 (3, 4, 5).

## 🧠 Propiedades de BFS

- Garantiza el **camino más corto** en grafos no ponderados.
- Funciona en grafos **dirigidos y no dirigidos**.
- Es equivalente a un **recorrido por niveles** en árboles.
- Permite detectar **componentes conexas**.

---

## ⏱️ Complejidad del algoritmo

| Tipo | Complejidad |
|----|------------|
| Tiempo | **O(V + E)** |
| Memoria | **O(V)** |

Donde:

- **V** es el número de vértices
- **E** es el número de aristas

---

## 🛠️ Implementación del algoritmo BFS

### Usando listas de adyacencia

```python
from collections import deque

def bfs(grafo, inicio):
    """
    Recorre el grafo en anchura desde el nodo 'inicio' e imprime
    el orden de visita. El grafo es un dict[int, list[int]].
    """
    visitados = set([inicio])
    cola = deque([inicio])

    orden = []  # Para almacenar el orden de visita

    while cola:
        nodo = cola.popleft()
        orden.append(nodo)

        for vecino in grafo.get(nodo, []):
            if vecino not in visitados:
                visitados.add(vecino)
                cola.append(vecino)

    return orden

# Ejemplo de uso
if __name__ == "__main__":
    grafo = {
        0: [1, 2],
        1: [0, 3, 4],
        2: [0, 5],
        3: [1],
        4: [1, 5],
        5: [2, 4]
    }

    orden_visita = bfs(grafo, 0)
    print("Orden BFS desde 0:", orden_visita)
```

**Salida esperada:**

```
Orden BFS desde 0: [0, 1, 2, 3, 4, 5]
```

---

### Cálculo de **distancias mínimas** (en número de aristas)

```python
from collections import deque

def bfs_distancias(grafo, inicio):
    """
    Devuelve un diccionario con la distancia mínima desde 'inicio'
    hasta cada nodo alcanzable. Distancia en número de aristas.
    """
    dist = {inicio: 0}
    cola = deque([inicio])

    while cola:
        nodo = cola.popleft()
        for vecino in grafo.get(nodo, []):
            if vecino not in dist:
                dist[vecino] = dist[nodo] + 1
                cola.append(vecino)
    return dist

# Ejemplo de uso
if __name__ == "__main__":
    grafo = {
        0: [1, 2],
        1: [0, 3, 4],
        2: [0, 5],
        3: [1],
        4: [1, 5],
        5: [2, 4]
    }

    dist = bfs_distancias(grafo, 0)
    print("Distancias mínimas desde 0:", dist)
```

**Salida esperada:**

```
Distancias mínimas desde 0: {0: 0, 1: 1, 2: 1, 3: 2, 4: 2, 5: 2}
```

---

### Implementación equivalente en **C++** (con nodos numéricos)

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> bfs(const vector<vector<int>>& grafo, int inicio) {
    int n = (int)grafo.size();
    vector<int> orden; orden.reserve(n);
    vector<char> visitado(n, false);
    queue<int> q;

    visitado[inicio] = true;
    q.push(inicio);

    while (!q.empty()) {
        int u = q.front(); q.pop();
        orden.push_back(u);
        for (int v : grafo[u]) {
            if (!visitado[v]) {
                visitado[v] = true;
                q.push(v);
            }
        }
    }
    return orden;
}

vector<int> bfs_dist(const vector<vector<int>>& grafo, int inicio) {
    int n = (int)grafo.size();
    const int INF = 1e9;
    vector<int> dist(n, INF);
    queue<int> q;

    dist[inicio] = 0;
    q.push(inicio);

    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : grafo[u]) {
            if (dist[v] == INF) {
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }
    return dist;
}

int main() {
    // Grafo no dirigido con 6 nodos (0..5)
    vector<vector<int>> grafo(6);
    grafo[0] = {1, 2};
    grafo[1] = {0, 3, 4};
    grafo[2] = {0, 5};
    grafo[3] = {1};
    grafo[4] = {1, 5};
    grafo[5] = {2, 4};

    auto orden = bfs(grafo, 0);
    cout << "Orden BFS desde 0: ";
    for (int x : orden) cout << x << ' ';
    cout << "\n";

    auto dist = bfs_dist(grafo, 0);
    cout << "Distancias mínimas desde 0: ";
    for (int i = 0; i < (int)dist.size(); ++i) {
        if (dist[i] < (int)1e9) cout << i << ":" << dist[i] << ' ';
        else cout << i << ":INF ";
    }
    cout << "\n";
}
```

**Salida esperada (aprox.):**

```
Orden BFS desde 0: 0 1 2 3 4 5 
Distancias mínimas desde 0: 0:0 1:1 2:1 3:2 4:2 5:2 
```

---

## ⚠️ Errores comunes

- No marcar nodos como visitados (o hacerlo **después** de encolar, provocando duplicados).
- Usar una pila en lugar de una cola.
- Esperar rutas de menor **costo** en grafos **ponderados** (para eso usar Dijkstra).

---

## 📝 Ejercicios propuestos

1. Implementar BFS en un grafo **dirigido** con nodos numéricos y reportar el **orden** de visita.
2. Determinar si un grafo no dirigido es **conexo** con BFS desde el nodo 0.
3. Calcular la **distancia mínima** desde 0 a cada nodo (o `INF` si no es alcanzable).
4. Modelar un **laberinto** como grafo grid (celdas libres = nodos) y resolverlo con BFS.
