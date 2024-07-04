/*
JavaQuest merupakan sebuah game kuis sederhana yang dibuat dengan bahasa pemrograman Java.
Tujuan utama dari permainan ini adalah untuk menguji pengetahuan pemain tentang berbagai macam pertanyaan yang menarik.

Tujuan:
Memberikan tantangan kuis
 interaktif kepada pemain.
Memperkenalkan pemain dengan berbagai pertanyan kuis yang menarik.
Pemain bisa belajar sambil bermain.

Fitur-fitur Utama:
Pertanyaan dan Opsi Jawaban: Game ini menyajikan pertanyaan dengan empat opsi jawaban.
Timer: Terdapat timer untuk membatasi waktu menjawab setiap pertanyaan.
Skor: Skor pemain diperbarui setiap kali menjawab pertanyaan dengan benar.
Suara: Game ini memiliki efek suara untuk memberikan pengalaman bermain yang lebih menarik.

Cara Bermain:
Pemain akan diberikan dengan pertanyaan dengan empat opsi jawaban.
Pemain harus memilih jawaban yang menurutnya benar.
Setelah memilih, pemain harus mengklik tombol "Submit" untuk mengirim jawaban.
Jawaban akan diperiksa. Jika benar, skor akan bertambah.
Jika jawaban salah, pemain akan kehilangan kesempatan. Jika kesempatan habis, permainan berakhir.
Permainan berakhir setelah semua pertanyaan dijawab atau waktu habis.

Target:
1. Siswa maupun mahasiswa tergantung level dan kesulitan soalnya
2. Guru / dosen / pengajar
3. Orang tua / wali murid /wali siswa

metode: 16
1. JavaQuest() - Constructor untuk menginisialisasi permainan.
2. main(String[] args) - Metode utama untuk memulai aplikasi.
3. setupLayout() - Metode untuk mengatur tata letak komponen GUI.
4. pengaturTimer() - Metode untuk mengatur timer permainan.
5. Soal(int NomerSoal) - Metode untuk menampilkan pertanyaan.
6. tampilkanGambar() - Metode untuk menampilkan gambar dalam soal gambar.
7. sembunyikanGambar() - Metode untuk menyembunyikan gambar.
8. kirimJawaban() - Metode untuk memeriksa dan mengirim jawaban.
9. muatSuara(String path) - Metode untuk memuat file suara.
10.setVolume(Clip clip, float volume) - Metode untuk mengatur volume.
11.mainkanBacksound() - Metode untuk memainkan backsound permainan.
12.mainkanSuaraBenar() - Metode untuk memainkan suara saat jawaban benar.
13.mainkanSuaraSalah() - Metode untuk memainkan suara saat jawaban salah.
14.mainkanSuaraWaktuHabis() - Metode untuk memainkan suara saat waktu habis.
15.GagalKesempatan() - Metode untuk menangani kondisi ketika kesempatan habis.
16.GameSelesai() - Metode untuk menangani kondisi ketika permainan selesai

Total variabel: 18
1. labelPertanyaan
2. opsiJawaban
3. grupOpsi
4. tombolKirim
5. labelTimer
6. labelSkor
7. labelGambar
8. skor
9. jumlahPertanyaan
10.waktuTersisa
11. nomorPertanyaan
12. jawabanBenar
13. permainanSelesai
14. waktunyaHabis
15. isSoundOn
16. tampilkanGambarFlag
17. kesempatan
18. timer
*/

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.sound.sampled.AudioInputStream;
import javax.sound.sampled.AudioSystem;
import javax.sound.sampled.Clip;
import java.io.File;
import javax.sound.sampled.FloatControl;

public class JavaQuest2 extends JFrame {
    private JLabel labelPertanyaan;  
    private JRadioButton[]opsiJawaban; 
    private ButtonGroup grupOpsi; 
    private JButton tombolKirim; 
    private JLabel labelTimer;  
    private JLabel labelSkor;  
    private JLabel labelGambar;     

