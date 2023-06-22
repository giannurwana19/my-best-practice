# Membuat select2 pagination dengan codeignter 3

tambahkan select2 (buka situs resminya)

*Note*: "jika pada admin template, maka load css select2 terlebih dahulu, kemudian diikuti css dari admin templatenya" 

```html
<link href="https://cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/css/select2.min.css" rel="stylesheet" />
<script src="https://cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/js/select2.min.js"></script>
```

file Controller (Codeigniter)
```php
  public function get_data_barang()
  {
      if ($_SERVER['REQUEST_METHOD'] == 'POST') {

          // Load library dan helper yang diperlukan
          $this->load->helper('url');
          $this->load->library('pagination');

          // Konfigurasi pagination
          $config['base_url']   = base_url('jenis_dokumen/get_data');
          $config['total_rows'] = $this->db->count_all('barang');  // Jumlah total baris data
          $config['per_page']   = 10;                                      // Jumlah data per halaman

          // Inisialisasi pagination
          $this->pagination->initialize($config);

          // Ambil data dari database menggunakan limit dan offset
          $offset             = $this->input->post('page') ? ($this->input->post('page') - 1) * $config['per_page'] : 0;
          $term               = trim($this->input->post('term'));
          $m_tipe_dokumen_id  = $this->input->post('m_tipe_dokumen_id');
          $m_jenis_dokumen_id = $this->input->post('m_jenis_dokumen_id');

          $this->db->select('id, nomor_ph AS `text`');
          $this->db->from('barang');
          $this->db->where('m_tipe_dokumen_id', $m_tipe_dokumen_id);
          $this->db->where('m_jenis_dokumen_id', $m_jenis_dokumen_id);
          $this->db->where('nomor LIKE', '%' . $term . '%');

          $this->db->limit($config['per_page'], $offset);

          $query      = $this->db->get();
          $results    = $query->result();
          $more_pages = count($results) >= $config['per_page'];

          $data = [
              "results"    => $results,
              "pagination" => [
                  "more" => $more_pages,
              ],
              'term'               => $term,
              'm_tipe_dokumen_id'  => $m_tipe_dokumen_id,
              'm_jenis_dokumen_id' => $m_jenis_dokumen_id,
          ];

          http_response_code(200);
          header("Content-Type: application/json");
          echo json_encode($data);
      } else {
          show_404();
      }
  }
```

file controller (laravel)

```php
  public function getCoverageArea(Request $request)
  {
      $term = trim($request->term);
      $posts = DB::table('area_sap')
          ->select(DB::raw("district_code as id, CONCAT(district_code, ' (', district_name, ', ', city_name,  ' - ', provinsi_name , ')') AS `text`"))
          ->where('district_name', 'LIKE',  '%' . $term . '%')
          ->orWhere('city_name', 'LIKE',  '%' . $term . '%')
          ->orWhere('provinsi_name', 'LIKE',  '%' . $term . '%')
          ->orderBy('provinsi_name', 'asc')->simplePaginate(10);

      $morePages = true;

      if (empty($posts->nextPageUrl())) {
          $morePages = false;
      }

      $results = array(
          "results" => $posts->items(),
          "pagination" => array(
              "more" => $morePages
          )
      );

      return response()->json($results);
  }
```

file javascript
```js
	function get_data_barang() {
		let m_tipe_dokumen_id = $('[name=m_tipe_dokumen_id]').val();
		let m_jenis_dokumen_id = $('[name=m_jenis_dokumen_id]').val();

		if (m_tipe_dokumen_id && m_jenis_dokumen_id) {
			$('[name=barang_id]').empty();

			$('[name=barang_id]').select2({
				placeholder: 'Pilih Nomor Produk Hukum',
				allowClear: true,
				ajax: {
					url: "barang/get_data_barang",
					dataType: 'json',
					delay: 250,
					method: 'post',
					data: function(params) {
						return {
							m_tipe_dokumen_id,
							m_jenis_dokumen_id,
							term: params.term || '',
							page: params.page || 1
						}
					},
					cache: true
				},
				language: {
					searching: function() {
						return 'Mencari...';
					},
					loadingMore: function() {
						return 'Memuat hasil lainnya...';
					},
					noResults: function() {
						return 'Tidak ada hasil yang ditemukan.';
					},
					placeholder: function() {
						return 'Pilih opsi...';
					}
				}
			});
		}
	}
```

pada saat tambah
```js
get_data_barang();
```

pada saat edit (buat element baru, kemudian isi nilainya)
```js
  let selectedNomor = 2;
  let selectedNomorText = "Indomie Bawang";

  let optionSelectedFromArea = $('<option selected="selected"></option>').val(selectedNomor)
    .text(selectedNomorText);

  $('[name=barang_id]').append(optionSelectedFromArea).trigger('change');
```

