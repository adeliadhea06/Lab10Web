# Lab10Web

Nama : Dhea Dwi Adelia

NIM : 312210116

Kelas : TI.22.A.1

## Instruksi Praktikum

1. Persiapkan text editor misalnya VSCode.
   
2. Buat folder baru dengan nama lab10_php_oop pada docroot webserver (htdocs)
   
3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.

## Langkah-langkah Praktikum
1. Buat file php baru dengan nama `mobil.php`

         <?php
         /**
          * Program sederhana pendefinisian class dan pemanggilan class.
          */
         
          class Mobil
          {
             private $warna;
             private $merk;
             private $harga;
         
             public function __construct()
             {
                 $this->warna = "Biru";
                 $this->merk = "BMW";
                 $this->harga = "10000000";
             }
         
             public function gantiWarna ($warnaBaru)
             {
                 $this->warna = $warnaBaru;
             }
         
             public function tampilWarna ()
             {
                 echo "Warna mobilnya : ". $this->warna;
             }
          }
         
          // membuat objek mobil
          $a = new Mobil();
          $b = new Mobil();
         
          // memanggil objek
          echo "<b>Mobil pertama</b><br>";
          $a->tampilWarna();
          echo "<br>Mobil pertama ganti warna<br>";
          $a->gantiWarna("Merah");
          $a->tampilWarna();
         
         
          // memanggil objek
          echo "<br><b>Mobil kedua</b><br>";
          $b->gantiWarna("Hijau");
          $b->tampilWarna();
          ?>


![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/095e39b2-8275-4af2-be5b-6d1aaa217e58)


- **Class Library**
Class Library merupakan pustaka kode program yang dapat digunakan bersama pada beberapa file yang berbeda (konsep modularisasi). Class library menyimpan fungsi-fungsi atau class object komponen untuk memudahkan dalam proses development aplikasi.

2. Buat file php baru dengan nama `form.php`

         <?php
         /**
          * Nama Class: Form
          * Deskripsi: Class untuk membuat form inputan text sederhana
          */
         
         class Form
         {
             private $fields = array();
             private $action;
             private $submit = "Submit Form";
             private $jumField = 0;
             public function __construct($action, $submit)
             {
                 $this->action = $action;
                 $this->submit = $submit;
             }
             public function displayForm()
             {
                 echo "<form action='" . $this->action . "' method='POST'>";
                 echo '<table width="100%" border="0">';
                 for ($j = 0; $j < count($this->fields); $j++) {
                     echo "<tr><td
             align='right'>" . $this->fields[$j]['label'] . "</td>";
                     echo "<td><input type='text'
             name='" . $this->fields[$j]['name'] . "'></td></tr>";
                 }
                 echo "<tr><td colspan='2'>";
                 echo "<input type='submit' value='" . $this->submit . "'></td></tr>";
                 echo "</table>";
             }
         
             public function addField($name, $label)
             {
                 $this->fields[$this->jumField]['name'] = $name;
                 $this->fields[$this->jumField]['label'] = $label;
                 $this->jumField++;
             }
         
         }
         ?>


3. Buat file php baru dengan nama `form_input.php`

         <?php
         /**
         * Program memanfaatkan Program 10.2 untuk membuat form inputan sederhana.
         **/
         include "form.php";
         echo "<html><head><title>Mahasiswa</title></head><body>";
         $form = new Form("","Input Form");
         $form->addField("txtnim", "Nim");
         $form->addField("txtnama", "Nama");
         $form->addField("txtalamat", "Alamat");
         echo "<h3>Silahkan isi form berikut ini :</h3>";
         $form->displayForm();
         echo "</body></html>";
         ?>

![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/532eda93-0439-44be-a21d-0593a5c3d0c8)


