# ENTER KEY TO SEARCH DATATABLE

1. tambahkan secara terpisah

```js
<script>
    $(document).ready(function () {
        var table = $('#myTable').DataTable({
            "processing": true,
            "serverSide": true,
            "ajax": "server-side-script.php",
        });

        // Fungsi pencarian saat menekan tombol Enter
        $('#myTable_filter input').unbind().bind('keyup', function (e) {
            if (e.keyCode === 13) {
                table.search(this.value).draw();
            }
        });
    });
</script>
```

2. tambahkan function initComplete

```js
<script>
    var table = $('#myTable').DataTable({
        "processing": true,
        "serverSide": true,
        "ajax": "server-side-script.php",,
        "initComplete": function() {
            // Menambahkan placeholder dengan pesan petunjuk
            $('#table_filter input').attr('placeholder', 'Tekan Enter untuk mencari');

            // Menangkap event keyup pada kolom pencarian
            $('#table_filter input').unbind().bind('keyup', function(e) {
                if (e.keyCode === 13) { // 13 adalah kode tombol Enter
                    table.search(this.value).draw();
                }
            });
        }
    });
</script>
```