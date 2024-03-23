# Reflection
## Commit 1 Reflection notes 
    ```fn handle_connection(mut stream: TcpStream) ```
    Fungsi ```handle_connection``` bertugas untuk menangani setiap koneksi yang masuk ke server. Fungsi ini menerima koneksi          ```TCP (Transmission Control Protocol)``` yang merupakan parameter ```stream```.
    let buf_reader = BufReader::new(&mut stream);
    BufReader untuk membaca data dari koneksi ```TCP```.
    ```buf_reader.lines()``` 
    Metode ini mengembalikan iterator yang menghasilkan text Bufreader. 