4. Buat file dengan nama `database.php`

         <?php
         
         class Database
         {
             protected $host;
             protected $user;
             protected $password;
             protected $db_name;
             protected $conn;
         
             public function __construct()
             {
                 $this->getConfig();
                 $this->conn = new mysqli($this->host, $this->user, $this->password, $this->db_name);
                 if ($this->conn->connect_error) {
                     die("Connection failed: " . $this->conn->connect_error);
                 }
             }
         
             private function getConfig()
             {
                 include_once("config.php");
                 $this->host = $config['host'];
                 $this->user = $config['username'];
                 $this->password = $config['password'];
                 $this->db_name = $config['db_name'];
             }
             
             public function query($sql)
             {
                 return $this->conn->query($sql);
             }
         
             public function get($table, $where = null)
             {
                 if ($where) {
                     $where = " WHERE " . $where;
                 }
                 $sql = "SELECT * FROM " . $table . $where;
                 $sql = $this->conn->query($sql);
                 $sql = $sql->fetch_assoc();
                 return $sql;
             }
         
             public function insert($table, $data)
             {
                 if (is_array($data)) {
                     foreach ($data as $key => $val) {
                         $column[] = $key;
                         $value[] = "'{$val}'";
                     }
                     $columns = implode(",", $column);
                     $values = implode(",", $value);
                 }
                 $sql = "INSERT INTO " . $table . " (" . $columns . ") VALUES (" . $values . ")";
                 $sql = $this->conn->query($sql);
                 if ($sql == true) {
                     return $sql;
                 } else {
                     return false;
                 }
             }
         
             public function update($table, $data, $where)
             {
                 $update_value = "";
                 if (is_array($data)) {
                     foreach ($data as $key => $val) {
                         $update_value[] = "$key='{$val}'";
                     }
                     $update_value = implode(",", $update_value);
                 }
         
                 $sql = "UPDATE " . $table . " SET " . $update_value . " WHERE " . $where;
                 $sql = $this->conn->query($sql);
                 if ($sql == true) {
                     return true;
                 } else {
                     return false;
                 }
             }
         
         
             public function delete($table, $filter)
             {
                 $sql = "DELETE FROM " . $table . " " . $filter;
                 $sql = $this->conn->query($sql);
                 if ($sql == true) {
                     return true;
                 } else {
                     return false;
                 }
             }
         }
         ?>
   

## Pertanyaan dan tugas
1. Membuat file `config.php`

         <?php
         
         return [
             'host' => 'localhost',
             'username' => 'root',
             'password' => '',
             'db_name' => 'latihan1',
         ];
      
         ?>

2. Membuat file `database.php`

         <?php
         
         class Database
         {
             protected $host;
             protected $user;
             protected $password;
             protected $db_name;
             protected $conn;
         
             public function __construct()
             {
                 $this->getConfig();
                 $this->conn = new mysqli($this->host, $this->user, $this->password, $this->db_name);
                 if ($this->conn->connect_error) {
                     die("Connection failed: " . $this->conn->connect_error);
                 }
             }
         
             private function getConfig()
             {
                 include_once("config.php");
                 $this->host = $config['host'];
                 $this->user = $config['username'];
                 $this->password = $config['password'];
                 $this->db_name = $config['db_name'];
             }
             
             public function query($sql)
             {
                 return $this->conn->query($sql);
             }
         
             public function get($table, $where = null)
             {
                 if ($where) {
                     $where = " WHERE " . $where;
                 }
                 $sql = "SELECT * FROM " . $table . $where;
                 $sql = $this->conn->query($sql);
                 $sql = $sql->fetch_assoc();
                 return $sql;
             }
         
             public function insert($table, $data)
             {
                 if (is_array($data)) {
                     foreach ($data as $key => $val) {
                         $column[] = $key;
                         $value[] = "'{$val}'";
                     }
                     $columns = implode(",", $column);
                     $values = implode(",", $value);
                 }
                 $sql = "INSERT INTO " . $table . " (" . $columns . ") VALUES (" . $values . ")";
                 $sql = $this->conn->query($sql);
                 if ($sql == true) {
                     return $sql;
                 } else {
                     return false;
                 }
             }
         
             public function update($table, $data, $where)
             {
                 $update_value = "";
                 if (is_array($data)) {
                     foreach ($data as $key => $val) {
                         $update_value[] = "$key='{$val}'";
                     }
                     $update_value = implode(",", $update_value);
                 }
         
                 $sql = "UPDATE " . $table . " SET " . $update_value . " WHERE " . $where;
                 $sql = $this->conn->query($sql);
                 if ($sql == true) {
                     return true;
                 } else {
                     return false;
                 }
             }
         
         
             public function delete($table, $filter)
             {
                 $sql = "DELETE FROM " . $table . " " . $filter;
                 $sql = $this->conn->query($sql);
                 if ($sql == true) {
                     return true;
                 } else {
                     return false;
                 }
             }
         }
         ?>
   
3. Konfigurasikan dengan praktikum sebelumnya

-> **Output Tugas :**

## `home.php`

![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/b4e45e68-7404-4065-822c-275f81bdb604)


## `about.php`

   ![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/18505c09-f4fa-4a78-a7c4-c44064f87986)


## `kontak.php`

   ![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/30022dd4-68f7-4127-8420-0c9aa4ed1f4e)


## `tambah.php`

   ![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/676b0424-5921-49b7-800f-58248a31535f)


## `ubah.php`

   ![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/8db68828-a023-47f3-b275-9496b0ab3f42)


## `index.php`

   ![image](https://github.com/adeliadhea06/Lab10Web/assets/115794875/d64efd3b-beb9-4495-917f-141742f919a0)


## Selesai, Terima Kasih
