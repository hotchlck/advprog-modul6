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
![Screenshot 2024-03-23 133932](https://github.com/hotchlck/advprog-modul6/assets/126342746/10d07625-7617-4170-b9b2-cf9d351e01fb)
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
