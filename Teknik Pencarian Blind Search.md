# Rizky Rizaldi Mastiyanto - 5311421085
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

<img width="637" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/1c7570de-2dc5-4cde-bbc5-06044c2d60bd">

Algoritma BFS (Breadth-First Search) bekerja dengan cara berikut:

1. Pertama, semua node diatur ke warna putih, jarak maksimum, dan tanpa pendahulu.
2. Node awal (dalam hal ini `n3`) diatur ke warna abu-abu, jarak 0, dan tanpa pendahulu.
3. Node awal ditambahkan ke antrian.
4. Selama antrian tidak kosong, node pertama dihapus dari antrian dan semua tetangganya yang berwarna putih diatur ke warna abu-abu, jaraknya ditambah 1 dari node sebelumnya, dan pendahulunya diatur ke node sebelumnya. Kemudian mereka ditambahkan ke antrian.
5. Node yang dikeluarkan dari antrian diatur ke warna hitam dan dicetak.

Dengan demikian, urutan pencetakan node adalah urutan kunjungan BFS.

Untuk menentukan bagaimana algoritma menemukan node 8, 6, dan 7, kita perlu melihat urutan kunjungan:

1. Node 3 adalah titik awal.
2. Node 4 adalah tetangga pertama dari node 3 yang dikunjungi.
3. Node 5 dan 6 adalah tetangga dari node 4. Karena BFS mengunjungi semua tetangga sebelum pindah ke level berikutnya, keduanya dikunjungi sebelum pindah ke node 7 atau 8.
4. Node 7 adalah tetangga dari node 5 dan dikunjungi sebelum pindah ke node 8.
5. Akhirnya, node 8 dikunjungi sebagai tetangga dari node 6 dan 7.

Jadi urutan kunjungan adalah: `3 -> 4 -> 5 -> 6 -> 7 -> 8`. Harap diperhatikan bahwa urutan kunjungan antara node pada level yang sama (misalnya antara node 5 dan 6) dapat bervariasi tergantung pada implementasi.

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

<img width="636" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/c197a76e-0979-4a07-b2f5-58218b3ebe06">

Algoritma BFS (Breadth-First Search) bekerja dengan cara berikut:

1. Pertama, semua node diatur ke warna putih, jarak maksimum, dan tanpa pendahulu.
2. Node awal (dalam hal ini `n0`) diatur ke warna abu-abu, jarak 0, dan tanpa pendahulu.
3. Node awal ditambahkan ke antrian.
4. Selama antrian tidak kosong, node pertama dihapus dari antrian dan semua tetangganya yang berwarna putih diatur ke warna abu-abu, jaraknya ditambah 1 dari node sebelumnya, dan pendahulunya diatur ke node sebelumnya. Kemudian mereka ditambahkan ke antrian.
5. Node yang dikeluarkan dari antrian diatur ke warna hitam dan dicetak.

Dengan demikian, urutan pencetakan node adalah urutan kunjungan BFS.

Untuk menentukan bagaimana algoritma menemukan node 5, kita perlu melihat urutan kunjungan:

1. Node 0 adalah titik awal.
2. Node 1 dan 2 adalah tetangga pertama dari node 0 yang dikunjungi.
3. Karena BFS mengunjungi semua tetangga sebelum pindah ke level berikutnya, node 1 dan 2 dikunjungi sebelum pindah ke node lainnya.
4. Node 5 adalah tetangga dari node 2 dan dikunjungi setelah semua tetangga dari node 1 dan 2 telah dikunjungi.

Jadi urutan kunjungan adalah: `0 -> 1 -> 2 -> 3 -> 4 -> 5`. Harap diperhatikan bahwa urutan kunjungan antara node pada level yang sama (misalnya antara node 3 dan 4) dapat bervariasi tergantung pada implementasi.

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

<img width="749" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/d032e5f4-4d85-45a5-96eb-bc42166e641d">

Algoritma BFS (Breadth-First Search) bekerja dengan cara berikut:

1. Pertama, semua node diatur ke warna putih, jarak maksimum, dan tanpa pendahulu.
2. Node awal (dalam hal ini `n1`) diatur ke warna abu-abu, jarak 0, dan tanpa pendahulu.
3. Node awal ditambahkan ke antrian.
4. Selama antrian tidak kosong, node pertama dihapus dari antrian dan semua tetangganya yang berwarna putih diatur ke warna abu-abu, jaraknya ditambah 1 dari node sebelumnya, dan pendahulunya diatur ke node sebelumnya. Kemudian mereka ditambahkan ke antrian.
5. Node yang dikeluarkan dari antrian diatur ke warna hitam dan dicetak.

Dengan demikian, urutan pencetakan node adalah urutan kunjungan BFS.

Untuk menentukan bagaimana algoritma menemukan node 9, kita perlu melihat urutan kunjungan:

1. Node 1 adalah titik awal.
2. Node 2, 3, dan 4 adalah tetangga pertama dari node 1 yang dikunjungi.
3. Karena BFS mengunjungi semua tetangga sebelum pindah ke level berikutnya, node 2, 3, dan 4 dikunjungi sebelum pindah ke node lainnya.
4. Node 5 adalah tetangga dari node 2 dan dikunjungi setelah semua tetangga dari node 1 telah dikunjungi.
5. Akhirnya, node 9 adalah tetangga dari node 5 dan dikunjungi setelah semua tetangga dari node 2 telah dikunjungi.

Jadi urutan kunjungan adalah: `1 -> 2 -> 3 -> 4 -> 5 -> 9`. Harap diperhatikan bahwa urutan kunjungan antara node pada level yang sama (misalnya antara node 3 dan 4) dapat bervariasi tergantung pada implementasi.

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

<img width="730" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/0d95bbac-60d0-4561-8688-962c3b9bc2d5">

Berikut adalah langkah-langkah bagaimana algoritma BFS menemukan node C dari kode:

1. Mulai dari node 'F' (karena `graph.bfs(n6);` di mana n6 adalah node 'F').
2. Tambahkan node 'F' ke dalam antrian dan tandai sebagai abu-abu (sedang dikunjungi).
3. Kunjungi semua tetangga dari node 'F', yaitu node 'B', 'G', dan 'C', tambahkan ke antrian dan tandai sebagai abu-abu.
4. Node 'F' sekarang ditandai sebagai hitam (sudah dikunjungi), dan kita lanjut ke node berikutnya dalam antrian, yaitu node 'B'.
5. Kunjungi semua tetangga dari node 'B' yang belum dikunjungi, yaitu node 'A'. Tambahkan ke antrian dan tandai sebagai abu-abu.
6. Node 'B' sekarang ditandai sebagai hitam, dan kita lanjut ke node berikutnya dalam antrian, yaitu node 'G'.
7. Kunjungi semua tetangga dari node 'G' yang belum dikunjungi, tidak ada dalam hal ini.
8. Node 'G' sekarang ditandai sebagai hitam, dan kita lanjut ke node berikutnya dalam antrian, yaitu node 'C'.
9. Kita telah menemukan node 'C'. Algoritma BFS akan berhenti jika kita mencari node tertentu dan telah menemukannya.

Dengan demikian, algoritma BFS dapat menemukan node 'C' dari kode program dengan cara di atas.
