# Rizky Rizaldi Mastiyanto
# 5311421085
# 1. Tentukan bagaimana algoritma BFS di atas dapat menentukan node ke 8, 6, dan 7.
# Code:

    import java.util.ArrayDeque;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Queue;
    import java.util.Set;

    public class NewMain {
    public enum NodeColour { WHITE, GRAY, BLACK }

    public static class Node {
        int data;
        int distance;
        Node predecessor;
        NodeColour colour;

        public Node(int data) {
            this.data = data;
        }

        public String toString() {
            return "(" + data + ",d=" + distance + ")";
        }
    }

    Map<Node, List<Node>> nodes;

    public NewMain() {
        nodes = new HashMap<Node, List<Node>>();
    }

    public void addEdge(Node n1, Node n2) {
        if (nodes.containsKey(n1)) {
            nodes.get(n1).add(n2);
        } else {
            ArrayList<Node> list = new ArrayList<Node>();
            list.add(n2);
            nodes.put(n1, list);
        }
    }

    public void bfs(Node s) {
        Set<Node> keys = nodes.keySet();
        for (Node u : keys) {
            if (u != s) {
                u.colour = NodeColour.WHITE;
                u.distance = Integer.MAX_VALUE;
                u.predecessor = null;
            }
        }
        s.colour = NodeColour.GRAY;
        s.distance = 0;
        s.predecessor = null;
        Queue<Node> q = new ArrayDeque<Node>();
        q.add(s);
        while (!q.isEmpty()) {
            Node u = q.remove();
            List<Node> adj_u = nodes.get(u);
            if (adj_u != null) {
                for (Node v : adj_u) {
                    if (v.colour == NodeColour.WHITE) {
                        v.colour = NodeColour.GRAY;
                        v.distance = u.distance + 1;
                        v.predecessor = u;
                        q.add(v);
                    }
                }
            }
            u.colour = NodeColour.BLACK;
            System.out.print(u + " ");
        }
    }

    public static void main(String[] args) {
        NewMain graph = new NewMain();
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);
        Node n7 = new Node(7);
        Node n8 = new Node(8);

        graph.addEdge(n1, n2);
        graph.addEdge(n2, n1);
        graph.addEdge(n2, n3);
        graph.addEdge(n3, n4);
        graph.addEdge(n3, n2);
        graph.addEdge(n4, n3);
        graph.addEdge(n4, n5);
        graph.addEdge(n4, n6);
        graph.addEdge(n5, n4);
        graph.addEdge(n5, n6);
        graph.addEdge(n5, n7);
        graph.addEdge(n6, n4);
        graph.addEdge(n6, n5);
        graph.addEdge(n6, n7);
        graph.addEdge(n6, n8);
        graph.addEdge(n7, n5);
        graph.addEdge(n7, n6);
        graph.addEdge(n7, n8);
        graph.addEdge(n8, n6);
        graph.addEdge(n8, n7);
        graph.bfs(n3);
    }
    }

Hasil Output dari Code diatas adalah:

