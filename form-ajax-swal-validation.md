# FORM AJAX WITH SWEET ALERT 2 & JQUERY VALIDATION

```js
$(document).ready(function() {
    $("#formSuratMasuk").validate({
        rules: {
            no_surat: {
                required: true
            },
            kode_surat_id: {
                required: true
            },
            tipe_surat_keluar_id: {
                required: true
            },
            tanggal_surat: {
                required: true
            },
            pengirim: {
                required: true
            },
            perihal: {
                required: true,
            },
            berkas: {
                extension: 'pdf',
                filesize: {{ $maxSize }},
            }
        },
        messages: {
            no_surat: {
                required: 'nomor surat wajib diisi',
            },
            kode_surat_id: {
                required: 'kode surat wajib diisi',
            },
            tipe_surat_keluar_id: {
                required: 'tipe surat keluar wajid diisi',
            },
            tanggal_surat: {
                required: 'tanggal surat wajib diisi',
            },
            pengirim: {
                required: 'pengirim wajib diisi'
            },
            perihal: {
                required: 'perihal wajib diisi',
            },
            berkas: {
                extension: 'berkas file harus bertipe .pdf',
                filesize: 'berkas file tidak boleh lebih dari 5MB',
            }
        },
        submitHandler: function(form) {
            Swal.fire({
                title: 'Konfirmasi',
                text: "Apakah Anda yakin dengan data yang disimpan?",
                icon: 'question',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                confirmButtonText: 'Ya',
                cancelButtonColor: '#d33',
                cancelButtonText: 'Tidak',
            }).then((result) => {
                if (result.value) {
                    showLoading();
                    $.ajax({
                        type: form.method,
                        url: form.action,
                        data: new FormData(form),
                        processData: false,
                        contentType: false,
                        success: function(response) {
                            hideLoading();
                            Swal.fire({
                                title: 'Berhasil!',
                                text: response.message,
                                icon: 'success',
                                confirmButtonColor: '#3085d6',
                                confirmButtonText: 'Ya',
                                timer: 3000,
                            }).then(() => {
                                //
                            });
                        },
                        error: function(err, text) {
                            hideLoading();
                            Swal.fire({
                                title: 'Error',
                                text: err.responseJSON.message,
                                icon: 'error',
                                confirmButtonColor: '#3085d6',
                                confirmButtonText: 'Ya',
                                timer: 3000,
                            });
                        }
                    });
                }
            });
        },
        errorElement: 'span',
        errorPlacement: function(error, element) {
            error.addClass('invalid-feedback');
            element.closest('.form-group').append(error);
        },
        highlight: function(element, errorClass, validClass) {
            $(element).addClass('is-invalid');
        },
        unhighlight: function(element, errorClass, validClass) {
            $(element).removeClass('is-invalid');
        }
    });
});
```