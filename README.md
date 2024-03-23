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

## Commit 2 R
