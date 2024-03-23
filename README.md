# Reflection
## Commit 1 Reflection notes 
Fungsi ```handle_connection``` bertugas untuk menangani setiap koneksi yang masuk ke server. Fungsi ini menerima koneksi ```TCP (Transmission Control Protocol)``` yang merupakan parameter ```stream```.
  - ```mut stream: TcpStream:```
    Parameter ini merupakan TCP stream dari koneksi klien. Kata kunci mut menunjukkan bahwa stream ini bersifat mutable, sehingga dapat diubah. 
  - ```let buf_reader = BufReader::new(&mut stream);:```
    Untuk membuat pembaca buffer baru yang membungkus stream, baris ini akan membaca dari stream.
  - ```let http_request: Vec<_> = buf_reader.lines()...collect();:```
    Bagian ini membaca setiap baris dari HTTP request dan mengumpulkannya ke dalam vektor. Setiap baris mewakili satu header dalam HTTP request.
  - ```println!("Request: {:#?}", http_request);:```
    Baris ini mencetak HTTP request ke konsol.
Dapat disimpulkan bahwa, fungsi ```handle_connection``` merupakan tempat sebagian besar logika server web berada. fungsi ini bertugas untuk menerima HTTP request dari user, mencatatnya, dan kemudian menutup koneksi.

## Commit 2 Reflection notes
![Screenshot 2024-03-23 133932](https://github.com/hotchlck/advprog-modul6/assets/126342746/abf1ab8d-5862-4f59-ad4c-4b3596a15cb6)

```
  let status_line = "HTTP/1.1 200 OK"; 
    let contents = fs::read_to_string("hello.html").unwrap(); 
    let length = contents.len(); 
 
    let response = 
        format!("{status_line}\r\nContent-Length: 
{length}\r\n\r\n{contents}"); 
 
    stream.write_all(response.as_bytes()).unwrap();
```
  - Variabel ```status_line``` menyimpan status respons HTTP yang menandakan bahwa request telah berhasil diproses.
  - Fungsi ```read_to_string ```digunakan untuk membaca isi file hello.html dan menyimpannya dalam variabel contents. unwrap() digunakan untuk mengambil hasil jika operasi berhasil, atau menghentikan eksekusi dengan ‘panic’ jika terjadi kesalahan.
  - Variabel ```length``` menyimpan panjang dari konten yang dibaca, yang akan digunakan untuk menentukan nilai ```Content-Length``` dalam header HTTP.
  - Menggunakan makro ```format!``` untuk membuat string yang berisi respons HTTP lengkap, termasuk status line, header Content-Length, dan isi file HTML.
  - Metode ```write_all``` digunakan untuk menulis string respons ke stream, yang merupakan koneksi jaringan ke client. as_bytes() mengonversi string ke array byte, dan unwrap() dipanggil untuk menangani potensi kesalahan saat menulis ke stream.

Kode ini bertujuan  untuk menangani HTTP request dari user, membaca file HTML, dan mengirimkan respons HTTP.

## Commit 3 Reflection notes 
![Screenshot 2024-03-23 133946](https://github.com/hotchlck/advprog-modul6/assets/126342746/fa25c720-4963-4da5-a53a-324ad41b8da9)
Aplikasi akan memberikan response static file 404.html apabila ada request yang tidak sesuai. 
```
  fn handle_connection(mut stream: TcpStream) {
      let buf_reader = BufReader::new(&mut stream);
  ```rust
  fn handle_connection(mut stream: TcpStream) {
      let buf_reader = BufReader::new(&mut stream);
      let request_line = buf_reader.lines().next().unwrap().unwrap();
  
      if request_line == "GET / HTTP/1.1" {
          let status_line = "HTTP/1.1 200 OK";
          let contents = fs::read_to_string("hello.html").unwrap();
          let length = contents.len();
  
          let response = format!(
              "{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}"
          );
  
          stream.write_all(response.as_bytes()).unwrap();
      } else {
          let status_line = "HTTP/1.1 404 NOT FOUND";
          let contents = fs::read_to_string("404.html").unwrap();
          let length = contents.len();
  
          let response = format!(
              "{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}"
          );
  
          stream.write_all(response.as_bytes()).unwrap();
      }
  }
```
Dilakukan refactoring pada kode tersebut : 
```
    fn handle_connection(mut stream: TcpStream) {
      let buf_reader = BufReader::new(&mut stream);
      let request_line = buf_reader.lines().next().unwrap().unwrap();
  
      let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
          ("HTTP/1.1 200 OK", "hello.html")
      } else {
          ("HTTP/1.1 404 NOT FOUND", "404.html")
      };
  
      let contents = fs::read_to_string(filename).unwrap();
      let length = contents.len();
  
      let response =
          format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");
  
      stream.write_all(response.as_bytes()).unwrap();
  }
```
Dengan merapikan kode melalui refactoring, kita dapat memastikan bahwa blok if-else hanya memuat elemen-elemen yang benar-benar berbeda antara kedua situasi, dan mnegeliminasi kode yang ditulis berulang-ulang.

## Commit 4 Reflection notes 
Pada server single-threaded, terdapat dua jenis request: satu untuk path utama ```("/")``` dan satu lagi untuk path ```"/sleep"``. Request ke path utama langsung diproses, sedangkan permintaan ke ```"/sleep"``` memerlukan waktu tunggu selama sepuluh detik.
Jika request ke ```"/sleep"``` dilakukan terlebih dahulu, request berikutnya, bahkan ke path utama, harus menunggu sampai request ```"/sleep"``` selesai karena server memproses request secara berurutan. 
Ini menunjukkan keterbatasan server single-threaded, di mana request yang membutuhkan waktu lama akan menghambat pemrosesan request lainnya, dan masalah ini akan bertambah buruk dengan peningkatan request dari user.

Dalam versi baru fungsi ```handle_connection```, ada penanganan khusus untuk request ke ```"/sleep"``` yang meniru request yang membutuhkan waktu lama untuk diproses dengan menambahkan jeda sepuluh detik. 
Ketika dua jendela browser dibuka secara bersamaan, satu dengan alamat ```127.0.0.1/sleep``` dan yang lainnya dengan ```127.0.0.1```, server tidak akan memproses request kedua sampai jeda pada request pertama selesai, 
menunjukkan bagaimana server single-threaded menangani permintaan secara serial.
