# BEST PRACTICE JQUERY DATATABLE

file html
```html
<table id="tableData" class="table table-bordered table-striped" width="100%">
    <thead>
        <tr class="info">
            <th>No</th>
            <th>Perusahaan</th>
            <th>Alamat</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
    </tbody>
</table>
```

file js
```js
var table = $('#tableData').DataTable({
    language: {
        emptyTable: "Tidak ada data yang ditemukan",
        loadingRecords: "Sedang memuat...",
    },
    processing: true,
    serverSide: true,
    order: [],
    ajax: {
        url: "<?php echo site_url('api/perusahaan') ?>",
        type: "POST",
        data: function(data) {
            data.tahun = $('[name=tahun]').val()
        }
    },
    columns: [{
            data: null,
            title: 'No',
            render: function(data, type, row, meta) {
                var currentPage = table.page.info().page;
                var rowNumber = currentPage * table.page.len() + (meta.row + 1);

                return rowNumber + '.';
            },
            orderable: false,
            searchable: false,
        }, {
            data: "nama_perusahaan",
            orderable: false,
        },
        {
            data: "alamat",
            orderable: false,
        },
        {
            targets: -1,
            data: null,
            orderable: false,
            defaultContent: '',
            render: function(data, type, row) {
                var perusahaanId = row.id_perusahaan;
                var aksiHtml = /* html */ `
                <div class="btn-group" role="group">
                    <div class="btn-group" role="group">
                        <button type="button" class="btn btn-primary btn-sm dropdown-toggle" data-bs-toggle="dropdown" aria-expanded="false">
                            Aksi
                        </button>
                        <ul class="dropdown-menu">
                            <li><a class="dropdown-item btn-detail" data-id="${perusahaanId}" href="#">Ubah</a></li>
                            <li><a class="dropdown-item btn-delete" data-id="${perusahaanId}" href="#">Hapus</a></li>
                        </ul>
                    </div>
                </div>
                `;
                return aksiHtml;
            },
        },
    ],
    initComplete: function() {
        $('#tableData_filter input').attr('placeholder', 'Tekan Enter untuk cari');

        $('#tableData_filter input').unbind().bind('keyup', function(e) {
            if (e.keyCode === 13) {
                table.search(this.value).draw();
            }
        });
    }
});

function reload_table() {
    table.ajax.reload();
}

$('[name=tahun]').on('change', function() {
    reload_table();
})
```