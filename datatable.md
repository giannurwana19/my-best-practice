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
        search: "Cari:",
        processing: 'Sedang memproses...',
        lengthMenu: "Tampilkan _MENU_ data per halaman",
        info: "Menampilkan _START_ sampai _END_ dari _TOTAL_ data",
        emptyTable: "Tidak ada data yang tersedia",
        infoEmpty: "Menampilkan 0 sampai 0 dari 0 data",
        zeroRecords: "Data tidak ditemukan",
        infoFiltered: "(difilter dari _MAX_ total data)",
        paginate: {
            first: "Pertama",
            last: "Terakhir",
            next: "Selanjutnya",
            previous: "Sebelumnya"
        }
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

versi lengkap 

file js
```js
var table = $('#tableData').DataTable({
    processing: true,
    serverSide: true,
    order: [],
    preDrawCallback: function(settings) {
        //
    },
    drawCallback: function(settings) {
        hideLoading();
    },
    ajax: {
        url: "{{ route('bookings.datatable') }}",
        type: "POST",
        data: function(data) {
            data.transport_type_id = $('[name=transport_type_id]').val();
            data.tgl_awal = $('[name=tgl_awal]').val();
            data.tgl_akhir = $('[name=tgl_akhir]').val();
            data.origin = $('[name=origin]').val();
            data.destination = $('[name=destination]').val();
            data.booking_status_id = $('[name=booking_status_id]').val();
            data.booking_no = $('[name=booking_no]').val();
            data.move_type = $('[name=move_type]').val();
            data.load_type = $('[name=load_type]').val();
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
        },
        {
            data: 'booking_no',
            name: 'booking_no',
            title: 'Booking No'
        },
        {
            data: 'booking_transport.transport_label',
            name: 'transport_type_id',
            title: 'Transport Type',
            searchable: false,
        },
        {
            data: 'load_type',
            name: 'load_type',
            title: 'Load Type'
        },
        {
            data: 'move_type',
            name: 'move_type',
            title: 'Move Type'
        },
        {
            data: 'booking_status.status_label',
            name: 'booking_status_id',
            title: 'Status',
            searchable: false,
        },
        {
            data: 'origin_name',
            name: 'origin',
            title: 'Origin',
            searchable: false,
            orderable: false
        },
        {
            data: 'destination_name',
            name: 'destination',
            title: 'Destination',
            searchable: false,
            orderable: false
        },
        {
            data: 'request_date_formatted',
            name: 'created_at',
            title: 'Request Date'
        },
        {
            data: 'action',
            name: 'action',
            title: 'Aksi',
            orderable: false,
            searchable: false,
        }
    ],
    initComplete: function() {
        $('#tableData_filter input').attr('placeholder', 'Tekan Enter untuk cari');

        $('#tableData_filter input').unbind().bind('keyup', function(e) {
            if (e.keyCode === 13) {
                table.search(this.value).draw();
            }
        });
    },
    language: {
        emptyTable: "Tidak ada data yang ditemukan",
        loadingRecords: "Sedang memuat...",
        search: "Cari:",
        processing: 'Sedang memproses...',
        lengthMenu: "Tampilkan _MENU_ data per halaman",
        info: "Menampilkan _START_ sampai _END_ dari _TOTAL_ data",
        emptyTable: "Tidak ada data yang tersedia",
        infoEmpty: "Menampilkan 0 sampai 0 dari 0 data",
        zeroRecords: "Data tidak ditemukan",
        infoFiltered: "(difilter dari _MAX_ total data)",
        paginate: {
            first: "Pertama",
            last: "Terakhir",
            next: "Selanjutnya",
            previous: "Sebelumnya"
        }
    },
});
```