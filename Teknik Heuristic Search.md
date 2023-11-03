# Rizky Rizaldi Mastiyanto 
# 5311421085
# 1. Pelajari class EightPuzzleSearch, EightPuzzleSpace, dan Node.
- Kelas EightPuzzleSearch digunakan dalam konteks permainan teka-teki delapan kotak untuk menerapkan algoritma pencarian solusi.
- Kelas EightPuzzleSpace menggambarkan keadaan dalam permainan teka-teki delapan kotak, mencakup data tentang keadaan, langkah-langkah, dan aturan permainan.
- Objek Node digunakan dalam algoritma pencarian, mewakili satu keadaan dalam ruang pencarian dengan informasi tentang keadaan, biaya, atau jarak.

# 2. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya Gambar 5.8. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal state. Analisa dan bedakan dengan solusi pada point 1.
# Code:
    package JavaApplivation2;
    import java.util.Vector;

    class Node {
    int[] state = new int[9];
    int cost;
    Node parent = null;
    Vector<Node> successors = new Vector<Node>();

    Node(int s[], Node parent) {
        this.parent = parent;
        for (int i = 0; i < 9; i++)
            state[i] = s[i];
    }

    public Node() {
    }

    public String toString() {
        String s = "";
        for (int i = 0; i < 9; i++) {
            s = s + state[i] + " ";
        }
        return s;
    }

    public boolean equals(Object node) {
        Node n = (Node) node;
        boolean result = true;
        for (int i = 0; i < 9; i++) {
            if (n.state[i] != state[i])
                result = false;
        }
        return result;
    }

    Vector<Node> getPath(Vector<Node> v) {
        v.insertElementAt(this, 0);
        if (parent != null)
            v = parent.getPath(v);
        return v;
    }

    Vector<Node> getPath() {
        return getPath(new Vector<Node>());
    }

    public static void main(String args[]) {
        new Node().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSpace {
    Node getRoot() {
        int initialState[] = {3, 1, 2, 4, 7, 5, 6, 8, 0};
        return new Node(initialState, null);
    }

    Node getGoal() {
        int goalState[] = {1, 2, 3, 4, 0, 8, 5, 6, 7};
        return new Node(goalState, null);
    }

    Vector<Node> getSuccessors(Node parent) {
        Vector<Node> successors = new Vector<Node>();
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                if (parent.state[(r * 3) + c] == 0) {
                    if (r > 0) {
                        successors.add(transformState(r - 1, c, r, c, parent));
                    }
                    if (r < 2) {
                        successors.add(transformState(r + 1, c, r, c, parent));
                    }
                    if (c > 0) {
                        successors.add(transformState(r, c - 1, r, c, parent));
                    }
                    if (c < 2) {
                        successors.add(transformState(r, c + 1, r, c, parent));
                    }
                }
            }
        }
        parent.successors = successors;
        return successors;
    }

    Node transformState(int r0, int c0, int r1, int c1, Node parent) {
        int[] s = parent.state;
        int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
        newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
        newState[(r0 * 3) + c0] = 0;
        return new Node(newState, parent);
    }

    public static void main(String args[]) {
        new EightPuzzleSpace().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSearch {
    EightPuzzleSpace space = new EightPuzzleSpace();
    Vector<Node> open = new Vector<Node>();
    Vector<Node> closed = new Vector<Node>();

    int h1Cost(Node node) {
        int cost = 0;
        for (int i = 0; i < node.state.length; i++) {
            if (node.state[i] != i) cost++;
        }
        return cost;
    }

    int h2Cost(Node node) {
        int cost = 0;
        int state[] = node.state;
        for (int i = 0; i < state.length; i++) {
            int v0 = i, v1 = state[i];
            if (v1 == 0)
                continue;
            int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
            int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
            cost += c;
        }
        return cost;
    }

    /* Boleh diubah dengan memakai heuristic h1 atau h2 */
    int hCost(Node node) {
        return h1Cost(node);
    }

    Node getBestNode(Vector nodes) {
        int index = 0, minCost = Integer.MAX_VALUE;
        for (int i = 0; i < nodes.size(); i++) {
            Node node = (Node) nodes.elementAt(i);
            if (node.cost < minCost) {
                minCost = node.cost;
                index = i;
            }
        }
        Node bestNode = (Node) nodes.remove(index);
        return bestNode;
    }

    int getPreviousCost(Node node) {
        int i = open.indexOf(node);
        int cost = Integer.MAX_VALUE;
        if (i != -1) {
            cost = open.get(i).cost;
        } else {
            i = closed.indexOf(node);
            if (i != -1)
                cost = closed.get(i).cost;
        }
        return cost;
    }

    void printPath(Vector path) {
        for (int i = 0; i < path.size(); i++) {
            System.out.print(" " + path.elementAt(i) + "\n");
        }
    }

    void run() {
        Node root = space.getRoot();
        Node goal = space.getGoal();
        Node solution = null;
        open.add(root);
        System.out.print("\nRoot: " + root + "\n\n");
        while (open.size() > 0) {
            Node node = getBestNode(open);
            int pathLength = node.getPath().size();
            closed.add(node);
            if (node.equals(goal)) {
                solution = node;
                break;
            }
            Vector<Node> successors = space.getSuccessors(node);
            for (int i = 0; i < successors.size(); i++) {
                Node successor = successors.get(i);
                int cost = hCost(successor) + pathLength + 1;
                int previousCost;
                previousCost = getPreviousCost(successor);
                boolean inClosed;
                inClosed = closed.contains(successor);
                boolean inOpen = open.contains(successor);
                if (!(inClosed || inOpen) || cost < previousCost) {
                    if (inClosed)
                        closed.remove(successor);
                    if (!inOpen)
                        open.add(successor);
                    successor.cost = cost;
                    successor.parent = node;
                }
            }
        }
        if (solution != null) {
            Vector path = solution.getPath();
            System.out.print("\nSolution found\n");
            printPath(path);
        }
    }

    public static void main(String args[]) {
        new EightPuzzleSearch().run();
    }
    }
![image](https://github.com/rizkyrizaldim/pic/assets/148876602/025d962b-6536-41ac-b2c9-e243115eb232)


Dalam kode Eight Puzzle yang diberikan, proses untuk mencapai goal state dari initial state terletak di dalam class `EightPuzzleSearch`. Class ini menggunakan algoritma pencarian A* dengan opsi pemilihan heuristic `h1` atau `h2`. Berikut adalah langkah-langkah yang diambil untuk mencapai goal state:

Pertama-tama, initial state dan goal state diinisialisasi. Selanjutnya, sebuah node awal (root) dibuat dengan menggunakan initial state. Node awal ini kemudian dimasukkan ke dalam daftar `open`.

Selama daftar `open` masih memiliki elemen, langkah-langkah berikut diulang:
1. Memilih node dengan biaya terkecil, yang dihitung berdasarkan heuristic dan panjang path.
2. Menambahkan node terpilih ke dalam daftar `closed` untuk menghindari pengulangan.
3. Jika node terpilih adalah goal state, maka solusi telah ditemukan dan proses pencarian berakhir.
4. Mendapatkan semua node penerus dari node terpilih.
5. Untuk setiap node penerus, biaya baru dihitung dengan mempertimbangkan heuristic, panjang path, dan biaya sebelumnya.
6. Jika node penerus belum pernah ada di daftar `open` atau memiliki biaya yang lebih rendah, node penerus ditambahkan ke daftar `open` dengan biaya yang diperbarui.

Langkah-langkah ini diulang sampai solusi ditemukan atau semua kemungkinan solusi telah dieksplorasi. Hasil akhir dari proses ini adalah pencetakan langkah-langkah solusi pada output.

Class `EightPuzzleSearch` bertanggung jawab untuk melaksanakan algoritma pencarian A* dan mengelola logika pencarian. `EightPuzzleSpace` menyediakan informasi tentang initial state dan goal state, serta menghasilkan node-node awal. Sedangkan `Node` mewakili setiap keadaan dalam permainan Eight Puzzle dan menyimpan informasi tentang keadaan, biaya, dan node induknya. Ketiga class ini bekerja sama untuk mencari dan mengeksekusi solusi dari permainan Eight Puzzle.

# 3. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya Gambar 5.9. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal state. Analisa dan bedakan dengan solusi pada point 1 dan 2.
# Code:
    package JavaApplivation2;
    import java.util.Vector;

    class Node {
    int[] state = new int[9];
    int cost;
    Node parent = null;
    Vector<Node> successors = new Vector<Node>();

    Node(int s[], Node parent) {
        this.parent = parent;
        for (int i = 0; i < 9; i++)
            state[i] = s[i];
    }

    public Node() {
    }

    public String toString() {
        String s = "";
        for (int i = 0; i < 9; i++) {
            s = s + state[i] + " ";
        }
        return s;
    }

    public boolean equals(Object node) {
        Node n = (Node) node;
        boolean result = true;
        for (int i = 0; i < 9; i++) {
            if (n.state[i] != state[i])
                result = false;
        }
        return result;
    }

    Vector<Node> getPath(Vector<Node> v) {
        v.insertElementAt(this, 0);
        if (parent != null)
            v = parent.getPath(v);
        return v;
    }

    Vector<Node> getPath() {
        return getPath(new Vector<Node>());
    }
    }

    class EightPuzzleSpace {
    Node getRoot() {
        int initialState[] ={1, 5, 3, 4, 6, 8, 2, 7, 0};
        return new Node(initialState, null);
    }

    Node getGoal() {
        int goalState[] = {7, 6, 5, 8, 0, 4, 1, 2, 3};
        return new Node(goalState, null);
    }

    Vector<Node> getSuccessors(Node parent) {
        Vector<Node> successors = new Vector<Node>();
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                if (parent.state[(r * 3) + c] == 0) {
                    if (r > 0) {
                        successors.add(transformState(r - 1, c, r, c, parent));
                    }
                    if (r < 2) {
                        successors.add(transformState(r + 1, c, r, c, parent));
                    }
                    if (c > 0) {
                        successors.add(transformState(r, c - 1, r, c, parent));
                    }
                    if (c < 2) {
                        successors.add(transformState(r, c + 1, r, c, parent));
                    }
                }
            }
        }
        parent.successors = successors;
        return successors;
    }

    Node transformState(int r0, int c0, int r1, int c1, Node parent) {
        int[] s = parent.state;
        int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
        newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
        newState[(r0 * 3) + c0] = 0;
        return new Node(newState, parent);
    }
    
    public static void main(String args[]) {
        new EightPuzzleSpace().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSearch {
    EightPuzzleSpace space = new EightPuzzleSpace();
    Vector<Node> open = new Vector<Node>();
    Vector<Node> closed = new Vector<Node>();

    int h1Cost(Node node) {
        int cost = 0;
        for (int i = 0; i < node.state.length; i++) {
        if (node.state[i] != i) cost++; }
        return cost;
        }

    int h2Cost(Node node) {
        int cost = 0;
        int state[] = node.state;
        for (int i = 0; i < state.length; i++) {
            int v0 = i, v1 = state[i];
            if (v1 == 0)
                continue;
            int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
            int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
            cost += c;
        }
        return cost;
    }
    /*boleh diubah dengan memakai heuristic h1 atau h2 */
    int hCost(Node node) {
    return h1Cost(node);
    }
    Node getBestNode(Vector nodes) {
        int index = 0, minCost = Integer.MAX_VALUE;
        for (int i = 0; i < nodes.size(); i++) {
            Node node = (Node)nodes.elementAt(i);
            if (node.cost < minCost) {
                minCost = node.cost;
                index = i;
            }
        }
        Node bestNode = (Node)nodes.remove(index);
        return bestNode;
    }

    int getPreviousCost(Node node) {
        int i = open.indexOf(node);
        int cost = Integer.MAX_VALUE;
        if (i != -1) {
            cost = open.get(i).cost;
        } else {
            i = closed.indexOf(node);
            if (i != -1)
                cost = closed.get(i).cost;
        }
        return cost;
    }

    void printPath(Vector path) {
        for (int i = 0; i < path.size(); i++) {
            System.out.print(" " + path.elementAt(i) + "\n");
        }
    }

    void run() {
        Node root = space.getRoot();
        Node goal = space.getGoal();
        Node solution = null;
        open.add(root);
        System.out.print("\nRoot: " + root + "\n\n");
        while (open.size() > 0) {
            Node node = getBestNode(open);
            int pathLength = node.getPath().size();
            closed.add(node);
            if (node.equals(goal)) {
                solution = node;
                break;
            }
            Vector<Node> successors = space.getSuccessors(node);
            for (int i = 0; i < successors.size(); i++) {
                Node successor = successors.get(i);
                int cost = hCost(successor) + pathLength + 1;
                int previousCost;
                previousCost = getPreviousCost(successor);
                boolean inClosed;
                inClosed = closed.contains(successor);
                boolean inOpen = open.contains(successor);
                if (!(inClosed || inOpen) || cost < previousCost) {
                    if (inClosed)
                        closed.remove(successor);
                    if (!inOpen)
                        open.add(successor);
                    successor.cost = cost;
                    successor.parent = node;
                }
            }
        }
        if (solution != null) {
            Vector path = solution.getPath();
            System.out.print("\nSolution found\n");
            printPath(path);
        }
    }

    public static void main(String args[]) {
        new EightPuzzleSearch().run();
    }
    }
![image](https://github.com/rizkyrizaldim/pic/assets/148876602/b9ab896f-fa6b-46be-ba6b-9787fa7e85d8)

Implementasi kode di atas merupakan penggunaan algoritma pencarian A* untuk menyelesaikan masalah 8-puzzle. Dalam algoritma ini, terdapat dua jenis heuristik yang dapat digunakan, yaitu h1Cost dan h2Cost, yang berpengaruh pada jalannya algoritma. Berikut adalah langkah-langkah yang diperlukan untuk mencapai keadaan goal:

1. Dimulai dengan keadaan awal (Root) yang diberikan, yaitu {1, 5, 3, 4, 6, 8, 2, 7, 0}.
2. Tujuan akhir yang ingin dicapai adalah {7, 6, 5, 8, 0, 4, 1, 2, 3}.
3. Algoritma A* dimulai dengan memeriksa semua kemungkinan langkah dari keadaan awal, mempertimbangkan posisi kotak yang kosong, dan menggunakan salah satu dari dua heuristik yang tersedia, yaitu h1Cost atau h2Cost, untuk menghitung biaya.

Algoritma kemudian mencari jalur terbaik menuju keadaan goal dengan mempertimbangkan biaya total (termasuk biaya heuristik, jarak yang sudah ditempuh, dan biaya sebelumnya). Setiap langkah menuju keadaan goal dipilih berdasarkan prioritas biaya terendah, yang ditentukan oleh heuristik yang digunakan. Proses ini terus berlanjut hingga keadaan goal tercapai.

Penting untuk diingat bahwa penggunaan heuristik h1Cost dan h2Cost dapat mempengaruhi perhitungan biaya dan jalannya algoritma. Kedua heuristik ini bisa menghasilkan solusi yang berbeda dalam beberapa kasus. Heuristik h1Cost menghitung biaya dengan cara menghitung jumlah angka yang tidak berada di posisi yang benar, sedangkan h2Cost menghitung biaya berdasarkan jarak Manhatten antara angka yang salah dan posisi yang benar.

# 4. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya Gambar 5.10. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal state. Analisa dan bedakan dengan solusi pada point 1, 2, dan 3.
# Code:
    package JavaApplivation2;
    import java.util.Vector;
  
    class Node {
      int[] state = new int[9];
      int cost;
      Node parent = null;
      Vector<Node> successors = new Vector<Node>();
  
      Node(int s[], Node parent) {
          this.parent = parent;
          for (int i = 0; i < 9; i++)
              state[i] = s[i];
      }
  
      public Node() {
      }
  
      public String toString() {
          String s = "";
          for (int i = 0; i < 9; i++) {
              s = s + state[i] + " ";
          }
          return s;
      }
  
      public boolean equals(Object node) {
          Node n = (Node) node;
          boolean result = true;
          for (int i = 0; i < 9; i++) {
              if (n.state[i] != state[i])
                  result = false;
          }
          return result;
      }
  
      Vector<Node> getPath(Vector<Node> v) {
          v.insertElementAt(this, 0);
          if (parent != null)
              v = parent.getPath(v);
          return v;
      }
  
      Vector<Node> getPath() {
          return getPath(new Vector<Node>());
      }
      public static void main(String args[]) {
          new Node().run();
      }
  
      private void run() {
      }
    }
    
    class EightPuzzleSpace {
      Node getRoot() {
          int initialState[] ={1, 2, 3, 4, 5, 6, 7, 8, 0};
          return new Node(initialState, null);
      }
  
      Node getGoal() {
          int goalState[] = {1, 2, 3, 4, 0, 5, 6, 7, 8};
          return new Node(goalState, null);
      }
  
      Vector<Node> getSuccessors(Node parent) {
          Vector<Node> successors = new Vector<Node>();
          for (int r = 0; r < 3; r++) {
              for (int c = 0; c < 3; c++) {
                  if (parent.state[(r * 3) + c] == 0) {
                      if (r > 0) {
                          successors.add(transformState(r - 1, c, r, c, parent));
                      }
                      if (r < 2) {
                          successors.add(transformState(r + 1, c, r, c, parent));
                      }
                      if (c > 0) {
                          successors.add(transformState(r, c - 1, r, c, parent));
                      }
                      if (c < 2) {
                          successors.add(transformState(r, c + 1, r, c, parent));
                      }
                  }
              }
          }
          parent.successors = successors;
          return successors;
      }
  
      Node transformState(int r0, int c0, int r1, int c1, Node parent) {
          int[] s = parent.state;
          int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
          newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
          newState[(r0 * 3) + c0] = 0;
          return new Node(newState, parent);
      }
      
      public static void main(String args[]) {
          new EightPuzzleSpace().run();
      }
  
      private void run() {
      }
  
  
      
      
    }
  
    class EightPuzzleSearch {
      EightPuzzleSpace space = new EightPuzzleSpace();
      Vector<Node> open = new Vector<Node>();
      Vector<Node> closed = new Vector<Node>();
  
      int h1Cost(Node node) {
          int cost = 0;
          for (int i = 0; i < node.state.length; i++) {
          if (node.state[i] != i) cost++; }
          return cost;
          }
  
      int h2Cost(Node node) {
          int cost = 0;
          int state[] = node.state;
          for (int i = 0; i < state.length; i++) {
              int v0 = i, v1 = state[i];
              if (v1 == 0)
                  continue;
              int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
              int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
              cost += c;
          }
          return cost;
      }
    /*boleh diubah dengan memakai heuristic h1 atau h2 */
      int hCost(Node node) {
      return h1Cost(node);
      }
      Node getBestNode(Vector nodes) {
          int index = 0, minCost = Integer.MAX_VALUE;
          for (int i = 0; i < nodes.size(); i++) {
              Node node = (Node)nodes.elementAt(i);
              if (node.cost < minCost) {
                  minCost = node.cost;
                  index = i;
              }
          }
          Node bestNode = (Node)nodes.remove(index);
          return bestNode;
      }
  
      int getPreviousCost(Node node) {
          int i = open.indexOf(node);
          int cost = Integer.MAX_VALUE;
          if (i != -1) {
              cost = open.get(i).cost;
          } else {
              i = closed.indexOf(node);
              if (i != -1)
                  cost = closed.get(i).cost;
          }
          return cost;
      }
  
      void printPath(Vector path) {
          for (int i = 0; i < path.size(); i++) {
              System.out.print(" " + path.elementAt(i) + "\n");
          }
      }
  
      void run() {
          Node root = space.getRoot();
          Node goal = space.getGoal();
          Node solution = null;
          open.add(root);
          System.out.print("\nRoot: " + root + "\n\n");
          while (open.size() > 0) {
              Node node = getBestNode(open);
              int pathLength = node.getPath().size();
              closed.add(node);
              if (node.equals(goal)) {
                  solution = node;
                  break;
              }
              Vector<Node> successors = space.getSuccessors(node);
              for (int i = 0; i < successors.size(); i++) {
                  Node successor = successors.get(i);
                  int cost = hCost(successor) + pathLength + 1;
                  int previousCost;
                  previousCost = getPreviousCost(successor);
                  boolean inClosed;
                  inClosed = closed.contains(successor);
                  boolean inOpen = open.contains(successor);
                  if (!(inClosed || inOpen) || cost < previousCost) {
                      if (inClosed)
                          closed.remove(successor);
                      if (!inOpen)
                          open.add(successor);
                      successor.cost = cost;
                      successor.parent = node;
                  }
              }
          }
          if (solution != null) {
              Vector path = solution.getPath();
              System.out.print("\nSolution found\n");
              printPath(path);
          }
      }
  
      public static void main(String args[]) {
          new EightPuzzleSearch().run();
      }
    }
![image](https://github.com/rizkyrizaldim/pic/assets/148876602/477c4a54-c22a-4fcd-8941-3cc4bdb21fc3)

Implementasi kode di atas merupakan penggunaan algoritma pencarian A* (A-star) untuk menyelesaikan masalah 8-puzzle. Dalam implementasi ini, terdapat dua heuristik yang dapat digunakan, yaitu h1Cost dan h2Cost, yang mempengaruhi jalannya algoritma. Heuristik ini digunakan untuk menghitung biaya setiap langkah dalam pencarian solusi.

Untuk menentukan langkah-langkah mencapai goal state dalam implementasi "code 3", langkah-langkah yang harus diambil adalah sebagai berikut:

1. Kondisi Awal (Root):
   - {1, 2, 3, 4, 5, 6, 7, 8, 0}

2. Kondisi Goal:
   - {1, 2, 3, 4, 0, 5, 6, 7, 8}

3. Algoritma A* dimulai dengan keadaan awal (Root).
4. Algoritma mencari langkah-langkah untuk mencapai goal state.
5. Algoritma menggunakan heuristik h1Cost atau h2Cost untuk menghitung biaya langkah-langkah.
6. Algoritma memilih langkah-langkah dengan biaya terendah berdasarkan heuristik yang dipilih.
7. Algoritma terus berjalan hingga mencapai goal state.

Dibandingkan dengan solusi pada poin 1, code 1, dan code 2, solusi pada "code 3" juga menggunakan algoritma A* untuk menyelesaikan masalah 8-puzzle. Namun, perbedaan heuristik yang digunakan dapat mempengaruhi urutan langkah-langkah dan waktu yang diperlukan untuk mencapai goal state. Hasil langkah-langkah spesifik yang menghasilkan goal state {1, 2, 3, 4, 0, 5, 6, 7, 8} akan terdapat dalam keluaran dari implementasi "code 3" setelah dijalankan.

# 5. Ubahlah initial dan goal state dari program dan class-class di atas sehingga bentuk initial dan goal statenya Gambar 5.11. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal state.
# Code:
    package JavaApplication2;
    import java.util.Vector;

    class Node {
    char[] state = new char[9];
    int cost;
    Node parent = null;
    Vector<Node> successors = new Vector<Node>();

    Node(char s[], Node parent) {
        this.parent = parent;
        for (int i = 0; i < 9; i++)
            state[i] = s[i];
    }

    public Node() {
    }

    public String toString() {
        String s = "";
        for (int i = 0; i < 9; i++) {
            s = s + state[i] + " ";
        }
        return s;
    }

    public boolean equals(Object node) {
        Node n = (Node) node;
        boolean result = true;
        for (int i = 0; i < 9; i++) {
            if (n.state[i] != state[i])
                result = false;
        }
        return result;
    }

    Vector<Node> getPath(Vector<Node> v) {
        v.insertElementAt(this, 0);
        if (parent != null)
            v = parent.getPath(v);
        return v;
    }

    Vector<Node> getPath() {
        return getPath(new Vector<Node>());
    }

    public static void main(String args[]) {
        new Node().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSpace {
    Node getRoot() {
        char initialState[] = {'D', 'B', 'E', 'A', 'F', 'G', 'H', 'C', 'x'};
        return new Node(initialState, null);
    }

    Node getGoal() {
        char goalState[] = {'A', 'H', 'G', 'B', 'x', 'F', 'C', 'D', 'E'};
        return new Node(goalState, null);
    }

    Vector<Node> getSuccessors(Node parent) {
        Vector<Node> successors = new Vector<Node>();
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                if (parent.state[(r * 3) + c] == 'x') {
                    if (r > 0) {
                        successors.add(transformState(r - 1, c, r, c, parent));
                    }
                    if (r < 2) {
                        successors.add(transformState(r + 1, c, r, c, parent));
                    }
                    if (c > 0) {
                        successors.add(transformState(r, c - 1, r, c, parent));
                    }
                    if (c < 2) {
                        successors.add(transformState(r, c + 1, r, c, parent));
                    }
                }
            }
        }
        parent.successors = successors;
        return successors;
    }

    Node transformState(int r0, int c0, int r1, int c1, Node parent) {
        char[] s = parent.state;
        char[] newState = {'x', 'x', 'x', 'x', 'x', 'x', 'x', 'x', 'x'};
        newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
        newState[(r0 * 3) + c0] = 'x';
        return new Node(newState, parent);
    }

    public static void main(String args[]) {
        new EightPuzzleSpace().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSearch {
    EightPuzzleSpace space = new EightPuzzleSpace();
    Vector<Node> open = new Vector<Node>();
    Vector<Node> closed = new Vector<Node>();

    int h1Cost(Node node) {
        int cost = 0;
        for (int i = 0; i < node.state.length; i++) {
            if (node.state[i] != 'x')
                cost++;
        }
        return cost;
    }

    int h2Cost(Node node) {
        int cost = 0;
        char state[] = node.state;
        for (int i = 0; i < state.length; i++) {
            char v = state[i];
            if (v != 'x') {
                int row0 = i / 3, col0 = i % 3;
                int row1 = i / 3, col1 = i % 3;
                if (v != 'x') {
                    int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
                    cost += c;
                }
            }
        }
        return cost;
    }

    int hCost(Node node) {
        return h1Cost(node);
    }

    Node getBestNode(Vector nodes) {
        int index = 0, minCost = Integer.MAX_VALUE;
        for (int i = 0; i < nodes.size(); i++) {
            Node node = (Node) nodes.elementAt(i);
            if (node.cost < minCost) {
                minCost = node.cost;
                index = i;
            }
        }
        Node bestNode = (Node) nodes.remove(index);
        return bestNode;
    }

    int getPreviousCost(Node node) {
        int i = open.indexOf(node);
        int cost = Integer.MAX_VALUE;
        if (i != -1) {
            cost = open.get(i).cost;
        } else {
            i = closed.indexOf(node);
            if (i != -1)
                cost = closed.get(i).cost;
        }
        return cost;
    }

    void printPath(Vector path) {
        for (int i = 0; i < path.size(); i++) {
            System.out.print(" " + path.elementAt(i) + "\n");
        }
    }

    void run() {
        Node root = space.getRoot();
        Node goal = space.getGoal();
        Node solution = null;
        open.add(root);
        System.out.print("\nRoot: " + root + "\n\n");
        while (open.size() > 0) {
            Node node = getBestNode(open);
            int pathLength = node.getPath().size();
            closed.add(node);
            if (node.equals(goal)) {
                solution = node;
                break;
            }
            Vector<Node> successors = space.getSuccessors(node);
            for (int i = 0; i < successors.size(); i++) {
                Node successor = successors.get(i);
                int cost = hCost(successor) + pathLength + 1;
                int previousCost;
                previousCost = getPreviousCost(successor);
                boolean inClosed;
                inClosed = closed.contains(successor);
                boolean inOpen = open.contains(successor);
                if (!(inClosed || inOpen) || cost < previousCost) {
                    if (inClosed)
                        closed.remove(successor);
                    if (!inOpen)
                        open.add(successor);
                    successor.cost = cost;
                    successor.parent = node;
                }
            }
        }
        if (solution != null) {
            Vector path = solution.getPath();
            System.out.print("\nSolution found\n");
            printPath(path);
        }
    }

    public static void main(String args[]) {
        new EightPuzzleSearch().run();
    }
    }
<img width="305" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/a522c30e-72c4-4d50-a1e4-5753d9690c06">

Untuk menentukan langkah-langkah agar puzzle mencapai goal state dalam Code 4, Anda dapat menggunakan algoritma pencarian seperti A* Search yang memanfaatkan dua heuristik, yaitu h1Cost dan h2Cost, yang telah didefinisikan dalam kode tersebut. Berikut adalah langkah-langkah umum yang dapat diikuti:

Langkah pertama adalah melakukan inisialisasi pada node awal menggunakan kondisi awal dari puzzle. Selanjutnya, siapkan dua daftar, yaitu open list dan closed list. Tempatkan node awal ke dalam open list untuk memulai pencarian.

Selama open list masih memiliki elemen, langkah-langkah berikut akan dilakukan:
1. Pilih node dengan biaya terendah dari open list (biaya f dihitung sebagai jumlah biaya heuristik dan panjang path).
2. Periksa apakah node tersebut merupakan goal state. Jika ya, maka puzzle telah terselesaikan, dan pencarian dapat diakhiri.
3. Hasilkan semua node turunan dari node saat ini.
4. Untuk setiap node turunan, hitung biaya f (dengan menggunakan salah satu dari h1Cost atau h2Cost) dan panjang path dari node awal hingga node saat ini.
5. Jika node turunan sudah ada dalam open list dan biaya f yang baru lebih rendah, perbarui biaya dan parent dari node tersebut.
6. Jika node turunan sudah ada dalam closed list dan biaya f yang baru lebih rendah, perbarui biaya dan parent dari node tersebut. Selanjutnya, pindahkan node dari closed list ke open list.
7. Jika node turunan belum ada dalam open list atau closed list, tambahkan node tersebut ke dalam open list.
8. Tandai node saat ini sebagai telah dieksplorasi dengan memindahkannya dari open list ke dalam closed list.
9. Jika pencarian selesai dan goal state telah ditemukan, kita dapat mengikuti parent nodes dari goal state untuk mendapatkan urutan langkah yang dibutuhkan untuk mencapai goal state.

Langkah-langkah ini akan terus diulang hingga mencapai goal state atau jika seluruh kemungkinan solusi telah dieksplorasi. Dengan demikian, algoritma A* Search dengan dua heuristik h1Cost dan h2Cost dapat digunakan untuk menentukan solusi dari puzzle pada Code 4.
