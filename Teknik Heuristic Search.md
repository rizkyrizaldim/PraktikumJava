# Rizky Rizaldi Mastiyanto - 5311421085
# 1. Pelajari class EightPuzzleSearch, EightPuzzleSpace, dan Node.
- EightPuzzleSearch: Kelas yang digunakan dalam konteks permainan teka-teki delapan kotak untuk mengimplementasikan algoritma pencarian solusi.
- EightPuzzleSpace: Kelas yang merepresentasikan ruang keadaan dalam permainan teka-teki delapan kotak, mencakup informasi tentang keadaan, langkah-langkah, dan aturan permainan.
- Node: Objek yang digunakan dalam algoritma pencarian, mewakili satu keadaan dalam ruang pencarian dengan informasi tentang keadaan, biaya, atau jarak.

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
<img width="390" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/9afe1897-97df-41a1-b182-2aa76aafa073">

Dalam kode Eight Puzzle yang telah diberikan, langkah-langkah untuk mencapai goal state dari initial state ditemukan di dalam class `EightPuzzleSearch`. Class `EightPuzzleSearch` mencari solusi menggunakan algoritma pencarian A* dengan pilihan penggunaan heuristic `h1` atau `h2`. Berikut adalah langkah-langkah untuk mencapai goal state:

1. Inisialisasi initial state dan goal state.
2. Membuat node awal (root) dengan initial state.
3. Memasukkan node awal ke dalam daftar `open`.
4. Selama daftar `open` tidak kosong, lakukan langkah-langkah berikut:
   a. Pilih node dengan biaya terkecil (biaya yang dihitung berdasarkan heuristic dan panjang path).
   b. Tambahkan node terpilih ke dalam daftar `closed` untuk menghindari pengulangan.
   c. Jika node terpilih sama dengan goal state, maka solusi ditemukan, dan proses berakhir.
   d. Dapatkan semua node penerus dari node terpilih.
   e. Untuk setiap node penerus, hitung biaya baru berdasarkan heuristic, path length, dan biaya sebelumnya.
   f. Jika node penerus belum pernah ada di daftar `open` atau biayanya lebih rendah, tambahkan node penerus ke daftar `open` dengan biaya yang diupdate.

Langkah-langkah ini berulang hingga solusi ditemukan atau semua kemungkinan solusi dieksplorasi. Hasil akhir akan mencetak langkah-langkah solusi pada output.

Perbedaan antara class `EightPuzzleSearch`, `EightPuzzleSpace`, dan `Node` adalah sebagai berikut:
- `EightPuzzleSearch`: Bertanggung jawab untuk menjalankan algoritma pencarian A* dan mengatur logika pencarian.
- `EightPuzzleSpace`: Menyediakan informasi tentang initial state dan goal state, serta menghasilkan node-node awal.
- `Node`: Mewakili setiap keadaan dalam permainan Eight Puzzle dan menyimpan informasi tentang keadaan, biaya, dan node induknya.

Semua tiga class ini bekerja bersama-sama untuk mencari dan mengeksekusi solusi Eight Puzzle.

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
<img width="357" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/20fe61fe-d711-4bc4-bff0-84f7fc3067cb">

Code di atas merupakan implementasi algoritma pencarian A* (A-star) untuk menyelesaikan masalah 8-puzzle. Dalam algoritma ini, terdapat dua heuristik yang dapat digunakan, yaitu h1Cost dan h2Cost, yang dapat memengaruhi jalannya algoritma. Berikut adalah langkah-langkah yang diperlukan untuk mencapai goal state dalam implementasi kode tersebut:
1. Kondisi Awal (Root):
    - {1, 5, 3, 4, 6, 8, 2, 7, 0}
2. Kondisi Goal:
    - {7, 6, 5, 8, 0, 4, 1, 2, 3}
3. Jalannya Algoritma:
    - Algoritma A* dimulai dengan kondisi awal (Root).
    - Algoritma memeriksa semua langkah yang mungkin dari kondisi awal keadaan, mempertimbangkan kotak yang kosong, dan menggunakan salah satu heuristik (h1Cost atau h2Cost) untuk menghitung biaya.
- Algoritma mencari jalur yang paling efisien ke goal state dengan mempertimbangkan biaya total (biaya heuristik, jarak sejauh ini, dan biaya sebelumnya).
- Setiap langkah diambil untuk mencapai goal state dipilih berdasarkan prioritas biaya terendah, yang ditentukan oleh heuristik yang digunakan.
- Algoritma terus berjalan hingga mencapai goal state.

