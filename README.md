
# Aplikasi Penghitung Hari

Sebuah aplikasi yang dapat digunakan untuk menghitung jumlah hari, tahun kabisat, dan selisih antara dua tanggal, yang ditujukan untuk menyelesaikan Tugas PBO ke-4.

#### Source Code PenghitungHariFrame.java
```java
import java.time.DayOfWeek;
import java.time.LocalDate;
import java.time.YearMonth;
import java.time.temporal.ChronoUnit;

/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/GUIForms/JFrame.java to edit this template
 */

/**
 *
 * @author MyBook Z Series
 */
public class PenghitungHariFrame extends javax.swing.JFrame {
    
    // Array untuk memetakan nama hari dalam bahasa Indonesia
    private static final String[] NAMA_HARI_INDO = {
        "Minggu", // SUNDAY
        "Senin",  // MONDAY
        "Selasa", // TUESDAY
        "Rabu",   // WEDNESDAY
        "Kamis",  // THURSDAY
        "Jumat",  // FRIDAY
        "Sabtu"   // SATURDAY
    };
    /**
     * Creates new form PenghitungHariFrame
     */
    public PenghitungHariFrame() {
        initComponents();
        
        updateFieldsFromCalendarSatu();
        updateFieldsFromCalendarDua();
        
        // Listener untuk buttonHitung
        buttonHitung.addActionListener(e -> hitungHari());
        
        // Menambahkan listener untuk jCalendarSatu
        jCalendarSatu.getYearChooser().addPropertyChangeListener(evt -> {
            if ("year".equals(evt.getPropertyName())) {
                updateFieldsFromCalendarSatu();
            }
        });

        jCalendarSatu.getMonthChooser().addPropertyChangeListener(evt -> {
            if ("month".equals(evt.getPropertyName())) {
                updateFieldsFromCalendarSatu();
            }
        });

        // Menambahkan listener untuk tahun dan bulan di jCalendarDua
        jCalendarDua.getYearChooser().addPropertyChangeListener(evt -> {
            if ("year".equals(evt.getPropertyName())) {
                updateFieldsFromCalendarDua();
            }
        });

        jCalendarDua.getMonthChooser().addPropertyChangeListener(evt -> {
            if ("month".equals(evt.getPropertyName())) {
                updateFieldsFromCalendarDua();
            }
        });
        
        // Listener untuk comboBulanSatu (bulan pertama)
        comboBulanSatu.addActionListener(e -> updateCalendarFromComboBulanSatu());

        // Listener untuk comboBulanDua (bulan kedua)
        comboBulanDua.addActionListener(e -> updateCalendarFromComboBulanDua());

        // Listener untuk spinnerTahunSatu (tahun pertama)
        spinnerTahunSatu.addChangeListener(e -> updateCalendarFromSpinnerTahunSatu());

        // Listener untuk spinnerTahunDua (tahun kedua)
        spinnerTahunDua.addChangeListener(e -> updateCalendarFromSpinnerTahunDua());
    }
    
    private void hitungHari() {
        // Ambil tahun, bulan, dan tanggal dari jCalendarSatu
        int tahunPertama = jCalendarSatu.getYearChooser().getYear();
        int bulanPertama = jCalendarSatu.getMonthChooser().getMonth() + 1; // getMonth() dimulai dari 0
        int tanggalPertama = jCalendarSatu.getDayChooser().getDay();

        // Ambil tahun, bulan, dan tanggal dari jCalendarDua
        int tahunKedua = jCalendarDua.getYearChooser().getYear();
        int bulanKedua = jCalendarDua.getMonthChooser().getMonth() + 1; // getMonth() dimulai dari 0
        int tanggalKedua = jCalendarDua.getDayChooser().getDay();

        // Membuat objek LocalDate untuk tanggal yang dipilih di kedua kalender
        LocalDate tanggalYangDipilihBulanSatu = LocalDate.of(tahunPertama, bulanPertama, tanggalPertama);
        LocalDate tanggalYangDipilihBulanDua = LocalDate.of(tahunKedua, bulanKedua, tanggalKedua);

        // Hitung tanggal pertama dan terakhir bulan pertama
        LocalDate tanggalPertamaBulanSatu = LocalDate.of(tahunPertama, bulanPertama, 1);
        YearMonth yearMonthSatu = YearMonth.of(tahunPertama, bulanPertama);
        LocalDate tanggalTerakhirBulanSatu = yearMonthSatu.atEndOfMonth();

        // Hitung tanggal pertama dan terakhir bulan kedua
        LocalDate tanggalPertamaBulanDua = LocalDate.of(tahunKedua, bulanKedua, 1);
        YearMonth yearMonthDua = YearMonth.of(tahunKedua, bulanKedua);
        LocalDate tanggalTerakhirBulanDua = yearMonthDua.atEndOfMonth();

        // Nama hari pertama dan terakhir untuk bulan pertama
        DayOfWeek hariPertamaBulanSatu = tanggalPertamaBulanSatu.getDayOfWeek();
        DayOfWeek hariTerakhirBulanSatu = tanggalTerakhirBulanSatu.getDayOfWeek();

        // Nama hari pertama dan terakhir untuk bulan kedua
        DayOfWeek hariPertamaBulanDua = tanggalPertamaBulanDua.getDayOfWeek();
        DayOfWeek hariTerakhirBulanDua = tanggalTerakhirBulanDua.getDayOfWeek();

        // Menambahkan 1 untuk menyesuaikan hari, agar Senin menjadi index 0
        String namaHariPertamaBulanSatu = NAMA_HARI_INDO[(hariPertamaBulanSatu.ordinal() + 1) % 7];
        String namaHariTerakhirBulanSatu = NAMA_HARI_INDO[(hariTerakhirBulanSatu.ordinal() + 1) % 7];
        String namaHariPertamaBulanDua = NAMA_HARI_INDO[(hariPertamaBulanDua.ordinal() + 1) % 7];
        String namaHariTerakhirBulanDua = NAMA_HARI_INDO[(hariTerakhirBulanDua.ordinal() + 1) % 7];

        // Masukkan hasil ke JTextField
        hasilHariPertamaBulanSatu.setText(namaHariPertamaBulanSatu);
        hasilHariTerakhirBulanSatu.setText(namaHariTerakhirBulanSatu);
        hasilHariPertamaBulanDua.setText(namaHariPertamaBulanDua);
        hasilHariTerakhirBulanDua.setText(namaHariTerakhirBulanDua);

        // Hitung jumlah hari pada bulan pertama dan kedua
        int jumlahHariPertama = yearMonthSatu.lengthOfMonth();
        int jumlahHariKedua = yearMonthDua.lengthOfMonth();

        // Masukkan jumlah hari ke JTextField
        hasilJumlahTanggalPertama.setText(String.valueOf(jumlahHariPertama));
        hasilJumlahTanggalKedua.setText(String.valueOf(jumlahHariKedua));

        // Tentukan apakah tahun pertama dan kedua adalah tahun kabisat
        String tahunKabisatPertama = isLeapYear(tahunPertama) ? "Ya" : "Tidak";
        String tahunKabisatKedua = isLeapYear(tahunKedua) ? "Ya" : "Tidak";

        // Masukkan hasil kabisat ke JTextField
        hasilTahunKabisatPertama.setText(tahunKabisatPertama);
        hasilTahunKabisatKedua.setText(tahunKabisatKedua);

        // Hitung selisih hari antara tanggal pertama yang dipilih di kalender pertama dan kalender kedua
        long selisihHari = ChronoUnit.DAYS.between(tanggalYangDipilihBulanSatu, tanggalYangDipilihBulanDua);

        // Masukkan hasil selisih hari ke JTextField
        hasilSelisih.setText(String.valueOf(selisihHari));
    }

    // Fungsi untuk memeriksa apakah tahun kabisat
    private boolean isLeapYear(int year) {
        return (year % 4 == 0 && (year % 100 != 0 || year % 400 == 0));
    }
    
    private void updateFieldsFromCalendarSatu() {
        // Mendapatkan tanggal yang dipilih dari jCalendarSatu
        java.util.Date selectedDate = jCalendarSatu.getDate();

        // Menggunakan Calendar untuk ekstrak tahun, bulan, dan tanggal
        java.util.Calendar calendar = java.util.Calendar.getInstance();
        calendar.setTime(selectedDate);

        // Mendapatkan tahun dan bulan
        int year = calendar.get(java.util.Calendar.YEAR);
        int month = calendar.get(java.util.Calendar.MONTH); // Bulan mulai dari 0 (Januari = 0)
        int day = calendar.get(java.util.Calendar.DAY_OF_MONTH);

        // Mengatur bulan pada comboBulanSatu (karena bulan dimulai dari 0)
        comboBulanSatu.setSelectedIndex(month);

        // Mengatur tahun pada spinnerTahunSatu
        spinnerTahunSatu.setValue(year);
    }

    private void updateFieldsFromCalendarDua() {
        // Mendapatkan tanggal yang dipilih dari jCalendarDua
        java.util.Date selectedDate = jCalendarDua.getDate();

        // Menggunakan Calendar untuk ekstrak tahun, bulan, dan tanggal
        java.util.Calendar calendar = java.util.Calendar.getInstance();
        calendar.setTime(selectedDate);

        // Mendapatkan tahun dan bulan
        int year = calendar.get(java.util.Calendar.YEAR);
        int month = calendar.get(java.util.Calendar.MONTH); // Bulan mulai dari 0 (Januari = 0)
        int day = calendar.get(java.util.Calendar.DAY_OF_MONTH);

        // Mengatur bulan pada comboBulanDua (karena bulan dimulai dari 0)
        comboBulanDua.setSelectedIndex(month);

        // Mengatur tahun pada spinnerTahunDua
        spinnerTahunDua.setValue(year);
    }
    
    private void updateCalendarFromComboBulanSatu() {
        // Mendapatkan bulan dan tahun dari comboBulanSatu dan spinnerTahunSatu
        int selectedMonth = comboBulanSatu.getSelectedIndex();  // Bulan mulai dari 0 (Januari = 0)
        int selectedYear = (int) spinnerTahunSatu.getValue();

        // Memperbarui bulan dan tahun pada jCalendarSatu
        jCalendarSatu.getMonthChooser().setMonth(selectedMonth);
        jCalendarSatu.getYearChooser().setYear(selectedYear);
    }

    private void updateCalendarFromComboBulanDua() {
        // Mendapatkan bulan dan tahun dari comboBulanDua dan spinnerTahunDua
        int selectedMonth = comboBulanDua.getSelectedIndex();  // Bulan mulai dari 0 (Januari = 0)
        int selectedYear = (int) spinnerTahunDua.getValue();

        // Memperbarui bulan dan tahun pada jCalendarDua
        jCalendarDua.getMonthChooser().setMonth(selectedMonth);
        jCalendarDua.getYearChooser().setYear(selectedYear);
    }

    private void updateCalendarFromSpinnerTahunSatu() {
        // Mendapatkan bulan yang dipilih di comboBulanSatu dan tahun dari spinnerTahunSatu
        int selectedMonth = comboBulanSatu.getSelectedIndex();  // Bulan mulai dari 0 (Januari = 0)
        int selectedYear = (int) spinnerTahunSatu.getValue();

        // Memperbarui tahun dan bulan pada jCalendarSatu
        jCalendarSatu.getMonthChooser().setMonth(selectedMonth);
        jCalendarSatu.getYearChooser().setYear(selectedYear);
    }

    private void updateCalendarFromSpinnerTahunDua() {
        // Mendapatkan bulan yang dipilih di comboBulanDua dan tahun dari spinnerTahunDua
        int selectedMonth = comboBulanDua.getSelectedIndex();  // Bulan mulai dari 0 (Januari = 0)
        int selectedYear = (int) spinnerTahunDua.getValue();

        // Memperbarui tahun dan bulan pada jCalendarDua
        jCalendarDua.getMonthChooser().setMonth(selectedMonth);
        jCalendarDua.getYearChooser().setYear(selectedYear);
    }


    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {
        java.awt.GridBagConstraints gridBagConstraints;

        panelUtama = new javax.swing.JPanel();
        jLabel1 = new javax.swing.JLabel();
        buttonHitung = new javax.swing.JButton();
        jCalendarDua = new com.toedter.calendar.JCalendar();
        jCalendarSatu = new com.toedter.calendar.JCalendar();
        jLabel4 = new javax.swing.JLabel();
        jLabel5 = new javax.swing.JLabel();
        panelHasil = new javax.swing.JPanel();
        jLabel6 = new javax.swing.JLabel();
        jLabel7 = new javax.swing.JLabel();
        hasilJumlahTanggalPertama = new javax.swing.JTextField();
        hasilTahunKabisatKedua = new javax.swing.JTextField();
        jLabel8 = new javax.swing.JLabel();
        hasilSelisih = new javax.swing.JTextField();
        jLabel9 = new javax.swing.JLabel();
        jLabel10 = new javax.swing.JLabel();
        hasilHariPertamaBulanSatu = new javax.swing.JTextField();
        hasilHariPertamaBulanDua = new javax.swing.JTextField();
        jLabel11 = new javax.swing.JLabel();
        hasilJumlahTanggalKedua = new javax.swing.JTextField();
        jLabel12 = new javax.swing.JLabel();
        hasilTahunKabisatPertama = new javax.swing.JTextField();
        jLabel15 = new javax.swing.JLabel();
        jLabel16 = new javax.swing.JLabel();
        hasilHariTerakhirBulanSatu = new javax.swing.JTextField();
        hasilHariTerakhirBulanDua = new javax.swing.JTextField();
        panelPertama = new javax.swing.JPanel();
        jLabel2 = new javax.swing.JLabel();
        comboBulanSatu = new javax.swing.JComboBox<>();
        jLabel3 = new javax.swing.JLabel();
        spinnerTahunSatu = new javax.swing.JSpinner();
        panelKedua = new javax.swing.JPanel();
        jLabel13 = new javax.swing.JLabel();
        comboBulanDua = new javax.swing.JComboBox<>();
        jLabel14 = new javax.swing.JLabel();
        spinnerTahunDua = new javax.swing.JSpinner();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        panelUtama.setBackground(new java.awt.Color(255, 204, 153));
        panelUtama.setBorder(javax.swing.BorderFactory.createTitledBorder(""));
        panelUtama.setLayout(new java.awt.GridBagLayout());

        jLabel1.setFont(new java.awt.Font("Segoe UI", 1, 24)); // NOI18N
        jLabel1.setText("Aplikasi Penghitung Hari");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 0;
        gridBagConstraints.gridwidth = 2;
        gridBagConstraints.insets = new java.awt.Insets(0, 0, 17, 0);
        panelUtama.add(jLabel1, gridBagConstraints);

        buttonHitung.setText("Hitung");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 4;
        gridBagConstraints.gridwidth = 2;
        gridBagConstraints.ipadx = 18;
        gridBagConstraints.insets = new java.awt.Insets(5, 5, 5, 5);
        panelUtama.add(buttonHitung, gridBagConstraints);

        jCalendarDua.setBorder(javax.swing.BorderFactory.createBevelBorder(javax.swing.border.BevelBorder.RAISED));
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 3;
        gridBagConstraints.insets = new java.awt.Insets(0, 14, 0, 14);
        panelUtama.add(jCalendarDua, gridBagConstraints);

        jCalendarSatu.setBorder(javax.swing.BorderFactory.createBevelBorder(javax.swing.border.BevelBorder.RAISED));
        jCalendarSatu.setDate(new java.util.Date(1705231491000L));
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 3;
        gridBagConstraints.insets = new java.awt.Insets(0, 14, 0, 14);
        panelUtama.add(jCalendarSatu, gridBagConstraints);

        jLabel4.setFont(new java.awt.Font("Segoe UI", 1, 12)); // NOI18N
        jLabel4.setText("Tanggal Pertama");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 2;
        gridBagConstraints.insets = new java.awt.Insets(14, 14, 8, 14);
        panelUtama.add(jLabel4, gridBagConstraints);

        jLabel5.setFont(new java.awt.Font("Segoe UI", 1, 12)); // NOI18N
        jLabel5.setText("Tanggal Kedua");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 2;
        gridBagConstraints.insets = new java.awt.Insets(14, 14, 8, 14);
        panelUtama.add(jLabel5, gridBagConstraints);

        panelHasil.setBackground(new java.awt.Color(255, 204, 153));
        panelHasil.setBorder(javax.swing.BorderFactory.createTitledBorder(null, "Hasil", javax.swing.border.TitledBorder.CENTER, javax.swing.border.TitledBorder.DEFAULT_POSITION));
        panelHasil.setLayout(new java.awt.GridBagLayout());

        jLabel6.setText("Jumlah Hari Bulan Pertama :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel6, gridBagConstraints);

        jLabel7.setText("Kabisat Tahun Kedua :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 3;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel7, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilJumlahTanggalPertama, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 3;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilTahunKabisatKedua, gridBagConstraints);

        jLabel8.setText("Jumlah Selisih Hari :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 8;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel8, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 8;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilSelisih, gridBagConstraints);

        jLabel9.setText("Hari Pertama Bulan Pertama :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 4;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel9, gridBagConstraints);

        jLabel10.setText("Hari Pertama Bulan Kedua :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 6;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel10, gridBagConstraints);

        hasilHariPertamaBulanSatu.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                hasilHariPertamaBulanSatuActionPerformed(evt);
            }
        });
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 4;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilHariPertamaBulanSatu, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 6;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilHariPertamaBulanDua, gridBagConstraints);

        jLabel11.setText("Jumlah Hari Bulan Kedua :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 1;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel11, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 1;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilJumlahTanggalKedua, gridBagConstraints);

        jLabel12.setText("Kabisat Tahun Pertama :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 2;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel12, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 2;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilTahunKabisatPertama, gridBagConstraints);

        jLabel15.setText("Hari Terakhir Bulan Pertama :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 5;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel15, gridBagConstraints);

        jLabel16.setText("Hari Terakhir Bulan Kedua :");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 7;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_START;
        panelHasil.add(jLabel16, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 5;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilHariTerakhirBulanSatu, gridBagConstraints);
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 7;
        gridBagConstraints.ipadx = 150;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.LINE_END;
        panelHasil.add(hasilHariTerakhirBulanDua, gridBagConstraints);

        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 5;
        gridBagConstraints.gridwidth = 2;
        gridBagConstraints.ipadx = 200;
        gridBagConstraints.ipady = 50;
        gridBagConstraints.insets = new java.awt.Insets(10, 0, 10, 0);
        panelUtama.add(panelHasil, gridBagConstraints);

        panelPertama.setBorder(javax.swing.BorderFactory.createBevelBorder(javax.swing.border.BevelBorder.RAISED));
        panelPertama.setLayout(new java.awt.GridBagLayout());

        jLabel2.setText("Pilih Bulan Pertama");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 0;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelPertama.add(jLabel2, gridBagConstraints);

        comboBulanSatu.setModel(new javax.swing.DefaultComboBoxModel<>(new String[] { "January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "Desember" }));
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 0;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelPertama.add(comboBulanSatu, gridBagConstraints);

        jLabel3.setText("Pilih Tahun Pertama");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 1;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelPertama.add(jLabel3, gridBagConstraints);

        spinnerTahunSatu.setEditor(new javax.swing.JSpinner.NumberEditor(spinnerTahunSatu, "0000"));
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 1;
        gridBagConstraints.ipadx = 55;
        gridBagConstraints.anchor = java.awt.GridBagConstraints.NORTHWEST;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelPertama.add(spinnerTahunSatu, gridBagConstraints);

        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 1;
        panelUtama.add(panelPertama, gridBagConstraints);

        panelKedua.setBorder(javax.swing.BorderFactory.createBevelBorder(javax.swing.border.BevelBorder.RAISED));
        panelKedua.setLayout(new java.awt.GridBagLayout());

        jLabel13.setText("Pilih Bulan Kedua");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 0;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelKedua.add(jLabel13, gridBagConstraints);

        comboBulanDua.setModel(new javax.swing.DefaultComboBoxModel<>(new String[] { "January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "Desember" }));
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 0;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelKedua.add(comboBulanDua, gridBagConstraints);

        jLabel14.setText("Pilih Tahun Kedua");
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 0;
        gridBagConstraints.gridy = 1;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelKedua.add(jLabel14, gridBagConstraints);

        spinnerTahunDua.setModel(new javax.swing.SpinnerNumberModel());
        spinnerTahunDua.setEditor(new javax.swing.JSpinner.NumberEditor(spinnerTahunDua, "0000"));
        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 1;
        gridBagConstraints.ipadx = 55;
        gridBagConstraints.insets = new java.awt.Insets(4, 4, 4, 4);
        panelKedua.add(spinnerTahunDua, gridBagConstraints);

        gridBagConstraints = new java.awt.GridBagConstraints();
        gridBagConstraints.gridx = 1;
        gridBagConstraints.gridy = 1;
        panelUtama.add(panelKedua, gridBagConstraints);

        getContentPane().add(panelUtama, java.awt.BorderLayout.CENTER);

        pack();
    }// </editor-fold>                        

    private void hasilHariPertamaBulanSatuActionPerformed(java.awt.event.ActionEvent evt) {                                                          
        // TODO add your handling code here:
    }                                                         
    
    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(PenghitungHariFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(PenghitungHariFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(PenghitungHariFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(PenghitungHariFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>
        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new PenghitungHariFrame().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JButton buttonHitung;
    private javax.swing.JComboBox<String> comboBulanDua;
    private javax.swing.JComboBox<String> comboBulanSatu;
    private javax.swing.JTextField hasilHariPertamaBulanDua;
    private javax.swing.JTextField hasilHariPertamaBulanSatu;
    private javax.swing.JTextField hasilHariTerakhirBulanDua;
    private javax.swing.JTextField hasilHariTerakhirBulanSatu;
    private javax.swing.JTextField hasilJumlahTanggalKedua;
    private javax.swing.JTextField hasilJumlahTanggalPertama;
    private javax.swing.JTextField hasilSelisih;
    private javax.swing.JTextField hasilTahunKabisatKedua;
    private javax.swing.JTextField hasilTahunKabisatPertama;
    private com.toedter.calendar.JCalendar jCalendarDua;
    private com.toedter.calendar.JCalendar jCalendarSatu;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JLabel jLabel10;
    private javax.swing.JLabel jLabel11;
    private javax.swing.JLabel jLabel12;
    private javax.swing.JLabel jLabel13;
    private javax.swing.JLabel jLabel14;
    private javax.swing.JLabel jLabel15;
    private javax.swing.JLabel jLabel16;
    private javax.swing.JLabel jLabel2;
    private javax.swing.JLabel jLabel3;
    private javax.swing.JLabel jLabel4;
    private javax.swing.JLabel jLabel5;
    private javax.swing.JLabel jLabel6;
    private javax.swing.JLabel jLabel7;
    private javax.swing.JLabel jLabel8;
    private javax.swing.JLabel jLabel9;
    private javax.swing.JPanel panelHasil;
    private javax.swing.JPanel panelKedua;
    private javax.swing.JPanel panelPertama;
    private javax.swing.JPanel panelUtama;
    private javax.swing.JSpinner spinnerTahunDua;
    private javax.swing.JSpinner spinnerTahunSatu;
    // End of variables declaration                   
}
```
## Fitur Utama
- Menghitung jumlah hari pada bulan yang dipilih
- Menentukan apakah tahun yang dipilih adalah kabisat atau tidak

## Fitur Tambahan (Variasi)
- Menampilkan hari pertama dan hari terakhir pada bulan pertama dan kedua
- Menghitung selisih antara tanggal pertama dan tanggal kedua

## Screenshot
![{1F8F67EE-79F6-4953-9D8F-5C38BF61D175}](https://github.com/user-attachments/assets/b5c3e77e-612b-4d9e-b172-0fa9e55437c5)

## Referensi

 - [Modul PBO2 Tugas 4](https://drive.google.com/file/d/117U7vgoWA98vy3oesXkIiRlZkyVaPFoO/view)


## Biodata Pembuat
- Nama : Bintang Putra Setiawan
- Kelas : 5B Reg Pagi Banjarmasin
- NPM : 2210010539
- [@Bintangps01](https://github.com/Bintangps01)
