# Rizky Rizaldi Mastiyanto (5311421085)
# Praktek Best First Search
# Permainan Tic-Tac-Toe 3x3
Tujuan Praktikum untuk meningkatkan pemahaman mahasiswa terhadap code permainan tic tac toe. Selain itu, modul 6
memberikan pengetahuan tentang Object Oriented Programming menggunakan bahasa pemrograman
Java terutama Java Swing dan Japplet.
1. Buka NetBeans dan buat proyek Java baru.
2. Buat dua file Java: TicTacToe.java dan TicTacToeGUI.java.
3. Masukkan kode berikut ke dalam file "TicTacToe.java":

        public class TicTacToe {
        public static void main(String[] args) {
        TicTacToeGUI game = new TicTacToeGUI();
        game.startGame();
        }
        }
4. Masukkan kode berikut ke dalam file "TicTacToeGUI.java":

       import java.awt.*;
       import java.awt.event.*;
       import javax.swing.*;

        public class TicTacToeGUI {
        private JFrame frame;
        private JButton[][] buttons;
        private char currentPlayer;
        private JLabel statusLabel;

        public TicTacToeGUI() {
        frame = new JFrame("Tic-Tac-Toe");
        buttons = new JButton[3][3];
        currentPlayer = 'X';

        statusLabel = new JLabel("Player " + currentPlayer + "'s turn");
        statusLabel.setFont(new Font("Arial", Font.PLAIN, 18));

        frame.setLayout(new GridLayout(4, 3));

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

        JButton resetButton = new JButton("Reset");
        resetButton.setFont(new Font("Arial", Font.PLAIN, 18));
        resetButton.addActionListener(new ResetListener());
        frame.add(statusLabel);
        frame.add(resetButton);
        }

        public void startGame() {
        frame.setSize(300, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
        }

        private class ButtonListener implements ActionListener {
        private int row;
        private int col;

        public ButtonListener(int row, int col) {
            this.row = row;
            this.col = col;
        }

        public void actionPerformed(ActionEvent e) {
            if (buttons[row][col].getText().equals("") && currentPlayer != ' ') {
                buttons[row][col].setText(currentPlayer + "");
                buttons[row][col].setForeground(currentPlayer == 'X' ? Color.BLUE : Color.RED);
                buttons[row][col].setFont(new Font("Arial", Font.BOLD, 48));
                currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
                statusLabel.setText("Player " + currentPlayer + "'s turn");

                if (checkForWin()) {
                    statusLabel.setText("Player " + currentPlayer + " wins!");
                    currentPlayer = ' ';
                }
            }
        }
        }

        private class ResetListener implements ActionListener {
        public void actionPerformed(ActionEvent e) {
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    buttons[i][j].setText("");
                    buttons[i][j].setBackground(Color.WHITE);
                }
            }
            currentPlayer = 'X';
            statusLabel.setText("Player " + currentPlayer + "'s turn");
        }
        }

        private boolean checkForWin() {
        // Check rows, columns, and diagonals for a win
        for (int i = 0; i < 3; i++) {
            if (buttons[i][0].getText().equals(buttons[i][1].getText()) &&
                buttons[i][1].getText().equals(buttons[i][2].getText()) &&
                !buttons[i][0].getText().equals("")) {
                highlightWinningLine(i, 0, i, 2);
                return true;
            }

            if (buttons[0][i].getText().equals(buttons[1][i].getText()) &&
                buttons[1][i].getText().equals(buttons[2][i].getText()) &&
                !buttons[0][i].getText().equals("")) {
                highlightWinningLine(0, i, 2, i);
                return true;
            }
        }

        if (buttons[0][0].getText().equals(buttons[1][1].getText()) &&
            buttons[1][1].getText().equals(buttons[2][2].getText()) &&
            !buttons[0][0].getText().equals("")) {
            highlightWinningLine(0, 0, 2, 2);
            return true;
        }

        if (buttons[0][2].getText().equals(buttons[1][1].getText()) &&
            buttons[1][1].getText().equals(buttons[2][0].getText()) &&
            !buttons[0][2].getText().equals("")) {
            highlightWinningLine(0, 2, 2, 0);
            return true;
        }

        return false;
        }

        private void highlightWinningLine(int row1, int col1, int row2, int col2) {
        buttons[row1][col1].setBackground(Color.GREEN);
        buttons[row2][col2].setBackground(Color.GREEN);
        }
        }