Perbedaan antara penggunaan h1Cost dan h2Cost akan memengaruhi perhitungan biaya dan jalannya algoritma. Kedua heuristik ini mungkin menghasilkan solusi yang berbeda dalam beberapa kasus. Heuristik h1Cost menghitung biaya dengan menghitung jumlah angka yang tidak berada di tempat yang benar, sementara h2Cost menghitung biaya berdasarkan jarak Manhatten antara angka-angka yang salah dan posisi yang benar.

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
<img width="338" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/bc999852-3349-4cbd-a175-b185c303ab85">

Code di atas adalah implementasi algoritma pencarian A* (A-star) untuk menyelesaikan masalah 8-puzzle. Dalam implementasi ini, ada dua heuristik yang dapat digunakan: h1Cost dan h2Cost, yang memengaruhi jalannya algoritma. Heuristik ini digunakan untuk menghitung biaya setiap langkah dalam pencarian solusi. Anda diminta untuk menentukan langkah-langkah yang harus diambil untuk mencapai goal state dan membandingkannya dengan solusi pada poin 1, code 1, dan code 2.

Langkah-langkah untuk menentukan solusi 8-puzzle dengan menggunakan implementasi di "code 3" adalah sebagai berikut:

1. Kondisi Awal (Root):
   - {1, 2, 3, 4, 5, 6, 7, 8, 0}

2. Kondisi Goal:
   - {1, 2, 3, 4, 0, 5, 6, 7, 8}

3. Jalannya Algoritma:
   - Algoritma A* dimulai dengan kondisi awal (Root).
   - Algoritma mencari langkah-langkah untuk mencapai goal state.
   - Algoritma menggunakan heuristik h1Cost atau h2Cost untuk menghitung biaya langkah-langkah.
   - Algoritma memilih langkah-langkah yang memiliki biaya terendah berdasarkan heuristik yang dipilih.
   - Algoritma terus berjalan hingga mencapai goal state.

Perbandingan dengan solusi pada poin 1, code 1, dan code 2:
- Solusi pada poin 1, code 1, dan code 2 juga menggunakan algoritma A* untuk menyelesaikan masalah 8-puzzle, tetapi mungkin menggunakan heuristik yang berbeda.
- Perbedaan heuristik yang digunakan dapat memengaruhi urutan langkah-langkah dan waktu yang dibutuhkan untuk mencapai goal state.

Solusi pada "code 3" di atas akan memberikan langkah-langkah spesifik yang menghasilkan goal state {1, 2, 3, 4, 0, 5, 6, 7, 8} berdasarkan heuristik yang dipilih (h1Cost atau h2Cost). Langkah-langkah tersebut dapat ditemukan dalam keluaran dari implementasi "code 3" setelah dijalankan.

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

Untuk menentukan langkah-langkah agar puzzle mencapai goal state pada Code 4, Anda dapat menggunakan algoritma pencarian seperti A* Search dengan menggunakan dua heuristik, yaitu h1Cost dan h2Cost yang telah didefinisikan dalam kode. Langkah-langkah umumnya adalah sebagai berikut:

1. Inisialisasi node awal dengan kondisi awal puzzle.
2. Inisialisasi open list dan closed list. Letakkan node awal ke dalam open list.
3. Selama open list tidak kosong, lakukan langkah-langkah berikut:
   a. Ambil node dengan biaya terendah dari open list (node dengan biaya f = biaya heuristik + panjang path).
   b. Jika node tersebut adalah goal state, maka puzzle telah terselesaikan. Akhiri pencarian.
   c. Generate semua node turunan dari node saat ini.
   d. Untuk setiap node turunan, hitung biaya f (dengan salah satu dari h1Cost atau h2Cost) dan panjang path dari node awal hingga node saat ini.
   e. Jika node turunan sudah ada dalam open list dan biaya f yang baru lebih rendah, perbarui biaya dan parent dari node tersebut.
   f. Jika node turunan sudah ada dalam closed list dan biaya f yang baru lebih rendah, perbarui biaya dan parent dari node tersebut, lalu pindahkan node dari closed list ke open list.
   g. Jika node turunan belum ada dalam open list atau closed list, tambahkan node tersebut ke open list.
   h. Tandai node saat ini sebagai telah dieksplorasi dengan memindahkannya ke dalam closed list.

4. Jika pencarian selesai dan goal state telah ditemukan, kita dapat mengikuti parent nodes dari goal state untuk mendapatkan urutan langkah yang dibutuhkan untuk mencapai goal state.
