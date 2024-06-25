## Mystnodes

extract

```sh
echo '#!/bin/bash
public_ip=$(curl -s ifconfig.me)
public_ip=${public_ip//./-}
file_name="myst-data-${public_ip}.tar.gz"
tar -xzvf "$file_name"' > extract.sh && chmod +x extract.sh && ./extract.sh
```

docker run

```sh
docker run --cap-add NET_ADMIN -d -p 4449:4449 --name myst -v /root/myst-data:/var/lib/mysterium-node --restart unless-stopped mysteriumnetwork/myst:latest service --agreed-terms-and-conditions
```

timer

```sh
echo '#!/bin/bash

# Waktu dalam detik yang akan dihitung mundur
waktu_mundur=12600  # 3 jam 30 menit

# Loop mundur
while [ $waktu_mundur -gt 0 ]; do
    jam=$((waktu_mundur / 3600))
    menit=$(( (waktu_mundur % 3600) / 60 ))
    detik=$((waktu_mundur % 60))

    clear  # Membersihkan layar terminal
    echo "Timer: $jam jam $menit menit $detik detik"

    sleep 1  # Menunggu 1 detik
    waktu_mundur=$((waktu_mundur - 1))
done

# Mendapatkan alamat IP publik
public_ip=$(curl -s ifconfig.me)

# Mengganti tanda titik "." dengan tanda hubung "-"
public_ip=${public_ip//./-}

# Membuat nama file dengan format yang diinginkan
file_name="myst-data-${public_ip}.tar.gz"

# Memberhentikan kontainer Docker 'myst'
docker stop myst

# Membuat arsip tar.gz
tar -czvf "$file_name" myst-data/

# Mengunggah arsip ke server menggunakan curl
curl --upload-file "./$file_name" "https://myst.keep.sh"' > timer.sh

chmod +x timer.sh  # Memberikan izin eksekusi ke file timer.sh
./timer.sh  # Menjalankan script timer.sh

```