![image](https://github.com/rizkyrizaldim/pic/assets/148876602/7edb6b86-b31e-498d-8b00-d4a4d2348a4f)

Pertama, semua node diinisialisasi dengan status "belum dikunjungi", jarak maksimum, dan tanpa node pendahulu.

Node awal, yaitu `n3`, ditandai sebagai node pertama yang akan dikunjungi dengan status "sedang dikunjungi", jarak 0, dan tanpa node pendahulu.

Node awal kemudian dimasukkan ke dalam antrian.

Selama antrian belum kosong, node pertama diambil dari antrian dan semua tetangganya yang belum dikunjungi diubah statusnya menjadi "sedang dikunjungi", jaraknya diupdate dengan jarak dari node sebelumnya ditambah 1, dan node sebelumnya diatur sebagai pendahulunya. Selanjutnya, mereka dimasukkan ke dalam antrian.

Node yang telah selesai diproses diubah statusnya menjadi "telah dikunjungi" dan dicetak.

Dengan metode ini, kita dapat menentukan bagaimana algoritma menemukan node 8, 6, dan 7 dengan melihat urutan kunjungan:

1. Node 3 adalah titik awal.
2. Node 4 adalah tetangga pertama dari node 3 yang dikunjungi.
3. Node 5 dan 6 adalah tetangga dari node 4. Karena algoritma BFS mengunjungi semua tetangga sebelum melanjutkan ke level berikutnya, keduanya akan dikunjungi sebelum pindah ke node 7 atau 8.
4. Node 7 adalah tetangga dari node 5 dan akan dikunjungi sebelum pindah ke node 8.
5. Akhirnya, node 8 akan dikunjungi sebagai tetangga dari node 6 dan 7.

Jadi, urutan kunjungan akan menjadi: `3 -> 4 -> 5 -> 6 -> 7 -> 8`. Harap diingat bahwa urutan kunjungan antara node pada level yang sama (misalnya antara node 5 dan 6) dapat berbeda tergantung pada implementasi algoritma.

# 2. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.4 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 5.
# Code:
    import java.util.ArrayDeque;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Queue;
    import java.util.Set;

    public class NewMain {
    public enum NodeColour { WHITE, GRAY, BLACK }

    public static class Node {
        int data;
        int distance;
        Node predecessor;
        NodeColour colour;

        public Node(int data) {
            this.data = data;
        }

        public String toString() {
            return "(" + data + ", d=" + distance + ")";
        }
    }

    Map<Node, List<Node>> nodes;

    public NewMain() {
        nodes = new HashMap<Node, List<Node>>();
    }

    public void addEdge(Node n1, Node n2) {
        if (nodes.containsKey(n1)) {
            nodes.get(n1).add(n2);
        } else {
            ArrayList<Node> list = new ArrayList<Node>();
            list.add(n2);
            nodes.put(n1, list);
        }
    }

    public void bfs(Node s) {
        Set<Node> keys = nodes.keySet();
        for (Node u : keys) {
            if (u != s) {
                u.colour = NodeColour.WHITE;
                u.distance = Integer.MAX_VALUE;
                u.predecessor = null;
            }
        }
        s.colour = NodeColour.GRAY;
        s.distance = 0;
        s.predecessor = null;
        Queue<Node> q = new ArrayDeque<Node>();
        q.add(s);
        while (!q.isEmpty()) {
            Node u = q.poll(); //q.poll()
            List<Node> adj_u = nodes.get(u);
            if (adj_u != null) {
                for (Node v : adj_u) {
                    if (v.colour == NodeColour.WHITE) {
                        v.colour = NodeColour.GRAY;
                        v.distance = u.distance + 1;
                        v.predecessor = u;
                        q.add(v);
                    }
                }
            }
            u.colour = NodeColour.BLACK;
            System.out.print(u + " ");
        }
    }

    public static void main(String[] args) {
        NewMain graph = new NewMain(); // "NewMain"
        Node n0 = new Node(0);
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);

        graph.addEdge(n0, n1);
        graph.addEdge(n0, n2);

        graph.addEdge(n1, n0);
        graph.addEdge(n1, n3);
        graph.addEdge(n1, n4);

        graph.addEdge(n2, n0);
        graph.addEdge(n2, n5);
        graph.addEdge(n2, n6);

        graph.addEdge(n3, n1);
        graph.addEdge(n3, n4);

        graph.addEdge(n4, n3);
        graph.addEdge(n4, n1);

        graph.addEdge(n5, n2);
        graph.addEdge(n5, n6);
        graph.addEdge(n5, n1);

        graph.addEdge(n6, n2);
        graph.addEdge(n6, n5);
        graph.addEdge(n6, n1);

        graph.bfs(n0);
    }
    }

Hasil Output dari Code diatas adalah:

![image](https://github.com/rizkyrizaldim/pic/assets/148876602/65f4466d-fa9c-4b5d-8fbe-d1b53eb1c3d2)


Algoritma Breadth-First Search (BFS) berfungsi dengan langkah-langkah berikut:

Langkah pertama adalah menginisialisasi semua simpul dalam graf dengan atribut warna putih, jarak maksimum, dan tanpa pendahulu. Kemudian, simpul awal, yang dalam contoh ini disebut `n0`, diberi tanda abu-abu, dengan jarak awal 0, dan tidak memiliki simpul pendahulu. Simpul awal ini kemudian dimasukkan ke dalam antrian.

Selama antrian belum kosong, simpul pertama diambil dari antrian. Selanjutnya, semua tetangga dari simpul tersebut yang masih berwarna putih akan diubah menjadi abu-abu. Jarak mereka diperbarui sejauh satu langkah dari simpul sebelumnya, dan simpul sebelumnya ditetapkan sebagai pendahulu. Setelah itu, tetangga-tetangga ini kembali dimasukkan ke dalam antrian untuk dianalisis.

Simpul yang telah selesai diproses akan diubah statusnya menjadi hitam dan dicetak.

Dengan metode ini, kita dapat menentukan bagaimana algoritma menemukan node 5 dengan memperhatikan urutan kunjungan:

Langkah pertama adalah memulai dari simpul 0. Kemudian, simpul 1 dan 2, yang merupakan tetangga pertama dari simpul 0, akan dikunjungi. Karena BFS melakukan penelusuran terhadap seluruh tetangga sebelum melanjutkan ke level berikutnya, maka simpul 1 dan 2 akan dikunjungi sebelum berpindah ke simpul-simpul lainnya. Akhirnya, simpul 5, yang merupakan tetangga dari simpul 2, akan dikunjungi setelah semua tetangga dari simpul 1 dan 2 telah dijelajahi.

Oleh karena itu, urutan kunjungan akan menjadi: `0 -> 1 -> 2 -> 3 -> 4 -> 5`. Perlu diingat bahwa urutan kunjungan antara simpul-simpul pada level yang sama, seperti antara simpul 3 dan 4, dapat berubah tergantung pada cara implementasi dari algoritma.

# 3. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.5 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 9.
# Code:
    import java.util.ArrayDeque;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Queue;
    import java.util.Set;

    public class NewMain {
    public enum NodeColour { WHITE, GRAY, BLACK }

    public static class Node {
        int data;
        int distance;
        Node predecessor;
        NodeColour colour;

        public Node(int data) {
            this.data = data;
        }

        public String toString() {
            return "(" + data + ", d=" + distance + ")";
        }
    }

    Map<Node, List<Node>> nodes;

    public NewMain() {
        nodes = new HashMap<Node, List<Node>>();
    }

    public void addEdge(Node n1, Node n2) {
        if (nodes.containsKey(n1)) {
            nodes.get(n1).add(n2);
        } else {
            ArrayList<Node> list = new ArrayList<Node>();
            list.add(n2);
            nodes.put(n1, list);
        }
    }

    public void bfs(Node s) {
        Set<Node> keys = nodes.keySet();
        for (Node u : keys) {
            if (u != s) {
                u.colour = NodeColour.WHITE;
                u.distance = Integer.MAX_VALUE;
                u.predecessor = null;
            }
        }
        s.colour = NodeColour.GRAY;
        s.distance = 0;
        s.predecessor = null;
        Queue<Node> q = new ArrayDeque<Node>();
        q.add(s);
        while (!q.isEmpty()) {
            Node u = q.poll();
            List<Node> adj_u = nodes.get(u);
            if (adj_u != null) {
                for (Node v : adj_u) {
                    if (v.colour == NodeColour.WHITE) {
                        v.colour = NodeColour.GRAY;
                        v.distance = u.distance + 1;
                        v.predecessor = u;
                        q.add(v);
                    }
                }
            }
            u.colour = NodeColour.BLACK;
            System.out.print(u + " ");
        }
    }

    public static void main(String[] args) {
        NewMain graph = new NewMain(); //NewMain
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);
        Node n7 = new Node(7);
        Node n8 = new Node(8);
        Node n9 = new Node(9);
        Node n10 = new Node(10);
        Node n11 = new Node(11);
        Node n12 = new Node(12);

        // Tambahkan edge sesuai dengan yang diinginkan
        graph.addEdge(n1, n2);
        graph.addEdge(n1, n3);
        graph.addEdge(n1, n4);

        graph.addEdge(n2, n1);
        graph.addEdge(n2, n5);
        graph.addEdge(n2, n6);

        graph.addEdge(n3, n1);

        graph.addEdge(n4, n1);
        graph.addEdge(n4, n7);
        graph.addEdge(n4, n8);

        graph.addEdge(n5, n2);
        graph.addEdge(n5, n9);
        graph.addEdge(n5, n10);
        graph.addEdge(n5, n6);

        graph.addEdge(n6, n2);
        graph.addEdge(n6, n5);

        graph.addEdge(n7, n4);
        graph.addEdge(n7, n11);
        graph.addEdge(n7, n12);
        graph.addEdge(n7, n8);

        graph.addEdge(n8, n4);
        graph.addEdge(n8, n7);

        graph.addEdge(n9, n5);
        graph.addEdge(n9, n10);

        graph.addEdge(n10, n5);
        graph.addEdge(n10, n9);

        graph.addEdge(n11, n7);
        graph.addEdge(n11, n12);

        graph.addEdge(n12, n7);
        graph.addEdge(n12, n11);

        graph.bfs(n1);
    }
    }

