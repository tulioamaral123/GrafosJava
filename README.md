# GrafosJava — Roteamento em mapas reais com Dijkstra

Aplicação desktop em **Java (Swing)** que transforma mapas reais do **OpenStreetMap** em um **grafo ponderado** e calcula a **menor rota** entre dois pontos usando o **algoritmo de Dijkstra** com fila de prioridade.

O projeto inclui mapas do **Campus Samambaia (Campus 2) da UFG** e arredores, em Goiânia, prontos para uso.

## Funcionalidades

- **Importação de mapas OSM (`.osm`)**: leitura via SAX em duas etapas. Primeiro são mapeados todos os nós com suas coordenadas e depois as vias (`highway`) viram arestas. Vias de mão única (`oneway=yes`) geram arestas direcionadas.
- **Importação de grafos `.poly`**: formato de texto simples com vértices (coordenadas) e arestas.
- **Conversor `.osm` → `.poly`** (`OsmToPolyConverter`): ferramenta de linha de comando que gera o arquivo `.poly` a partir de um mapa OSM.
- **Menor caminho com Dijkstra**: você seleciona a origem e o destino clicando no mapa, e a rota é destacada na tela.
- **Estatísticas do algoritmo**: tempo de execução (ms), número de nós explorados e custo total do caminho.
- **Edição interativa do grafo**: adicionar vértices e arestas, remover elementos e rotular arestas com seus pesos.

## Algoritmo

O menor caminho é calculado com **Dijkstra** usando `PriorityQueue`, com complexidade **O((V + E) log V)**.
O peso de cada aresta é a distância entre os dois nós, calculada a partir das coordenadas. Por enquanto é usada a distância euclidiana, uma aproximação adequada para áreas pequenas como um campus.

## Estrutura do projeto

```
GrafosJava/
├── Main.java                 # Ponto de entrada (inicia a GUI na thread do Swing)
├── OsmToPolyConverter.java   # CLI: converte .osm em .poly
├── gui/
│   ├── NavigationApp.java    # Janela principal, controles e estatísticas
│   └── GraphPanel.java       # Desenho do grafo e da rota
├── io/
│   ├── OsmConverter.java     # Leitura de .osm (SAX) → Graph
│   └── PolyReader.java       # Leitura de .poly → Graph
├── model/
│   ├── Graph.java            # Lista de adjacência + Dijkstra
│   ├── Node.java / Edge.java
│   └── OsmNode.java / OsmWay.java
└── Mapas/                    # Mapas de exemplo (UFG e região)
```

## Como executar

Requisitos: **Java 8 ou superior**.

```bash
# Compilar
javac -d bin *.java gui/*.java io/*.java model/*.java

# Executar a aplicação
java -cp bin Main
```

Na janela, clique em **Importar .osm** ou **Importar .poly** e escolha um arquivo da pasta `Mapas/`. Depois, selecione a **Origem** e o **Destino** no mapa e clique em **Traçar Menor Caminho**.

### Converter um mapa OSM para `.poly`

```bash
java -cp bin OsmToPolyConverter Mapas/ufg.osm
# gera Mapas/ufg.poly
```

Para usar outra região, exporte a área desejada em [openstreetmap.org](https://www.openstreetmap.org/export) (formato `.osm`) e importe na aplicação.

## Mapas incluídos

| Arquivo | Descrição |
|---|---|
| `Campus2UFG&Regiao.osm` / `.poly` | Campus 2 da UFG e região ao redor |
| `ufg.osm` | Área do campus da UFG |
| `map.osm` / `map.poly` | Mapa menor, bom para testes rápidos |

## Próximos passos

- Usar a fórmula de **Haversine** para distâncias geográficas mais precisas
- Implementar o **A\*** e comparar o número de nós explorados com o Dijkstra
- Considerar o tipo de via e a velocidade no cálculo do peso (rota mais rápida em vez de mais curta)

## Tecnologias

Java · Swing · SAX (XML) · Estruturas de dados (grafos, lista de adjacência, fila de prioridade)

## Licença

Distribuído sob a licença MIT.
