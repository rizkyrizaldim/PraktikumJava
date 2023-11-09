# Rizky Rizaldi Mastiyanto (5311421085)
# Praktek Best First Search
# Permainan Tic-Tac-Toe 3x3
Tujuan Praktikum untuk meningkatkan pemahaman mahasiswa terhadap code permainan tic tac toe. Selain itu, modul 6
memberikan pengetahuan tentang Object Oriented Programming menggunakan bahasa pemrograman
Java terutama Java Swing dan Japplet.

Berikut adalah penjelasan dari coding

1. Import Package

       import java.awt.*; 
       import java.awt.event.*;
       import javax.swing.*;
   
   Baris ini mengimpor paket-paket yang diperlukan dari Java untuk membuat antarmuka grafis. java.awt dan javax.swing menyediakan kelas kelas yang diperlukan untuk membuat jendela, tombol, panel, dan elemen-elemen antarmuka pengguna lainnya.

2. Deklarasi Kelas "TicTacToeGUI":

        public class TicTacToeGUI {

    Kelas ini digunakan untuk membuat antarmuka pengguna grafis (GUI) permainan Tic-Tac-Toe.

3. Atribut-Atribut Kelas:

       private JFrame frame;
       private JButton[][] buttons;
       private char currentPlayer;
       private JLabel statusLabel;

   - private JFrame frame;: Ini adalah deklarasi dari variabel frame yang merupakan objek dari kelas JFrame. JFrame adalah jendela utama dari aplikasi GUI dalam Java Swing. Jendela ini menyediakan kerangka kerja untuk menampung komponen-komponen GUI lainnya.
   - private JButton[][] buttons;: Ini adalah deklarasi dari variabel buttons yang merupakan sebuah array dari tombol (JButton). Dalam konteks permainan papan, setiap tombol mungkin akan mewakili satu sel di papan permainan.
   - private char currentPlayer;: Variabel currentPlayer digunakan untuk melacak pemain yang saat ini sedang bermain. Dalam banyak permainan papan, seperti Tic-Tac-Toe, hanya ada dua pemain (biasanya 'X' dan 'O').
   - private JLabel statusLabel;: Ini adalah deklarasi dari variabel statusLabel yang merupakan objek dari kelas JLabel. JLabel digunakan untuk menampilkan teks atau informasi statis di antarmuka pengguna.

4. Konstruktor Kelas "TicTacToeGUI":

       public TicTacToeGUI() {

    Deklarasi dari konstruktor untuk kelas TicTacToeGUI.

5. Mengatur Layout Frame:

       frame.setLayout(new GridLayout(4, 3));

    Mengatur tata letak (layout) dari frame menggunakan GridLayout. GridLayout adalah salah satu jenis tata letak yang disediakan oleh Java Swing. Tata letak ini mengatur komponen-komponen GUI ke dalam kotak dengan ukuran yang sama. Dalam hal ini, grid layout memiliki 4 baris dan 3 kolom.

6. Membuat Tombol-Tombol:

        for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
        buttons[i][j] = new JButton("");
        buttons[i][j].setFont(new Font("Arial", Font.PLAIN, 48));
        buttons[i][j].setBackground(Color.WHITE);
        buttons[i][j].setFocusPainted(false);
        buttons[i][j].addActionListener(new ButtonListener(i, j));
        frame.add(buttons[i][j]);
        }
        }

    Baris kode di atas adalah loop bersarang yang digunakan untuk membuat dan mengonfigurasi tombol-tombol dalam permainan Tic-Tac-Toe. Kode ini secara efektif membuat grid 3x3 dari tombol-tombol yang siap untuk digunakan dalam permainan Tic-Tac-Toe. Setiap tombol telah dikonfigurasi dengan propertis dan perilaku tertentu yang akan mempengaruhi tampilan dan interaksi dengan pengguna.

7. Membuat Tombol Reset:

        JButton resetButton = new JButton("Reset");
        resetButton.setFont(new Font("Arial", Font.PLAIN, 18));
        resetButton.addActionListener(new ResetListener());
        frame.add(statusLabel);
        frame.add(resetButton);

    Baris kode di atas menambahkan sebuah tombol reset ke antarmuka pengguna permainan Tic-Tac-Toe. 

8. Listener untuk Tombol Sel ('ButtonListener'):

        private class ButtonListener implements ActionListener {
        // ...
        }

    ButtonListener adalah kelas yang bertugas menangani klik tombol di papan permainan. Ketika seorang pemain mengklik tombol, fungsi ini akan menempatkan tanda (X atau O) di tombol tersebut, lalu memeriksa apakah ada pemenang atau tidak.

9. Listener untuk Tombol Reset ('ResetListener'):

        private class ResetListener implements ActionListener {
        // ...
        }

    
   ResetListener adalah kelas yang berfungsi untuk menangani klik tombol "Reset". Ketika tombol ini dipencet, permainan akan dikembalikan ke kondisi awal.

10. Metode "checkForWin":

        private boolean checkForWin() {
        // ...
        }

      
    Fungsi ini digunakan untuk mengevaluasi apakah terdapat pemenang setelah setiap langkah. Ini mengecek baris, kolom, dan diagonal untuk menentukan apakah ada tiga tanda yang serupa berurutan.

11. Metode "highlightWinningLine":

        private void highlightWinningLine(int row1, int col1, int row2, int col2) {
        buttons[row1][col1].setBackground(Color.GREEN);
        buttons[row2][col2].setBackground(Color.GREEN);
        }

     Fungsi ini bertugas untuk menonjolkan garis pemenang dengan mengubah latar belakang tombol-tombol yang membentuk garis pemenang menjadi warna hijau.

12. Metode "startGame":

        public void startGame() {
        frame.setSize(300, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
        }

      
    Fungsi ini bertugas untuk memulai permainan dengan mengonfigurasi dimensi frame, mengatur perilaku saat tombol penutup di klik, dan menampilkan frame ke layar.

13. Run dan kita memiliki permainan Tic-Tac-Toe 3x3 dengan tombol reset dan keterangan pemenang.
# Tampilan Output Code diatas :
![image](https://github.com/rizkyrizaldim/pic/assets/148876602/450263f4-335f-405f-a9f8-235487d1c32c) 
![image](https://github.com/rizkyrizaldim/pic/assets/148876602/e34f430e-f289-45eb-8ade-f7cf1690ba2f)

