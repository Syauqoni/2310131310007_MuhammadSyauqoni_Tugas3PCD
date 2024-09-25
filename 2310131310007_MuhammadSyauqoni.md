<div align="center">

### TUGAS 3  
### PEMROSESAN CITRA DIGITAL  
### (ABKC6306)

<img src="logoULM.jpg" alt="drawing" width="200"/>

### DISUSUN OLEH:  
Muhammad Syauqoni (2310131310007)  
### DOSEN PENGAMPU:  
Dr. Harja Santana Purba, M.Kom./Novan A.B. Saputra, S.Kom., M.T

### PROGRAM STUDI PENDIDIKAN KOMPUTER  
### FAKULTAS KEGURUAN DAN ILMU PENDIDIKAN  
### UNIVERSITAS LAMBUNG MANGKURAT  
### 2024


<br>
<br>
<br>
<br>
<br>
<br>

</div>

## Daftar Isi

- [Daftar Isi](#daftar-isi)
  - [Pendahuluan](#pendahuluan)
  - [Halftoning](#halftoning)
  - [Pattering](#pattering)
  - [contoh Program Pattering](#contoh-program-pattering)
  - [Output](#output)
  - [Dithering](#dithering)
  - [Contoh Program Dithering](#contoh-program-dithering)
  - [Output](#output-1)
  - [Daftar Pustaka](#daftar-pustaka)
  

<br>
<br>
<br> 

### Pendahuluan
Dalam era digital saat ini, pemrosesan citra memainkan peran penting dalam berbagai aplikasi, mulai dari desain grafis hingga pengolahan gambar medis. Di antara teknik-teknik tersebut, halftoning, pattering, dan dithering muncul sebagai metode yang efektif untuk mengatasi keterbatasan dalam representasi warna dan detail gambar.

Halftoning adalah teknik yang digunakan untuk mensimulasikan gradasi warna atau tingkat abu-abu pada media yang terbatas, seperti cetakan hitam-putih. Pattering, di sisi lain, merujuk pada penciptaan pola berulang atau terstruktur dalam gambar. Sementara itu, dithering adalah metode yang digunakan untuk mengatasi masalah kuantisasi dalam pemrosesan gambar.  
  
<br>

### Halftoning
    Halftoning atau analog halftoning adalah proses yang mensimulasikan corak abu-abu dengan memvariasikan  
    ukuran titik-titik hitam kecil yang tersusun dalam pola yang teratur. Teknik ini digunakan dalam  
    percetakan, serta industri penerbitan. Jika Anda memeriksa foto di koran, Anda akan melihat bahwa  
    gambar tersebut tersusun dari titik-titik hitam meskipun tampak tersusun dari abu-abu. pada Gambar  
    dibawah, titik kecil yang terletak di tengah disimulasikan dalam halftoning digital dengan mengisi  
    sel halftone tengah; Demikian pula, titik berukuran sedang yang terletak di sudut kiri atas  
    disimulasikan dengan mengisi empat sel di sudut kiri atas. Titik besar yang menutupi sebagian besar  
    area pada gambar ketiga disimulasikan dengan mengisi semua sel halftone.

<img src="halftoning.png" alt="drawing" width="200" />


### Pattering
    Pattering adalah teknik halftoning yang menggunakan pola tetap (pattern) untuk menggantikan intensitas  
    skala abu-abu dengan area hitam-putih. Pola yang lebih padat digunakan untuk area gelap, sementara  
    pola yang lebih ringan digunakan untuk area terang. Teknik ini biasanya diterapkan dalam bentuk  
    grid (kotak-kotak) yang telah ditentukan sebelumnya untuk mensimulasikan tingkat kecerahan.

<img src="pattering.1.png" alt="drawing" width="500" high="500"/>

### contoh Program Pattering

        
        function halftone_image = halftoning(input_image)
            if size(input_image, 3) == 3
                input_image = rgb2gray(input_image);
            end
            
            input_image = double(input_image) / 255;

            dither_matrix = [0  0  0.5  0.5;
                            0  0.5  0  0.5;
                            0.5  0  0  0.5;
                            0.5  0.5  0.5  0];
            
            [dither_rows, dither_cols] = size(dither_matrix);
            
            [rows, cols] = size(input_image);
            
            halftone_image = zeros(rows, cols);
            
            for i = 1:rows
                for j = 1:cols
                    dither_value = dither_matrix(mod(i-1, dither_rows) + 1, mod(j-1, dither_cols) + 1);
                    if input_image(i, j) > dither_value
                        halftone_image(i, j) = 1; % Putih
                    else
                        halftone_image(i, j) = 0; % Hitam
                    end
                end
            end
            
            
            imshow(halftone_image, 'InitialMagnification', 'fit');
            colormap(gray);
            title('Halftone Image');
        end


        input_image = imread('logoULM.jpg'); % Ganti dengan path citra Anda
        halftoning(input_image);

        imshow(img);  
            



### Output

<img src="halftoning_pattering.png" alt="drawing" width="500" high="500"/>

<br>
<br>
<br>

### Dithering
    Dithering adalah metode halftoning yang menambahkan noise terkontrol ke dalam gambar untuk mensimulasikan  
    gradasi abu-abu. Metode ini lebih canggih dibandingkan pattering karena menghasilkan tampilan yang  
    lebih halus dengan mendistribusikan kesalahan kuantisasi  di antara piksel. Dengan dithering,  
    gambar hitam-putih dapat terlihat seolah-olah memiliki banyak tingkat kecerahan yang berbeda.  
    Masalah dithering yang umum terjadi adalah menghasilkan artefak pola yang diperkenalkan oleh  
    matriks thresholding tetap. Gambar berikut menunjukkan contoh operasi dithering.
      
<img src="DIthering.1.png" alt="drawing" width="500" high="500"/>
    
    

### Contoh Program Dithering

        img = imread('logoULM.jpg');

        if size(img, 3) == 3
        img = rgb2gray(img);
        endif

        img = double(img);

        [rows, cols] = size(img);

        for y = 1:rows
            for x = 1:cols
                old_pixel = img(y, x);
                new_pixel = 0;
                if old_pixel > 128
                    new_pixel = 255;
                endif
                img(y, x) = new_pixel;
                quant_error = old_pixel - new_pixel;
                
                if x + 1 <= cols
                    img(y, x + 1) = img(y, x + 1) + quant_error * 7 / 16;
                endif
                if y + 1 <= rows && x > 1
                    img(y + 1, x - 1) = img(y + 1, x - 1) + quant_error * 3 / 16;
                endif
                if y + 1 <= rows
                    img(y + 1, x) = img(y + 1, x) + quant_error * 5 / 16;
                endif
                if y + 1 <= rows && x + 1 <= cols
                    img(y + 1, x + 1) = img(y + 1, x + 1) + quant_error * 1 / 16;
                endif
            endfor
        endfor

        img = uint8(img);

        imshow(img);

### Output

<img src="Dithering.png" alt="drawing" width="500" high="500"/>

### Daftar Pustaka

[Digital Halftoning](https://people.ece.ubc.ca/irenek/techpaps/introip/manual04.html)
[Malay K. Pakhira(2011). Digital Image Processing and Pattern Recognition. Prentice-Hall of India Pvt.Limited.](https://www.google.co.id/books/edition/Digital_Image_Processing_and_Pattern_Rec/bQ4dGTfo8-sC?hl=id&gbpv=1&dq=pattering+dan+dithering&pg=PA54&printsec=frontcoverhttps://www.google.co.id/books/edition/Digital_Image_Processing_and_Pattern_Rec/bQ4dGTfo8-sC?hl=id&gbpv=1&dq=pattering+dan+dithering&pg=PA54&printsec=frontcove)  