Hasil Output dari Code diatas adalah:

![image](https://github.com/rizkyrizaldim/pic/assets/148876602/5e0cb494-6681-4991-bc10-4f3118bf2340)


Algoritma Breadth-First Search (BFS) beroperasi dengan langkah-langkah berikut:

Pertama-tama, seluruh simpul dalam graf diinisialisasi dengan atribut warna putih, jarak maksimum, dan tanpa pendahulu. Node awal, yang dalam contoh ini adalah `n1`, diberi atribut warna abu-abu, jarak 0, dan tidak memiliki pendahulu. Node awal kemudian dimasukkan ke dalam antrian.

Selama antrian belum kosong, simpul pertama diambil dari antrian. Seluruh tetangga yang masih berwarna putih diubah statusnya menjadi abu-abu, dengan jarak yang diperbarui sejauh satu langkah dari simpul sebelumnya, dan simpul sebelumnya ditetapkan sebagai pendahulunya. Selanjutnya, mereka dimasukkan kembali ke dalam antrian. Setelah proses ini, simpul yang telah selesai diproses diubah statusnya menjadi hitam dan dicetak.

Dengan menggunakan metode ini, kita dapat menentukan bagaimana algoritma menemukan simpul 9 dengan memperhatikan urutan kunjungan:

Node 1 adalah titik awal dari proses. Node 2, 3, dan 4 adalah tetangga pertama dari node 1 yang dikunjungi. Karena BFS mengunjungi seluruh tetangga sebelum beralih ke tingkat berikutnya, node 2, 3, dan 4 akan dikunjungi sebelum memindahkan fokus ke simpul lainnya. Node 5 adalah tetangga dari node 2 dan akan dikunjungi setelah semua tetangga dari node 1 telah dikunjungi. Akhirnya, simpul 9 adalah tetangga dari simpul 5 dan akan dikunjungi setelah seluruh tetangga dari node 2 telah dikunjungi.

Oleh karena itu, urutan kunjungan adalah: `1 -> 2 -> 3 -> 4 -> 5 -> 9`. Penting untuk dicatat bahwa urutan kunjungan di antara simpul yang berada pada tingkat yang sama (seperti antara simpul 3 dan 4) dapat bervariasi tergantung pada implementasi algoritma.

# 4. Ubahlah kode program di atas sehingga bentuk tree seperti Gambar 6 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node C.
# Code:
    import java.util.ArrayDeque;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Queue;
    import java.util.Set;

    public class NewMain {
    public enum NodeColour { WHITE, GRAY, BLACK }

    public static class Node {
        char data;
        int distance;
        Node predecessor;
        NodeColour colour;

        public Node(char data) {
            this.data = data;
        }

        public String toString() {
            return "(" + data + ", d=" + distance + ")";
        }
    }

    Map<Node, List<Node>> nodes;

    public NewMain() {
        nodes = new HashMap<Node, List<Node>>();
    }

    public void addEdge(Node n1, Node n2) {
        if (nodes.containsKey(n1)) {
            nodes.get(n1).add(n2);
        } else {
            ArrayList<Node> list = new ArrayList<Node>();
            list.add(n2);
            nodes.put(n1, list);
        }
    }

    public void bfs(Node s) {
        Set<Node> keys = nodes.keySet();
        for (Node u : keys) {
            if (u != s) {
                u.colour = NodeColour.WHITE;
                u.distance = Integer.MAX_VALUE;
                u.predecessor = null;
            }
        }
        s.colour = NodeColour.GRAY;
        s.distance = 0;
        s.predecessor = null;
        Queue<Node> q = new ArrayDeque<Node>();
        q.add(s);
        while (!q.isEmpty()) {
            Node u = q.poll();
            List<Node> adj_u = nodes.get(u);
            if (adj_u != null) {
                for (Node v : adj_u) {
                    if (v.colour == NodeColour.WHITE) {
                        v.colour = NodeColour.GRAY;
                        v.distance = u.distance + 1;
                        v.predecessor = u;
                        q.add(v);
                    }
                }
            }
            u.colour = NodeColour.BLACK;
            System.out.print(u + " ");
        }
    }

    public static void main(String[] args) {
        NewMain graph = new NewMain();
        Node n1 = new Node('A');
        Node n2 = new Node('B');
        Node n3 = new Node('C');
        Node n4 = new Node('D');
        Node n5 = new Node('E');
        Node n6 = new Node('F');
        Node n7 = new Node('G');
        Node n8 = new Node('H');
        Node n9 = new Node('I');

        graph.addEdge(n1, n2);
        graph.addEdge(n1, n4);

        graph.addEdge(n2, n6);
        graph.addEdge(n2, n1);
        graph.addEdge(n2, n4);

        graph.addEdge(n3, n4);
        graph.addEdge(n3, n5);

        graph.addEdge(n4, n2);
        graph.addEdge(n4, n3);
        graph.addEdge(n4, n5);

        graph.addEdge(n5, n4);
        graph.addEdge(n5, n3);

        graph.addEdge(n6, n2);
        graph.addEdge(n6, n7);
        
        graph.addEdge(n7, n6);
        graph.addEdge(n7, n9);

        graph.addEdge(n8, n9);
        
        graph.addEdge(n9, n8);

        graph.bfs(n6);
    }
    }

Hasil Output dari Code diatas adalah:

![image](https://github.com/rizkyrizaldim/pic/assets/148876602/682321a0-38e0-4d73-a34c-7eca74c5994c)


Berikut adalah langkah-langkah untuk menjelaskan bagaimana algoritma BFS menemukan node 'C' dari kode program:

Pertama-tama, algoritma dimulai dari node 'F', yang ditetapkan sebagai titik awal dengan perintah `graph.bfs(n6);` di mana n6 mewakili node 'F'. Node 'F' ditambahkan ke dalam antrian dan ditandai sebagai sedang dalam proses kunjungan.

Setelah itu, algoritma mengunjungi semua tetangga dari node 'F', yaitu node 'B', 'G', dan 'C'. Masing-masing dari mereka ditambahkan ke antrian dan ditandai sebagai sedang dalam proses kunjungan.

Setelah semua tetangga dari node 'F' telah dikunjungi, node 'F' sendiri ditandai sebagai sudah selesai dikunjungi (berwarna hitam). Algoritma melanjutkan dengan memeriksa node berikutnya dalam antrian, yaitu node 'B'.

Node 'B' kemudian dijelajahi, dan jika ada tetangga yang belum dikunjungi, mereka akan ditambahkan ke antrian dan ditandai sebagai sedang dalam proses kunjungan. Setelah semua tetangga dari 'B' telah dijelajahi, 'B' sendiri ditandai sebagai sudah selesai dikunjungi.

Proses ini berlanjut sampai algoritma mencapai node 'C'. Ketika node 'C' dijelajahi, algoritma menyadari bahwa itulah node yang dicari. Pencarian dihentikan karena node 'C' telah ditemukan.

Dengan demikian, algoritma BFS berhasil menemukan node 'C' dengan mengikuti langkah-langkah di atas.