    private int kesempatan = 3;
    private int skor = 0;
    private int jumlahPertanyaan = 7;
    private int waktuTersisa = 60;
    private int nomorPertanyaan = 0;
    private int[] jawabanBenar = {0, 0, 3, 2, 0, 1, 2};
    private Timer timer; 
    private Clip klipAudio;  
    private boolean permainanSelesai = false;
    private boolean waktunyaHabis = false;
    private boolean isSoundOn = true; 
    private boolean tampilkanGambarFlag = false;  
    

    public JavaQuest2() {
        setTitle("Game JavaQuest");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
        setSize(400, 600); 

        JPanel mainPanel = new JPanel(new BorderLayout());
        JPanel gamePanel = new JPanel(new GridLayout(10, 1));

        labelPertanyaan = new JLabel(); 
        opsiJawaban = new JRadioButton[4]; 
        grupOpsi = new ButtonGroup(); 
        tombolKirim = new JButton("Submit");
        labelTimer = new JLabel("Waktu: " + waktuTersisa + " detik");
        labelSkor = new JLabel("Skor: " + skor); 
        labelGambar = new JLabel();

        gamePanel.add(labelSkor);
        gamePanel.add(new JLabel("Anda memiliki " + waktuTersisa + " detik untuk menjawab setiap pertanyaan."));
        gamePanel.add(labelPertanyaan);
        for (int i = 0; i < opsiJawaban.length; i++) {
            opsiJawaban[i] = new JRadioButton();
            grupOpsi.add(opsiJawaban[i]); 
            gamePanel.add(opsiJawaban[i]); 
        }
        gamePanel.add(tombolKirim);
        gamePanel.add(labelTimer);
        gamePanel.add(labelGambar);
        
        mainkanBacksound();
        setupLayout(); 
        setVisible(true); 
        add(gamePanel); 
        pengaturTimer();
        Soal(nomorPertanyaan); 
        setVisible(true); 

        tombolKirim.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                kirimJawaban();
            }
        });
    }

    //method untuk menampilkan soal
    private void Soal(int NomerSoal) {
        pengaturTimer(); 
        timer.start();
        if (NomerSoal < jumlahPertanyaan) {
            String[] questions = {
                    "Siapakah presiden pertama Republik Indonesia?",
                    "Kapan Proklamasi Kemerdekaan Indonesia terjadi?",
                    "Hewan berkaki dua?",
                    "Nama Hewan diatas adalah ?",
                    "Siapakah penemu bola lampu?",
                    "Apa ibukota Jepang?",
                    "Siapakah orang dalam gambar ini?",
            };
            String[][] answers = {
                    {"Soekarno", "Soeharto", "Joko Widodo", "Megawati Soekarnoputri"},
                    {"17 Agustus 1945", "17 Agustus 1946", "17 Agustus 1947", "17 Agustus 1948"},
                    {"Sapi", "Kucing", "Macan", "Ayam"},
                    {"Sapi", "Kucing", "Banteng ", "Hiu "},
                    {"Thomas Edison", "Isaac Newton", "Albert Einstein", "BJ. Habibie"},
                    {"Kyoto", "Tokyo", "Osaka", "Nagasaki"},
                    {"Soekarno", "Soeharto", "Joko Widodo", "Megawati Soekarnoputri"},
            };

            if (NomerSoal == 3 || NomerSoal == 6) {
                tampilkanGambar(); 
            } else {
                sembunyikanGambar(); 
            }

            labelPertanyaan.setText("Pertanyaan " + (NomerSoal + 1) + ": " + questions[NomerSoal]);

            for (int i = 0; i < opsiJawaban.length; i++) {
                opsiJawaban[i].setText(answers[NomerSoal][i]); 
            }
        }
    }

    //method untuk menmpilakan gambar dalam soal gambar
    private void tampilkanGambar() {
        if (nomorPertanyaan == 3) {
            String imagePath = "C:\\Users\\V14\\IdeaProjects\\untitled1\\image\\Banteng.jpg";
            ImageIcon imageIcon = new ImageIcon(new ImageIcon(imagePath).getImage().getScaledInstance(200, 200, Image.SCALE_DEFAULT));
            labelGambar.setIcon(imageIcon);
        }else if (nomorPertanyaan == 6) {
            String imagePath = "C:\\Users\\V14\\IdeaProjects\\untitled1\\image\\fotoPresiden.jpg";
            ImageIcon imageIcon = new ImageIcon(new ImageIcon(imagePath).getImage().getScaledInstance(200, 200, Image.SCALE_DEFAULT));
            labelGambar.setIcon(imageIcon); 
        }
    }

    // Method untuk menyembunyikan gambar pada imageLabel dengan menghapus ikonnya
    private void sembunyikanGambar() {
        labelGambar.setIcon(null); 
    }

    // Method untuk mengatur tata letak komponen dalam GUI menggunakan GridBagLayout
    private void setupLayout() {
        setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 50, 5, 50);
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        gbc.anchor = GridBagConstraints.CENTER; 
        Dimension ukuranGambar = new Dimension(200, 200); 
        Dimension ukuranTombol = new Dimension(150, 30); 
        labelGambar.setPreferredSize(ukuranGambar);
        add(labelGambar, gbc);
        gbc.gridy++;
        gbc.gridwidth = 0; 
        gbc.anchor = GridBagConstraints.CENTER;
        Dimension ukuranPertanyaan = new Dimension(350, 100);
        Dimension ukuranJawaban = new Dimension(300, 25);
        labelPertanyaan.setPreferredSize(ukuranPertanyaan);
        add(labelPertanyaan, gbc);

        for (int i = 0; i < opsiJawaban.length; i++) {
            gbc.gridy++;
            opsiJawaban[i].setPreferredSize(ukuranJawaban);
            add(opsiJawaban[i], gbc);
        }

        gbc.gridy++;
        gbc.anchor = GridBagConstraints.CENTER;
        tombolKirim.setPreferredSize(ukuranTombol);
        add(tombolKirim, gbc);
        setLocationRelativeTo(null);
        setVisible(true);
    }
    
   

    private void kirimJawaban() {
        int[] jawabanBenar = {0, 0, 3, 2, 0, 1, 2}; 
        boolean statusJawaban = false;
        int indeksJawabanBenar = jawabanBenar[nomorPertanyaan]; 
        int indeksJawabanTerpilih = 0; 

        for (int i = 0; i < opsiJawaban.length; i++) {
            if (opsiJawaban[i].isSelected()) {
                indeksJawabanTerpilih = i; 
                if (i == indeksJawabanBenar) {
                    statusJawaban = true;
                    break;
                }
            }
        }

        if (!statusJawaban) {
            kesempatan--;
            if (kesempatan <= 0) {
                tampilkanGambarFlag = true; 
                GagalKesempatan();
            } else {
                mainkanSuaraSalah(); 
                JOptionPane.showMessageDialog(null, "Jawaban Anda salah. Anda memiliki " + kesempatan + " kesempatan lagi.");
            }
        } else {
            skor++; 
            labelSkor.setText("Skor: " + skor); 
            nomorPertanyaan++;
            if (nomorPertanyaan == jumlahPertanyaan) {
                tampilkanGambarFlag = true; 
                GameSelesai();
                pengaturTimer();
            } else {
                mainkanSuaraBenar(); 
                pengaturTimer(); 
                Soal(nomorPertanyaan);
            }
        }
    }

    // Metode untuk memainkan file audio
    private Clip muatSuara(String path) {
        try {
            File audioFile = new File(path); 
            if (audioFile.exists()) {
                AudioInputStream audioInputStream = AudioSystem.getAudioInputStream(audioFile);
                Clip audioClip = AudioSystem.getClip();
                audioClip.open(audioInputStream);
                return audioClip; 
            } else {
                System.err.println("File audio tidak ditemukan: " + path); 
            }
        } catch (Exception e) {
            e.printStackTrace(); 
        }
        return null; 
    }

    private void setVolume(Clip clip, float volume) {
        if (clip != null && clip.isControlSupported(FloatControl.Type.MASTER_GAIN)) {
            FloatControl control = (FloatControl) clip.getControl(FloatControl.Type.MASTER_GAIN);
            float range = control.getMaximum() - control.getMinimum();
            float gain = (range * volume) + control.getMinimum();
            control.setValue(gain);
        }
    }

    private void mainkanBacksound() {
        Clip backsound = muatSuara("C:\\Users\\V14\\IdeaProjects\\untitled1\\Suara\\backSoundGame.wav");
        if (backsound != null) {
            setVolume(backsound, 0.5f); 
            backsound.loop(Clip.LOOP_CONTINUOUSLY); 
            backsound.start(); 
        }
    }

    private void mainkanSuaraBenar() {
        Clip suaraBenar = muatSuara("C:\\Users\\V14\\IdeaProjects\\untitled1\\Suara\\JawabanBenar.wav");
        if (suaraBenar != null) {
            suaraBenar.start(); 
        }
    }
    
    private void mainkanSuaraSalah() {    
        Clip suaraSalah = muatSuara("C:\\Users\\V14\\IdeaProjects\\untitled1\\Suara\\JawabanSalah.wav");
        if (suaraSalah != null) {
            suaraSalah.start();
        }
    }

    private void pengaturTimer() {
        timer = new Timer(2000, new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                if (waktuTersisa > 0 && !permainanSelesai) {
                    waktuTersisa--;
                    labelTimer.setText("Waktu: " + waktuTersisa + " detik");
                } else {
                    if (waktuTersisa <= 0 && !waktunyaHabis) {
                        mainkanSuaraWaktuHabis();
                        waktunyaHabis = true;
                    }
                    if (waktunyaHabis) {
                        timer.stop();
                        mainkanSuaraWaktuHabis();
                    } else if (nomorPertanyaan == jumlahPertanyaan) {
                        GameSelesai();
                        timer.stop(); 
                    } else {
                        Soal(nomorPertanyaan);
                    }
                }
            }
        });
    }

    private void mainkanSuaraWaktuHabis() {
        Clip suaraWaktuHabis = muatSuara("C:\\Users\\V14\\IdeaProjects\\untitled1\\Suara\\WaktuHabis.wav");
        if (suaraWaktuHabis != null) {
            suaraWaktuHabis.start(); 
            String messageLine1 = "Sayang Sekali Waktunya sudah habis\n" + "GameOver! Skor Anda: " + skor + " dari " + jumlahPertanyaan + ".\n"; 
            JOptionPane.showMessageDialog(null, messageLine1);
            System.exit(0);
        }
    }

    private void GagalKesempatan() {
        Clip suaraKesempatanGagal = muatSuara("C:\\Users\\V14\\IdeaProjects\\untitled1\\Suara\\AyoCobalagi.wav");
        timer.stop();
    
        if (suaraKesempatanGagal != null) {
            suaraKesempatanGagal.start(); 
            String messageLine1 = "Kesempatan habis! Game Over! Semangat! Ayo Coba Lagi\n"; 
            String messageLine2 = "Permainan selesai! Skor Anda: " + skor + " dari " + jumlahPertanyaan + ".\n";
            JOptionPane.showMessageDialog(null, messageLine1);
            JOptionPane.showMessageDialog(null, messageLine2); 
            System.exit(0);
        }
    }

    private void GameSelesai() {
        if (!permainanSelesai) {
            timer.stop(); 
            permainanSelesai = true; 
            Clip suaraSelesai = muatSuara("C:\\Users\\V14\\IdeaProjects\\untitled1\\Suara\\saatGameBerakhir.wav");
            if (suaraSelesai != null) {
                suaraSelesai.start();

                String message = "Kamu berhasil! Permainan selesai! Skor Anda: " + skor;
                JOptionPane.showMessageDialog(null, message);
                System.exit(0);
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new JavaQuest();
            }
        });
    }
}