5. Run dan kita memiliki permainan Tic-Tac-Toe 3x3 dengan tombol reset dan keterangan pemenang.
# Tampilan Output Code diatas :
<img width="342" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/33b70992-a8d0-4608-9a83-b8bc702e08ca">
<img width="322" alt="image" src="https://github.com/RadityWisnu/Artificial-Intelligence-Tasks/assets/148683085/68ba1366-ea1c-4999-b28e-079bb7e9c759">

# Penjelasan Code :
1. Import Package

       import java.awt.*;
       import java.awt.event.*;
       import javax.swing.*;
   
   Ini adalah bagian untuk mengimpor package-package yang diperlukan untuk membuat GUI dalam Java.

2. Deklarasi Kelas "TicTacToeGUI":

        public class TicTacToeGUI {

    Kelas ini digunakan untuk membuat antarmuka pengguna grafis (GUI) permainan Tic-Tac-Toe.

3. Atribut-Atribut Kelas:

       private JFrame frame;
       private JButton[][] buttons;
       private char currentPlayer;
       private JLabel statusLabel;

   - 'frame' adalah frame utama dari aplikasi.
   - 'buttons' adalah array dari tombol-tombol yang merepresentasikan sel di papan permainan.
   - 'currentPlayer' adalah karakter yang menandakan giliran pemain saat ini ('X' atau 'O').
   - 'statusLabel' adalah label yang digunakan untuk menampilkan pesan status permainan.

4. Konstruktor Kelas "TicTacToeGUI":

       public TicTacToeGUI() {

    Ini adalah konstruktor untuk kelas TicTacToeGUI. Di dalam konstruktor, kita menginisialisasi atribut-atribut, membuat frame, dan menambahkan tombol-tombol serta label ke dalam frame.

5. Mengatur Layout Frame:

       frame.setLayout(new GridLayout(4, 3));

    Menggunakan layout 'GridLayout' dengan 4 baris dan 3 kolom untuk mengatur tata letak tombol dan label.

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

    Kode ini membuat tombol-tombol untuk setiap sel di papan permainan. Setiap tombol memiliki font, warna latar belakang, dan listener untuk menangani klik tombol.

7. Membuat Tombol Reset:

        JButton resetButton = new JButton("Reset");
        resetButton.setFont(new Font("Arial", Font.PLAIN, 18));
        resetButton.addActionListener(new ResetListener());
        frame.add(statusLabel);
        frame.add(resetButton);

    Membuat tombol "Reset" yang akan digunakan untuk me-reset permainan. 

8. Listener untuk Tombol Sel ('ButtonListener'):

        private class ButtonListener implements ActionListener {
        // ...
        }

    Kelas ini digunakan untuk menangani klik tombol sel di papan permainan. Jika seorang pemain mengklik tombol, fungsi ini akan menampilkan tanda (X atau O) di tombol dan memeriksa apakah ada pemenang.

9. Listener untuk Tombol Reset ('ResetListener'):

        private class ResetListener implements ActionListener {
        // ...
        }

    Kelas ini digunakan untuk menangani klik tombol "Reset". Saat tombol ini diklik, permainan akan di-reset ke keadaan awal.

10. Metode "checkForWin":

        private boolean checkForWin() {
        // ...
        }

      Metode ini digunakan untuk memeriksa apakah ada pemenang setelah setiap langkah. Ini memeriksa baris, kolom, dan diagonal untuk menentukan apakah ada tiga tanda yang sama berturut-turut.

11. Metode "highlightWinningLine":

        private void highlightWinningLine(int row1, int col1, int row2, int col2) {
        buttons[row1][col1].setBackground(Color.GREEN);
        buttons[row2][col2].setBackground(Color.GREEN);
        }

      Metode ini digunakan untuk menyorot garis pemenang dengan mengubah warna latar belakang tombol-tombol yang membentuk garis pemenang menjadi hijau.

12. Metode "startGame":

        public void startGame() {
        frame.setSize(300, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
        }

      Metode ini digunakan untuk memulai permainan dengan mengatur ukuran frame, menentukan operasi penutupan frame, dan menampilkannya.

