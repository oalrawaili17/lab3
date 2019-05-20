# lab3
lab3 361
#ifndef ADJMATRIX_H
#define ADJMATRIX_H
#include <iostream>
#include <queue>
#include <limits>
#include <vector>

class GraphM
{
  int vertexCount;
  //WHITE means Undiscovered, GRAY means Discovered, BLACK menas Processed
  enum class Color{ WHITE, GRAY, BLACK };

  const int imax = std::numeric_limits<int>::max();

  struct Vertex
  {
    int id;
    Color color;
    size_t distance;

    Vertex(const int vertex, Color clr, int imax) : id(vertex),
                                                    color(clr),
                                                    distance(imax)
                                                    {};
  };
  std::vector< std::vector<bool> > adjMatrix; //adjacency adjMatrix
  std::vector<Vertex> vertices;
public:
    GraphM(int size)
  {
    vertexCount = size;
    adjMatrix.resize(vertexCount, std::vector<bool>(vertexCount));
    for(int i = 0; i < vertexCount; i++)
    {
      vertices.push_back( Vertex(i, Color::WHITE, imax));
      for(int j = 0; j < vertexCount; j++)
        adjMatrix[i][j] = false;
    }
  }
    ~GraphM() {};

    void BFS(const int);
    void addEdge(int, int);
    void printPath(const int,  const int) const;

};

void GraphM::BFS(const int src)
{
    const auto s = vertices[src].id;
    vertices[s].color = Color::GRAY;
    vertices[s].distance = 0;


    std::queue<int> Q;
    Q.push(vertices[s].id);

    while(!Q.empty())
    {
        auto u = Q.front();
        Q.pop();

        for (int j = 0; j < vertexCount; j++)
        {
            if(vertices[j].color == Color::WHITE && adjMatrix[u][j] == true)
            {
                vertices[j].color = Color::GRAY;
                vertices[j].distance = vertices[u].distance + 1;
                Q.push(j);
            }
        }
        vertices[u].color = Color::BLACK;
    }
}
// take u and v and add them to the matrix.
void GraphM::addEdge(int u, int v)
{
    adjMatrix[u][v] = true;
  adjMatrix[v][u] = true;
}
// to print the shortest path.
void GraphM::printPath(const int src, const int ver) const
{
    auto s = vertices[src].id;
    auto v = vertices[ver].id;

  std::cout << s;
  for(int j = s + 1; j <= v; j++)
  {
    if(adjMatrix[s][j] == true)
    {
      std::cout << " --> "<< j;
      s = j;
    }
  }
}



#endif // ADJMATRIX_H
