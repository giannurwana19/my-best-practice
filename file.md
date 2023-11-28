# Bekerja dengan file

### Upload file codeigniter 3
```php
$config['upload_path'] = './assets/files/';
$config['allowed_types'] = 'jpg|jpeg|png|pdf';
$config['max_size'] = 3072;
$config['file_name'] = 'file-bukti-pemesanan-hotel' . uniqid();

$this->load->library('upload', $config);

if ($this->upload->do_upload('bukti_pemesanan_hotel')) {
  $fileData = $this->upload->data();
  $fileName = $fileData['file_name']; // nama dari file name
} else {
	http_response_code(400);
	header("Content-Type: application/json");
	echo json_encode([
    'success' => false,
    'message' => $this->upload->display_errors(),
  ]);
  return;
}
```

### Upload file multiple codeigniter 3
```php
if (isset($_FILES['file_dokumen'])) {
    if (count($_FILES['file_dokumen']['name']) > 0) {
        if (isset($detail_pengaduan->file_dokumen)) {
            $json_document = json_decode($detail_pengaduan->file_dokumen);
        } else {
            $json_document = array();
        }

        $filesCount =  count($_FILES['file_dokumen']['name']);

        for ($i = 0; $i < $filesCount; $i++) {
            $_FILES['dokumen']['name'] = $_FILES['file_dokumen']['name'][$i];
            $_FILES['dokumen']['type'] = $_FILES['file_dokumen']['type'][$i];
            $_FILES['dokumen']['tmp_name'] = $_FILES['file_dokumen']['tmp_name'][$i];
            $_FILES['dokumen']['error'] = $_FILES['file_dokumen']['error'][$i];
            $_FILES['dokumen']['size'] = $_FILES['file_dokumen']['size'][$i];

            $config_foto['upload_path'] = './service/uploads/';
            $config_foto['allowed_types'] = 'jpg|jpeg|png|pdf';
            $config_foto['max_size'] = 2048;
            $config_foto['file_name'] = 'foto-dokumen-' . uniqid();

            $this->load->library('upload', $config_foto);
            $this->upload->initialize($config_foto);

            if ($this->upload->do_upload('dokumen')) {
                $fileData = $this->upload->data();
                $fileName = $fileData['file_name']; // nama dari file
            }else {
              http_response_code(400);
              header("Content-Type: application/json");
              echo json_encode([
                'success' => false,
                'message' => $this->upload->display_errors(),
              ]);
              return;
            }
        }
    }
}
```

### Hapus file PHP Native
```php
$link_image = './assets/files/' . $galeri->image;

if (file_exists($link_image)) {
  unlink($link_image);
}
```